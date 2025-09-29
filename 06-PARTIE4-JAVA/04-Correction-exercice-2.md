# Correction complète — SRP (Single Responsibility Principle)

## 1) Problème identifié

La classe `Report` regroupe **trois responsabilités** :

1. **génération** du contenu,
2. **sauvegarde** sur disque,
3. **envoi** par e-mail.

Conséquences :

* Modifications fréquentes et non corrélées (format du rapport, chemin fichier, méthode d’envoi) dans **une seule classe** → classe instable.
* Couplage de préoccupations hétérogènes → **difficile à maintenir** et à **faire évoluer**.
* Violation du **S de SOLID (SRP)** : une classe doit avoir **une seule raison de changer**.

---

## 2) Objectif demandé

Découper en **trois classes** focalisées :

* `ReportGenerator` : produit une chaîne (contenu du rapport).
* `ReportSaver` : persiste le contenu (fichier).
* `ReportSender` : expédie le contenu (e-mail).
* `App.java` orchestre l’ensemble dans `main`.

---

## 3) Arborescence (ASCII)

```
project-root/
└─ src/
   ├─ ReportGenerator.java   # Génère le contenu du rapport
   ├─ ReportSaver.java       # Sauvegarde le rapport (fichier)
   ├─ ReportSender.java      # Envoie le rapport (e-mail, simulé ici)
   └─ App.java               # Point d’entrée, orchestre le flux
```

---

## 4) Diagramme de dépendances (ASCII)

```
+------------------+      +----------------+      +----------------+
| ReportGenerator  | ---> |  (String)      | ---> |  ReportSaver   |
+------------------+      |  report data   |      +----------------+
            \             +----------------+                \
             \______________________________________________ \
                                                           v  v
                                                     +-------------+
                                                     | ReportSender|
                                                     +-------------+

                   +------------------+
                   |       App        |
                   |  (main orches.)  |
                   +------------------+
                    |  creates/uses
                    v
  ReportGenerator, ReportSaver, ReportSender
```

---

## 5) Code final (SRP appliqué)

### `src/ReportGenerator.java`

```java
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

public class ReportGenerator {

    /**
     * Génère le contenu du rapport.
     * Responsabilité unique : format/structure du texte.
     */
    public String generate() {
        LocalDateTime now = LocalDateTime.now();
        String ts = now.format(DateTimeFormatter.ISO_LOCAL_DATE_TIME);

        StringBuilder sb = new StringBuilder();
        sb.append("=== REPORT ===\n");
        sb.append("Generated at: ").append(ts).append("\n");
        sb.append("Summary: All systems nominal.\n");
        sb.append("- Metric A: 42\n");
        sb.append("- Metric B: 3.14\n");
        sb.append("- Metric C: OK\n");
        return sb.toString();
    }
}
```

### `src/ReportSaver.java`

```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;

public class ReportSaver {

    /**
     * Sauvegarde le rapport dans un fichier.
     * Responsabilité unique : persistance sur le système de fichiers.
     */
    public void saveToFile(String content, String filePath) {
        try {
            Path path = Paths.get(filePath);
            Path parent = path.getParent();
            if (parent != null) {
                Files.createDirectories(parent);
            }
            Files.writeString(path, content);
            System.out.println("Report saved to: " + path.toAbsolutePath());
        } catch (IOException e) {
            throw new RuntimeException("Échec de la sauvegarde du rapport: " + e.getMessage(), e);
        }
    }
}
```

### `src/ReportSender.java`

```java
public class ReportSender {

    /**
     * Envoie le rapport par e-mail (simulation).
     * Responsabilité unique : transport/notification.
     * En production : brancher un SMTP / API e-mail (JavaMail, SES, SendGrid, etc.)
     */
    public void sendByEmail(String content, String toEmail) {
        // Simulation d'envoi
        System.out.println("Sending report to: " + toEmail);
        System.out.println("---- EMAIL BODY START ----");
        System.out.println(content);
        System.out.println("---- EMAIL BODY END ----");
    }
}
```

### `src/App.java` (point d’entrée — orchestration)

```java
public class App {

    /**
     * Usage:
     *   java App                         -> écrit ./out/report.txt, envoie à admin@example.com
     *   java App out/myrep.txt           -> écrit out/myrep.txt, envoie à admin@example.com
     *   java App out/myrep.txt bob@ex.com-> écrit out/myrep.txt, envoie à bob@ex.com
     */
    public static void main(String[] args) {
        String outPath = (args.length > 0) ? args[0] : "out/report.txt";
        String email   = (args.length > 1) ? args[1] : "admin@example.com";

        // 1) Génération
        ReportGenerator generator = new ReportGenerator();
        String report = generator.generate();

        // 2) Sauvegarde
        ReportSaver saver = new ReportSaver();
        saver.saveToFile(report, outPath);

        // 3) Envoi
        ReportSender sender = new ReportSender();
        sender.sendByEmail(report, email);

        System.out.println("Flow completed: generated, saved, sent.");
    }
}
```



## 6) Explications (exhaustif)

### a) SRP respecté

* `ReportGenerator` **change** si le **format** ou le **contenu** du rapport évolue.
* `ReportSaver` **change** si la **stratégie de persistance** change (nouveau répertoire, permissions, S3…).
* `ReportSender` **change** si le **canal d’envoi** évolue (SMTP, API, webhook…).
  → Chaque classe a **une seule raison de changer**.

### b) Bénéfices

* **Lisibilité** : chaque classe est courte et ciblée.
* **Évolutivité** : changer l’un des aspects n’impacte pas les autres.
* **Testabilité** : on peut valider chaque responsabilité séparément.
* **Réutilisation** : `ReportSaver`/`ReportSender` peuvent servir à d’autres contenus.

### c) Variantes possibles (optionnel)

* Introduire des **interfaces** (`ReportPersistence`, `ReportNotifier`) si vous prévoyez plusieurs implémentations (fichier, base, S3 ; e-mail, Slack, SMS).
* Ajouter un **DTO** `Report` (objet dédié) si le rapport devient structuré (en-tête, sections, métadonnées).
* Introduire un **facade/orchestrator** si le flux devient plus complexe (ex. `ReportService` qui appelle Generator/Saver/Sender) ; `App` resterait le point d’entrée.



## 7) Checklist qualité

* [x] Une classe = une responsabilité.
* [x] `App` ne contient que l’**orchestration** (pas de logique métier lourde).
* [x] Le code de sauvegarde ne “connaît” rien du format du rapport.
* [x] Le code d’envoi ne “connaît” rien du système de fichiers.
* [x] Points d’extension évidents (ajouter PDF/HTML, écrire vers S3, envoyer via SMTP/API).


