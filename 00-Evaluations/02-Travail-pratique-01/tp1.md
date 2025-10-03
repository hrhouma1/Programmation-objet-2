# À remettre : **un seul rapport Word (DOCX)**

* Fichier unique : `NOM_Prenom_SOLID_TP.docx`
* Contenu : pour **chaque exercice**, inclure :

  1. **Objectif en 2–3 lignes**
  2. **Code pertinent** (avant/après si demandé)
 3. **Captures d’écran obligatoires** :

     * Terminal montrant la **compilation** et **l’exécution**
     * Arborescence du projet (`data/`, `out/`, `src/`)
     * Fichiers générés (ex. `out/*.txt`, `out/audit.log`) **ouverts** dans l’éditeur dans certains exercices, où cela est demandé.
  5. **Justification(s)** demandées (SOLID, couplage, etc.) en 5–10 lignes
* Aucune bibliothèque externe. Java 17+ uniquement.

<br/>
<br/>

# Exercice 1 — Couplage fort

**Mauvaise pratique (à corriger vers interface + injection)**

```java
class MySQLDatabase {
    void save(String data) { System.out.println("MySQL: " + data); }
}

class UserService {
    private MySQLDatabase db = new MySQLDatabase(); // couplage fort

    void register(String user) {
        db.save(user);
    }

    public static void main(String[] args) {
        new UserService().register("alice");
    }
}
```

**À faire :** créer `interface Database`, implémentation `MySQLDatabase`, puis injecter `Database` via le **constructeur** de `UserService`.
Le `main` doit démontrer qu’on peut changer d’implémentation **sans modifier** `UserService` (ex. `InMemoryDatabase`).
Justifier brièvement **DIP** et **couplage faible** dans le rapport (5–10 lignes).





<br/>
<br/>








# Exercice 2 — Diagnostic de conception 

## But (2–3 lignes)

Diagnostiquer des **violations SOLID** et proposer une **refactorisation minimale**. L’objectif est d’identifier précisément les problèmes (DIP, couplage au détail, etc.) puis de proposer un **code corrigé succinct**.

## Code fourni (à diagnostiquer)

> **Exécutez ce code tel quel**, puis faites le diagnostic demandé.

```java
import java.nio.file.*;
import java.nio.charset.StandardCharsets;

class FileLogger {
    // ÉCRITURE réelle (UTF-8)
    void write(String path, String msg) {
        try {
            Path p = Path.of(path);
            if (p.getParent() != null) Files.createDirectories(p.getParent());
            Files.writeString(p, msg + System.lineSeparator(), StandardCharsets.UTF_8,
                              StandardOpenOption.CREATE, StandardOpenOption.APPEND);
            System.out.println("[FileLogger] wrote -> " + p.toAbsolutePath());
        } catch (Exception e) {
            System.out.println("[FileLogger] write error: " + e.getMessage());
        }
    }

    // LECTURE réelle (UTF-8)
    String readAll(String path) {
        try {
            return Files.readString(Path.of(path), StandardCharsets.UTF_8);
        } catch (Exception e) {
            System.out.println("[FileLogger] read error: " + e.getMessage());
            return "";
        }
    }
}
```

```java
// Service métier qui hérite d'un logger de fichiers
class AuditService extends FileLogger {
    void audit(String event) {
        write("out/audit.log", "AUDIT :: " + event);
    }
    String dumpLog() {
        return readAll("out/audit.log");
    }
}
```

```java
public class App {
    public static void main(String[] args) {
        AuditService svc = new AuditService();
        svc.audit("USER_LOGIN");
        svc.audit("USER_DOWNLOAD");

        String content = svc.dumpLog();
        System.out.println("---- LOG CONTENT ----");
        System.out.println(content);
        System.out.println("---------------------");
    }
}
```

## Ce que vous devez faire

1. **Exécuter tel quel** et joindre des **captures** :

   * Compilation **et** exécution,
   * Arborescence (`data/`, `out/`, `src/`),
   * Fichier `out/audit.log` **ouvert** dans l’éditeur (si généré).

2. **Rédiger un diagnostic (5–10 lignes)** :

   * Nommer les **principes SOLID non respectés** (ex. DIP, éventuellement LSP si argumenté),
   * Qualifier le **type de couplage** (fort au détail concret : héritage utilitaire + chemin de fichier dans le service),
   * Expliquer les **impacts** (testabilité, substitution, extension à d’autres backends de log, etc.).

3. **Proposer un code corrigé minimal** (extraits) :

   * Introduire une **abstraction** (ex. `interface Logger { void log(String message); }`),
   * Fournir au moins **une implémentation** (ex. fichier UTF-8 qui encapsule le chemin) et/ou une console,
   * Faire dépendre `AuditService` de **l’abstraction** (composition + injection par constructeur),
   * **Sans** `extends FileLogger`, **sans** `new` d’implémentation **à l’intérieur** de `AuditService`.

## Livrables (dans le DOCX)

* Diagnostic (5–10 lignes) clair et structuré,
* Extraits **avant / après** (suffisants pour montrer la correction),
* Captures d’écran (compilation + exécution + arborescence + fichier/logs),
* Brève justification des **choix de conception** (DIP, couplage faible, ...).



<br/>
<br/>












## Exercice 3

**Mauvaise pratique (à corriger vers composition + interface + injection)**

```java
// Ne modifiez pas ce bloc pour le diagnostic. Dupliquez/le pour travailler.
import java.nio.file.*;
import java.nio.charset.StandardCharsets;

class FileLogger {
    // Détail d'implémentation concret (écriture fichier)
    void write(String path, String msg) {
        try {
            Path p = Path.of(path);
            if (p.getParent() != null) Files.createDirectories(p.getParent());
            Files.writeString(p, msg + System.lineSeparator(), StandardCharsets.UTF_8,
                              StandardOpenOption.CREATE, StandardOpenOption.APPEND);
            System.out.println("[FileLogger] " + p.toAbsolutePath());
        } catch (Exception e) {
            System.out.println("[FileLogger] " + e.getMessage());
        }
    }
}

// Service métier qui "hérite" juste pour réutiliser write(...)
// => Couplage fort à une classe concrète + faux is-a
class AuditService extends FileLogger {
    void audit(String event) {
        write("out/audit.log", "AUDIT :: " + event); // dépend du détail (fichier) et du parent concret
    }
}

// (Exemple minimal de déclenchement, à déplacer/adapter si nécessaire)
class App3Bad {
    public static void main(String[] args) {
        new AuditService().audit("USER_LOGIN");
    }
}
```

**À faire :**

1. **Diagnostic (3–6 lignes)**

   * Expliquez pourquoi **hériter** de `FileLogger` crée un **couplage fort** (relation *is-a* non pertinente, dépendance à un **détail** concret, difficulté à remplacer la stratégie de log).
   * Mentionnez **DIP** (le service haut niveau doit dépendre d’une **abstraction**) et non d’une classe concrète, ainsi que l’impact sur la **testabilité**.

2. **Refactorisation (obligatoire)**

   * Concevez une **abstraction** de journalisation (ex. `Logger` avec la/les méthode(s) que vous jugez pertinentes).
   * Fournissez **au moins deux implémentations** interchangeables, dont **une** qui écrit **réellement** dans un fichier (UTF-8, crée le dossier si nécessaire) et **une autre** sans fichier (ex. console).
   * Réécrivez `AuditService` pour **ne plus étendre** `FileLogger` : utilisez la **composition** et **injectez** l’abstraction via le **constructeur** (aucun `new ImplConcrète()` à l’intérieur du service).
   * Le **point d’entrée** doit être **`class App`** et **orchestrer uniquement** : choisir l’implémentation (par argument de ligne de commande ou petit fichier de config) puis appeler `audit(...)` au moins **deux fois** avec des événements différents.

   **Point d’entrée imposé :**

   ```java
   class App {
       public static void main(String[] args) {
           // Choisir l'implémentation de log (ex. "file" ou "console")
           // Injecter dans AuditService (constructeur)
           // Déclencher au moins deux événements d'audit
       }
   }
   ```

3. **Contraintes & interdits**

   * **Interdit :** conserver `extends` entre le service et une classe de log ; instancier l’implémentation concrète **à l’intérieur** du service.
   * **Obligatoire :** si l’implémentation choisie est « fichier », l’écriture est **réelle** (UTF-8) dans `out/audit.log`.
   * **Encapsulez** les chemins/stratégies dans l’implémentation, pas dans le service.

4. **Questions pièges (répondez, 2–3 lignes chacune)**

   * **Q1.** « L’héritage **réduit** le couplage par rapport à la composition puisqu’on réutilise du code sans dépendre d’un objet. Vrai/Faux ? Justifiez. »
   * **Q2.** « Mettre `write(...)` en `protected` dans la classe parente facilite le test du service héritant, donc c’est un bon design. Vrai/Faux ? Parlez de la **surface de test** et de la **substitution**. »
   * **Q3.** « Si on passe à l’avenir de File → Syslog → Cloud Logging, l’héritage initial reste scalable. Vrai/Faux ? Justifiez en termes d’**ouvert/fermé** et de **point de décision**. »

**Livrables attendus (Ex3)**

* Code refactoré (abstraction + implémentations + service avec composition/injection).
* `class App` démontrant au **runtime** le **changement d’implémentation** sans modifier `AuditService`.
* Fichier de sortie `out/audit.log` présent si implémentation « fichier » sélectionnée.
* Explications entre 4 et 10 lignes : diagnostic synthétique, choix de l’abstraction, et rappel **DIP vs héritage**.




<br/>
<br/>



## Exercice 3 — SRP (trop de responsabilités) — **Lecture/Écriture réelles**


- Expliquez c'est quoi le SRP ?
  
**Mauvaise pratique (à découper en 2 classes : lecteur vs écrivain)**

```java
class Report {
    void readFromFileTxt() { System.out.println("read"); }
    void writeInFileTxt() { System.out.println("write"); }
}
```

**À faire :**

* Remplacer la classe « fourre-tout » par **deux classes distinctes** :

  * `ReportReader` : **lit** un fichier d’entrée (ex. `data/input.txt`, UTF-8) et **retourne** une chaîne.
  * `ReportWriter` : **écrit** une chaîne dans un fichier de sortie (ex. `out/report.txt`, UTF-8 ; crée le dossier si nécessaire).
* La méthode `main` **orchestre uniquement** : **Lecture → Écriture** (pas d’autre responsabilité).
* Interdits : fusionner lecture/écriture dans une seule classe ; remplacer l’I/O par des `System.out.println`.
* Exigences minimales I/O :

  * Entrée : `data/input.txt` (au moins 3 lignes non vides).
  * Sortie : `out/report.txt` **créé/réécrit** à l’exécution.
* Justifier brièvement **SRP** (une classe = une raison de changer) dans un fichier .txt (5–10 lignes).

**Point d’entrée imposé :**

```java
class App {
    public static void main(String[] args) {
        Report r = new Report();
        r.readFromFileTxt();   // doit lire un vrai fichier (pas un print)
        r.writeInFileTxt();    // doit écrire un vrai fichier (pas un print)
    }
}
```

> Remarque : le code ci-dessus est uniquement le **gabarit d’appel** exigé pour l’énoncé.
> Vous devez concevoir les classes finales (`ReportReader`, `ReportWriter`, etc.) et **adapter** `Report`/`App` en conséquence, tout en respectant **SRP** et les contraintes I/O.
