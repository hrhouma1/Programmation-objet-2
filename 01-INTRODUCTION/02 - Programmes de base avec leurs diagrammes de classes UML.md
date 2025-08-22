
# Programmes de base avec leurs diagrammes de classes UML


# **Programme 1 – HelloWorld**

### Code Java

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Bonjour, monde !");
    }
}
```

### Diagramme de classe UML

```
-------------------------
|     HelloWorld         |
-------------------------
| + main(args : String[])|
-------------------------
```

* Une seule classe.
* Une seule méthode statique `main`.


# **Programme 2 – Classe Étudiant**

### Code Java

```java
class Etudiant {
    String nom;
    int age;

    // Constructeur
    Etudiant(String nom, int age) {
        this.nom = nom;
        this.age = age;
    }

    // Méthode
    void afficherInfos() {
        System.out.println("Nom : " + nom + ", Age : " + age);
    }
}

public class TestEtudiant {
    public static void main(String[] args) {
        Etudiant e1 = new Etudiant("Alice", 20);
        Etudiant e2 = new Etudiant("Bob", 22);

        e1.afficherInfos();
        e2.afficherInfos();
    }
}
```

### Diagramme de classes UML

```
-------------------------
|       Etudiant         |
-------------------------
| - nom : String         |
| - age : int            |
-------------------------
| + Etudiant(nom, age)   |
| + afficherInfos()      |
-------------------------

-------------------------
|      TestEtudiant      |
-------------------------
| + main(args : String[])|
-------------------------
```



# **Programme 3 – Étudiant et Cours (Association)**

### Code Java

```java
class Etudiant {
    String nom;

    Etudiant(String nom) {
        this.nom = nom;
    }
}

class Cours {
    String titre;

    Cours(String titre) {
        this.titre = titre;
    }

    void inscrire(Etudiant e) {
        System.out.println(e.nom + " est inscrit au cours " + titre);
    }
}

public class TestAssociation {
    public static void main(String[] args) {
        Etudiant e1 = new Etudiant("Alice");
        Cours c1 = new Cours("Programmation Java");

        c1.inscrire(e1);
    }
}
```

### Diagramme UML

```
-------------------------         -------------------------
|       Etudiant         |         |         Cours          |
-------------------------         -------------------------
| - nom : String         |         | - titre : String       |
-------------------------         -------------------------
| + Etudiant(nom)        |         | + Cours(titre)         |
|                        |         | + inscrire(e:Etudiant) |
-------------------------         -------------------------

Relation : Etudiant ----> Cours (association : "inscrit à")
```



# **Programme 4 – Université et Étudiants (Agrégation)**

### Code Java

```java
import java.util.ArrayList;

class Etudiant {
    String nom;
    Etudiant(String nom) { this.nom = nom; }
}

class Universite {
    String nom;
    ArrayList<Etudiant> etudiants = new ArrayList<>();

    Universite(String nom) {
        this.nom = nom;
    }

    void ajouterEtudiant(Etudiant e) {
        etudiants.add(e);
    }
}

public class TestAgregation {
    public static void main(String[] args) {
        Universite u1 = new Universite("UQAM");
        Etudiant e1 = new Etudiant("Alice");
        Etudiant e2 = new Etudiant("Bob");

        u1.ajouterEtudiant(e1);
        u1.ajouterEtudiant(e2);

        System.out.println("Université " + u1.nom + " contient " + u1.etudiants.size() + " étudiants.");
    }
}
```

### Diagramme UML

```
-------------------------         -------------------------
|       Universite       |<>------|       Etudiant         |
-------------------------         -------------------------
| - nom : String         |        | - nom : String         |
| - etudiants : List     |        -------------------------
-------------------------         | + Etudiant(nom)        |
| + ajouterEtudiant()    |        -------------------------
-------------------------
```

Symbole ◇ = agrégation.

---

# **Programme 5 – Voiture et Moteur (Composition)**

### Code Java

```java
class Moteur {
    String type;

    Moteur(String type) {
        this.type = type;
    }
}

class Voiture {
    String marque;
    Moteur moteur; // composition

    Voiture(String marque, String typeMoteur) {
        this.marque = marque;
        this.moteur = new Moteur(typeMoteur);
    }
}

public class TestComposition {
    public static void main(String[] args) {
        Voiture v1 = new Voiture("Toyota", "Essence");
        System.out.println("Voiture " + v1.marque + " avec moteur " + v1.moteur.type);
    }
}
```

### Diagramme UML

```
-------------------------        -------------------------
|         Voiture        |◆------|         Moteur         |
-------------------------        -------------------------
| - marque : String      |       | - type : String        |
| - moteur : Moteur      |       -------------------------
-------------------------        | + Moteur(type)         |
| + Voiture(marque,type) |       -------------------------
-------------------------
```

Symbole ◆ = composition.



Ces **5 programmes** permettent de couvrir :

* Un programme minimal (HelloWorld)
* Une classe simple avec attributs et méthodes
* Une **association**
* Une **agrégation**
* Une **composition**

