# **Cours UML**

## **Objectif**  
- Ce cours propose une approche détaillée d'UML (**Unified Modeling Language**) en utilisant uniquement du texte structuré. 
- Il couvre tous les aspects essentiels : visibilité des attributs et méthodes, bonnes pratiques de modélisation, types de relations entre classes, et de nombreux exercices corrigés.

---

## **Table des matières**  

1. [Introduction à UML](#section-1)  
2. [Pourquoi UML est-il utile ?](#section-2)  
3. [Les concepts fondamentaux](#section-3)  
   - [Visibilité des attributs et méthodes](#section-3-1)  
   - [Types d’attributs et méthodes](#section-3-2)  
   - [Relations entre classes](#section-3-3)  
4. [Les types de diagrammes UML](#section-4)  
   - [Diagramme de cas d'utilisation](#section-4-1)  
   - [Diagramme de classes](#section-4-2)  
   - [Diagramme de séquence](#section-4-3)  
   - [Diagramme d'activités](#section-4-4)  
5. [Représentation textuelle des diagrammes](#section-5)  
6. [Exercices pratiques](#section-6)  
7. [Corrections détaillées](#section-7)  

---

## **Section 1 : Introduction à UML**  
<a name="section-1"></a>  

UML (**Unified Modeling Language**) est un langage permettant de modéliser des logiciels avant leur développement. Il aide à structurer un programme sous forme d’objets, d’interactions et de processus.

Il sert à répondre à des questions comme :  
- Quels sont les éléments du système ?  
- Comment ces éléments interagissent-ils ?  
- Quelles sont les étapes du fonctionnement du système ?  

---

## **Section 2 : Pourquoi UML est-il utile ?**  
<a name="section-2"></a>  

UML est utilisé pour :  
- **Faciliter la communication entre les parties prenantes** d’un projet.  
- **Aider à la conception logicielle** avant d’écrire le code.  
- **Fournir une documentation claire** et maintenable.  

---

## **Section 3 : Concepts fondamentaux**  
<a name="section-3"></a>  

### **Visibilité des attributs et méthodes**  
<a name="section-3-1"></a>  

Chaque attribut ou méthode dans une classe UML possède un **niveau de visibilité**, indiqué par un symbole :  
- **Public (+)** : Accessible depuis n'importe quelle autre classe.  
- **Privé (-)** : Accessible uniquement à l’intérieur de la classe.  
- **Protégé (#)** : Accessible dans la classe et ses sous-classes.  
- **Package (~)** : Accessible uniquement dans le même package.  

Exemple structuré en texte :  
```
Classe : CompteBancaire  
Attributs :  
   - solde : float (privé)  
   + titulaire : string (public)  
Méthodes :  
   + deposerArgent(montant : float)  
   - calculerInterets()  
```

---

### **Types d’attributs et méthodes**  
<a name="section-3-2"></a>  

**Attributs courants** :  
- Chaîne de caractères : `string`  
- Nombres : `int`, `float`  
- Booléens : `true/false`  
- Collections : `liste`, `dictionnaire`  

**Méthodes courantes** :  
- Constructeur : Permet d'initialiser une classe.  
- Getters/Setters : Permettent de récupérer ou modifier des valeurs privées.  

Exemple structuré :  
```
Classe : Utilisateur  
Attributs :  
   - motDePasse : string (privé)  
   + nom : string (public)  
Méthodes :  
   + seConnecter(mdp : string) : bool  
   + recupererNom() : string  
   - crypterMotDePasse() : string  
```

---

### **Relations entre classes**  
<a name="section-3-3"></a>  

UML définit plusieurs types de relations entre classes :  

1. **Association** (relation simple entre objets)  
   ```
   Classe : Client  
   Classe : Commande  
   Relation : Un client peut passer plusieurs commandes.  
   ```
2. **Héritage** (relation "est un")  
   ```
   Classe : Animal  
   Classe : Chien (hérite de Animal)  
   ```
3. **Composition** (relation "fait partie de")  
   ```
   Classe : Voiture  
   Classe : Moteur  
   Relation : Une voiture a un moteur (forte dépendance).  
   ```

---

## **Section 4 : Les types de diagrammes UML**  
<a name="section-4"></a>  

### **Diagramme de cas d'utilisation**  
<a name="section-4-1"></a>  

```
Acteur : Utilisateur  
Cas d'utilisation : Réserver un billet  
Cas d'utilisation : Consulter son historique  
```

---

### **Diagramme de classes**  
<a name="section-4-2"></a>  

```
+------------------+  
|    Utilisateur   |  
+------------------+  
| - nom : string   |  
| - email : string |  
+------------------+  
| + seConnecter()  |  
| + recupererNom() |  
+------------------+  
```

---

### **Diagramme de séquence**  
<a name="section-4-3"></a>  

```
1. L'utilisateur saisit ses identifiants.  
2. Le système vérifie la validité.  
3. Si valides, affichage de l'accueil.  
4. Sinon, affichage d'une erreur.  
```

---

### **Diagramme d'activités**  
<a name="section-4-4"></a>  

```
1. Début  
2. L’utilisateur entre ses informations  
3. Le système vérifie les données  
   - Si correctes : Confirmation  
   - Sinon : Demande de correction  
4. Fin  
```

---

## **Section 5 : Représentation textuelle des diagrammes**  
<a name="section-5"></a>  

### **Diagramme de classes en ASCII**  

```
+------------------+  
|    Client        |  
+------------------+  
| - nom : string   |  
| - email : string |  
+------------------+  
| + passerCommande() |  
+------------------+  
        |  
        v  
+------------------+  
|    Commande      |  
+------------------+  
| - numero : int   |  
| - date : string  |  
+------------------+  
| + valider()      |  
+------------------+  
```

---

## **Section 6 : Exercices pratiques**  
<a name="section-6"></a>  

### **Exercice 1 : Identifier des cas d'utilisation**  
Définir les acteurs et cas d'utilisation d’un site e-commerce.

### **Exercice 2 : Construire une classe**  
Créer une classe `Produit` avec des attributs et méthodes adaptées.

### **Exercice 3 : Comprendre un flux d'interaction**  
Lister les étapes de paiement d’une commande en ligne.

---

## **Section 7 : Corrections détaillées**  
<a name="section-7"></a>  

### **Correction Exercice 1**  

**Acteurs :**  
- Client  
- Administrateur  

**Cas d’utilisation :**  
- Ajouter un produit au panier  
- Valider une commande  
- Modifier les stocks  

---

### **Correction Exercice 2**  

```
Classe : Produit  
Attributs :  
   + nom : string  
   + prix : float  
   + stock : int  
Méthodes :  
   + afficherDetails()  
   + modifierStock()  
```

---

### **Correction Exercice 3**  

```
1. L’utilisateur valide son panier.  
2. Il saisit ses informations bancaires.  
3. Le système vérifie la transaction.  
4. Si acceptée, confirmation.  
5. Sinon, message d’erreur.  
```



