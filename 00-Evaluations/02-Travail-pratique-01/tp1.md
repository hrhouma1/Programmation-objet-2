## Exercice 1 — Couplage fort

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
Justifier brièvement **DIP** et **couplage faible** dans un README (5–10 lignes).



## Exercice 3 — SRP (trop de responsabilités) — **Lecture/Écriture réelles**

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
