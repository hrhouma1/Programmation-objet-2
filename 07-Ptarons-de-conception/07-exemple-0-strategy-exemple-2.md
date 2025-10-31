# 01 – ÉNONCÉ

On veut construire un petit programme de **calcul** qui peut faire **plusieurs opérations** sur deux nombres :

* addition,
* soustraction,
* multiplication.

Le point important : **le reste du programme ne doit pas changer** quand on change l’opération.
On veut appliquer le **patron de conception Strategy** :

> Le contexte (la partie “fixe” du programme) ne connaît pas le détail du calcul.
> Il appelle une **stratégie** qui, elle, réalise le calcul (addition, soustraction, multiplication).

L’utilisateur pourra :

* Créer un contexte de calcul,
* Lui donner une stratégie (addition, soustraction, multiplication),
* Appeler toujours la même méthode `execute(a, b)`,
* Changer de stratégie sans réécrire le code du contexte.

C’est exactement le cas “il y a un nombre d’algorithmes similaires” → on les rend **interchangeables**.

<br/>

# 02 – Résumé

| Élément            | Rôle                                                                 |
| ------------------ | -------------------------------------------------------------------- |
| `StrategyIF`       | Interface qui définit l’opération de calcul.                         |
| `StrategyAdd`      | Calcule `a + b`.                                                     |
| `StrategySubtract` | Calcule `a - b`.                                                     |
| `StrategyMultiply` | Calcule `a * b`.                                                     |
| `Context`          | Classe qui utilise une stratégie pour faire le calcul.               |
| Ligne clé          | `return strategy.execute(a, b);` → c’est l’appel à la **stratégie**. |

> Ainsi, `Context` ne met pas de `if (op == ...)` à l’intérieur.
> Il délègue le calcul à l’objet stratégie qui a été fourni.

<br/>

# 03 – Correction

```java
// ===== Interface STRATEGY =====
interface StrategyIF {
    int execute(int a, int b);
}
```

```java
// ===== Stratégie 1 : Addition =====
class StrategyAdd implements StrategyIF {
    public int execute(int a, int b) {
        System.out.println("Called StrategyAdd's execute()");
        return a + b;
    }
}
```

```java
// ===== Stratégie 2 : Soustraction =====
class StrategySubtract implements StrategyIF {
    public int execute(int a, int b) {
        System.out.println("Called StrategySubtract's execute()");
        return a - b;
    }
}
```

```java
// ===== Stratégie 3 : Multiplication =====
class StrategyMultiply implements StrategyIF {
    public int execute(int a, int b) {
        System.out.println("Called StrategyMultiply's execute()");
        return a * b;
    }
}
```

```java
// ===== CONTEXTE : utilise une stratégie, mais ne connaît pas le détail =====
class Context {
    private StrategyIF strategy;

    // On fournit la stratégie au moment de créer le contexte
    public Context(StrategyIF strategy) {
        this.strategy = strategy;
    }

    // Méthode métier : elle est toujours la même
    public int execute(int a, int b) {
        return strategy.execute(a, b);   // <-- STRATEGY ICI
    }
}
```

```java
// ===== Programme principal =====
public class StrategyExample {
    public static void main(String[] args) {

        Context context;
        int result;

        // 1. Contexte avec la stratégie ADD
        context = new Context(new StrategyAdd());
        result = context.execute(3, 4);    // 3 + 4
        System.out.println("Résultat ADD = " + result);

        // 2. Contexte avec la stratégie SUBTRACT
        context = new Context(new StrategySubtract());
        result = context.execute(3, 4);    // 3 - 4
        System.out.println("Résultat SUB = " + result);

        // 3. Contexte avec la stratégie MULTIPLY
        context = new Context(new StrategyMultiply());
        result = context.execute(3, 4);    // 3 * 4
        System.out.println("Résultat MUL = " + result);
    }
}
```

<br/>

# 04 – Explication claire

* Le **contexte** (`Context`) a **toujours** la même méthode : `execute(int a, int b)`.

* Il ne met **pas** de :

  ```java
  if (op == "add") ...
  else if (op == "sub") ...
  ```

* À la place, il dit simplement :

  ```java
  return strategy.execute(a, b);
  ```

  → c’est la **ligne clé** du patron Strategy.

* Le **choix** de l’algorithme est fait **avant** (dans le `main`) en créant le contexte avec la bonne stratégie :

  ```java
  new Context(new StrategyAdd());
  ```

* Si demain tu ajoutes `StrategyDivide`, tu n’as pas besoin de modifier `Context` → tu ajoutes juste une nouvelle classe qui implémente `StrategyIF`.

C’est ce qu’on résume souvent par :

> “Permet de définir une famille d’algorithmes, de les encapsuler et de les rendre interchangeables. Pas besoin de faire de `if else` ou `switch case` dans le contexte.”

<br/>

# 05 – Contexte & Stratégies

**Contexte :** `Context`
→ Il reçoit une stratégie (objet qui sait faire le calcul).
→ Il expose une méthode stable (`execute(a, b)`).

**Stratégie (interface) :** `StrategyIF`
→ Elle définit la forme de l’algorithme : `int execute(int a, int b);`

**Stratégies concrètes :**
→ `StrategyAdd`, `StrategySubtract`, `StrategyMultiply`
→ Chacune contient le VRAI algorithme.

**Ligne où la stratégie est utilisée :**

```java
return strategy.execute(a, b);
```

C’est là que le contexte dit : “je ne calcule pas moi-même, je laisse la stratégie le faire”.
