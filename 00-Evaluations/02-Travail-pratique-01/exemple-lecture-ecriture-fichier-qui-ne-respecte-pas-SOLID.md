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



<br/>
<br/>

# Annexe - Fichier `FileLogger.java`

```java
// FileLogger.java — Classe utilitaire concrète trop puissante (mauvais design)
```

* **// …** : commentaire de ligne, ignoré par le compilateur.
* Message pédagogique : cette classe est volontairement « trop puissante » → **anti-pattern**.

```java
import java.nio.charset.StandardCharsets;
```

* **import** : rend visibles les types d’un autre package sans préfixe complet.
* **`java.nio.charset.StandardCharsets`** (package **JDK** standard) :

  * Fournit des constantes d’ensembles de caractères (ex. **`StandardCharsets.UTF_8`**).
  * Sert à encoder/décoder du texte.
  * **Package** : `java.nio.charset`.

```java
import java.nio.file.*;
```

* **import …*** : importe **tous** les types publics du package.
* **`java.nio.file`** (package JDK) contient :

  * **`Path`** : représentation d’un chemin (abstrait, indépendant OS).
  * **`Files`** : méthodes utilitaires d’E/S (lecture/écriture/création…).
  * **`StandardOpenOption`** : options d’ouverture (CREATE, APPEND, WRITE, …).
  * **Package** : `java.nio.file`.

```java
public class FileLogger {
```

* **public** : classe accessible depuis **n’importe quel package**.
* **class FileLogger** : déclare un **type** concret nommé `FileLogger`.
* **Package implicite** : (aucun `package` déclaré) → **package par défaut** (déconseillé).

```java
    // On expose en protected (surface inutilement large pour les sous-classes)
```

* Commentaire : intention (mauvais choix d’accès).

```java
    protected void write(String path, String msg) {
```

* **protected** : visible dans la classe, ses **sous-classes** (même autre package) et dans le **même package**.
* **`void write(String path, String msg)`** : méthode qui n’a **pas de valeur de retour**.
* **`String`** vient de **`java.lang.String`** (import implicite, `java.lang` est toujours importé).
* Paramètres :

  * `path` : chemin du fichier, **texte**.
  * `msg` : message à écrire.

```java
        try {
```

* **try** : début d’un bloc qui peut lever des exceptions → on va **catch** ensuite.

```java
            Path p = Path.of(path);
```

* **`Path`** (de `java.nio.file`) : objet représentant le chemin.
* **`Path.of(String)`** : fabrique un `Path` depuis la chaîne `path`.

```java
            if (p.getParent() != null) Files.createDirectories(p.getParent());
```

* **`p.getParent()`** : renvoie le **répertoire parent** du chemin (ou `null` si aucun).
* **`Files.createDirectories(Path)`** : créé le(s) dossier(s) **manquants** de façon récursive.
* **`Files`** vient de `java.nio.file`.

```java
            Files.writeString(p, msg + System.lineSeparator(),
                    StandardCharsets.UTF_8,
                    StandardOpenOption.CREATE, StandardOpenOption.APPEND, StandardOpenOption.WRITE);
```

* **`Files.writeString(Path, CharSequence, Charset, OpenOption...)`** :

  * Écrit du **texte** dans un fichier.
  * **Paramètre 1** : `p` (cible).
  * **Paramètre 2** : `msg + System.lineSeparator()`

    * **`System`** (de `java.lang.System`) → import implicite.
    * **`lineSeparator()`** : séparateur de ligne propre à l’OS (`\n` Linux/macOS, `\r\n` Windows).
  * **Paramètre 3** : **`StandardCharsets.UTF_8`** (charset → `java.nio.charset`).
  * **Paramètre 4…** : **`StandardOpenOption`** (de `java.nio.file`) :

    * `CREATE` : crée le fichier s’il n’existe pas.
    * `APPEND` : ajoute à la fin.
    * `WRITE` : ouvre en écriture.

```java
            System.out.println("[FileLogger] wrote -> " + p.toAbsolutePath());
```

* **`System.out`** : flux de sortie standard (console).
* **`println`** : affiche une ligne.
* **`p.toAbsolutePath()`** : convertit le `Path` en chemin **absolu** (avec racine).

```java
        } catch (Exception e) {
```

* **`catch (Exception e)`** : attrape **toutes** les exceptions de type `java.lang.Exception` (trop large).
* **`Exception`** vient de `java.lang.Exception` (import implicite).

```java
            System.out.println("[FileLogger] write error: " + e.getMessage());
```

* Affiche le message d’erreur (mais **ne remonte pas** l’exception → erreurs potentiellement silencieuses).

```java
        }
    }
```

* Fin du `catch`, fin de la méthode `write`.

```java
    // Méthode “pratique” qui fige un chemin et mélange politique & détail
```

* Commentaire : indique le **mauvais design** (chemin en dur).

```java
    protected void writeAudit(String msg) {
        write("out/audit.log", msg); // chemin en dur ici (détail) → fuite dans la hiérarchie
    }
```

* **`writeAudit`** : méthode **helper** qui **impose** le chemin `"out/audit.log"`.
* **Mauvais point de conception** :

  * Le **chemin** (détail d’infrastructure) se retrouve **dans la superclasse** → toutes les sous-classes y sont **couplées**.
  * Violations possibles : **DIP**, **OCP**, responsabilité mélangée.

```java
}
```

* Fin de la classe `FileLogger`.

---

# Fichier `AuditService.java`

```java
// AuditService.java — Hérite juste pour réutiliser write(...): faux is-a (mauvais)
```

* Commentaire : ce **service métier** hérite d’un **logger** (faux **is-a**).

```java
public class AuditService extends FileLogger {
```

* **public** : accessible partout.
* **`extends FileLogger`** : **héritage** → `AuditService` est un **FileLogger** (ce qui est faux conceptuellement).
* **Problème** : le service **métier** devient un **détail technique** (logger) → **couplage fort**.

```java
    // Le service métier connaît le format ET le chemin (via la superclasse)
```

* Commentaire : alerte sur le couplage aux **détails**.

```java
    public void audit(String event) {
```

* Méthode publique de service métier.

```java
        // Couplage fort au détail (fichier + format de message) : pas d’abstraction
        writeAudit("AUDIT :: " + event);
```

* **`writeAudit`** vient de la superclasse (**héritage**).
* Le **format** `"AUDIT :: "` est fixé **ici** ; le **chemin** l’est dans la **superclasse** → double couplage **au détail**.
* Aucune **abstraction** (`Logger`/`Appender`) → violation **DIP**.

```java
    }
```

* Fin de `audit`.

```java
    // On ajoute une méthode “utile” mais qui n'a rien à faire ici (SRP violé)
    public void auditUser(String username, String action) {
        audit("USER=" + username + " ACTION=" + action);
    }
```

* **`auditUser`** : logique **métier** et **formatage** dans le service de log → mélange de responsabilités (**SRP violé**).
* Concaténation de `String` (type : `java.lang.String`).

```java
}
```

* Fin de la classe `AuditService`.



# Fichier `App.java`

```java
// App.java — Point d'entrée qui fait TOUT (orchestration + logique + I/O) : SRP violé
// Pas d'arguments, pas de stratégie interchangeable. On fige d'emblée le service concret.
```

* Commentaires : **anti-pattern** → l’entrée fait **trop de choses** et **aucune injection**.

```java
public class App {
```

* Classe `App` publique (point d’entrée standard d’une appli Java).

```java
    // Variable globale statique (mauvaise pratique) pour partager l'état
    private static AuditService SERVICE = new AuditService();
```

* **`private`** : visible seulement dans `App`.
* **`static`** : lié à la **classe** (un seul exemplaire partagé).
* **`AuditService SERVICE = new AuditService();`** :

  * Instancie **en dur** un type **concret** (couplage fort).
  * **Pas d’injection** ; impossible de substituer par une autre stratégie (console/syslog/cloud).

```java
    public static void main(String[] args) {
```

* **Point d’entrée** officiel Java.
* **`String[] args`** : arguments de ligne de commande (ici **ignorés**).

```java
        // Logique applicative mélangée ici (aucune injection, aucun choix d'implémentation)
        SERVICE.audit("APP_START");
        SERVICE.auditUser("alice", "LOGIN");
        SERVICE.auditUser("alice", "DOWNLOAD:file.txt");
        SERVICE.audit("APP_STOP");
```

* **Mauvaise pratique** : `App` fait **à la fois** l’orchestration **et** une partie de la **logique** (construction d’événements).
* Appel de méthodes **métier** sur un service **couplé** à une implémentation **fichier** via héritage.

```java
        // “Validation” à la main : on dit juste que c'est ok...
        System.out.println("Done. (Regardez out/audit.log)");
```

* **`System.out`** vient de **`java.lang.System`** (import implicite).
* Affiche un message au lieu de **vérifier**/tester réellement le contenu du fichier.

```java
    }
}
```

* Fin de `main`, fin de `App`.



## Récapitulatif — D’où viennent les classes utilisées ?

| Symbole / Classe                      | Package JDK                                 | Rôle principal                             |
| ------------------------------------- | ------------------------------------------- | ------------------------------------------ |
| `String`, `System`, `Exception`       | `java.lang` (import implicite)              | Chaînes, E/S console, exceptions           |
| `StandardCharsets`                    | `java.nio.charset`                          | Constantes d’encodage (`UTF_8`)            |
| `Path`, `Files`, `StandardOpenOption` | `java.nio.file`                             | Chemins, E/S fichiers, options d’ouverture |
| `println`, `lineSeparator()`          | `java.lang.System.out` / `java.lang.System` | Sortie console, séparateur de ligne        |

> Remarque : aucun `package` n’est déclaré en tête des fichiers → tout est dans le **package par défaut** (mauvaise pratique pour un vrai projet).


## Pourquoi c’est mal développé (résumé diagnostic à réutiliser)

* **Faux is-a** : `AuditService extends FileLogger` → le service métier **n’est pas** un logger.
* **DIP violé** : dépendance à une **classe concrète** (pas d’interface), **aucune injection**, **tests** difficiles.
* **Couplage au détail** : chemin `"out/audit.log"` figé côté parent, format de message figé côté enfant.
* **SRP violé** : `App` mélange orchestration, logique et validation ; `AuditService` mélange formatage bas niveau et métier.
* **Surface `protected`** excessive : incite à la réutilisation par héritage au lieu de composition.
* **Pas d’extensibilité** : pour passer à Console/Syslog/Cloud, il faut **modifier** la hiérarchie actuelle (OCP non respecté).


