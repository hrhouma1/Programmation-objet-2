# **Glossaire des Flèches UML pour les Diagrammes de Classes**  

## **Introduction**
Les diagrammes de classes UML permettent de modéliser la structure d'un système en représentant les classes et leurs relations. Les relations entre classes sont exprimées à l'aide de différentes flèches et symboles. Ce document décrit en détail ces relations et fournit un tableau récapitulatif des symboles utilisés.

---

## **1. Les Relations UML et leurs Symboles**

### **1.1 Association**
- **Définition** : Une association représente une relation entre deux classes. Elle exprime une connexion logique entre elles.
- **Notation UML** : Ligne continue `────`
- **Exemple** : Une classe `Voiture` peut être associée à une classe `Conducteur`.

```plaintext
Conducteur ──── Voiture
```

- **Variantes** :
  - **Association unidirectionnelle** : Une des classes connaît l’autre, mais pas l’inverse.
    - Notation UML : `────▶`
    - Exemple : Un `Conducteur` peut connaître sa `Voiture`, mais une `Voiture` ne connaît pas forcément son `Conducteur`.
  
    ```plaintext
    Conducteur ────▶ Voiture
    ```

### **1.2 Dépendance**
- **Définition** : Une classe dépend d’une autre d’une manière temporaire (exemple : utilisation d'un service ou appel de méthode).
- **Notation UML** : Flèche en pointillé `- - - -▶`
- **Exemple** : Une classe `Client` dépend d’une classe `Service`.

```plaintext
Client - - - -▶ Service
```

### **1.3 Généralisation (Héritage)**
- **Définition** : Une classe fille hérite des attributs et méthodes d’une classe mère.
- **Notation UML** : Flèche avec un triangle creux `────▷`
- **Exemple** : Une classe `Chien` hérite d’une classe `Animal`.

```plaintext
Animal
  ▲
  │
Chien
```

### **1.4 Réalisation (Implémentation d’Interface)**
- **Définition** : Une classe implémente les opérations définies dans une interface.
- **Notation UML** : Flèche en pointillé avec un triangle creux `- - - -▷`
- **Exemple** : Une classe `Voiture` implémente une interface `MoyenDeTransport`.

```plaintext
MoyenDeTransport
  ▲
  │
- - - -▷ Voiture
```

### **1.5 Agrégation**
- **Définition** : Une relation "partie-tout" faible, où les parties peuvent exister indépendamment du tout.
- **Notation UML** : Ligne continue avec un losange vide `◇────`
- **Exemple** : Une `Flotte` de véhicules contient plusieurs `Voitures`, mais les `Voitures` peuvent exister sans la `Flotte`.

```plaintext
Flotte ◇──── Voiture
```

- **Modèle générique** :
```plaintext
Tout ◇──── Partie
```

### **1.6 Composition**
- **Définition** : Une relation "partie-tout" forte où les parties n’existent pas sans le tout.
- **Notation UML** : Ligne continue avec un losange rempli `◆────`
- **Exemple** : Un `CorpsHumain` possède un `Cœur`. Si le `CorpsHumain` disparaît, le `Cœur` aussi.

```plaintext
CorpsHumain ◆──── Cœur
```

- **Modèle générique** :
```plaintext
Tout ◆──── Partie
```

---

## **2. Multiplicités et Navigabilité**
Les associations peuvent inclure des **multiplicités**, qui définissent le nombre d'instances impliquées dans une relation.

| **Notation** | **Signification** |
|-------------|------------------|
| `1`         | Exactement une instance |
| `0..*`      | De zéro à plusieurs instances |
| `1..*`      | Au moins une instance |
| `n..m`      | Entre `n` et `m` instances |

Exemple : Un `Professeur` peut enseigner à plusieurs `Étudiants`, mais chaque `Étudiant` a un seul `Professeur`.

```plaintext
Professeur 1 ──── * Étudiant
```

La **navigabilité** est indiquée par une flèche :

- `────▶` : L’association est unidirectionnelle.
- `────` : L’association est bidirectionnelle (absence de flèche).

---

## **3. Tableau Récapitulatif des Symboles UML**
Ce tableau récapitule les principales relations UML et leurs notations.

| **Relation**         | **Signification**                                         | **Représentation UML** |
|----------------------|--------------------------------------------------------|------------------------|
| **Association**      | Relation entre deux classes (interaction, collaboration). | Ligne continue `────` |
| **Association unidirectionnelle** | Une classe connaît l'autre, mais pas l'inverse. | Ligne avec une flèche simple `────▶` |
| **Dépendance**       | Une classe dépend temporairement d'une autre. | Flèche en pointillé `- - - -▶` |
| **Généralisation (Héritage)** | Une classe hérite des attributs et méthodes d'une autre. | Flèche triangle creux `────▷` |
| **Réalisation (Implémentation d'Interface)** | Une classe implémente une interface. | Flèche en pointillé avec un triangle `- - - -▷` |
| **Agrégation**       | Relation "partie-tout" faible (le tout peut exister sans les parties). | Ligne avec losange vide `Tout ◇──── Partie` |
| **Composition**      | Relation "partie-tout" forte (le tout possède et contrôle les parties). | Ligne avec losange rempli `Tout ◆──── Partie` |
| **Multiplicité**     | Définit combien d'instances sont impliquées. | `1`, `0..*`, `1..*`, etc. |
| **Navigabilité**     | Indique la direction dans laquelle l'association peut être parcourue. | Flèche directionnelle sur une association `────▶` |

---

## **4. Conclusion**
Les relations UML permettent de structurer et de modéliser efficacement les interactions entre classes dans un système logiciel. Ce glossaire vous aidera à comprendre et appliquer correctement ces concepts dans vos diagrammes de classes.

