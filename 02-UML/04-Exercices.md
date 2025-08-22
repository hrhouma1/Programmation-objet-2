## **Exercice 1 : Encapsulation et visibilités**
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
   
2. **Encapsulation UML** :
   - Les attributs doivent être **privés** (`-`).
   - Les méthodes doivent être **publiques** (`+`).
   - Les getters et setters permettent **l’accès contrôlé** aux attributs.

3. **Représentez cette classe en UML avec ses visibilités**.

---

## **Exercice 2 : Héritage et classe abstraite**
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
   
3. **Relation UML** :
   - **Généralisation (flèche vide `►`)** entre `Personne` et ses sous-classes.
   - `calculerSalaire()` est **abstraite** (italique ou `{abstract}`).

---

## **Exercice 3 : Composition et agrégation**
### **Contexte :**  
Un système de gestion de bibliothèque doit représenter **les livres, les auteurs et les bibliothèques**.

### **Instructions :**  
1. Créez les classes :
   - `Livre` avec `titre : String`, `ISBN : String`, `anneePublication : int`
   - `Auteur` avec `nom : String`, `nationalite : String`
   - `Bibliotheque` avec `nom : String`, `adresse : String`

2. **Relations UML** :
   - **Un livre est écrit par au moins un auteur**, mais un auteur peut avoir écrit plusieurs livres (**cardinalité** `1..*` pour `Auteur`, `0..*` pour `Livre`).
   - **Une bibliothèque contient plusieurs livres** :
     - **Composition (`♦`)** : un livre **n'existe pas** sans une bibliothèque (`1..*` pour `Livre`, `1` pour `Bibliotheque`).
   - Un livre **peut exister sans bibliothèque**, mais **pas sans auteur**.

3. **Représentez cette relation avec un diagramme UML et les cardinalités**.

---

## **Exercice 4 : Interfaces et polymorphisme**
### **Contexte :**  
Un système de paiement supporte **plusieurs moyens de paiement**.

### **Instructions :**  
1. Créez une **interface** `MoyenPaiement` avec :
   - `+ payer(montant: float) : bool`

2. Créez **deux classes** qui implémentent cette interface :
   - `CarteBancaire` avec `numeroCarte : String`
   - `PayPal` avec `email : String`

3. **Relation UML** :
   - **Interface (`<<interface>>`)** pour `MoyenPaiement`.
   - **Implémentation (`- - -►`)** pour `CarteBancaire` et `PayPal`.

4. **Ajoutez une classe `Transaction`** qui possède :
   - `montant : float`
   - Une **association** vers `MoyenPaiement` (`1..1`).

5. **Représentez le diagramme UML avec les relations et les cardinalités**.

---

## **Exercice 5 : Surcharge et redéfinition de méthode**
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

4. **Relation UML** :
   - **Héritage** entre `Produit` et ses sous-classes.
   - **Méthode redéfinie** (`afficherDetails()` avec signature identique dans les sous-classes).

5. **Représentez le diagramme UML avec les bonnes cardinalités**.

---

Ces exercices couvrent **tous les concepts UML essentiels** :  
- **Encapsulation** (privé, public, protégé)  
- **Héritage et classe abstraite**  
- **Composition et agrégation**  
- **Interfaces et polymorphisme**  
- **Redéfinition et surcharge de méthodes**  

