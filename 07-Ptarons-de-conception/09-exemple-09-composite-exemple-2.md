
# Exemple 2 composite

> **Un programme de formation** contient **des cours**.
> Un **cours** contient **des modules**.
> Un **module** contient **des leçons**.
> On veut **afficher tout le plan** d’un coup.

C’est exactement un **Composite** : certains éléments **contiennent d’autres éléments** (programme, cours, module), d’autres sont **feuilles** (leçon).

---

# 01 – ÉNONCÉ

On veut modéliser la structure d’un **programme de formation**.

* Tout en haut : **Programme** (“Formation Développeur Fullstack”)
* Dans ce programme : plusieurs **Cours** (“HTML/CSS”, “Java”, “Docker”)
* Dans chaque cours : plusieurs **Modules** (“Module 1 – Introduction”, “Module 2 – Composants”, etc.)
* Dans chaque module : plusieurs **Leçons** (“Qu’est-ce que HTML ?”, “Créer sa première page”, etc.)

Objectif :
👉 **imprimer tout le plan du programme** avec **un seul appel**, sans tester “est-ce que c’est un cours ? un module ? une leçon ?”.

On applique le **patron Composite** :

> On définit **une interface commune** `ElementProgramme` avec une méthode `afficher(int niveau)`,
> puis on crée :
>
> * des **composites** (Programme, Cours, Module) qui ont une liste d’éléments,
> * et des **feuilles** (Leçon) qui n’ont pas d’enfants.

---

# 02 – Résumé

| Élément            | Rôle                                                         |
| ------------------ | ------------------------------------------------------------ |
| `ElementProgramme` | **Interface commune** : tout le monde sait s’“afficher”.     |
| `Lecon`            | **Feuille** : élément terminal (pas d’enfants).              |
| `Module`           | **Composite** : contient plusieurs leçons.                   |
| `Cours`            | **Composite** : contient plusieurs modules.                  |
| `Programme`        | **Composite racine** : contient plusieurs cours.             |
| Ligne clé          | `for (ElementProgramme e : enfants) e.afficher(niveau + 1);` |

> Le client appelle `programme.afficher(0);` et **tout** s’affiche (cours → modules → leçons).

---

# 03 – Correction

```java
// ===== 1. Interface commune =====
interface ElementProgramme {
    void afficher(int niveau);
}
```

```java
// ===== 2. Feuille : Leçon =====
class Lecon implements ElementProgramme {
    private String titre;

    public Lecon(String titre) {
        this.titre = titre;
    }

    @Override
    public void afficher(int niveau) {
        System.out.println("  ".repeat(niveau) + "- Leçon : " + titre);
    }
}
```

```java
// ===== 3. Composite de 1er niveau : Module =====
import java.util.ArrayList;
import java.util.List;

class Module implements ElementProgramme {
    private String titre;
    private List<ElementProgramme> elements = new ArrayList<>();

    public Module(String titre) {
        this.titre = titre;
    }

    public void add(ElementProgramme e) {
        elements.add(e);
    }

    @Override
    public void afficher(int niveau) {
        System.out.println("  ".repeat(niveau) + "Module : " + titre);
        for (ElementProgramme e : elements) {
            e.afficher(niveau + 1);   // <-- COMPOSITE ICI
        }
    }
}
```

```java
// ===== 4. Composite de 2e niveau : Cours =====
class Cours implements ElementProgramme {
    private String titre;
    private List<ElementProgramme> modules = new ArrayList<>();

    public Cours(String titre) {
        this.titre = titre;
    }

    public void add(ElementProgramme m) {
        modules.add(m);
    }

    @Override
    public void afficher(int niveau) {
        System.out.println("  ".repeat(niveau) + "Cours : " + titre);
        for (ElementProgramme m : modules) {
            m.afficher(niveau + 1);
        }
    }
}
```

```java
// ===== 5. Composite racine : Programme =====
class Programme implements ElementProgramme {
    private String titre;
    private List<ElementProgramme> cours = new ArrayList<>();

    public Programme(String titre) {
        this.titre = titre;
    }

    public void add(ElementProgramme c) {
        cours.add(c);
    }

    @Override
    public void afficher(int niveau) {
        System.out.println("  ".repeat(niveau) + "Programme : " + titre);
        for (ElementProgramme c : cours) {
            c.afficher(niveau + 1);
        }
    }
}
```

```java
// ===== 6. Programme principal =====
public class Main {
    public static void main(String[] args) {

        // Leçons
        Lecon l1 = new Lecon("1.1 – Qu’est-ce que HTML ?");
        Lecon l2 = new Lecon("1.2 – Première page");
        Lecon l3 = new Lecon("2.1 – Sélecteurs CSS");
        Lecon l4 = new Lecon("2.2 – Flexbox");

        // Modules
        Module m1 = new Module("Module 1 – HTML de base");
        m1.add(l1);
        m1.add(l2);

        Module m2 = new Module("Module 2 – CSS");
        m2.add(l3);
        m2.add(l4);

        // Cours
        Cours coursWeb = new Cours("Cours 1 – Développement Web");
        coursWeb.add(m1);
        coursWeb.add(m2);

        // Autre cours (vide pour l'exemple)
        Cours coursJava = new Cours("Cours 2 – Java");

        // Programme global
        Programme programme = new Programme("Programme – AEC Dév Web");
        programme.add(coursWeb);
        programme.add(coursJava);

        // Un seul appel :
        programme.afficher(0);
    }
}
```

**Sortie possible :**

```text
Programme : Programme – AEC Dév Web
  Cours : Cours 1 – Développement Web
    Module : Module 1 – HTML de base
      - Leçon : 1.1 – Qu’est-ce que HTML ?
      - Leçon : 1.2 – Première page
    Module : Module 2 – CSS
      - Leçon : 2.1 – Sélecteurs CSS
      - Leçon : 2.2 – Flexbox
  Cours : Cours 2 – Java
```

Tu vois : **un seul appel** → tout le plan s’imprime.

---

# 04 – Explication claire

* **Problème de départ** : certains éléments sont “simples” (leçon), d’autres sont “composés” (module, cours). Mais on veut **les traiter pareil**.

* **Solution Composite** : on donne une **même interface** à tout le monde (`ElementProgramme`), avec une méthode `afficher(...)`.

* Une **feuille** (leçon) → affiche juste son nom.

* Un **composite** (module, cours, programme) → s’affiche **puis** affiche ses enfants.

* La **ligne clé** (toujours la même) :

  ```java
  for (ElementProgramme e : elements) {
      e.afficher(niveau + 1);
  }
  ```

  → On ne se demande pas “est-ce un module ou une leçon ?”
  → On fait confiance au polymorphisme.

* **Pourquoi c’est pédagogique** ?
  Parce que tu peux dire à tes étudiants :
  “Remplace `Programme/Cours/Module/Leçon` par `Dossier/Sous-dossier/Fichier` → c’est la même idée.”

---

# 05 – Composite & Feuilles (version prof)

* **Component** : `ElementProgramme`
* **Leaf** : `Lecon`
* **Composite** : `Module`, `Cours`, `Programme`
* **Client** : appelle juste `programme.afficher(0);`

