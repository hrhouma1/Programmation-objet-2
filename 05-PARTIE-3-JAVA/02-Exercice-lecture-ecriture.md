
## `Main.java` — Lecture/Écriture de Fichiers (Scoreboard de Quiz)

```java
// Main.java
// Un seul fichier : lecture/écriture de fichiers + petit menu console + export JSON.
// Compile : javac Main.java
// Exécute : java Main
//
// Objectifs pédagogiques :
// - Lire un fichier texte ligne par ligne (BufferedReader, UTF-8)
// - Écrire en append / overwrite (BufferedWriter, Options NIO)
// - Créer le dossier data/ si nécessaire
// - Parser CSV simple, gérer les lignes invalides
// - Exporter en JSON (sans lib, string builder + escape)
// - Trier, filtrer (Top N)
// - try-with-resources + bonnes pratiques encodage

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.UncheckedIOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.StandardOpenOption;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.util.*;

public class Main {

    // ==========
    // CONSTANTES
    // ==========
    private static final Path DATA_DIR  = Path.of("data");
    private static final Path CSV_PATH  = DATA_DIR.resolve("scores.csv");
    private static final Path JSON_PATH = DATA_DIR.resolve("scores.json");
    private static final DateTimeFormatter TS_FMT = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");

    // =============
    // TYPE METIER
    // =============
    static class StudentScore {
        private final String name;
        private final int score;          // 0..100
        private final LocalDateTime when; // date d'enregistrement

        StudentScore(String name, int score, LocalDateTime when) {
            this.name = name;
            this.score = score;
            this.when = when;
        }
        public String name() { return name; }
        public int score() { return score; }
        public LocalDateTime when() { return when; }

        // CSV : name;score;timestamp
        public String toCsvLine() {
            return name.replace(";", ",") + ";" + score + ";" + TS_FMT.format(when);
        }

        public static Optional<StudentScore> fromCsvLine(String line) {
            if (line == null || line.isBlank() || line.startsWith("#")) return Optional.empty();
            String[] parts = line.split(";");
            if (parts.length != 3) return Optional.empty();
            String n = parts[0].trim();
            String s = parts[1].trim();
            String t = parts[2].trim();
            try {
                int sc = Integer.parseInt(s);
                LocalDateTime ts = LocalDateTime.parse(t, TS_FMT);
                if (n.isEmpty()) return Optional.empty();
                return Optional.of(new StudentScore(n, sc, ts));
            } catch (Exception e) {
                return Optional.empty();
            }
        }
    }

    // ==================
    // REPOSITORY FICHIERS
    // ==================
    static class ScoreRepository {

        public void ensureInitialized() {
            try {
                Files.createDirectories(DATA_DIR);
                if (!Files.exists(CSV_PATH)) {
                    try (BufferedWriter bw = Files.newBufferedWriter(
                            CSV_PATH, StandardCharsets.UTF_8,
                            StandardOpenOption.CREATE, StandardOpenOption.TRUNCATE_EXISTING)) {
                        bw.write("# name;score;timestamp");
                        bw.newLine();
                        // Quelques données de départ motivantes
                        write(bw, new StudentScore("Alice", 78, LocalDateTime.now().minusDays(2)));
                        write(bw, new StudentScore("Bob",   92, LocalDateTime.now().minusDays(1)));
                        write(bw, new StudentScore("Lina",  85, LocalDateTime.now().minusHours(10)));
                    }
                }
            } catch (IOException e) {
                throw new UncheckedIOException("Impossible d'initialiser le fichier " + CSV_PATH, e);
            }
        }

        public List<StudentScore> findAll() {
            List<StudentScore> out = new ArrayList<>();
            if (!Files.exists(CSV_PATH)) return out;
            try (BufferedReader br = Files.newBufferedReader(CSV_PATH, StandardCharsets.UTF_8)) {
                String line;
                while ((line = br.readLine()) != null) {
                    StudentScore.fromCsvLine(line).ifPresent(out::add);
                }
            } catch (IOException e) {
                throw new UncheckedIOException("Erreur de lecture: " + CSV_PATH, e);
            }
            return out;
        }

        public void add(StudentScore s) {
            try (BufferedWriter bw = Files.newBufferedWriter(
                    CSV_PATH, StandardCharsets.UTF_8,
                    StandardOpenOption.CREATE, StandardOpenOption.APPEND)) {
                write(bw, s);
            } catch (IOException e) {
                throw new UncheckedIOException("Erreur d'écriture (append): " + CSV_PATH, e);
            }
        }

        public void resetFileKeepingHeader() {
            try (BufferedWriter bw = Files.newBufferedWriter(
                    CSV_PATH, StandardCharsets.UTF_8,
                    StandardOpenOption.CREATE, StandardOpenOption.TRUNCATE_EXISTING)) {
                bw.write("# name;score;timestamp");
                bw.newLine();
            } catch (IOException e) {
                throw new UncheckedIOException("Erreur de réinitialisation: " + CSV_PATH, e);
            }
        }

        public void exportJson(Path out) {
            List<StudentScore> all = findAll();
            String json = toJson(all);
            try (BufferedWriter bw = Files.newBufferedWriter(
                    out, StandardCharsets.UTF_8,
                    StandardOpenOption.CREATE, StandardOpenOption.TRUNCATE_EXISTING)) {
                bw.write(json);
            } catch (IOException e) {
                throw new UncheckedIOException("Erreur d'écriture JSON: " + out, e);
            }
        }

        // Helpers
        private static void write(BufferedWriter bw, StudentScore s) throws IOException {
            bw.write(s.toCsvLine());
            bw.newLine();
        }

        private static String toJson(List<StudentScore> list) {
            StringBuilder sb = new StringBuilder();
            sb.append("[\n");
            for (int i = 0; i < list.size(); i++) {
                StudentScore s = list.get(i);
                sb.append("  {");
                sb.append("\"name\":\"").append(jsonEscape(s.name())).append("\",");
                sb.append("\"score\":").append(s.score()).append(",");
                sb.append("\"timestamp\":\"").append(jsonEscape(TS_FMT.format(s.when()))).append("\"");
                sb.append("}");
                if (i < list.size() - 1) sb.append(",");
                sb.append("\n");
            }
            sb.append("]\n");
            return sb.toString();
        }

        private static String jsonEscape(String in) {
            return in.replace("\\", "\\\\").replace("\"", "\\\"");
        }
    }

    // ============
    // APPLICATION
    // ============
    public static void main(String[] args) {
        System.out.println("=== Scoreboard de Quiz (Lecture/Écriture Fichiers) ===");
        ScoreRepository repo = new ScoreRepository();
        repo.ensureInitialized();

        try (Scanner sc = new Scanner(System.in, StandardCharsets.UTF_8)) {
            boolean run = true;
            while (run) {
                showMenu();
                System.out.print("Choix > ");
                String choice = sc.nextLine().trim();

                switch (choice) {
                    case "1" -> listAll(repo);
                    case "2" -> addOne(repo, sc);
                    case "3" -> topN(repo, sc);
                    case "4" -> exportJson(repo);
                    case "5" -> reset(repo, sc);
                    case "0" -> run = false;
                    default  -> System.out.println("Option inconnue.");
                }
            }
        }
        System.out.println("=== Fin ===");
    }

    // ======
    // MENUS
    // ======
    private static void showMenu() {
        System.out.println("\nMenu :");
        System.out.println(" 1) Lister tous les scores (lecture CSV)");
        System.out.println(" 2) Ajouter un score (écriture append)");
        System.out.println(" 3) Top N (lecture + tri)");
        System.out.println(" 4) Exporter en JSON (écriture overwrite)");
        System.out.println(" 5) Réinitialiser le fichier (conserver l'entête)");
        System.out.println(" 0) Quitter");
    }

    private static void listAll(ScoreRepository repo) {
        List<StudentScore> all = repo.findAll();
        if (all.isEmpty()) {
            System.out.println("Aucune donnée.");
            return;
        }
        System.out.println("-- Tous les scores --");
        for (StudentScore s : all) {
            System.out.printf(" - %-15s | %3d | %s%n", s.name(), s.score(), TS_FMT.format(s.when()));
        }
    }

    private static void addOne(ScoreRepository repo, Scanner sc) {
        System.out.print("Nom : ");
        String name = sc.nextLine().trim();
        System.out.print("Score (0..100) : ");
        String raw = sc.nextLine().trim();
        try {
            int score = Integer.parseInt(raw);
            score = Math.max(0, Math.min(100, score)); // clamp
            StudentScore s = new StudentScore(name, score, LocalDateTime.now());
            repo.add(s);
            System.out.println("Ajouté ✔");
        } catch (NumberFormatException e) {
            System.out.println("Score invalide.");
        }
    }

    private static void topN(ScoreRepository repo, Scanner sc) {
        System.out.print("N = ");
        String raw = sc.nextLine().trim();
        int n;
        try { n = Integer.parseInt(raw); } catch (Exception e) { System.out.println("N invalide."); return; }
        n = Math.max(1, n);

        List<StudentScore> all = repo.findAll();
        all.sort(Comparator.comparingInt(StudentScore::score).reversed());
        System.out.println("-- TOP " + n + " --");
        for (int i = 0; i < Math.min(n, all.size()); i++) {
            StudentScore s = all.get(i);
            System.out.printf("%2d) %-15s | %3d | %s%n", i+1, s.name(), s.score(), TS_FMT.format(s.when()));
        }
    }

    private static void exportJson(ScoreRepository repo) {
        repo.exportJson(JSON_PATH);
        System.out.println("Export JSON -> " + JSON_PATH.toAbsolutePath());
    }

    private static void reset(ScoreRepository repo, Scanner sc) {
        System.out.print("Confirmer (oui/non) : ");
        String confirm = sc.nextLine().trim().toLowerCase(Locale.ROOT);
        if (confirm.startsWith("o")) {
            repo.resetFileKeepingHeader();
            System.out.println("Réinitialisé ✔");
        } else {
            System.out.println("Annulé.");
        }
    }
}
```

---

## Comment utiliser

1. Sauvegarde le bloc dans **`Main.java`**.
2. Compile :

```bash
javac Main.java
```

3. Exécute :

```bash
java Main
```

* Le programme crée **`data/scores.csv`** s’il n’existe pas (avec 3 exemples).
* Tu peux **ajouter** des scores, **lire** le CSV, générer un **Top N**, **exporter JSON** → `data/scores.json`, **réinitialiser** le CSV (en gardant l’entête).

---

## Ce que vous allez apprendre (clés)

* Lecture **ligne par ligne** via `BufferedReader` (UTF-8).
* Écriture **append** et **overwrite** avec `StandardOpenOption`.
* Gestion des **dossiers** (`Files.createDirectories`).
* **Parsing CSV** simple, lignes invalides ignorées en sécurité.
* **Export JSON** sans dépendance (construction + échappement).
* Tri avec `Comparator`, contrôle d’entrées (`Scanner`), **try-with-resources**.

---

## Défis bonus (à faire sans modifier la structure)

1. **Validation stricte** : refuser les noms vides, afficher un message si la ligne CSV est corrompue (et compter les erreurs).
2. **Moyenne & médiane** : ajouter un menu “Statistiques” qui calcule moyenne, médiane et écart-type des scores.
3. **Filtre par date** : lister les scores saisis “aujourd’hui”.
4. **Sauvegarde JSON “jolie”** : indenter avec 2 espaces (actuellement c’est déjà lisible, mais rends l’indentation paramétrable).
5. **Import JSON → CSV** : proposer une option qui remplace le CSV actuel à partir d’un JSON (même structure).



# Annexe - exemple de csv


- Le programme crée automatiquement `data/scores.csv` au premier lancement, avec un en-tête et 3 exemples.

Si tu veux **partir de ton propre CSV**, place un fichier à `data/scores.csv` (UTF-8) au **format exact** :

```
# name;score;timestamp
Alice;78;2025-09-17 14:20:05
Bob;92;2025-09-18 10:30:00
Lina;85;2025-09-19 09:01:00
```

* **Délimiteur** : `;` (point-virgule).
* **Date/heure** : `yyyy-MM-dd HH:mm:ss` (24h).
* La ligne qui commence par `#` est un commentaire (facultatif).

### Adapter si ton CSV est différent

* Si ton fichier utilise la **virgule**: remplace `line.split(";")` par `line.split(",")`.
* Si le **format de date** diffère: change `DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss")` par ton pattern.

En bref : lance `java Main` → le dossier `data/` et le `scores.csv` seront créés tout seuls.
