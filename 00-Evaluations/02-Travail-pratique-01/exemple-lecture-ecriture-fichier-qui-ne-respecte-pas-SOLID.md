> ⚠️ **Intentionnellement mauvais** : à utiliser pour le “avant”/diagnostic.

```java
// FileLogger.java — Classe utilitaire concrète trop puissante (mauvais design)
import java.nio.charset.StandardCharsets;
import java.nio.file.*;

public class FileLogger {
    // On expose en protected (surface inutilement large pour les sous-classes)
    protected void write(String path, String msg) {
        try {
            Path p = Path.of(path);
            if (p.getParent() != null) Files.createDirectories(p.getParent());
            Files.writeString(p, msg + System.lineSeparator(),
                    StandardCharsets.UTF_8,
                    StandardOpenOption.CREATE, StandardOpenOption.APPEND, StandardOpenOption.WRITE);
            System.out.println("[FileLogger] wrote -> " + p.toAbsolutePath());
        } catch (Exception e) {
            System.out.println("[FileLogger] write error: " + e.getMessage());
        }
    }

    // Méthode “pratique” qui fige un chemin et mélange politique & détail
    protected void writeAudit(String msg) {
        write("out/audit.log", msg); // chemin en dur ici (détail) → fuite dans la hiérarchie
    }
}
```

```java
// AuditService.java — Hérite juste pour réutiliser write(...): faux is-a (mauvais)
public class AuditService extends FileLogger {
    // Le service métier connaît le format ET le chemin (via la superclasse)
    public void audit(String event) {
        // Couplage fort au détail (fichier + format de message) : pas d’abstraction
        writeAudit("AUDIT :: " + event);
    }

    // On ajoute une méthode “utile” mais qui n'a rien à faire ici (SRP violé)
    public void auditUser(String username, String action) {
        audit("USER=" + username + " ACTION=" + action);
    }
}
```

```java
// App.java — Point d'entrée qui fait TOUT (orchestration + logique + I/O) : SRP violé
// Pas d'arguments, pas de stratégie interchangeable. On fige d'emblée le service concret.
public class App {
    // Variable globale statique (mauvaise pratique) pour partager l'état
    private static AuditService SERVICE = new AuditService();

    public static void main(String[] args) {
        // Logique applicative mélangée ici (aucune injection, aucun choix d'implémentation)
        SERVICE.audit("APP_START");
        SERVICE.auditUser("alice", "LOGIN");
        SERVICE.auditUser("alice", "DOWNLOAD:file.txt");
        SERVICE.audit("APP_STOP");

        // “Validation” à la main : on dit juste que c'est ok...
        System.out.println("Done. (Regardez out/audit.log)");
    }
}
```

### Pourquoi c’est **mal conçu** (à mettre dans notre diagnostic)

* **Héritage utilitaire** : `AuditService extends FileLogger` → **faux is-a** ; le service métier devient un type de logger, ce qu’il n’est pas.
* **DIP violé** : `AuditService` dépend d’une **classe concrète** (et non d’une abstraction). **Aucune injection** possible → testabilité faible.
* **Chemin en dur** (`out/audit.log`) et **format du message** “AUDIT :: …” disséminés → **couplage au détail**.
* **SRP violé** : `App` orchestre **et** gère de la logique “métier” (construction des événements), `AuditService` gère aussi du formatage bas niveau.
* **Surface protégée excessive** (`protected`) dans `FileLogger` → augmente le couplage et les risques d’abus côté sous-classes.
* **Aucune extensibilité** : passer à Console/Syslog/Cloud nécessite de **modifier** la hiérarchie/les classes existantes au lieu d’ajouter une implémentation.
