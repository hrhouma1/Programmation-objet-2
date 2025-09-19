# 1) Les bases I/O en Java (panorama rapide)

* **Flux de caractères** (texte) : `FileReader` / `FileWriter` (mais préfèrer ↦ `BufferedReader`/`BufferedWriter` + UTF-8).
* **Flux d’octets** (binaire) : `FileInputStream` / `FileOutputStream`.
* **NIO modernisé** : `Files.newBufferedReader`, `Files.newBufferedWriter`, `Files.readString`, `Files.write`.
* **Toujours** utiliser `try-with-resources` pour fermer automatiquement les fichiers.

> Astuce : `FileReader`/`FileWriter` utilisent l’**encodage par défaut de la machine**. Pour du texte (UTF-8), privilégie `Files.newBufferedReader(path, UTF_8)` et `Files.newBufferedWriter(path, UTF_8)`.

---

# 2) Lire un fichier texte (3 façons)

### 2.1. Classique (Reader + Buffer)

```java
import java.io.*;
import java.nio.charset.StandardCharsets;

public class ReadDemo1 {
    public static void main(String[] args) {
        File file = new File("data/input.txt");

        try (Reader r = new InputStreamReader(new FileInputStream(file), StandardCharsets.UTF_8);
             BufferedReader br = new BufferedReader(r)) {

            String line;
            while ((line = br.readLine()) != null) {
                System.out.println(line); // traite la ligne
            }
        } catch (IOException e) {
            e.printStackTrace(); // à remplacer par un logging dans un vrai projet
        }
    }
}
```

### 2.2. NIO (conseillé pour UTF-8)

```java
import java.nio.file.*;
import java.nio.charset.StandardCharsets;
import java.io.*;
import java.util.stream.Stream;

public class ReadDemo2 {
    public static void main(String[] args) {
        Path path = Paths.get("data/input.txt");
        try (BufferedReader br = Files.newBufferedReader(path, StandardCharsets.UTF_8)) {
            for (String line; (line = br.readLine()) != null; ) {
                System.out.println(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### 2.3. Lecture en une fois (petits fichiers)

```java
import java.nio.file.*;
import java.nio.charset.StandardCharsets;
import java.io.IOException;

public class ReadDemo3 {
    public static void main(String[] args) throws IOException {
        String content = Files.readString(Path.of("data/input.txt"), StandardCharsets.UTF_8);
        System.out.println(content);
    }
}
```

> Référence utilitaire côté lecture (avec Apache Commons IO/IOUtils) dans tes fichiers fournis : chargement d’un fichier entier en String (utile pour du JSON ou des petits fichiers).&#x20;

---

# 3) Écrire dans un fichier texte (5 recettes)

### 3.1. Classique (Writer + Buffer)

```java
import java.io.*;
import java.nio.charset.StandardCharsets;

public class WriteDemo1 {
    public static void main(String[] args) {
        File file = new File("data/out.txt");

        try (Writer w = new OutputStreamWriter(new FileOutputStream(file), StandardCharsets.UTF_8);
             BufferedWriter bw = new BufferedWriter(w)) {

            bw.write("Bonjour !");
            bw.newLine();
            bw.write("Deuxième ligne.");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### 3.2. Append (ajouter à la fin)

```java
try (Writer w = new OutputStreamWriter(new FileOutputStream("data/log.txt", true),
                                       StandardCharsets.UTF_8);
     BufferedWriter bw = new BufferedWriter(w)) {

    bw.write("Nouvelle entrée de log");
    bw.newLine();
}
```

### 3.3. NIO (conseillé)

```java
import java.nio.file.*;
import java.nio.charset.StandardCharsets;
import java.io.IOException;

public class WriteDemo2 {
    public static void main(String[] args) throws IOException {
        Path p = Path.of("data/out2.txt");
        Files.writeString(p, "Texte initial\n", StandardCharsets.UTF_8);
        Files.writeString(p, "Ligne ajoutée\n", StandardCharsets.UTF_8, StandardOpenOption.APPEND);
    }
}
```

### 3.4. Tout le contenu d’un coup

```java
Files.write(Path.of("data/out3.txt"),
            "Contenu complet\n".getBytes(StandardCharsets.UTF_8));
```

### 3.5. Utilitaire fourni (Apache Commons IO)

Tu as un utilitaire `FileWriter.saveStringIntoFile(path, content)` qui encapsule l’écriture UTF-8 via `FileUtils.writeStringToFile`. Très pratique pour simplifier le code d’exemples.&#x20;

---

# 4) Lire/Écrire du **JSON** (aligné sur tes exemples)

Tu as un exemple complet qui **crée** un objet JSON d’employé (`net.sf.json` / json-lib), puis le **sauvegarde** via ton utilitaire d’écriture :
— construction d’objets/arrays, puis `utilities.FileWriter.saveStringIntoFile("json/employee.json", employee.toString());`&#x20;

### 4.1. Lire un JSON existant en String puis parser

```java
import java.nio.file.*;
import java.nio.charset.StandardCharsets;
import java.io.IOException;
import net.sf.json.JSONObject; // json-lib

public class JsonRead {
    public static void main(String[] args) throws IOException {
        String json = Files.readString(Path.of("data/student.json"), StandardCharsets.UTF_8);
        JSONObject obj = JSONObject.fromObject(json); // parse
        System.out.println("Nom : " + obj.optString("name"));
        System.out.println("Âge : " + obj.optInt("age"));
    }
}
```

### 4.2. Écrire (créer) un JSON et le sauvegarder

```java
import net.sf.json.JSONArray;
import net.sf.json.JSONObject;
import java.io.IOException;

public class JsonWrite {
    public static void main(String[] args) throws IOException {
        JSONObject student = new JSONObject();
        student.accumulate("name", "Alice");
        student.accumulate("age", 21);

        JSONArray skills = new JSONArray();
        skills.add("Java");
        skills.add("SQL");
        student.accumulate("skills", skills);

        // Utilitaire fourni (UTF-8)
        utilities.FileWriter.saveStringIntoFile("data/student_out.json", student.toString());
    }
}
```

> Si tu préfères rester **100% JDK**, Java n’embarque pas de parser JSON natif complet. Tu as choisi `net.sf.json` dans tes exemples : garde la cohérence pour ce cours. La lecture côté String peut réutiliser l’utilitaire lecture/IOUtils (voir référence lecture). &#x20;

---

# 5) Bonnes pratiques indispensables

1. **Encodage** : force **UTF-8** partout (`StandardCharsets.UTF_8`), évite `new FileReader(...)`/`new FileWriter(...)` sans contrôle d’encodage.
2. **try-with-resources** : ferme automatiquement les flux (moins de fuites).
3. **Taille des fichiers** : `Files.readString` convient aux **petits** fichiers ; pour gros fichiers, lis **ligne par ligne** avec `BufferedReader`.
4. **Chemins** : utilise `Path` (`Paths.get(...)`, `Path.of(...)`) plutôt que `new File(...)` pour rester moderne.
5. **Créer les dossiers** : `Files.createDirectories(path.getParent())` avant d’écrire si nécessaire.
6. **Append vs overwrite** : ajoute `StandardOpenOption.APPEND` pour ne pas écraser.
7. **Erreurs** : journalise/propages proprement `IOException` (ne pas avaler l’exception silencieusement).

---

# 6) Exercices guidés (progressifs)

### Exercice A — Lire un `.txt` et compter les lignes

* **But** : afficher le nombre de lignes non vides d’un fichier `data/notes.txt`.
* **Indications** : `Files.newBufferedReader(..., UTF_8)`, boucler, `line.isBlank()`.

### Exercice B — Logger simple en append

* **But** : écrire dans `logs/app.log` la date/heure + message à chaque exécution.
* **Indications** : `Files.writeString(..., APPEND, CREATE)` ; formater la date (`LocalDateTime`).

### Exercice C — Charger `student.json` et l’enrichir

* **But** : lire `data/student.json`, ajouter un champ `"validated": true`, sauvegarder `data/student_validated.json`.
* **Indications** : `Files.readString(..., UTF_8)` → `JSONObject.fromObject(...)` → `accumulate(...)` → utilitaire `saveStringIntoFile(...)`. &#x20;

### Exercice D — Transformateur CSV→JSON

* **But** : lire un `data/students.csv` (`name;age;email`) et produire `data/students.json` (array d’objets).
* **Indications** : `BufferedReader`, `split(";")`, `JSONArray`/`JSONObject`, écriture UTF-8.

---

# 7) Mini-projet (consolidation)

**“Annuaire Étudiants”**

1. Démarrer avec un `students.json` (ex. ton `student.json` comme gabarit).
2. Implémenter :

   * `List<Student> readAll()` : lit JSON → objets.
   * `void add(Student s)` : ajoute un étudiant → réécrit le fichier.
   * `Optional<Student> findByEmail(String email)` : recherche.
3. Interface CLI minimaliste :

   * `list`, `add`, `find email@example.com`.
4. I/O : `Files.readString` / `Files.writeString` (UTF-8) ou tes utilitaires (cohérent avec tes exemples). &#x20;

---

# 8) Références à tes fichiers fournis (pour continuité)

* **Utilitaire de lecture** (String ← fichier via IOUtils) : pratique pour charger du JSON à parser.&#x20;
* **Utilitaire d’écriture** (UTF-8 via FileUtils) : une ligne pour sauvegarder un `String`.&#x20;
* **Exemple de création JSON** (employee + projects) : modèle à imiter pour `student.json`.&#x20;
