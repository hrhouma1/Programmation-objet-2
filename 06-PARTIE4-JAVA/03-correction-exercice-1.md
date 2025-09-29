# 1) Problème identifié

Dans la version initiale, la classe `UserService` crée directement une instance de `MySQLDatabase` :

```java
private MySQLDatabase db = new MySQLDatabase();
```

Cela introduit un **couplage fort** :

* **Testabilité réduite** : on ne peut pas substituer une fausse base de données pour les tests.
* **Extensibilité limitée** : changer de type de base nécessite de modifier `UserService`.
* **Violation du principe D de SOLID** (*Dependency Inversion*) : un module de haut niveau (`UserService`) dépend d’un module de bas niveau (`MySQLDatabase`) au lieu de dépendre d’une abstraction.



# 2) Objectif demandé

* Introduire une **abstraction** `Database` (interface).
* Adapter `MySQLDatabase` pour implémenter cette interface.
* **Injecter** une implémentation de `Database` dans `UserService` via son **constructeur** (constructor injection).
* Déplacer la logique de lancement dans une classe `App` pour séparer responsabilité métier et exécution.



# 3) Nouvelle arborescence du projet (ASCII)

```
project-root/
└─ src/
   ├─ Database.java           # interface commune
   ├─ MySQLDatabase.java      # implémentation concrète MySQL
   ├─ InMemoryDatabase.java   # implémentation alternative pour démo / cours
   ├─ UserService.java        # logique métier, dépend d'une abstraction
   └─ App.java                # point d’entrée de l’application
```



# 4) Diagramme des dépendances (ASCII)

```
                  +------------------+
                  |   Database (I)   |<----------------------+
                  +------------------+                       |
                          ^                                   |
                          |                                   |
           +--------------+---------------+                   |
           |                              |                   |
+-----------------------+     +-----------------------+       |
| MySQLDatabase (impl.) |     | InMemoryDatabase (impl)|       |
+-----------------------+     +-----------------------+       |
                                                               |
                             +--------------------+            |
                             |    UserService     |------------+
                             +--------------------+
                                      ^
                                      |
                                      |  Injected at runtime
                                      |
                             +--------------------+
                             |       App          |
                             +--------------------+
```



# 5) Code final

### `Database.java` — l’abstraction

```java
public interface Database {
    void save(String data);
}
```

### `MySQLDatabase.java` — implémentation concrète

```java
public class MySQLDatabase implements Database {
    @Override
    public void save(String data) {
        // Simulation d’une insertion en base réelle
        System.out.println("MySQL: " + data);
    }
}
```

### `InMemoryDatabase.java` — implémentation alternative (utile pour la démo)

```java
import java.util.ArrayList;
import java.util.List;

public class InMemoryDatabase implements Database {
    private final List<String> storage = new ArrayList<>();

    @Override
    public void save(String data) {
        storage.add(data);
        System.out.println("InMemoryDB saved: " + data);
    }

    // Méthode utilitaire pour consulter l’état interne
    public List<String> getStorage() {
        return storage;
    }
}
```

### `UserService.java` — logique métier découplée

```java
public class UserService {
    private final Database db; // dépend de l’abstraction, pas d’une classe concrète

    // Injection via le constructeur (obligatoire et sûr)
    public UserService(Database db) {
        if (db == null) {
            throw new IllegalArgumentException("Database ne peut pas être null");
        }
        this.db = db;
    }

    public void register(String user) {
        // Ici on pourrait valider / transformer l’utilisateur
        db.save(user);
    }
}
```

### `App.java` — point d’entrée unique

```java
public class App {

    /**
     * Point d’entrée de l’application.
     * Usage :
     *   java App               -> par défaut MySQLDatabase, enregistre "alice"
     *   java App memory bob    -> InMemoryDatabase, enregistre "bob"
     *   java App mysql  charly -> MySQLDatabase, enregistre "charly"
     */
    public static void main(String[] args) {
        // Choix du backend en fonction des arguments
        String backend = (args.length > 0) ? args[0].toLowerCase() : "mysql";
        String user    = (args.length > 1) ? args[1] : "alice";

        Database db;
        switch (backend) {
            case "memory":
                db = new InMemoryDatabase();
                break;
            case "mysql":
            default:
                db = new MySQLDatabase();
                break;
        }

        // Injection de l’implémentation choisie dans UserService
        UserService service = new UserService(db);
        service.register(user);

        // Affichage spécial si on utilise InMemoryDatabase
        if (db instanceof InMemoryDatabase) {
            System.out.println("État interne InMemory: " + ((InMemoryDatabase) db).getStorage());
        }
    }
}
```



# 6) Explications détaillées (exhaustif)

### a) Pourquoi l’interface `Database` ?

* Elle **découple** `UserService` du type concret.
* Elle permet d’ajouter facilement de nouvelles implémentations (PostgreSQL, fichier, API REST) sans toucher au code métier.
* Elle favorise la **substitution** et la réutilisation (principe Liskov + DIP).

### b) Pourquoi l’injection par constructeur ?

* Garantit que l’objet `UserService` est **valide** dès sa création.
* Évite l’état mutable ou l’oubli d’un setter.
* Facilite l’injection par un framework DI (Spring, Guice) plus tard.

### c) Pourquoi un `App.java` séparé ?

* **Séparation des responsabilités** : `UserService` gère la logique métier, `App` gère la configuration et le choix d’implémentation.
* Permet de changer le “mode” d’exécution (prod, test, démo) sans toucher au code métier.
* Permet d’avoir un point d’entrée clair pour `java App`.

### d) Avantages obtenus

* Respect du principe **D** de SOLID (Dependency Inversion).
* **Testabilité** : injection d’une base mémoire pour vérifier le comportement.
* **Extensibilité** : remplacement transparent de la couche persistance.
* **Lisibilité** : code métier plus simple, moins de dépendances cachées.

### e) Alternatives possibles

* **Setter injection** si la dépendance est optionnelle.
* **Factory** ou **Provider** si la création est complexe.
* **Framework DI** (Spring/Guice) pour configurer l’injection via annotations.



# 7) Checklist qualité

* [x] `UserService` ne dépend que de `Database`.
* [x] Implémentations interchangeables (`MySQLDatabase` / `InMemoryDatabase`).
* [x] Injection par constructeur, champ `final`.
* [x] Point d’entrée `App` qui illustre l’usage réel.
* [x] Séparation nette responsabilité métier / exécution.



# 8) Étapes suivantes possibles (améliorations)

1. Introduire un objet `User` au lieu d’un `String` pour plus de cohérence SRP.
2. Ajouter un `UserRepository` si la logique de persistence s’étend.
3. Remplacer `System.out.println` par un vrai logger (SLF4J/Logback).
4. Intégrer des tests unitaires avec JUnit + Mockito (mock de `Database`).
5. Mettre en place un conteneur DI pour la configuration automatique.



> Avec cette structure, l’exemple est **parfaitement aligné avec SOLID**.
> `App.java` est le seul endroit où tu choisis l’implémentation de `Database` à injecter dans `UserService`.
