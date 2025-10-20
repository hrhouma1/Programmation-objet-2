# 01- ÉNONCÉ

On souhaite concevoir une classe **`ImprimanteVirtuelle`** qui représente une imprimante logique unique pour tout un département.
Cette imprimante virtuelle doit être capable d’utiliser **différentes imprimantes physiques** (par exemple HP, Epson, Canon, etc.), sans que l’utilisateur ait besoin de connaître les détails techniques de chacune.

L’objectif est d’appliquer le **patron de conception *Délégation*** :

> La classe `ImprimanteVirtuelle` **ne réalise pas directement l’impression**,
> mais **délègue** cette tâche à un objet d’une autre classe qui implémente le comportement d’impression réel.

L’utilisateur pourra :

* Créer une imprimante virtuelle,
* Lui associer une imprimante physique (HP, Epson, etc.),
* Et changer cette imprimante à tout moment sans modifier le code principal.



# 02 - Résumé 

| Élément                           | Rôle                                                        |
| --------------------------------- | ----------------------------------------------------------- |
| `Imprimante`                      | Interface qui définit le comportement attendu.              |
| `ImprimanteHP`, `ImprimanteEpson` | Classes concrètes qui réalisent l’impression.               |
| `ImprimanteVirtuelle`             | Classe déléguante qui redirige la tâche.                    |
| Ligne clé                         | `imprimante.imprimer(document);` → c’est **la délégation**. |

> Ainsi, `ImprimanteVirtuelle` ne connaît pas les détails techniques des imprimantes physiques.
> Elle **délègue** simplement la responsabilité d’imprimer au bon objet, ce qui illustre parfaitement le **patron de conception Délégation**.

# 03 - Correction 

```java
// ===== Interface commune =====
interface Imprimante {
    void imprimer(String document);
}
```

```java
// ===== Implémentations concrètes =====
class ImprimanteHP implements Imprimante {
    public void imprimer(String document) {
        System.out.println("[HP] Impression du document : " + document);
    }
}

```


```java
class ImprimanteEpson implements Imprimante {
    public void imprimer(String document) {
        System.out.println("[Epson] Impression du document : " + document);
    }
}
```

```java
// ===== Classe qui DÉLÈGUE =====
class ImprimanteVirtuelle {
    private Imprimante imprimante; // référence vers une vraie imprimante

    // On "branche" une imprimante physique à la virtuelle
    public ImprimanteVirtuelle(Imprimante imprimante) {
        this.imprimante = imprimante;
    }

    // On peut changer la cible de délégation
    public void setImprimante(Imprimante imprimante) {
        this.imprimante = imprimante;
    }

    // *** DÉLÉGATION ici ***
    // ImprimanteVirtuelle ne fait pas l’impression elle-même.
    // Elle appelle simplement la méthode de l’objet Imprimante (HP ou Epson).
    public void imprimer(String document) {
        System.out.println("→ ImprimanteVirtuelle reçoit la demande d'impression");
        imprimante.imprimer(document); // <-- DÉLÉGATION
    }
}

```

```java

// ===== Programme principal =====
public class Main {
    public static void main(String[] args) {
        // On crée deux imprimantes physiques
        Imprimante hp = new ImprimanteHP();
        Imprimante epson = new ImprimanteEpson();

        // On crée une imprimante virtuelle qui délègue à HP
        ImprimanteVirtuelle virtuelle = new ImprimanteVirtuelle(hp);

        // 1. L'utilisateur imprime un document → la virtuelle délègue à HP
        virtuelle.imprimer("rapport.pdf");

        // 2. On change la cible de délégation vers Epson
        virtuelle.setImprimante(epson);

        // 3. Maintenant la même commande va vers l’imprimante Epson
        virtuelle.imprimer("facture.docx");
    }
}
```


# 04 - Explication claire :

* `ImprimanteVirtuelle` **ne sait pas comment imprimer** → elle **transfère la responsabilité** à un autre objet (`Imprimante`).
* La ligne clé est :

  ```java
  imprimante.imprimer(document);
  ```

  👉 C’est **l’acte de délégation** : la classe `ImprimanteVirtuelle` reçoit la requête, mais la **fait exécuter par un autre objet**.
* En changeant la cible avec `setImprimante(...)`, on change **le comportement sans réécrire le code**.

💡 **En résumé :**

> La délégation = une classe ne fait pas le travail elle-même,
> mais appelle un autre objet pour le faire à sa place.



# 05 - Déléguant et Délégué 


**Déléguant :** `ImprimanteVirtuelle`
→ Elle reçoit la demande d’impression et la transmet à un autre objet.

**Délégué :** `ImprimanteHP` ou `ImprimanteEpson`
→ Ce sont les objets qui réalisent effectivement l’impression.

**Ligne où la délégation a lieu :**

```java
imprimante.imprimer(document);
```

Ici, `ImprimanteVirtuelle` ne fait pas le travail elle-même.
Elle délègue la tâche à l’objet `imprimante`, qui exécute la méthode réelle.


