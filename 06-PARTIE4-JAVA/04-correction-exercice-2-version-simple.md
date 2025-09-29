# Arborescence (ASCII)

```
project-root/
└─ src/
   ├─ ReportGenerator.java   // génère du texte (print)
   ├─ ReportSaver.java       // "sauvegarde" (print)
   ├─ ReportSender.java      // "envoie" (print)
   └─ App.java               // orchestre tout
```

# Diagramme (ASCII)

```
App (main) -> ReportGenerator -> (String)
           -> ReportSaver     -> (prints "saved")
           -> ReportSender    -> (prints "sent")
```

# Code (tout simple, uniquement des prints)

## `src/ReportGenerator.java`

```java
public class ReportGenerator {

    // Responsabilité: produire du contenu (simulé ici)
    public String generate() {
        System.out.println("[Generator] Démarrage de la génération...");
        String content = "=== REPORT ===\n"
                + "Generated at: 2025-09-29\n"
                + "Summary: All systems nominal.\n"
                + "- Metric A: 42\n"
                + "- Metric B: 3.14\n"
                + "- Metric C: OK\n";
        System.out.println("[Generator] Génération terminée.");
        return content;
    }
}
```

## `src/ReportSaver.java`

```java
public class ReportSaver {

    // Responsabilité: "sauvegarder" (ici: simple affichage)
    public void saveToFile(String content, String filePath) {
        System.out.println("[Saver] Sauvegarde demandée vers: " + filePath);
        System.out.println("--------[SAUVEGARDE SIMULÉE]--------");
        System.out.println(content);
        System.out.println("--------[FIN SAUVEGARDE]------------");
        System.out.println("[Saver] Sauvegarde (simulée) terminée.");
    }
}
```

## `src/ReportSender.java`

```java
public class ReportSender {

    // Responsabilité: "envoyer" (ici: simple affichage)
    public void sendByEmail(String content, String toEmail) {
        System.out.println("[Sender] Envoi vers: " + toEmail);
        System.out.println("--------[EMAIL SIMULÉ]--------------");
        System.out.println("To: " + toEmail);
        System.out.println("Subject: Rapport quotidien");
        System.out.println("Body:\n" + content);
        System.out.println("--------[FIN EMAIL]-----------------");
        System.out.println("[Sender] Envoi (simulé) terminé.");
    }
}
```

## `src/App.java`

```java
public class App {

    /**
     * Usage:
     *   java App                          -> out/report.txt + admin@example.com
     *   java App out/rep.txt              -> out/rep.txt + admin@example.com
     *   java App out/rep.txt bob@ex.com   -> out/rep.txt + bob@ex.com
     */
    public static void main(String[] args) {
        String outPath = (args.length > 0) ? args[0] : "out/report.txt";
        String email   = (args.length > 1) ? args[1] : "admin@example.com";

        System.out.println("[App] Lancement du flux SRP (simple prints)");

        // 1) Générer
        ReportGenerator generator = new ReportGenerator();
        String report = generator.generate();

        // 2) Sauvegarder (simulé)
        ReportSaver saver = new ReportSaver();
        saver.saveToFile(report, outPath);

        // 3) Envoyer (simulé)
        ReportSender sender = new ReportSender();
        sender.sendByEmail(report, email);

        System.out.println("[App] Terminé: généré, 'sauvegardé', 'envoyé'.");
    }
}
```

# Points pédagogiques (rapides)

* **SRP** respecté : une classe = une responsabilité.
* `App` se contente **d’orchestrer**.
* Zéro complexité : **aucun fichier**, **aucun try/catch**, **tout en `println`** pour la démo en classe.

