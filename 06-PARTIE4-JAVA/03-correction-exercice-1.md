# Correction complète — Couplage fort → Injection via interface (DI)



## 1) Problème identifié

Dans le code initial `UserService` instancie directement `MySQLDatabase` (`new MySQLDatabase()`), ce qui crée un **couplage fort** entre `UserService` et l'implémentation concrète.
Résultats indésirables :

* difficile à tester (impossible de substituer un double de test),
* peu d'extensibilité (changer de base implique modifier la classe),
* violation du principe **D** de SOLID — *Dependency Inversion Principle* (les modules de haut niveau ne doivent pas dépendre des modules de bas niveau ; les deux doivent dépendre d'abstractions).



## 2) Objectif demandé

Créer une `interface Database`, conserver/adapter `MySQLDatabase` pour implémenter cette interface, et **injecter** `Database` dans `UserService` via le **constructeur** (constructor injection).



## 3) Arborescence (ASCII)

```
project-root/
├─ src/
│  ├─ Database.java
│  ├─ MySQLDatabase.java
│  ├─ InMemoryDatabase.java        # impl. de test / exemple
│  └─ UserService.java
└─ tests/
   └─ UserServiceTest.java         # test manuel simple (sans framework)
```

---

## 4) Diagramme de dépendance (ASCII)

```
UserService  <-- depends on --  Database (interface)
      ^
      |                          implementations
      |---- constructed-with ----> MySQLDatabase (implements Database)
      |---- constructed-with ----> InMemoryDatabase (implements Database)
```



## 5) Code corrigé (constructeur + interface)

### `Database.java`

```java
public interface Database {
    void save(String data);
}
```

### `MySQLDatabase.java`

```java
public class MySQLDatabase implements Database {
    @Override
    public void save(String data) {
        // Ici on simule un accès réel ; en production on mettra JDBC/ORM
        System.out.println("MySQL: " + data);
    }
}
```

### `InMemoryDatabase.java` (utilitaire pour tests / démonstration)

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

    // helper pour assertions dans les tests simples
    public List<String> getStorage() {
        return storage;
    }
}
```

### `UserService.java` (injection par constructeur)

```java
public class UserService {
    private final Database db; // dépend d'une abstraction

    // Constructor Injection
    public UserService(Database db) {
        if (db == null) {
            throw new IllegalArgumentException("Database ne peut pas être null");
        }
        this.db = db;
    }

    public void register(String user) {
        // ici on pourrait ajouter validation, hashing, etc. (SRP à respecter)
        db.save(user);
    }

    // Demo main - montre comment injecter une implémentation
    public static void main(String[] args) {
        Database mysql = new MySQLDatabase();
        UserService service = new UserService(mysql);
        service.register("alice");

        // Exemple avec InMemory pour démonstration
        InMemoryDatabase mem = new InMemoryDatabase();
        UserService testService = new UserService(mem);
        testService.register("bob");
        System.out.println("InMemory storage: " + mem.getStorage());
    }
}
```



## 6) Explications détaillées (exhaustif)

### a) Pourquoi l'interface ?

* Permet de **découpler** l'utilisation (UserService) de l'implémentation.
* Autorise l'ajout d'autres implémentations (`PostgresDatabase`, `MockDatabase`, `FileDatabase`, etc.) **sans modifier** `UserService`.
* Facilite les tests unitaires : on injecte un double (mock/stub) qui enregistre les appels.

### b) Pourquoi constructeur ?

* **Constructor injection** garantit que la dépendance est fournie au moment de la création de l'objet (objet immuable côté dépendances).
* Permet d'avoir un `final` field, évite `null` plus tard.
* Meilleur pour l'injection obligatoire (la classe ne peut fonctionner sans la dépendance).

### c) Alternatives — quand les utiliser

* **Setter injection** (méthode `setDatabase(Database db)`):

  * utile si la dépendance est optionnelle ou pour frameworks qui nécessitent un setter.
  * moins sûr (risque d'oublier d'appeler le setter → NPE).
* **Interface + Factory / Provider**:

  * si création de l'implémentation est complexe, utiliser un `DatabaseFactory` ou DI container.
* **DI frameworks** (Spring, Guice) :

  * dans des projets réels on laisserait le framework gérer l'injection.

### d) Single Responsibility Principle (SRP)

* `UserService` devrait se limiter à la logique métier utilisateur (validation, règles).
* Si la logique de persistence devient complexe, extraire `UserRepository` qui implémente `Database` ou appelle `Database`.
* Exemple de séparation :

  * `UserService` : valider utilisateur, appliquer règles métier.
  * `UserRepository` : persist et récupération des objets `User`.

### e) Testabilité

* En injectant `InMemoryDatabase` (ou un mock), on peut vérifier que `register` appelle `save` et tester le comportement métier sans accéder à une vraie DB.



## 7) Test manuel simple (sans Mockito) — `tests/UserServiceTest.java`

```java
public class UserServiceTest {
    public static void main(String[] args) {
        InMemoryDatabase mem = new InMemoryDatabase();
        UserService service = new UserService(mem);
        service.register("charlie");

        if (mem.getStorage().contains("charlie")) {
            System.out.println("TEST PASSED");
        } else {
            System.out.println("TEST FAILED");
        }
    }
}
```

> Pour des tests unitaires professionnels, utilisez JUnit + Mockito (mocks / verify).

<br/>

# Annexe 


## 8) Variantes utiles (exemples courts)

### Setter injection (moins recommandé si la dépendance est obligatoire)

```java
public class UserService {
    private Database db;

    public void setDatabase(Database db) { this.db = db; }

    public void register(String user) {
        if (db == null) throw new IllegalStateException("Database non initialisée");
        db.save(user);
    }
}
```

### Factory pattern (si création compliquée)

```java
public class DatabaseFactory {
    public static Database create(String type) {
        if ("mysql".equalsIgnoreCase(type)) return new MySQLDatabase();
        if ("memory".equalsIgnoreCase(type)) return new InMemoryDatabase();
        throw new IllegalArgumentException("Type inconnu");
    }
}
```



## 9) Check-list de qualité à appliquer au code final

* [x] `UserService` dépend d'une abstraction (`Database`).
* [x] Injection via constructeur (dépendance final).
* [x] Implémentation concrète `MySQLDatabase` inchangée en logique externe.
* [x] Exemples de test avec `InMemoryDatabase`.
* [x] Validation null/erreurs au constructeur.
* [x] Respect partiel des principes SOLID : DIP amélioré ; SRP à surveiller (séparer logique métier vs persistence si nécessaire).



## 10) Bonnes pratiques / suggestions d'amélioration progressive

1. Extraire `UserRepository` si `UserService` commence à contenir logique de stockage.
2. Ajouter des interfaces typesafe (e.g., `Repository<T>` générique).
3. Utiliser des DTO / objets `User` au lieu de `String user`.
4. Pour production : remplacer `System.out.println` par un logger et implémenter vraie couche persistante (JDBC / JPA).
5. Ajouter des tests unitaires avec JUnit + Mockito pour `verify(db).save(...)`.


