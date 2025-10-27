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
   public static synchronized DatabaseConnection getInstance() {
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

<br/>
<br/>

ANNEXE 1 (TRÈS IMPORTANT) - Aller un peu plus “forensique Java” 😈


- Objectif : vérifier si `conn1` et `conn2` sont le même objet, afficher clairement ce qu’on compare, et utiliser à la fois `equals`, `==` et l’opérateur ternaire.

*Utiilisez ce `Main.java` pédagogique que vous pouvez montrer en classe tel quel.*



## 1. Version complète de `Main.java`

```java
public class Main {
    public static void main(String[] args) {

        // On récupère deux "connexions"
        DatabaseConnection conn1 = DatabaseConnection.getInstance();
        DatabaseConnection conn2 = DatabaseConnection.getInstance();

        // 1. Afficher les références (en vrai, c'est juste toString() par défaut)
        //    Par défaut, toString() d'un objet affiche: nomDeClasse@adresseEnHexa
        System.out.println("Référence conn1 = " + conn1);
        System.out.println("Référence conn2 = " + conn2);

        // 2. Comparaison avec ==
        boolean memeObjet = (conn1 == conn2);
        System.out.println("conn1 == conn2 ? " + memeObjet);

        // 3. Comparaison avec equals()
        boolean egauxLogiquement = conn1.equals(conn2);
        System.out.println("conn1.equals(conn2) ? " + egauxLogiquement);

        // 4. Opérateur ternaire pour afficher un message clair
        String messageIdentite = (conn1 == conn2)
                ? "C'est exactement le même objet en mémoire."
                : "Ce sont deux objets différents.";
        System.out.println("Check identité (==): " + messageIdentite);

        String messageEquals = (conn1.equals(conn2))
                ? "equals() dit qu'ils sont considérés comme égaux."
                : "equals() dit qu'ils ne sont pas égaux.";
        System.out.println("Check logique (equals): " + messageEquals);

        // 5. Bonus: afficher le hashCode des deux objets
        //    hashCode() n'est pas garanti = adresse mémoire,
        //    mais ça aide à voir si c'est le même objet
        System.out.println("hashCode conn1 = " + conn1.hashCode());
        System.out.println("hashCode conn2 = " + conn2.hashCode());
    }
}
```

---

## 2. Pourquoi ça marche pour “voir les références” ?

### `System.out.println(conn1);`

* En Java, si vous affichez un objet sans faire `toString()` vous-même,
  Java appelle automatiquement `conn1.toString()`.
* La version par défaut (héritée de `Object`) ressemble à :
  `NomDeLaClasse@quelqueChoseEnHexa`
* Exemple possible :
  `DatabaseConnection@5e2de80c`

Si `conn1` et `conn2` affichent la même chose, c’est le même objet.

---

## 3. Pourquoi afficher aussi `hashCode()` ?

`hashCode()` par défaut retourne un entier qui dépend (souvent) de l’identité de l’objet.
Donc :

* Si `conn1.hashCode()` == `conn2.hashCode()`, c’est un indice fort que c’est le même objet.
* Dans un Singleton, ce sera identique.

C’est utile quand vous voulez montrer visuellement que tout pointe vers une seule instance partagée.

---

## 4. Récapitulatif express à montrer aux étudiant(e)s

* `==`
  Vérifie si c’est le **même objet exact** en mémoire.

* `equals()`
  Vérifie si les deux objets sont **considérés égaux** selon la logique métier.

  * Si vous n’avez pas redéfini `equals()`, ça revient au même que `==`.
  * Si vous redéfinissez `equals()`, vous pouvez dire “deux objets différents mais même contenu = égaux”.

* Opérateur ternaire `condition ? valeurSiVrai : valeurSiFaux`
  Sert à construire une chaîne claire sans `if`.

Exemple tiré du code :

```java
String messageIdentite = (conn1 == conn2)
        ? "C'est exactement le même objet en mémoire."
        : "Ce sont deux objets différents.";
System.out.println(messageIdentite);
```

---

## 5. Petit rappel sur la classe Singleton (pour que tout compile)

Je redonne la classe `DatabaseConnection` version simple compatible avec `Main` ci-dessus :

```java
public class DatabaseConnection {

    private static DatabaseConnection instance;

    private String status;

    private DatabaseConnection() {
        status = "CONNECTÉ";
        System.out.println("[INIT] Nouvelle connexion créée (objet DatabaseConnection)");
    }

    public static synchronized DatabaseConnection getInstance() {
        if (instance == null) {
            instance = new DatabaseConnection();
        }
        return instance;
    }

    public void execute(String action) {
        System.out.println("[EXEC] " + action + " | état=" + status);
    }
}
```



### Utilisation 

1. Vous exécutez le `main`.
2. Vous verrez :

   * que `[INIT]` ne s'affiche qu'une seule fois,
   * que les références imprimées pour `conn1` et `conn2` sont identiques,
   * que `==` et `equals()` donnent le même résultat,
   * que le ternaire permet d’expliquer clairement sans `if`.

C’est exactement l’autopsie d’un Singleton 
