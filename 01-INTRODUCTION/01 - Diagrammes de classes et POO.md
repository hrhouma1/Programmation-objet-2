# **Cours - Semaine 1 : Diagrammes de classes et POO**



## **1. Introduction générale**

### 1.1 Présentation du cours

* Ce cours introduit la **programmation orientée objet (POO)** et la **modélisation UML**.
* Objectifs :

  * Comprendre les concepts fondamentaux de la POO.
  * Installer un environnement de développement (Eclipse).
  * Écrire, compiler et exécuter un programme objet.
  * Représenter graphiquement les classes et leurs relations avec UML.



## **2. Programmation orientée objet (POO)**

### 2.1 Qu’est-ce que la POO ?

* La POO est une méthode de programmation qui repose sur la **modélisation du monde réel** à l’aide d’objets.
* Chaque objet est décrit par :

  * **Des attributs** (caractéristiques, variables)
  * **Des méthodes** (actions, fonctions liées)

### 2.2 Concepts fondamentaux

1. **Classe** : modèle ou plan de construction d’objets.
2. **Objet** : instance concrète d’une classe.
3. **Attributs** : données associées à l’objet.
4. **Méthodes** : comportements associés.
5. **Constructeur** : méthode spéciale pour créer un objet.

**Exemple (Java)** :

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
        System.out.println("Nom: " + nom + ", Age: " + age);
    }
}
```



## **3. Installation et configuration d’Eclipse**

### 3.1 Étapes d’installation

* Télécharger **Eclipse IDE for Java Developers** depuis le site officiel.
* Installer le **JDK (Java Development Kit)** avant Eclipse.
* Vérifier l’installation avec la commande :

  ```
  java -version
  ```

### 3.2 Création d’un projet

1. Ouvrir Eclipse.
2. `File > New > Java Project`.
3. Nommer le projet : `HelloWorld`.
4. Créer une classe `HelloWorld` avec une méthode `main`.

**Code exemple :**

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Bonjour, monde !");
    }
}
```

### 3.3 Structure d’un projet Eclipse

* `src` → contient le code source Java.
* `bin` → contient les fichiers `.class` compilés.



## **4. Gestion des erreurs et compilation**

### 4.1 Types d’erreurs

* **Syntaxe** : oubli d’un `;` ou d’une accolade.
* **Exécution** : division par zéro, accès null.
* **Logique** : erreur dans la conception (par ex. mauvais calcul).

### 4.2 Outils Eclipse

* **Console** : affiche les erreurs et résultats.
* **Debugger** : permet d’exécuter pas à pas (breakpoints).



## **5. Introduction à UML**

### 5.1 Qu’est-ce qu’UML ?

* UML (Unified Modeling Language) est un langage graphique de modélisation.
* Sert à représenter les **structures** (classes, objets) et **comportements** (séquences, cas d’utilisation).

### 5.2 Le diagramme de classes

* Il représente :

  * Les **classes**
  * Leurs **attributs et méthodes**
  * Leurs **relations**

**Notation UML d’une classe :**

```
-------------------------
|       NomClasse        |
-------------------------
| attributs              |
-------------------------
| méthodes()             |
-------------------------
```

**Exemple – Classe Étudiant :**

```
-------------------------
|       Etudiant         |
-------------------------
| - nom : String         |
| - age : int            |
-------------------------
| + afficherInfos()      |
-------------------------
```

---

## **6. Relations UML**

### 6.1 Association

* Lien simple entre deux classes.
* Exemple : *Un étudiant suit un cours.*

**Exemple UML :**

```
Etudiant -------- suit --------> Cours
```

### 6.2 Agrégation

* Relation « a un » faible, les objets peuvent exister indépendamment.
* Exemple : *Une université contient plusieurs étudiants.*

**Exemple UML :**

```
Université ◇──> Etudiant
```

### 6.3 Composition

* Relation forte, la destruction de l’objet entraîne la destruction des parties.
* Exemple : *Une voiture a un moteur (le moteur n’existe pas seul).*

**Exemple UML :**

```
Voiture ◆──> Moteur
```



## **7. Exercices pratiques**

### Exercice 1 : Premier programme

* Créez une classe `Etudiant` avec attributs `nom` et `age`.
* Ajoutez une méthode `afficherInfos()`.
* Instanciez deux étudiants et affichez leurs infos.

### Exercice 2 : Relation d’association

* Créez une classe `Cours` avec un attribut `titre`.
* Reliez un `Etudiant` et un `Cours` via une méthode `inscrire()`.

### Exercice 3 : Relation de composition

* Créez une classe `Voiture` qui contient un `Moteur`.
* Le `Moteur` doit être créé dans le constructeur de la `Voiture`.



## **8. Activité de consolidation**

* Construire le diagramme de classes UML suivant :

  * `Etudiant` (nom, âge)
  * `Cours` (titre, code)
  * Relation d’association : un étudiant peut suivre plusieurs cours.
  * `Université` (nom) reliée par agrégation à plusieurs `Etudiants`.
  * `Voiture` et `Moteur` reliés par composition.



## **9. Résumé de la séance**

* La POO permet de représenter le monde réel en **classes et objets**.
* Eclipse est l’environnement utilisé pour développer en Java.
* Un programme se compile et s’exécute, les erreurs doivent être corrigées.
* UML est un **langage graphique standard** pour documenter et concevoir.
* Trois types de relations essentielles : **association, agrégation, composition**.


