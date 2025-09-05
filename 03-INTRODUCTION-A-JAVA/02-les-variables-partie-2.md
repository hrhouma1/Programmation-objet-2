## Module 2 — Variables, Types et Opérateurs

### Série d’exercices (20)

> Conseils aux étudiant·e·s
>
> * Créez un fichier `ExoXX.java` par exercice.
> * Compilez : `javac ExoXX.java` puis exécutez : `java ExoXX`.
> * Remplissez la **Zone de réponse** (code et/ou commentaires).
> * Quand un exercice demande des **explications**, écrivez 1–3 lignes sous la zone prévue.

---

## Exercice 1 — Déclaration & Valeurs limites (primitifs)

**Objectif :** déclarer tous les types primitifs et afficher leurs valeurs/limes.
**Fais ça :**

1. Déclarez une variable de chaque type primitif avec une valeur.
2. Affichez chaque valeur.
3. Affichez `MIN_VALUE` et `MAX_VALUE` pour `byte`, `short`, `int`, `long`, `float`, `double`, `Character.BYTES`.
4. Expliquez en 1–2 lignes la différence entre `float` et `double`.

```java
public class Exo01 {
    public static void main(String[] args) {
        // TODO: déclarations + initialisations

        // TODO: affichages des valeurs

        // TODO: affichages des bornes
        // System.out.println(Byte.MIN_VALUE + " ... " + Byte.MAX_VALUE);

        // ===== Zone de réponse (explication courte) =====
        // Réponse :
        // ...
    }
}
```

---

## Exercice 2 — Tailles mémoire & conversions automatiques (widening)

**Objectif :** comprendre `BYTES` et les conversions implicites sûres.
**Fais ça :**

1. Affiche la taille (octets) de chaque type primitif avec `Xxx.BYTES`.
2. Montre `byte → short → int → long → float → double` et `char → int → long → float → double`.
3. Affiche les valeurs après chaque étape.

```java
public class Exo02 {
    public static void main(String[] args) {
        // TODO: tailles mémoire

        // TODO: conversions implicites en chaîne

        // ===== Zone de réponse =====
        // Qu’observez-vous sur la précision en passant vers float/double ?
        // ...
    }
}
```

---

## Exercice 3 — Conversions explicites (narrowing) & débordements

**Objectif :** pratiquer le casting explicite et voir un overflow.
**Fais ça :**

1. Convertis `double -> float -> long -> int -> short -> byte`.
2. Montre le cas `int n = 300; byte b = (byte) n;` et commente le résultat.

```java
public class Exo03 {
    public static void main(String[] args) {
        // TODO: conversions explicites

        int n = 300;
        byte b = (byte) n;
        System.out.println("300 en byte = " + b);

        // ===== Zone de réponse =====
        // Pourquoi ce résultat ?
        // ...
    }
}
```

---

## Exercice 4 — Précision float vs double

**Objectif :** mesurer un écart de précision.
**Fais ça :**

1. Additionne 10 000 fois `0.1f` dans un `float`.
2. Additionne 10 000 fois `0.1` dans un `double`.
3. Compare aux valeurs attendues (1000.0).

```java
public class Exo04 {
    public static void main(String[] args) {
        // TODO: boucles de somme float vs double

        // ===== Zone de réponse =====
        // Explique la différence observée :
        // ...
    }
}
```

---

## Exercice 5 — Strings (références) : pool, == vs equals

**Objectif :** différencier `==` et `.equals()` avec `String`.
**Fais ça :**

1. Crée `String a = "Java"; String b = "Java"; String c = new String("Java");`
2. Affiche `a==b`, `a==c`, `a.equals(b)`, `a.equals(c)` et commente.

```java
public class Exo05 {
    public static void main(String[] args) {
        // TODO: déclarations a, b, c

        // TODO: affichages comparaisons

        // ===== Zone de réponse =====
        // Explique le String pool et la différence == / equals :
        // ...
    }
}
```

---

## Exercice 6 — Déclaration, initialisation, constantes

**Objectif :** appliquer les règles de nommage et `final`.
**Fais ça :**

1. Déclare `int ageUtilisateur`, `double salaireAnnuel`, `char initiale`, `boolean estConnecte`.
2. Déclare `final double TAUX_TPS = 0.05;` et `final double TAUX_TVQ = 0.09975;`
3. Calcule le total TTC d’un montant HT saisi en code.

```java
public class Exo06 {
    public static void main(String[] args) {
        // TODO: déclarations + constantes final

        // TODO: calcul TTC

        // ===== Zone de réponse =====
        // Pourquoi utiliser final ici ?
        // ...
    }
}
```

---

## Exercice 7 — Opérateurs arithmétiques & d’assignation

**Objectif :** manipuler `+ - * / %` et `+= -= *= /= %=`.
**Fais ça :**

1. Déclare `int x=17, y=5;` affiche somme, diff, prod, div entière, modulo.
2. Applique successivement `x += y; x *= 2; x %= 7;` et affiche à chaque étape.

```java
public class Exo07 {
    public static void main(String[] args) {
        // TODO

        // ===== Zone de réponse =====
        // Pourquoi 17/5 en int donne ... ?
        // ...
    }
}
```

---

## Exercice 8 — Pré/Post-incrémentation

**Objectif :** distinguer `++i` et `i++`.
**Fais ça :**

1. Avec `int i=10;`, affiche : `++i`, `i++`, puis `i`.
2. Montre l’impact dans `int r = i++ + ++i;`.

```java
public class Exo08 {
    public static void main(String[] args) {
        // TODO

        // ===== Zone de réponse =====
        // Explique l’ordre d’évaluation :
        // ...
    }
}
```

---

## Exercice 9 — Comparaisons & booléens

**Objectif :** opérateurs `== != > < >= <=` et `&& || !`.
**Fais ça :**

1. Déclare `int age=20; boolean aPermis=true; boolean aVoiture=false;`
2. Calcule `peutConduire`, `peutConduireSeul`, `besoinFormation` (comme dans le cours).
3. Explique le court-circuit.

```java
public class Exo09 {
    public static void main(String[] args) {
        // TODO

        // ===== Zone de réponse =====
        // Où intervient le court-circuit ? Exemple :
        // ...
    }
}
```

---

## Exercice 10 — Table de vérité mini

**Objectif :** générer une petite table de vérité.
**Fais ça :**

1. Parcours toutes les combinaisons de `boolean a` et `boolean b`.
2. Affiche `a`, `b`, `a&&b`, `a||b`, `!a`.

```java
public class Exo10 {
    public static void main(String[] args) {
        // TODO: boucles ou tableau de booléens
    }
}
```

---

## Exercice 11 — IMC (Indice de Masse Corporelle)

**Objectif :** calcul avec `double` et interprétation.
**Fais ça :**

1. Déclare `double poidsKg`, `double tailleM`.
2. Calcule `imc = poids / (taille*taille)` et arrondis à 2 décimales.
3. Affiche la catégorie (minceur/normal/surpoids/obésité) selon seuils simples.

```java
public class Exo11 {
    public static void main(String[] args) {
        // TODO

        // ===== Zone de réponse =====
        // Pourquoi double plutôt que float ?
        // ...
    }
}
```

---

## Exercice 12 — Rendu de monnaie (cents sûrs)

**Objectif :** utiliser `int` pour éviter les erreurs binaires.
**Fais ça :**

1. Montant à rendre en dollars (double) → convertis en **cents (int)**.
2. Décompose en billets/pièces : 200\$, 100\$, 50\$, 20\$, 10\$, 5\$, 2\$, 1\$, 25c, 10c, 5c, 1c.
3. Utilise divisions & modulo.

```java
public class Exo12 {
    public static void main(String[] args) {
        // TODO: montant -> cents -> décomposition
    }
}
```

---

## Exercice 13 — Convertisseur de types texte↔nombres

**Objectif :** `Integer.parseInt`, `Double.parseDouble`, `String.valueOf`.
**Fais ça :**

1. Convertis `"123"` en `int` puis en `double`.
2. Convertis un `int` en `String`.
3. Teste une conversion invalide avec `try/catch NumberFormatException`.

```java
public class Exo13 {
    public static void main(String[] args) {
        // TODO

        // ===== Zone de réponse =====
        // Quand leverez-vous un NumberFormatException ?
        // ...
    }
}
```

---

## Exercice 14 — Calculatrice formatée

**Objectif :** utiliser `double` et `System.out.printf`.
**Fais ça :**

1. Déclare `double a=19.99, b=3.0;`
2. Affiche `a+b`, `a-b`, `a*b`, `a/b` avec 2 décimales via `printf`.

```java
public class Exo14 {
    public static void main(String[] args) {
        // TODO
    }
}
```

---

## Exercice 15 — Températures & constantes

**Objectif :** constantes `final` et conversions.
**Fais ça :**

1. `final double KELVIN_OFFSET = 273.15;`
2. Convertis Celsius→Kelvin, Celsius→Fahrenheit (`C*9/5+32`) avec `double`.
3. Affiche avec 1 décimale.

```java
public class Exo15 {
    public static void main(String[] args) {
        // TODO
    }
}
```

---

## Exercice 16 — Identifiant simple (char & règles)

**Objectif :** manipuler `char` et règles de nommage.
**Fais ça :**

1. Déclare un identifiant sous forme `L####` (ex. `A1234`) : 1 lettre majuscule + 4 chiffres.
2. Vérifie le format (sans regex) avec `Character.isUpperCase` et `Character.isDigit`.
3. Affiche “valide”/“invalide”.

```java
public class Exo16 {
    public static void main(String[] args) {
        // TODO: validation caractère par caractère
    }
}
```

---

## Exercice 17 — Bornes avant narrowing (sécuriser un cast)

**Objectif :** vérifier les bornes avant cast.
**Fais ça :**

1. Lis un `int` `n`. Si `n` ∈ \[`Byte.MIN_VALUE`, `Byte.MAX_VALUE`], cast vers `byte`; sinon, affiche “hors bornes”.
2. Idem pour `short`.

```java
public class Exo17 {
    public static void main(String[] args) {
        // TODO

        // ===== Zone de réponse =====
        // Pourquoi vérifier avant de caster ?
        // ...
    }
}
```

---

## Exercice 18 — Mini-benchmark précision

**Objectif :** comparer accumulation `int`, `long`, `double`.
**Fais ça :**

1. Additionne de 1 à 1\_000\_000 dans un `int` et un `long`.
2. Additionne 1e-6 un million de fois dans un `double`.
3. Commente la précision et les risques.

```java
public class Exo18 {
    public static void main(String[] args) {
        // TODO

        // ===== Zone de réponse =====
        // Commenter précision / risques d’overflow :
        // ...
    }
}
```

---

## Exercice 19 — Opérateurs logiques composés

**Objectif :** conditions d’éligibilité.
**Fais ça :**

1. Soit `age`, `revenuAnnuel`, `estResident`, `aCasier`.
2. Éligible si `(age >= 18) && estResident && (revenuAnnuel >= 25000) && !aCasier`.
3. Affiche le détail de chaque condition intermédiaire.

```java
public class Exo19 {
    public static void main(String[] args) {
        // TODO: variables + conditions + affichage explicite
    }
}
```

---

## Exercice 20 — Lecture de code (prédire la sortie)

**Objectif :** raisonner sur les opérateurs et l’ordre d’évaluation.
**Fais ça :**

1. Lisez le code ci-dessous, **écrivez d’abord la sortie attendue**, puis exécutez pour vérifier.

```java
public class Exo20 {
    public static void main(String[] args) {
        int a = 5, b = 2;
        System.out.println(a / b);     // ?
        System.out.println(a / (double)b); // ?
        int x = 10;
        System.out.println(++x);       // ?
        System.out.println(x++);       // ?
        System.out.println(x);         // ?
        String s1 = "Ja" + "va";
        String s2 = new String("Java");
        System.out.println(s1 == s2);      // ?
        System.out.println(s1.equals(s2)); // ?
        System.out.println( (true || f()) && (false || t()) ); // ?
    }
    static boolean f() { System.out.println("f()"); return false; }
    static boolean t() { System.out.println("t()"); return true; }
}
```

**Zone de réponse — sortie attendue (avant d’exécuter) :**

```
1)
2)
3)
4)
5)
6)
7)
8)
9)
```

---

## Barème suggéré (indicatif)

* Exos 1–10 : 40 pts (4 pts chacun, exactitude + clarté).
* Exos 11–19 : 49 pts (5–6 pts chacun, justesse + commentaires).
* Exo 20 : 11 pts (prédiction correcte + justification court-circuit).

---

### Bonus (facultatif)

* Ajoute un **menu** dans un `Main.java` pour lancer un exercice au choix (switch par numéro).
* Internationalise l’affichage (séparateur décimal via `Locale`).
