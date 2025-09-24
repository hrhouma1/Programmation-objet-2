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


<br/>

# Correction : Voir l'annexe 1

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


<br/>

# Annexe 1 


```java
// WriteAllDemos.java
// ------------------------------------------------------------
// 5 recettes complètes pour écrire dans des fichiers texte en Java
// Recette 3.1 : Classique (Writer + BufferedWriter)
// Recette 3.2 : Append (ajout en fin de fichier)
// Recette 3.3 : NIO (Files.writeString) — conseillée
// Recette 3.4 : Tout le contenu d’un coup (byte[])
// Recette 3.5 : Utilitaire (signature semblable à Apache Commons IO)
// ------------------------------------------------------------
// Compilation :  javac WriteAllDemos.java
// Exécution   :  java WriteAllDemos
// Les fichiers seront créés sous le dossier ./data
// ------------------------------------------------------------

import java.io.BufferedWriter;
import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStreamWriter;
import java.io.Writer;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.StandardOpenOption;

public class WriteAllDemos {

    public static void main(String[] args) {
        try {
            ensureDataDir();

            demo31_ClassicWriterBuffered();   // 3.1
            demo32_AppendAtEnd();             // 3.2
            demo33_NioWriteString();          // 3.3
            demo34_WriteAllBytes();           // 3.4
            demo35_UtilitySaveString();       // 3.5

            System.out.println("✅ Terminé. Vérifie le dossier ./data");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    // Crée le dossier ./data s’il n’existe pas
    private static void ensureDataDir() throws IOException {
        Path dataDir = Path.of("data");
        if (!Files.exists(dataDir)) {
            Files.createDirectories(dataDir);
        }
    }

    // ------------------------------------------------------------
    // 3.1. Classique (Writer + Buffer)
    // ------------------------------------------------------------
    private static void demo31_ClassicWriterBuffered() {
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

    // ------------------------------------------------------------
    // 3.2. Append (ajouter à la fin)
    // ------------------------------------------------------------
    private static void demo32_AppendAtEnd() {
        File file = new File("data/log.txt");

        try (Writer w = new OutputStreamWriter(new FileOutputStream(file, true), StandardCharsets.UTF_8);
             BufferedWriter bw = new BufferedWriter(w)) {

            bw.write("Nouvelle entrée de log");
            bw.newLine();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    // ------------------------------------------------------------
    // 3.3. NIO (conseillé)
    // ------------------------------------------------------------
    private static void demo33_NioWriteString() throws IOException {
        Path p = Path.of("data/out2.txt");
        Files.writeString(p, "Texte initial\n", StandardCharsets.UTF_8);
        Files.writeString(p, "Ligne ajoutée\n", StandardCharsets.UTF_8, StandardOpenOption.APPEND);
    }

    // ------------------------------------------------------------
    // 3.4. Tout le contenu d’un coup (byte[])
    // ------------------------------------------------------------
    private static void demo34_WriteAllBytes() throws IOException {
        Path p = Path.of("data/out3.txt");
        byte[] content = "Contenu complet\n".getBytes(StandardCharsets.UTF_8);
        Files.write(p, content); // écrase par défaut si le fichier existe
    }

    // ------------------------------------------------------------
    // 3.5. Utilitaire fourni (style Apache Commons IO)
    //
    // Tu disposes (selon ton projet) d’un utilitaire de type :
    //   FileWriterUtil.saveStringIntoFile(path, content)
    // qui encapsule l’écriture UTF-8.
    //
    // Ci-dessous, on implémente un utilitaire minimal avec la même signature.
    // Si tu utilises vraiment Apache Commons IO (FileUtils.writeStringToFile),
    // remplace simplement le corps de la méthode par l’appel à FileUtils.
    // ------------------------------------------------------------
    private static void demo35_UtilitySaveString() {
        try {
            FileWriterUtil.saveStringIntoFile("data/out_util.txt", "Hello depuis l’utilitaire (UTF-8)\n");
            FileWriterUtil.saveStringIntoFile("data/out_util.txt", "Deuxième ligne via utilitaire\n", true); // append
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    // ------------------------------------------------------------
    // Utilitaire minimal compatible avec la signature attendue.
    // Si tu as Apache Commons IO dans le classpath, tu peux remplacer
    // ces méthodes par :
    //
    //   import org.apache.commons.io.FileUtils;
    //   public static void saveStringIntoFile(String path, String content) throws IOException {
    //       FileUtils.writeStringToFile(new File(path), content, StandardCharsets.UTF_8, false);
    //   }
    //   public static void saveStringIntoFile(String path, String content, boolean append) throws IOException {
    //       FileUtils.writeStringToFile(new File(path), content, StandardCharsets.UTF_8, append);
    //   }
    // ------------------------------------------------------------
    static class FileWriterUtil {
        public static void saveStringIntoFile(String path, String content) throws IOException {
            // Écriture (overwrite)
            Files.writeString(Path.of(path), content, StandardCharsets.UTF_8);
        }

        public static void saveStringIntoFile(String path, String content, boolean append) throws IOException {
            Path p = Path.of(path);
            if (append) {
                Files.writeString(p, content, StandardCharsets.UTF_8,
                        Files.exists(p) ? StandardOpenOption.APPEND : StandardOpenOption.CREATE);
            } else {
                Files.writeString(p, content, StandardCharsets.UTF_8);
            }
        }
    }
}
```


<br/>


# Annexe 2




## Résumé pédagogique (avant de voir le code)

* **3.1 Classique (Writer + BufferedWriter)**

  * *Pourquoi* : maîtriser le flux d’écriture **caractère par caractère** avec un **tampon** (buffer) en **UTF-8**.
  * *Quand* : tu écris **plusieurs lignes petit à petit** (boucles, logs formatés, fusion de contenus).
  * *Différence* : tu contrôles finement `write()`, `newLine()`, et profites de la **mise en mémoire tampon** pour limiter les accès disque.

* **3.2 Append (ajouter à la fin)**

  * *Pourquoi* : rajouter du texte **sans écraser** l’existant (ex. logs).
  * *Quand* : fichiers de **journalisation**, rapport qui se **construit** au fil du temps.
  * *Différence* : la seule différence est **le drapeau `append = true`** à l’ouverture du flux.

* **3.3 NIO (Files.writeString) – recommandé**

  * *Pourquoi* : API moderne, **simple**, sûre, moins de code verbeux.
  * *Quand* : tu veux **écrire/vite** sans gérer toi-même les Writers et buffers.
  * *Différence* : on passe par `java.nio.file.Files` (NIO.2), avec **options** (`StandardOpenOption.APPEND`, etc.), **UTF-8** explicite.

* **3.4 Tout le contenu d’un coup (byte\[])**

  * *Pourquoi* : tu as **déjà** une chaîne/byte\[] complète et veux **écrire d’un seul coup**.
  * *Quand* : génération de **petits** fichiers complets, export ponctuel.
  * *Différence* : pas de flux haut niveau, **remplacement par défaut** du fichier (overwrite).

* **3.5 Utilitaire (style Apache Commons IO)**

  * *Pourquoi* : **centraliser** l’écriture UTF-8 dans **une méthode** réutilisable pour **raccourcir** tes exemples et éviter les erreurs.
  * *Quand* : tu veux une **signature simple** `saveStringIntoFile(path, content[, append])`.
  * *Différence* : on **encapsule** `Files.writeString` (ou `FileUtils.writeStringToFile` si tu ajoutes Apache Commons IO).

---

## Code complet, prêt à compiler/exécuter (avec commentaires exhaustifs)

```java
// WriteAllDemos.java
// ============================================================================
// Objectif pédagogique : montrer 5 façons d'écrire dans des fichiers texte en Java,
// avec explications claires sur l'utilité, les différences, les implications
// (tampon/buffer, append, overwrite, encodage, exceptions, API IO vs NIO).
//
// Compilation : javac WriteAllDemos.java
// Exécution   : java WriteAllDemos
// Sorties     : dossier ./data (créé au lancement)
//
// Points clés à retenir :
// - Toujours préciser l'encodage (UTF-8) pour éviter les caractères "bizarres".
// - "Append" = on ajoute au fichier existant sans l'écraser.
// - BufferedWriter = meilleures perfs quand on écrit petit à petit.
// - NIO (Files.writeString) = API moderne, concise, à privilégier en général.
// - Overwrite par défaut : re-crée le fichier à chaque écriture (écrase l'ancien).
// ============================================================================

import java.io.BufferedWriter;
import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStreamWriter;
import java.io.Writer;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.StandardOpenOption;

public class WriteAllDemos {

    public static void main(String[] args) {
        try {
            ensureDataDir(); // On s’assure que ./data existe avant toute écriture

            // 3.1 : Recette "classique" avec Writer + BufferedWriter
            // - Contrôle fin ligne par ligne, tampon mémoire pour réduire les I/O.
            demo31_ClassicWriterBuffered();

            // 3.2 : Même approche, mais en "append"
            // - Ajoute en fin de fichier sans supprimer le contenu précédent.
            demo32_AppendAtEnd();

            // 3.3 : NIO moderne (Files.writeString)
            // - Concis, lisible, facile à combiner avec des options.
            demo33_NioWriteString();

            // 3.4 : Écriture "tout d’un coup" à partir d’un byte[]
            // - Pratique si tu as déjà un bloc complet à écrire.
            demo34_WriteAllBytes();

            // 3.5 : Utilitaire (style Apache Commons IO)
            // - Centralise l’écriture UTF-8, très pratique pour tes TP/exemples.
            demo35_UtilitySaveString();

            System.out.println("OK ✅  — Vérifie le dossier ./data");
        } catch (IOException e) {
            // En démo, on affiche simplement la pile. En prod, loggue proprement.
            e.printStackTrace();
        }
    }

    // ----------------------------------------------------------------------------
    // Support : création du répertoire de sortie.
    // ----------------------------------------------------------------------------
    private static void ensureDataDir() throws IOException {
        Path dataDir = Path.of("data");
        // Files.exists : évite l’exception si le dossier est déjà là
        if (!Files.exists(dataDir)) {
            Files.createDirectories(dataDir); // Crée aussi les parents si besoin
        }
    }

    // ----------------------------------------------------------------------------
    // 3.1. Classique (Writer + Buffer)
    //
    // Pourquoi :
    // - Tu veux écrire progressivement (plusieurs écritures successives).
    // - Tu veux bénéficier d’un "BufferedWriter" pour limiter les accès disque :
    //   le texte est d’abord mis en mémoire, puis "vidé" (flush/close) en bloc.
    //
    // Détails :
    // - OutputStreamWriter transforme des octets en caractères selon un encodage.
    // - On force l’UTF-8 (StandardCharsets.UTF_8) pour des accents corrects.
    // - try-with-resources ferme automatiquement w et bw (flush + close sûrs).
    // ----------------------------------------------------------------------------
    private static void demo31_ClassicWriterBuffered() {
        File file = new File("data/out.txt");

        try (Writer w = new OutputStreamWriter(
                    new FileOutputStream(file),          // FileOutputStream = flux binaire vers le fichier
                    StandardCharsets.UTF_8               // encodage explicite pour transformer char -> bytes
                 );
             BufferedWriter bw = new BufferedWriter(w)   // tampon pour meilleures perfs sur petites écritures
        ) {
            bw.write("Bonjour !");
            bw.newLine(); // Ajoute le séparateur de ligne adapté au système (portable)
            bw.write("Deuxième ligne.");
            // Pas besoin d'appeler bw.flush() : le close() implicite du try s’en charge
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    // ----------------------------------------------------------------------------
    // 3.2. Append (ajouter à la fin)
    //
    // Pourquoi :
    // - Tu construis un fichier "au fil de l’eau" (journalisation, traces d’exécution).
    // - Tu NE veux PAS écraser le fichier existant.
    //
    // Détails :
    // - Le booléen "true" passé au FileOutputStream active le mode "append".
    // - Même combo Writer + BufferedWriter pour l’encodage et la perf.
    // ----------------------------------------------------------------------------
    private static void demo32_AppendAtEnd() {
        File file = new File("data/log.txt");

        try (Writer w = new OutputStreamWriter(
                    new FileOutputStream(file, true),     // append = true  ➜ on ajoute à la fin
                    StandardCharsets.UTF_8
                 );
             BufferedWriter bw = new BufferedWriter(w)
        ) {
            bw.write("Nouvelle entrée de log");
            bw.newLine();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    // ----------------------------------------------------------------------------
    // 3.3. NIO (conseillé)
    //
    // Pourquoi :
    // - API moderne (NIO.2), très lisible, moins de "plomberie".
    // - Facile d’ajouter des options (APPEND, CREATE, CREATE_NEW, TRUNCATE_EXISTING).
    //
    // Détails :
    // - Files.writeString écrit une chaîne directement dans un Path.
    // - Par défaut, sans option, si le fichier existe il est remplacé (overwrite).
    // - Avec APPEND, on ajoute à la fin (équivalent append=true).
    // ----------------------------------------------------------------------------
    private static void demo33_NioWriteString() throws IOException {
        Path p = Path.of("data/out2.txt");

        // Écriture initiale : si out2.txt existe, il sera écrasé.
        Files.writeString(p, "Texte initial\n", StandardCharsets.UTF_8);

        // Ajout d’une ligne : APPEND pour coller à la fin, sans écraser l’existant.
        Files.writeString(p, "Ligne ajoutée\n", StandardCharsets.UTF_8, StandardOpenOption.APPEND);
    }

    // ----------------------------------------------------------------------------
    // 3.4. Tout le contenu d’un coup (byte[])
    //
    // Pourquoi :
    // - Tu as déjà tout le contenu en mémoire (String/byte[]) et tu veux "dump" en une fois.
    // - Parfait pour des petits fichiers générés d’un coup (ex: export court, modèle fixe).
    //
    // Détails :
    // - Files.write(Path, byte[]) écrit le tableau tel quel (aucun Writer ici).
    // - Par défaut, si le fichier existe, il est écrasé (overwrite).
    // - Encodage : on décide nous-même en convertissant String -> byte[] en UTF-8.
    // ----------------------------------------------------------------------------
    private static void demo34_WriteAllBytes() throws IOException {
        Path p = Path.of("data/out3.txt");
        byte[] content = "Contenu complet\n".getBytes(StandardCharsets.UTF_8);
        Files.write(p, content); // overwrite par défaut ; utiliser APPEND si besoin
    }

    // ----------------------------------------------------------------------------
    // 3.5. Utilitaire fourni (style Apache Commons IO)
    //
    // Pourquoi :
    // - Tu répètes sans cesse la même écriture UTF-8 → on factorise dans une méthode.
    // - Tes TP/échantillons restent lisibles : une ligne pour écrire, c’est tout.
    //
    // Détails :
    // - Ici, FileWriterUtil encapsule Files.writeString (standard Java).
    // - Si tu ajoutes Apache Commons IO, tu peux remplacer l’implémentation par
    //   FileUtils.writeStringToFile(new File(path), content, UTF_8, append).
    // ----------------------------------------------------------------------------
    private static void demo35_UtilitySaveString() {
        try {
            // Écriture simple (overwrite)
            FileWriterUtil.saveStringIntoFile("data/out_util.txt", "Hello depuis l’utilitaire (UTF-8)\n");

            // Écriture en append (ajout en fin)
            FileWriterUtil.saveStringIntoFile("data/out_util.txt", "Deuxième ligne via utilitaire\n", true);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    // ============================================================================
    // Petit utilitaire réutilisable dans tes projets/TP
    // - Avantage : centraliser l’encodage, le choix "append vs overwrite", et
    //   homogénéiser ta façon d’écrire (évite l’oubli d’UTF-8 quelque part).
    // - Choix techniques :
    //   * overwrite = Files.writeString(path, content, UTF_8)  (écrase)
    //   * append    = Files.writeString(path, content, UTF_8, APPEND) (ajoute)
    //     - Note : si le fichier n’existe pas et que tu passes APPEND, Java le
    //       crée automatiquement avec CREATE (comportement fusionné par Files).
    // ============================================================================
    static class FileWriterUtil {
        public static void saveStringIntoFile(String path, String content) throws IOException {
            Files.writeString(Path.of(path), content, StandardCharsets.UTF_8);
        }
        public static void saveStringIntoFile(String path, String content, boolean append) throws IOException {
            Path p = Path.of(path);
            if (append) {
                Files.writeString(p, content, StandardCharsets.UTF_8,
                        // APPEND = ajoute ; CREATE = crée si manquant (sécurise l’appel)
                        StandardOpenOption.CREATE, StandardOpenOption.APPEND
                );
            } else {
                Files.writeString(p, content, StandardCharsets.UTF_8);
            }
        }
    }
}
```



## En bref : quelle méthode choisir ?

* **Tu écris ligne par ligne en boucle** → **3.1 Classique (BufferedWriter)**
* **Tu fais de la journalisation** (ne jamais écraser) → **3.2 Append** ou **3.3 NIO + APPEND**
* **Tu veux du code concis et moderne** → **3.3 NIO (Files.writeString)**
* **Tu as déjà tout le contenu d’un coup** → **3.4 byte\[] (Files.write)**
* **Tu veux simplifier tous tes exemples** → **3.5 Utilitaire** (encapsule UTF-8 + append)

