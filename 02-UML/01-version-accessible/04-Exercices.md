
## **Exercice 1 : Encapsulation et Visibilités**
### **Contexte :**  
Un système de gestion des employés doit assurer la protection des informations sensibles.

### **Instructions :**  
1. Créez une classe `Employe` avec :
   - **Attributs privés (-)** :
     - `nom : String`
     - `prenom : String`
     - `matricule : int`
     - `salaire : float`
   - **Méthodes publiques (+)** :
     - `+ getSalaire() : float`
     - `+ setSalaire(salaire: float) : void`
     - `+ calculerPrime() : float`
     - `+ afficherDetails() : void`

**Diagramme UML en ASCII :**
```
+----------------------+
|      Employe        |
+----------------------+
| - nom : String      |
| - prenom : String   |
| - matricule : int   |
| - salaire : float   |
+----------------------+
| + getSalaire()      |
| + setSalaire()      |
| + calculerPrime()   |
| + afficherDetails() |
+----------------------+
```
L'encapsulation est respectée avec **attributs privés (-)** et **méthodes publiques (+)**.

---

## **Exercice 2 : Héritage et Classe Abstraite**
### **Contexte :**  
Un système de gestion de personnel distingue **les employés à temps plein et les freelances**.

### **Instructions :**  
1. Créez une **classe abstraite** `Personne` :
   - **Attributs protégés (#)** :
     - `nom : String`
     - `email : String`
   - **Méthode abstraite (en italique)** :
     - `# calculerSalaire() : float`

2. Créez deux sous-classes :
   - `EmployeTempsPlein` avec l’attribut `salaireMensuel : float`
   - `Freelance` avec les attributs :
     - `tauxHoraire : float`
     - `heuresTravaillees : int`

**Diagramme UML en ASCII :**
```
              +---------------------+
              |     Personne (A)    |
              +---------------------+
              | # nom : String      |
              | # email : String    |
              +---------------------+
              | # calculerSalaire() |
              +---------------------+
                     |
        ---------------------------
        |                          |
+---------------------+    +----------------------+
| EmployeTempsPlein  |    |     Freelance        |
+---------------------+    +----------------------+
| + salaireMensuel   |    | + tauxHoraire        |
|                    |    | + heuresTravaillees  |
+---------------------+    +----------------------+
| + calculerSalaire()|    | + calculerSalaire()  |
+---------------------+    +----------------------+
```
L’héritage est représenté par une **flèche avec un triangle ouvert** (`Personne ► EmployeTempsPlein, Freelance`).
La méthode `calculerSalaire()` est **abstraite** et doit être implémentée dans les sous-classes.

---

## **Exercice 3 : Composition et Agrégation**
### **Contexte :**  
Un système de gestion de bibliothèque doit représenter **les livres, les auteurs et les bibliothèques**.

### **Relations UML** :
- **Un livre est écrit par au moins un auteur** (*association* `1..*` pour `Auteur`, `0..*` pour `Livre`).
- **Une bibliothèque contient plusieurs livres** (*composition* `♦`).
- **Un livre peut exister sans bibliothèque, mais pas sans auteur**.

**Diagramme UML en ASCII :**
```
+---------------------+       +---------------------+
|       Auteur       | 1..*  |        Livre        |
+---------------------+-------+---------------------+
| - nom : String     |       | - titre : String   |
| - nationalite : String|    | - ISBN : String    |
+---------------------+       | - anneePublication: int |
                              +---------------------+
                                |
                                | 0..*   (Composition)
                                |♦
+------------------------+       |
|     Bibliotheque      |-------+
+------------------------+
| - nom : String        |
| - adresse : String    |
+------------------------+
```
L’association entre `Livre` et `Auteur` est **multiples-à-multiples** (`1..*` pour `Auteur`, `0..*` pour `Livre`).
La **composition** (`♦`) signifie qu’une `Bibliotheque` **possède** des `Livre`.

---

## **Exercice 4 : Interfaces et Polymorphisme**
### **Contexte :**  
Un système de paiement supporte **plusieurs moyens de paiement**.

### **Instructions :**  
1. Créez une **interface** `MoyenPaiement` avec :
   - `+ payer(montant: float) : bool`

2. Créez **deux classes** qui implémentent cette interface :
   - `CarteBancaire` avec `numeroCarte : String`
   - `PayPal` avec `email : String`

3. Ajoutez une classe `Transaction` qui contient :
   - `montant : float`
   - Une **association** vers `MoyenPaiement` (`1..1`).

**Diagramme UML en ASCII :**
```
         <<interface>>
+----------------------+
|   MoyenPaiement     |
+----------------------+
| + payer(montant)    |
+----------------------+
       /       \
      /         \
     /           \
+----------------------+    +--------------------+
|    CarteBancaire     |    |       PayPal      |
+----------------------+    +--------------------+
| - numeroCarte : String |  | - email : String  |
+----------------------+    +--------------------+
| + payer(montant)     |    | + payer(montant)  |
+----------------------+    +--------------------+
           |
         1..1
           |
+----------------------+
|     Transaction     |
+----------------------+
| - montant : float   |
+----------------------+
| + effectuer()       |
+----------------------+
```
L’**interface `MoyenPaiement` est représentée avec `<<interface>>`**.  
Les classes `CarteBancaire` et `PayPal` **implémentent** cette interface (`- - -►` en UML).

---

## **Exercice 5 : Surcharge et Redéfinition de Méthode**
### **Contexte :**  
Un magasin gère **des produits avec des variantes spécifiques**.

### **Instructions :**  
1. Créez une classe `Produit` avec :
   - `nom : String`
   - `prix : float`
   - `+ afficherDetails() : void`

2. Ajoutez deux sous-classes :
   - `Vetement` avec `taille : String`, `couleur : String`
   - `Electronique` avec `garantie : int`, `puissance : float`

3. **Redéfinissez `afficherDetails()`** :
   - `Vetement` ajoute `taille` et `couleur`.
   - `Electronique` ajoute `garantie` et `puissance`.

**Diagramme UML en ASCII :**
```
      +----------------+
      |    Produit     |
      +----------------+
      | - nom : String |
      | - prix : float |
      +----------------+
      | + afficherDetails() |
      +----------------+
          |
  -------------------
  |                 |
+----------------+    +-----------------+
|   Vetement    |    |   Electronique   |
+----------------+    +-----------------+
| - taille : String |  | - garantie : int |
| - couleur : String|  | - puissance : float |
+----------------+    +-----------------+
| + afficherDetails()|  | + afficherDetails()|
+----------------+    +-----------------+
```
L’héritage est représenté par une **flèche ouverte (`►`)**.  
La méthode `afficherDetails()` est **redéfinie** dans les sous-classes.

