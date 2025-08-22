# **Les Cardinalités UML : Notations, Équivalences et Bonnes Pratiques**

## **Introduction**
Les cardinalités en UML définissent le nombre d’instances pouvant être impliquées dans une relation entre classes. Elles permettent d’exprimer les contraintes quantitatives d’une association, comme le nombre minimum et maximum d’objets pouvant participer à la relation.

Différents outils, documentations et langages utilisent des notations alternatives, ce qui peut prêter à confusion. Ce cours fournit une explication claire et exhaustive, ainsi que toutes les notations possibles, pour éviter toute ambiguïté lorsque vous travaillez avec UML.

---

## **1. Définition et Rôle des Cardinalités**
En UML, chaque relation entre classes peut être simple (une seule instance), optionnelle (peut exister ou non), multiple (peut lier plusieurs instances), ou bien fixée à une plage précise d’instances.

La cardinalité est généralement indiquée à chaque extrémité d’une association pour définir combien d’instances d’une classe peuvent être associées à une instance d’une autre classe.

### **Exemple UML simple**
```plaintext
Client 1 ──── 0..* Commande
```
Interprétation :
- Un Client peut avoir aucune ou plusieurs commandes (`0..*`).
- Une Commande appartient obligatoirement à un seul client (`1`).

---

## **2. Liste Complète des Cardinalités et leurs Variations**
Les différentes notations possibles sont listées ci-dessous, accompagnées de toutes leurs variantes et équivalences trouvables dans la littérature et les outils UML.

### **2.1 Cardinalité Unique (Exactement une instance)**
- **Signification** : Chaque instance de la classe A est associée exactement à une instance de la classe B.
- **Notations UML possibles** :
  - `1`
  - `1..1`
- **Équivalences possibles** :
  - `exactly 1`
  - `one and only one`
  - `(1)`
  - `{1}`
  - `[1]`

**Exemple UML** :
```plaintext
Utilisateur 1 ──── 1 Compte
```
Un Utilisateur doit posséder exactement un Compte, et un Compte appartient exactement à un Utilisateur.

---

### **2.2 Cardinalité Optionnelle (Zéro ou une instance)**
- **Signification** : Une instance de la classe A peut être associée à zéro ou une instance de la classe B.
- **Notations UML possibles** :
  - `0..1`
  - `?`
- **Équivalences possibles** :
  - `zero or one`
  - `[0,1]`
  - `{0,1}`
  - `(0,1)`
  - `0..n` (rare)

**Exemple UML** :
```plaintext
Employé 0..1 ──── Voiture de fonction
```
Un Employé peut avoir une Voiture de fonction, mais ce n’est pas obligatoire.

---

### **2.3 Cardinalité Illimitée (Aucun ou plusieurs)**
- **Signification** : Une instance de la classe A peut être associée à aucune, une ou plusieurs instances de la classe B.
- **Notations UML possibles** :
  - `*`
  - `0..*`
  - `n`
- **Équivalences possibles** :
  - `many`
  - `zero or more`
  - `[0..*]`
  - `{0..*}`
  - `(0,∞)`
  - `0..n`

**Exemple UML** :
```plaintext
Client 0..* ──── Commande
```
Un Client peut passer zéro, une ou plusieurs Commandes.

---

### **2.4 Cardinalité Minimale et Maximale (Plage de valeurs)**
- **Signification** : Une instance de la classe A peut être associée à un nombre précis d’instances de la classe B (exemple : entre 2 et 5 instances).
- **Notations UML possibles** :
  - `m..n`
  - `2..5`
- **Équivalences possibles** :
  - `at least m and at most n`
  - `[2,5]`
  - `{2,5}`
  - `(2,5)`
  - `min m, max n`

**Exemple UML** :
```plaintext
Équipe 2..5 ──── Joueur
```
Une Équipe doit avoir au minimum 2 et au maximum 5 Joueurs.

---

### **2.5 Cardinalité Obligatoire (1 ou plus)**
- **Signification** : Il doit y avoir au moins une instance de la classe B associée à chaque instance de la classe A.
- **Notations UML possibles** :
  - `1..*`
- **Équivalences possibles** :
  - `one or more`
  - `1 to many`
  - `[1..*]`
  - `{1..*}`
  - `(1,∞)`
  - `1..n`

**Exemple UML** :
```plaintext
Professeur 1..* ──── Étudiant
```
Un Professeur doit avoir au moins un Étudiant.

---

### **2.6 Cardinalité Fixe (Exactement N)**
- **Signification** : Il doit y avoir exactement `N` instances dans la relation.
- **Notations UML possibles** :
  - `n`
  - `exactly n`
- **Équivalences possibles** :
  - `precisely n`
  - `[n]`
  - `{n}`
  - `(n,n)`
  - `fixed n`

**Exemple UML** :
```plaintext
Voiture 4 ──── Roue
```
Une Voiture a exactement 4 Roues.

---

### **2.7 Cardinalité Obligatoire à partir de N (Minimum N)**
- **Signification** : Il doit y avoir au moins `N` instances associées.
- **Notations UML possibles** :
  - `n..*`
- **Équivalences possibles** :
  - `at least n`
  - `[n..*]`
  - `{n..*}`
  - `(n,∞)`
  - `n or more`

**Exemple UML** :
```plaintext
Équipe 2..* ──── Joueur
```
Une Équipe doit contenir au moins 2 Joueurs.

---

## **3. Tableau Récapitulatif des Notations**
| **Cardinalité**      | **Signification**                              | **Notations UML**                                        | **Autres équivalences trouvables**                 |
|----------------------|-----------------------------------------------|---------------------------------------------------------|---------------------------------------------------|
| **1 seule instance** | Exactement une instance                      | `1`, `1..1`                                             | `exactly 1`, `one and only one`, `(1)`, `{1}` |
| **Optionnelle**      | Zéro ou une instance                         | `0..1`, `?`                                             | `zero or one`, `[0,1]`, `{0,1}`, `(0,1)` |
| **Illimitée**        | Aucun ou plusieurs                           | `*`, `0..*`, `n`                                        | `many`, `zero or more`, `(0,∞)` |
| **Plage de valeurs** | Entre m et n instances                       | `m..n`, `2..5`                                          | `[2,5]`, `{2,5}`, `(2,5)` |
| **Obligatoire ≥ 1**  | Une instance minimum                         | `1..*`                                                  | `one or more`, `1 to many`, `(1,∞)` |

---

## **4. Conclusion**
Ce cours vous permet d’éviter toute confusion avec les différentes notations UML utilisées à travers divers outils et documentations. Il garantit une compréhension complète et permet d'utiliser correctement les cardinalités UML dans vos diagrammes.
