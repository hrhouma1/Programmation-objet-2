# Partie 1 — Visibilité des classes et des membres

## Exercice 1 — Attributs privés et publics

### Objectif

Montrer pourquoi un champ `private` n’est pas accessible à l’extérieur de sa classe et comment l’exposer correctement (getters/setters ou méthode d’affichage).

### Fichiers et code

**Arborescence**

```
src/
 ├── Personne.java
 └── Main.java
```

**`src/Personne.java`**

```java
public class Personne {
    // Champ public : accessible depuis partout (classe, package, sous-classe, hors package)
    public String nom;

    // Champ privé : accessible uniquement à l'intérieur de la classe Personne
    private int age;

    // Constructeur
    public Personne(String nom, int age) {
        this.nom = nom;
        this.age = age;
    }

    // Méthode publique d'affichage (expose l'information privée sans dévoiler le champ)
    public void afficherInfo() {
        System.out.println("Personne[nom=" + nom + ", age=" + age + "]");
    }

    // Getter/Setter (bonne pratique pour contrôler l'accès)
    public int getAge() { return age; }
    public void setAge(int age) {
        if (age < 0) throw new IllegalArgumentException("Age invalide");
        this.age = age;
    }
}
```

**`src/Main.java`**

```java
public class Main {
    public static void main(String[] args) {
        Personne p = new Personne("Alice", 30);

        // ACCÈS DIRECT AU CHAMP PUBLIC : OK
        System.out.println(p.nom); // "Alice"

        // ACCÈS DIRECT AU CHAMP PRIVÉ : ERREUR DE COMPILATION
        // System.out.println(p.age); // décommentez pour voir l'erreur

        // Accès correct via une méthode publique
        p.afficherInfo();             // Personne[nom=Alice, age=30]
        System.out.println(p.getAge()); // 30

        // Modification contrôlée via setter
        p.setAge(31);
        p.afficherInfo();             // Personne[nom=Alice, age=31]
    }
}
```

### Commandes

* macOS/Linux :

```bash
javac -d out src/Personne.java src/Main.java
java -cp out Main
```

* Windows PowerShell :

```powershell
javac -d out src\Personne.java src\Main.java
java -cp out Main
```

### Sortie attendue

```
Alice
Personne[nom=Alice, age=30]
30
Personne[nom=Alice, age=31]
```

### Erreur attendue si on décommente l’accès à `age`

```
Main.java:8: error: age has private access in Personne
        System.out.println(p.age);
                             ^
```

### Explication

* `private` restreint l’accès **à la classe uniquement**. On passe par une **API publique** (méthode ou getter) pour exposer l’information en respectant l’encapsulation.


<br/>
<br/>

## Exercice 2 — Méthode `protected` et héritage

### Objectif

Montrer l’accès à `protected` depuis une **sous-classe** et les limites depuis une classe extérieure.

### Arborescence

```
src/
 ├── Animal.java
 ├── Chien.java
 └── Main.java
```

**`src/Animal.java`**

```java
public class Animal {
    protected void manger() {
        System.out.println("Animal mange (méthode protégée).");
    }
}
```

**`src/Chien.java`**

```java
public class Chien extends Animal {
    public void dejeuner() {
        // Accès autorisé : on est DANS la sous-classe
        manger();
    }

    public void testerAccesDepuisSousClasse(Animal autre) {
        // ⚠ Nuance importante Java :
        // En dehors du package, une sous-classe peut accéder au membre protected
        // sur "this" ou une référence de type Chien, PAS sur une référence de type Animal
        // si elle n'est pas du même package.
        // Ici, comme nous sommes dans le même (défaut) package, ce point est moins visible,
        // mais retenez la règle.
    }
}
```

**`src/Main.java`**

```java
public class Main {
    public static void main(String[] args) {
        Chien c = new Chien();
        c.dejeuner(); // OK -> "Animal mange (méthode protégée)."

        // Appel direct depuis une classe extérieure : ERREUR si on décommente
        // c.manger(); // 'manger()' has protected access in Animal
    }
}
```

### Commandes

```bash
javac -d out src/Animal.java src/Chien.java src/Main.java
java -cp out Main
```

### Sortie attendue

```
Animal mange (méthode protégée).
```

### Explication clé (règle fine de `protected`)

* **Même package** : `protected` = accessible comme `(package)` + sous-classes.
* **Package différent** : accessible **uniquement** via l’héritage *et* sur `this` (ou référence du type de la sous-classe), **pas** sur une référence arbitraire du supertype.


<br/>
<br/>

## Exercice 3 — Package-private (visibilité par défaut)

### Objectif

Montrer que l’absence de modificateur ⇒ **visible dans le même package uniquement**.

### Étape A — même package : accès OK

**Arborescence**

```
src/
 └── com/
     └── test/
         ├── A.java
         ├── B.java
         └── Main.java
```

**`src/com/test/A.java`**

```java
package com.test;

class A { // pas de modificateur => package-private
    void saluer() { // idem
        System.out.println("Bonjour depuis A (package-private).");
    }
}
```

**`src/com/test/B.java`**

```java
package com.test;

class B {
    void appeler() {
        A a = new A();
        a.saluer(); // OK : même package
    }
}
```

**`src/com/test/Main.java`**

```java
package com.test;

public class Main {
    public static void main(String[] args) {
        new B().appeler(); // OK
    }
}
```

**Compilation/Exécution**

```bash
javac -d out $(find src -name "*.java")
java -cp out com.test.Main
```

Sortie :

```
Bonjour depuis A (package-private).
```

### Étape B — déplacer `B` dans `com.autre` : accès interdit

**Nouvelle arborescence**

```
src/
 ├── com/
 │   ├── autre/
 │   │   └── B.java
 │   └── test/
 │       ├── A.java
 │       └── Main.java
```

**`src/com/autre/B.java`**

```java
package com.autre;

import com.test.A; // sera inutile car A n'est pas public

class B {
    void appeler() {
        // A a = new A();    // ERREUR : A n'est pas public et n'est pas visible hors de com.test
        // a.saluer();        // ERREUR
    }
}
```

**Erreurs attendues**

```
B.java:3: error: A is not public in com.test; cannot be accessed from outside package
import com.test.A;
               ^
B.java:6: error: A is not public in com.test; cannot be accessed from outside package
        A a = new A();
        ^
```

### Explication

* Sans modificateur, **la classe et ses membres sont invisibles hors du package**.

---

## Exercice 4 — Tableau récapitulatif (corrigé)

| Tentative d’accès | Même classe | Même package | Sous-classe (même package) |                        Sous-classe (autre package) | Classe extérieure (autre package) |
| ----------------- | ----------: | -----------: | -------------------------: | -------------------------------------------------: | --------------------------------: |
| `private`         |           ✔ |            ✘ |                          ✘ |                                                  ✘ |                                 ✘ |
| *(package)*       |           ✔ |            ✔ |                          ✔ |                                                  ✘ |                                 ✘ |
| `protected`       |           ✔ |            ✔ |                          ✔ | ✔ *(via héritage, sur `this` ou type sous-classe)* |                                 ✘ |
| `public`          |           ✔ |            ✔ |                          ✔ |                                                  ✔ |                                 ✔ |

> Nuance cruciale pour `protected` hors package : **pas** d’accès sur une instance de la superclasse obtenue ailleurs ; l’accès se fait **dans le contexte d’héritage**.


<br/>
<br/>

# Partie 2 — Paquetages (packages)

## Exercice 5 — Organisation en packages

### Objectif

Créer des classes dans `com.banque`, les utiliser depuis `com.app`.

### Arborescence

```
src/
 └── com/
     ├── banque/
     │    ├── Compte.java
     │    └── Client.java
     └── app/
          └── Main.java
```

**`src/com/banque/Compte.java`**

```java
package com.banque;

public class Compte {
    private double solde;

    public Compte(double soldeInitial) {
        this.solde = soldeInitial;
    }

    public void deposer(double montant) {
        if (montant <= 0) throw new IllegalArgumentException("Montant invalide");
        solde += montant;
    }

    public boolean retirer(double montant) {
        if (montant <= 0) return false;
        if (montant > solde) return false;
        solde -= montant;
        return true;
    }

    public void afficherSolde() {
        System.out.println("Solde = " + solde + " $");
    }

    public double getSolde() { return solde; }
}
```

**`src/com/banque/Client.java`**

```java
package com.banque;

public class Client {
    private final String nom;
    private final Compte compte;

    public Client(String nom, double soldeInitial) {
        this.nom = nom;
        this.compte = new Compte(soldeInitial);
    }

    public void afficher() {
        System.out.print("Client " + nom + " - ");
        compte.afficherSolde();
    }

    public Compte getCompte() { return compte; }
}
```

**`src/com/app/Main.java`**

```java
package com.app;

// Import global (possible) ou import précis
import com.banque.Client;
// import com.banque.*; // également possible

public class Main {
    public static void main(String[] args) {
        Client c = new Client("Marie", 100.0);
        c.afficher();                  // Client Marie - Solde = 100.0 $
        c.getCompte().deposer(50.0);
        c.getCompte().afficherSolde(); // Solde = 150.0 $
    }
}
```

### Commandes

* macOS/Linux :

```bash
javac -d out $(find src -name "*.java")
java -cp out com.app.Main
```

* Windows PowerShell :

```powershell
$files = Get-ChildItem -Recurse src -Filter *.java | % { $_.FullName }
javac -d out $files
java -cp out com.app.Main
```

### Sortie attendue

```
Client Marie - Solde = 100.0 $
Solde = 150.0 $
```

### Points clés

* La **déclaration `package`** doit être la **première ligne** (hors commentaires).
* Le **chemin du fichier** doit **refléter** le package.

<br/>
<br/>

## Exercice 6 — Conflit de noms (mêmes classes, packages différents)

### Objectif

Montrer qu’on ne peut pas importer deux types **homonymes** de packages différents et qu’il faut utiliser le **nom qualifié complet**.

### Arborescence

```
src/
 └── com/
     ├── animaux/
     │    └── Chien.java
     ├── voitures/
     │    └── Chien.java
     └── app/
          └── Main.java
```

**`src/com/animaux/Chien.java`**

```java
package com.animaux;

public class Chien {
    public void aboyer() {
        System.out.println("Wouf !");
    }
}
```

**`src/com/voitures/Chien.java`**

```java
package com.voitures;

public class Chien {
    public void klaxonner() {
        System.out.println("Tut-tut (voiture Chien) !");
    }
}
```

**`src/com/app/Main.java`**

```java
package com.app;

// ⚠ On ÉVITE d'importer les deux "Chien", sinon ambiguïté.
// import com.animaux.Chien;
// import com.voitures.Chien; // Conflit si les deux sont importés

public class Main {
    public static void main(String[] args) {
        com.animaux.Chien chienAnimal = new com.animaux.Chien();
        chienAnimal.aboyer();

        com.voitures.Chien chienVoiture = new com.voitures.Chien();
        chienVoiture.klaxonner();

        // Astuce : on peut importer l’un et qualifier l’autre
        // import com.animaux.Chien;
        // com.voitures.Chien v = new com.voitures.Chien();
        // Chien a = new Chien();
    }
}
```

### Commandes

```bash
javac -d out $(find src -name "*.java")
java -cp out com.app.Main
```

### Sortie attendue

```
Wouf !
Tut-tut (voiture Chien) !
```

### Explication

* Java **n’autorise pas d’alias d’import** (pas de `as` comme en C#).
* Solution : **nom qualifié complet** pour au moins un des deux types.

<br/>
<br/>


## Exercice 7 — Accès entre packages (`public`, `protected`, package-private)

### Objectif

Observer ce qui est accessible depuis un autre package et comment accéder au `protected` via héritage dans un package différent.

### Étape A — Classe utilitaire dans `com.util`

**Arborescence**

```
src/
 └── com/
     ├── util/
     │    └── Outils.java
     └── app/
          └── Main.java
```

**`src/com/util/Outils.java`**

```java
package com.util;

public class Outils {
    public void afficherMessage() {
        System.out.println("Message public OK.");
    }

    protected void calculer() {
        System.out.println("Calcul (protected) OK.");
    }

    void helper() { // package-private
        System.out.println("Helper (package-private) OK.");
    }
}
```

**`src/com/app/Main.java`**

```java
package com.app;

import com.util.Outils;

public class Main {
    public static void main(String[] args) {
        Outils o = new Outils();
        o.afficherMessage(); // OK (public)

        // o.helper();      // ERREUR : package-private, non accessible hors de com.util
        // o.calculer();    // ERREUR : protected, pas accessible depuis classe extérieure d'un autre package

        // Accès au protected via héritage (voir Étape B)
        new OutilsAvance().tester();
    }
}
```

**Erreurs attendues si lignes décommentées**

```
Main.java:10: error: helper() is not public in Outils; cannot be accessed from outside package
Main.java:11: error: calculer() has protected access in Outils
```

### Étape B — Hériter d’`Outils` dans un autre package

**`src/com/app/OutilsAvance.java`**

```java
package com.app;

import com.util.Outils;

public class OutilsAvance extends Outils {
    public void tester() {
        // Accès autorisé : membre protected via héritage dans un autre package
        calculer(); // OK

        // ⚠ Interdit : protected sur une instance "pure" du supertype
        // new Outils().calculer(); // ERREUR : l'accès protected hors package fonctionne via "this" (ou une ref. de OutilsAvance), pas via une ref. de Outils
    }
}
```

**Compilation/Exécution**

```bash
javac -d out $(find src -name "*.java")
java -cp out com.app.Main
```

**Sortie attendue**

```
Message public OK.
Calcul (protected) OK.
```

### Explication (point fin)

* `public` : accessible partout.
* `package-private` : visible seulement dans **le même package** (`com.util` ici).
* `protected` :

  * dans le **même package** : accessible comme package-private.
  * dans un **autre package** : **uniquement via héritage**, et sur **`this`** (ou une référence du type de la sous-classe), **pas** sur `new Outils()`.

<br/>

# Encadrés « erreurs fréquentes » et bonnes pratiques

1. **Classe non `public` dans un fichier différent**

   * Si vous déclarez une classe `public`, **le fichier doit porter le même nom** (`public class Client` ⇒ `Client.java`).
   * Une seule classe `public` par fichier.

2. **Cohérence package ↔ dossier**

   * `package com.banque;` ⇒ fichier placé dans `.../com/banque/`.
   * Sinon : `ClassNotFoundException` au runtime ou erreurs de compilation.

3. **Imports inutiles**

   * Importer une classe **non-publique** d’un autre package échoue (cf. Ex. 3 Étape B).

4. **`protected` hors package**

   * Accessible **dans la sous-classe** seulement, **pas** sur une instance du supertype.
   * Piège courant : `new Super().protectedMethod()` dans la sous-classe **ne compile pas** si hors package.

5. **Éviter les champs publics mutables**

   * Préférez `private` + getters/setters (ou `record` si immuable).


<br/>
<br/>

## Annexes — Commandes utiles

* Compiler tout le projet (macOS/Linux) :

```bash
javac -d out $(find src -name "*.java")
```

* Exécuter une classe principale :

```bash
java -cp out com.app.Main
```

* Sous Windows PowerShell :

```powershell
$files = Get-ChildItem -Recurse src -Filter *.java | % { $_.FullName }
javac -d out $files
java -cp out com.app.Main
```

