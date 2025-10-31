# 01 – ÉNONCÉ

Dans une petite entreprise, il y a **5 employés** :

* 1 **Directeur général** (tout en haut),
* sous lui : **2 employés**

  * 1 **Gestionnaire**
  * 1 **Développeur** (individuel)
* le **Gestionnaire** a **2 développeurs** qui travaillent sous lui.

Schéma mental :

```text
Directeur général
├── Gestionnaire
│   ├── Développeur A
│   └── Développeur B
└── Développeur C
```

On veut **imprimer le nom et le salaire de tous les employés de haut en bas**.

On va utiliser le **patron de conception Composite** :

> Le Composite permet de traiter **de la même façon** un **objet simple** (un développeur) et un **objet composé** (un gestionnaire qui contient d’autres employés).
>
> Autrement dit : *“feuille”* et *“groupe”* ont **la même interface**.

L’idée :

* `Employee` → interface commune
* `Developer` → feuille (pas d’enfants)
* `Manager` → composite (a une liste d’employés)
* On appelle **la même méthode** sur le DG et ça descend tout seul.

<br/>

# 02 – Résumé

| Élément     | Rôle                                                          |
| ----------- | ------------------------------------------------------------- |
| `Employee`  | **Interface commune** : nom, salaire, affichage.              |
| `Developer` | **Feuille** : un employé simple, pas d’enfants.               |
| `Manager`   | **Composite** : contient une liste d’`Employee`.              |
| Ligne clé   | `for (Employee e : subordinates) e.print();` → **récursion**. |
| But         | Parcourir **toute la hiérarchie** avec **une seule méthode**. |

> Grâce à Composite, le client n’a pas besoin de savoir si c’est un gestionnaire ou un développeur : il appelle juste `print()`.

<br/>

# 03 – Correction

```java
// ===== 1. Interface commune =====
interface Employee {
    void print();    // afficher nom + salaire
}
```

```java
// ===== 2. Feuille : Développeur =====
class Developer implements Employee {
    private String name;
    private double salary;

    public Developer(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }

    @Override
    public void print() {
        System.out.println("Développeur : " + name + " | Salaire : " + salary);
    }
}
```

```java
// ===== 3. Composite : Manager (peut contenir d'autres employés) =====
import java.util.ArrayList;
import java.util.List;

class Manager implements Employee {
    private String name;
    private double salary;
    private List<Employee> subordinates = new ArrayList<>();

    public Manager(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }

    // Ajouter un employé sous ce manager
    public void add(Employee e) {
        subordinates.add(e);
    }

    // Retirer un employé (optionnel)
    public void remove(Employee e) {
        subordinates.remove(e);
    }

    @Override
    public void print() {
        // 1. Afficher moi-même
        System.out.println("Manager     : " + name + " | Salaire : " + salary);
        // 2. Puis afficher tous ceux qui sont sous moi (récursif)
        for (Employee e : subordinates) {
            e.print();  // <-- COMPOSITE ICI
        }
    }
}
```

```java
// ===== 4. Programme principal =====
public class Main {
    public static void main(String[] args) {

        // 1. Créer les employés simples (feuilles)
        Employee devA = new Developer("Dev A", 45000);
        Employee devB = new Developer("Dev B", 46000);
        Employee devC = new Developer("Dev C", 48000);

        // 2. Créer le gestionnaire et lui ajouter ses deux développeurs
        Manager gestionnaire = new Manager("Gestionnaire", 60000);
        gestionnaire.add(devA);
        gestionnaire.add(devB);

        // 3. Créer le directeur général (tout en haut)
        Manager directeurGeneral = new Manager("Directeur général", 90000);

        // Sous le directeur : le gestionnaire + un développeur direct
        directeurGeneral.add(gestionnaire);
        directeurGeneral.add(devC);

        // 4. Afficher TOUTE la hiérarchie
        directeurGeneral.print();
    }
}
```

**Sortie possible :**

```text
Manager     : Directeur général | Salaire : 90000.0
Manager     : Gestionnaire | Salaire : 60000.0
Développeur : Dev A | Salaire : 45000.0
Développeur : Dev B | Salaire : 46000.0
Développeur : Dev C | Salaire : 48000.0
```

Tu vois : on a juste fait **un seul appel** :

```java
directeurGeneral.print();
```

…et ça a imprimé **toute l’entreprise**.

<br/>

# 04 – Explication claire

* Dans **Composite**, tout le monde parle à la **même interface** (`Employee`).

* Un **Developer** sait juste s’afficher.

* Un **Manager** s’affiche **puis** demande à tous ceux qui sont sous lui de s’afficher.

* Comme un manager contient lui-même des `Employee`, et qu’un manager est un `Employee`, **ça peut descendre autant de niveaux qu’on veut** → c’est la **récursivité**.

* La **ligne clé** est dans `Manager.print()` :

  ```java
  for (Employee e : subordinates) {
      e.print();   // on ne sait pas si c'est un dev ou un autre manager
  }
  ```

  👉 C’est là que le patron Composite montre sa force : **on traite feuille et groupe de la même façon.**

* Du point de vue du **client** (le `main`), c’est super simple :

  ```java
  directeurGeneral.print();
  ```

  pas de `if (manager) ... else ...`.

<br/>

# 05 – Composite & Feuilles

**Composant (Component)** :
→ `Employee`
→ définit ce que tous les employés savent faire (`print()`).

**Feuilles (Leaf)** :
→ `Developer`
→ pas d’enfants, implémentation directe.

**Composite** :
→ `Manager`
→ a une **liste d’`Employee`**
→ appelle `print()` sur chacun.

<br/>

# ANENXE 1



Le problème que **Composite** résout, c’est exactement celui-là :

> **“Dans mon modèle, j’ai parfois UN objet simple… et parfois un GROUPE d’objets… et mon code devient moche parce que je dois traiter ces deux cas différemment.”**

Je te le déroule.

---

## 1. Le problème de départ

Dans plein d’apps, tu as une **structure en arbre** :

* Entreprise → managers → employés
* Système de fichiers → dossier → sous-dossier → fichier
* Cours → modules → leçons
* UI → conteneur → boutons, labels…

Dans ces structures, il y a **deux types** d’objets :

1. **les feuilles** → rien en dessous (un fichier, un développeur, une leçon)
2. **les composites** → contiennent d’autres objets (un dossier, un manager, un module)

👉 Et toi, dans ton code, tu veux faire **la même opération** sur les deux (afficher, calculer un coût, exporter…).

Sans Composite, tu te retrouves à faire ça partout :

```java
if (objet est un dossier) {
    // alors je parcours les enfants
} else {
    // c’est un fichier → j’affiche
}
```

ou

```java
if (objet est un manager) {
    // afficher lui + ses subordonnés
} else {
    // afficher juste l’employé
}
```

Résultat :

* du **if/else** partout
* du code **dupliqué**
* chaque fois que tu ajoutes un nouveau type de “groupe”, tu dois **revenir modifier** le code client.

---

## 2. Ce que fait Composite

Composite dit :

> “Et si **feuille** et **groupe** avaient **la même interface** ?
> Comme ça, le client ne se pose plus de question.”

Donc tu écris :

```java
interface Employee {
    void print();
}
```

et après ça, **que ce soit** un `Developer` **ou** un `Manager`, tu peux faire :

```java
employee.print();
```

sans `if`.

Le composite, lui, fait la récursion :

```java
for (Employee e : subordinates) {
    e.print();
}
```

---

## 3. Donc le problème exact

On peut le formuler comme ça (version prof) :

1. **Problème structurel** : représenter des objets organisés en hiérarchie (arbre) où certains objets peuvent contenir d’autres objets du même type.
2. **Problème d’uniformité** : permettre au code client de **manipuler de la même façon** un objet simple et un ensemble d’objets.
3. **Problème de complexité du client** : éviter que le client ait à tester sans arrêt “est-ce une feuille ou un composite ?”.

👉 **Composite** résout ces 3 problèmes en donnant **une interface commune** et en déplaçant la complexité **à l’intérieur** des composites.

---

## 4. Phrase à retenir

> “Sans Composite, le client doit savoir si c’est un élément seul ou un groupe.
> Avec Composite, le client s’en fiche : il appelle toujours la même méthode.”

C’est ça qu’il résout.

**Idée à retenir** :

> Composite = *“un objet peut contenir d’autres objets du même type”*
> → tu peux traiter “un seul employé” et “un groupe d’employés” **exactement pareil**.
