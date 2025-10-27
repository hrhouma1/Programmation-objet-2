- L’objectif est uniquement d’illustrer le principe : une seule « connexion » simulée, avec `System.out.println`.

### Fichier `DatabaseConnection.java`

```java
public class DatabaseConnection {

    // 1) Instance unique (static = partagée par toute la classe)
    private static DatabaseConnection instance;

    // 2) Un attribut pour représenter l'état de la "connexion"
    private String status;

    // 3) Constructeur privé (empêche l'utilisation de new à l'extérieur)
    private DatabaseConnection() {
        status = "CONNECTÉ";
        System.out.println("[INIT] Nouvelle connexion créée (objet DatabaseConnection)");
    }

    // 4) Point d'accès global
    public static DatabaseConnection getInstance() {
        if (instance == null) {
            // Première demande : on crée l'unique objet
            instance = new DatabaseConnection();
        }
        // Appels suivants : on renvoie toujours le même objet
        return instance;
    }

    // 5) Méthode pour "utiliser" la connexion
    public void execute(String action) {
        System.out.println("[EXEC] " + action + " | état=" + status);
    }
}
```

### Fichier `Main.java`

```java
public class Main {
    public static void main(String[] args) {

        // On "demande une connexion"
        DatabaseConnection conn1 = DatabaseConnection.getInstance();

        // On "redemande une connexion"
        DatabaseConnection conn2 = DatabaseConnection.getInstance();

        // On utilise la connexion
        conn1.execute("SELECT * FROM utilisateurs");
        conn2.execute("UPDATE utilisateurs SET actif=true");

        // Vérifier que c'est le MÊME objet
        System.out.println("conn1 == conn2 ? " + (conn1 == conn2));
    }
}
```

### Ce que vous verrez dans la console

Lors de l’exécution de `Main.main()`, vous verrez une sortie semblable à :

```text
[INIT] Nouvelle connexion créée (objet DatabaseConnection)
[EXEC] SELECT * FROM utilisateurs | état=CONNECTÉ
[EXEC] UPDATE utilisateurs SET actif=true | état=CONNECTÉ
conn1 == conn2 ? true
```

À observer :

* La ligne `[INIT]` apparaît **une seule fois** → l’objet n’est créé qu’une seule fois.
* Les deux variables `conn1` et `conn2` utilisent exactement la même instance → `true`.

---

### À retenir 

1. **Singleton =**

   * constructeur privé,
   * attribut `private static instance`,
   * méthode `public static getInstance()` qui retourne toujours le même objet.

2. **Pourquoi utiliser ce modèle ?**
   Pour garantir qu’il n’existe qu’UNE SEULE ressource partagée dans l’application (par exemple : connexion à une base, gestionnaire de configuration, journalisation).
