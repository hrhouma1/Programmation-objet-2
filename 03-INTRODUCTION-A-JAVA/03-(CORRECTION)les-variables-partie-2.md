# Exercice 1 — Déclaration & Valeurs limites (primitifs)

```java
public class Exo01 {
    public static void main(String[] args) {
        // 1) Déclarations + initialisations
        byte b = 100;
        short s = 30000;
        int i = 2147483647;          // Integer.MAX_VALUE
        long l = 9223372036854775807L; // Long.MAX_VALUE
        float f = 29.99f;
        double d = 299.99;
        char c = 'J';
        boolean bool = true;

        // 2) Affichages des valeurs
        System.out.println("=== Valeurs ===");
        System.out.println("byte b = " + b);
        System.out.println("short s = " + s);
        System.out.println("int i = " + i);
        System.out.println("long l = " + l);
        System.out.println("float f = " + f);
        System.out.println("double d = " + d);
        System.out.println("char c = " + c);
        System.out.println("boolean bool = " + bool);

        // 3) Affichages des bornes
        System.out.println("\n=== Bornes ===");
        System.out.println("byte:   " + Byte.MIN_VALUE    + " .. " + Byte.MAX_VALUE);
        System.out.println("short:  " + Short.MIN_VALUE   + " .. " + Short.MAX_VALUE);
        System.out.println("int:    " + Integer.MIN_VALUE + " .. " + Integer.MAX_VALUE);
        System.out.println("long:   " + Long.MIN_VALUE    + " .. " + Long.MAX_VALUE);
        System.out.println("float:  " + Float.MIN_VALUE   + " .. " + Float.MAX_VALUE);
        System.out.println("double: " + Double.MIN_VALUE  + " .. " + Double.MAX_VALUE);
        System.out.println("char:   " + Character.MIN_VALUE + " .. " + Character.MAX_VALUE
                + " (BYTES=" + Character.BYTES + ")");

        // ===== Explication courte =====
        System.out.println("\nfloat vs double :");
        System.out.println("- float = 32 bits (~7 chiffres significatifs).");
        System.out.println("- double = 64 bits (~15 chiffres significatifs), plus précis par défaut en Java.");
    }
}
```

**Explications détaillées :**

* `float` (32 bits) a \~7 chiffres de précision significative ; `double` (64 bits) en a \~15. Plus le type est large, plus on peut représenter précisément des décimaux.
* Les *MIN\_VALUE* de `float`/`double` représentent le **plus petit positif > 0** (très proche de 0), pas la borne négative (c’est un piège fréquent).


## Piège pour Character.MIN_VALUE

### 1) Caster en `int` (recommandé)

```java
System.out.println(
    "char (code units) : " + (int) Character.MIN_VALUE + " .. " + (int) Character.MAX_VALUE
    + " (BYTES=" + Character.BYTES + ")"
);
// => char (code units) : 0 .. 65535 (BYTES=2)
```

# Bonus - devinez le type

## 1) Copiez la classe utilitaire

Créez un fichier **`TypeName.java`** et **copiez** le code ci-dessous :

```java
// TypeName.java
public final class TypeName {
    private TypeName() {} // classe utilitaire : pas d'instance

    /** Retourne un nom de type simple (ex. "Integer", "Double", "String", "int[]", etc.). */
    public static String of(Object x) {
        if (x == null) return "null";
        return x.getClass().getSimpleName();
    }
}
```

> Notes :
>
> * Ça marche pour **objets**, **tableaux** et même pour les **primitifs** grâce à **l’autoboxing** (ex. `int` devient automatiquement `Integer` quand on l’envoie à `of(Object)`).
> * Si la valeur est `null`, on renvoie `"null"`.



## 2) Créez le programme de test

Créez un fichier **`DemoTypeName.java`** et **copiez** le code ci-dessous (il utilise exactement tes variables) :

```java
// DemoTypeName.java
public class DemoTypeName {
    public static void main(String[] args) {
        byte    b    = 100;
        short   s    = 30000;
        int     i    = 2147483647;              // Integer.MAX_VALUE
        long    l    = 9223372036854775807L;    // Long.MAX_VALUE
        float   f    = 29.99f;
        double  d    = 299.99;
        char    c    = 'J';
        boolean bool = true;

        // Tests sur primitives (afficheront le nom des "wrappers" : Byte, Short, Integer, etc.)
        System.out.println("b    -> " + TypeName.of(b));
        System.out.println("s    -> " + TypeName.of(s));
        System.out.println("i    -> " + TypeName.of(i));
        System.out.println("l    -> " + TypeName.of(l));
        System.out.println("f    -> " + TypeName.of(f));
        System.out.println("d    -> " + TypeName.of(d));
        System.out.println("c    -> " + TypeName.of(c));
        System.out.println("bool -> " + TypeName.of(bool));

        // Bonus : objets et tableaux
        String txt = "Java";
        int[] arr = {1, 2, 3};
        System.out.println("txt  -> " + TypeName.of(txt));  // String
        System.out.println("arr  -> " + TypeName.of(arr));  // int[]
        System.out.println("null -> " + TypeName.of(null)); // null
    }
}
```

## 3) Ce que vous devriez voir (exemple)

```
b    -> Byte
s    -> Short
i    -> Integer
l    -> Long
f    -> Float
d    -> Double
c    -> Character
bool -> Boolean
txt  -> String
arr  -> int[]
null -> null
```







## Exercice 1 — Déclaration & Valeurs limites (primitifs) — Explication détaillée

Les types primitifs sont stockés “par valeur”, sans objet, ce qui les rend très rapides et prédictibles en mémoire. Les bornes `MIN_VALUE`/`MAX_VALUE` des entiers indiquent la plage exacte en **complément à deux** (ex. `int` : −2³¹ à 2³¹−1), tandis que pour `float`/`double`, `MIN_VALUE` est le **plus petit positif** non nul (dénormalisés inclus), ce qui surprend souvent. `char` n’est pas un “caractère ASCII” mais un code **UTF-16** non signé de 16 bits, d’où sa plage 0..65535 et le fait qu’il ne stocke pas des lettres mais des **unités de code**. Par convention, Java choisit `int` comme entier par défaut et `double` comme réel par défaut, car leurs plages/precisions suffisent à la majorité des usages. Évitez de “forcer” des types plus petits pour “gagner de la mémoire” sans nécessité : la lisibilité, la précision et la stabilité passent d’abord.












<br/>

# Exercice 2 — Tailles mémoire & conversions automatiques (widening)

```java
public class Exo02 {
    public static void main(String[] args) {
        // 1) Tailles en octets
        System.out.println("Tailles (octets)");
        System.out.println("byte=" + Byte.BYTES + ", short=" + Short.BYTES + ", int=" + Integer.BYTES
                + ", long=" + Long.BYTES + ", float=" + Float.BYTES + ", double=" + Double.BYTES
                + ", char=" + Character.BYTES);

        // 2) Widening : entiers -> réels (automatique, sans perte 'théorique' de portée)
        byte b = 42;
        short sh = b;          // byte -> short
        int in = sh;           // short -> int
        long lo = in;          // int -> long
        float fl = lo;         // long -> float (perte possible de précision fractionnaire)
        double db = fl;        // float -> double

        System.out.println("\nWidening byte->...->double : " + b + " -> " + sh + " -> " + in + " -> " + lo + " -> " + fl + " -> " + db);

        // 3) Widening : char -> int -> ...
        char c = 'A'; // 65 en Unicode
        int code = c;
        long codeL = code;
        float codeF = codeL;
        double codeD = codeF;

        System.out.println("Widening char->...->double : '" + c + "' -> " + code + " -> " + codeL + " -> " + codeF + " -> " + codeD);

        System.out.println("\nObservation: passer vers float/double augmente la capacité de représentation,");
        System.out.println("mais les entiers convertis en float peuvent perdre de la précision au-delà de 2^24 (~16M).");
    }
}
```

**Explications détaillées :**

* Le **widening** (élargissement) est implicite quand il n’y a pas de risque de débordement de **portée** (ex. `int -> long`).
* Mais on peut **perdre de la précision** lors d’un passage vers `float` car un `float` ne peut pas représenter tous les entiers au-delà de \~16 millions exactement (limite du mantissa 24 bits).











## Exercice 2 — Tailles mémoire & conversions automatiques (widening) — Explication détaillée

Le **widening** est sûr du point de vue de la **portée** (range) : chaque valeur représentable dans le type source l’est dans le type cible. Néanmoins, passer d’un entier (`long`) à un flottant (`float`) peut perdre de la **précision** car un `float` ne possède qu’environ **24 bits** de mantisse, donc tous les entiers au-delà de 2²⁴ ne sont plus représentés exactement. À l’inverse, le passage `float → double` augmente la précision (mantisse \~53 bits), mais n’invente pas de l’exactitude si la valeur était déjà arrondie. Pour `char`, la conversion vers `int` expose son code numérique Unicode, ce qui est utile pour le tri, les comparaisons ou des calculs sur des codes. Retenir : widening = pas d’erreur de compilation, mais pas toujours absence de **perte d’information** (notamment entier → virgule flottante).






<br/>


# Exercice 3 — Conversions explicites (narrowing) & débordements

```java
public class Exo03 {
    public static void main(String[] args) {
        // 1) Narrowing : conversions explicites (risque de perte)
        double d = 19.99;
        float f = (float) d;   // double -> float
        long l = (long) f;     // float -> long (troncature)
        int i = (int) l;       // long -> int
        short s = (short) i;   // int -> short
        byte b = (byte) s;     // short -> byte

        System.out.println("d=" + d + " -> f=" + f + " -> l=" + l + " -> i=" + i + " -> s=" + s + " -> b=" + b);

        // 2) Overflow volontaire
        int n = 300;
        byte bb = (byte) n;
        System.out.println("300 en byte = " + bb);

        // Explication
        System.out.println("300 dépasse la plage du byte (-128..127). Le cast garde les 8 bits de poids faible,");
        System.out.println("ce qui donne 300 - 256 = 44 (ou interprétation en complément à deux).");
    }
}
```

**Explications détaillées :**

* **Narrowing** (rétrécissement) **oblige** un cast parce qu’il peut y avoir perte de **portée** et/ou **précision**.
* Pour `300 -> byte`, on ne garde que 8 bits → résultat **44** (300 − 256) ; représentation en **complément à deux** pour négatifs.










## Exercice 3 — Conversions explicites (narrowing) & débordements — Explication détaillée

Le **narrowing** nécessite un cast explicite parce que le compilateur ne peut pas garantir que la cible pourra contenir la valeur sans perte. La perte peut être de **portée** (ex. `int` 300 ne tient pas dans `byte`) ou de **fraction** (ex. `float` → `long` tronque la partie décimale). Pour les entiers, Java garde uniquement les **bits de poids faible** de la représentation binaire lors d’un cast réducteur ; c’est ainsi que 300 devient 44 en `byte` (300 − 256). Ce comportement est **déterministe**, mais peut produire des résultats surprenants si on oublie la plage. Bonne pratique : vérifier les bornes ou utiliser des types plus larges, surtout en E/S, calculs financiers ou données venues d’API externes.




<br/>


# Exercice 4 — Précision float vs double

```java
public class Exo04 {
    public static void main(String[] args) {
        float fs = 0f;
        for (int k = 0; k < 10000; k++) fs += 0.1f;

        double ds = 0.0;
        for (int k = 0; k < 10000; k++) ds += 0.1;

        System.out.println("Somme float (0.1f * 10000)  = " + fs);
        System.out.println("Somme double (0.1 * 10000) = " + ds);
        System.out.println("Valeur attendue = 1000.0");
    }
}
```

**Explications détaillées :**

* `0.1` **n’est pas représentable exactement** en binaire.
* `double` accumule l’erreur **moins** que `float` grâce à sa **mantisse** plus large. On obtient typiquement `float ≈ 999.99994` et `double ≈ 999.999999999…` ou `1000.000000000…` selon la JVM, mais rarement **exactement** 1000.



<br/>

## Exercice 4 — Précision float vs double — Explication détaillée

Les nombres décimaux “simples” comme 0,1 ne sont **pas représentables exactement** en binaire IEEE-754 ; ce qui est représenté est la **valeur la plus proche**. Sur 10 000 additions, ces micro-erreurs d’arrondi s’agrègent, créant un écart observable ; `double` accumule moins d’erreur que `float` grâce à sa mantisse plus longue. C’est pour cela qu’en calcul **scientifique**, **financier** (avec d’autres précautions) et **statistique**, `double` est préféré. Pour des montants d’argent, on n’utilise généralement **pas** `float`/`double` directement : on travaille en **entiers** (cents) ou avec `BigDecimal`. La règle d’or : éviter `float` sauf contrainte mémoire/perf, et accepter que `double` reste **approché** (impliquer un arrondi d’affichage).



# Exercice 5 — Strings (références) : pool, `==` vs `equals`

```java
public class Exo05 {
    public static void main(String[] args) {
        String a = "Java";                 // littéral -> interné (String pool)
        String b = "Java";                 // même objet interné que a
        String c = new String("Java");     // nouvel objet sur le tas

        System.out.println("a==b: " + (a == b));           // true (même référence pool)
        System.out.println("a==c: " + (a == c));           // false (références différentes)
        System.out.println("a.equals(b): " + a.equals(b)); // true (contenu)
        System.out.println("a.equals(c): " + a.equals(c)); // true (contenu)
    }
}
```

**Explications détaillées :**

* `==` compare les **références** (adresses), pas le contenu.
* `equals` compare le **contenu**.
* Les littéraux de chaîne sont **internés** par la JVM (String pool) : deux littéraux identiques pointent souvent vers le **même** objet.

---

# Exercice 6 — Déclaration, initialisation, constantes

```java
public class Exo06 {
    public static void main(String[] args) {
        int ageUtilisateur = 30;
        double salaireAnnuel = 45000.0;
        char initiale = 'J';
        boolean estConnecte = false;

        final double TAUX_TPS = 0.05;
        final double TAUX_TVQ = 0.09975;

        double montantHT = 100.00;
        double montantTTC = montantHT * (1 + TAUX_TPS + TAUX_TVQ);
        System.out.printf("Montant TTC pour %.2f = %.2f%n", montantHT, montantTTC);

        System.out.println("Constantes utilisées: TPS=" + TAUX_TPS + ", TVQ=" + TAUX_TVQ);
    }
}
```

**Explications détaillées :**

* `final` empêche la **modification** des valeurs fiscales (source de vérité).
* Les constantes **documentent** l’intention et **évitent** la duplication de littéraux magiques.

---

# Exercice 7 — Opérateurs arithmétiques & d’assignation

```java
public class Exo07 {
    public static void main(String[] args) {
        int x = 17, y = 5;
        System.out.println("x+y=" + (x + y));   // 22
        System.out.println("x-y=" + (x - y));   // 12
        System.out.println("x*y=" + (x * y));   // 85
        System.out.println("x/y=" + (x / y));   // 3 (division entière)
        System.out.println("x%y=" + (x % y));   // 2

        x += y;  // x=22
        System.out.println("x+=y -> " + x);
        x *= 2;  // x=44
        System.out.println("x*=2 -> " + x);
        x %= 7;  // 44%7 = 2
        System.out.println("x%%=7 -> " + x);
    }
}
```

**Explications détaillées :**

* En **division entière**, `17/5 = 3` (la partie décimale est **troncée**).
* `%` retourne le **reste** de la division.

---

# Exercice 8 — Pré/Post-incrémentation

```java
public class Exo08 {
    public static void main(String[] args) {
        int i = 10;
        System.out.println("++i = " + (++i)); // 11 (incrémente puis utilise)
        System.out.println("i++ = " + (i++)); // 11 (utilise puis incrémente)
        System.out.println("i   = " + i);     // 12

        i = 10;
        int r = i++ + ++i;
        // Détail: i++ renvoie 10 puis i devient 11; ++i incrémente i à 12 puis renvoie 12; r=10+12=22; i final=12
        System.out.println("r=" + r + ", i=" + i); // r=22, i=12
    }
}
```

**Explications détaillées :**

* `++i` : **pré**-incrémentation (modifie d’abord, puis évalue).
* `i++` : **post**-incrémentation (évalue, puis modifie).

---

# Exercice 9 — Comparaisons & booléens

```java
public class Exo09 {
    public static void main(String[] args) {
        int age = 20;
        boolean aPermisConduire = true;
        boolean aVoiture = false;

        boolean peutConduire = (age >= 18) && aPermisConduire;
        boolean peutConduireSeul = peutConduire && aVoiture;
        boolean besoinFormation = (age < 25) || !aPermisConduire;

        System.out.println("Peut conduire: " + peutConduire);
        System.out.println("Peut conduire seul: " + peutConduireSeul);
        System.out.println("Besoin de formation: " + besoinFormation);

        // Court-circuit:
        // '&&' : si la gauche est false, la droite n'est PAS évaluée.
        // '||' : si la gauche est true, la droite n'est PAS évaluée.
    }
}
```

**Explications détaillées :**

* Le **court-circuit** économise des évaluations et évite des effets secondaires inutiles (ex. appels coûteux).

---

# Exercice 10 — Table de vérité mini

```java
public class Exo10 {
    public static void main(String[] args) {
        boolean[] vals = {false, true};
        System.out.println("A\tB\tA&&B\tA||B\t!A");
        for (boolean A : vals) {
            for (boolean B : vals) {
                System.out.println(A + "\t" + B + "\t" + (A && B) + "\t" + (A || B) + "\t" + (!A));
            }
        }
    }
}
```

**Explications détaillées :**

* On parcourt les 4 combinaisons possibles `(false/true)²` et on évalue chaque opérateur logique.

---

# Exercice 11 — IMC (Indice de Masse Corporelle)

```java
public class Exo11 {
    public static void main(String[] args) {
        double poidsKg = 68.0;
        double tailleM = 1.72;
        double imc = poidsKg / (tailleM * tailleM);

        // Arrondi à 2 décimales
        double imc2d = Math.round(imc * 100.0) / 100.0;

        String cat;
        if (imc < 18.5) cat = "Insuffisance pondérale";
        else if (imc < 25) cat = "Poids normal";
        else if (imc < 30) cat = "Surpoids";
        else cat = "Obésité";

        System.out.printf("IMC=%.2f -> %s%n", imc2d, cat);
    }
}
```

**Explications détaillées :**

* `double` est préféré à `float` pour limiter l’erreur d’arrondi sur des calculs médicaux/financiers.
* Catégories standards : `<18.5`, `[18.5,25)`, `[25,30)`, `≥30`.

---

# Exercice 12 — Rendu de monnaie (cents sûrs)

```java
public class Exo12 {
    public static void main(String[] args) {
        double prix = 13.37;
        double paye = 20.00;
        int resteCents = (int) Math.round((paye - prix) * 100); // conversion sûre

        int[] denoms = {20000,10000,5000,2000,1000,500,200,100,25,10,5,1}; // en cents (ex: 200$=20000c)
        String[] noms  = {"200$","100$","50$","20$","10$","5$","2$","1$","25c","10c","5c","1c"};
        System.out.println("Rendu pour " + (paye - prix) + " $ : " + resteCents + " cents");
        for (int k = 0; k < denoms.length; k++) {
            int nb = resteCents / denoms[k];
            if (nb > 0) {
                System.out.println(noms[k] + " x " + nb);
                resteCents %= denoms[k];
            }
        }
    }
}
```

**Explications détaillées :**

* On travaille en **cents (int)** pour **éviter les erreurs binaires** sur `double`.
* `Math.round` garantit un arrondi correct avant la décomposition.

---

# Exercice 13 — Convertisseur de types texte ↔ nombres

```java
public class Exo13 {
    public static void main(String[] args) {
        String s = "123";
        int n = Integer.parseInt(s);
        double d = Double.parseDouble(s);
        System.out.println("String->int : " + n + ", String->double : " + d);

        int x = 456;
        String sx = String.valueOf(x);
        System.out.println("int->String : " + sx);

        // Conversion invalide
        try {
            String bad = "12a";
            int nb = Integer.parseInt(bad); // lève NumberFormatException
            System.out.println(nb); // jamais atteint
        } catch (NumberFormatException e) {
            System.out.println("Conversion invalide détectée: " + e.getMessage());
        }
    }
}
```

**Explications détaillées :**

* `parseInt/parseDouble` lèvent `NumberFormatException` si la chaîne n’est **pas un nombre** valide (ex. `12a`).
* `String.valueOf` est l’inverse : nombre → texte.

---

# Exercice 14 — Calculatrice formatée

```java
public class Exo14 {
    public static void main(String[] args) {
        double a = 19.99, b = 3.0;
        System.out.printf("a+b = %.2f%n", (a + b));
        System.out.printf("a-b = %.2f%n", (a - b));
        System.out.printf("a*b = %.2f%n", (a * b));
        System.out.printf("a/b = %.2f%n", (a / b));
    }
}
```

**Explications détaillées :**

* `printf` permet des **formats** (ici `%.2f` = 2 décimales).
* Utile pour **affichages financiers**.

---

# Exercice 15 — Températures & constantes

```java
public class Exo15 {
    public static void main(String[] args) {
        final double KELVIN_OFFSET = 273.15;
        double c = 21.0;

        double k = c + KELVIN_OFFSET;
        double f = c * 9.0 / 5.0 + 32.0;

        System.out.printf("C=%.1f -> K=%.1f, F=%.1f%n", c, k, f);
    }
}
```

**Explications détaillées :**

* `final` verrouille la **constante physique**.
* Utiliser des **littéraux double** (`9.0/5.0`) évite une division entière accidentelle.

---

# Exercice 16 — Identifiant simple (char & règles)

```java
public class Exo16 {
    public static void main(String[] args) {
        String id = "A1234"; // 1 lettre majuscule + 4 chiffres
        boolean ok = id.length() == 5
                && Character.isUpperCase(id.charAt(0))
                && Character.isDigit(id.charAt(1))
                && Character.isDigit(id.charAt(2))
                && Character.isDigit(id.charAt(3))
                && Character.isDigit(id.charAt(4));

        System.out.println(id + " -> " + (ok ? "valide" : "invalide"));
    }
}
```

**Explications détaillées :**

* On **n’utilise pas de regex** : on valide **caractère par caractère** avec les utilitaires `Character`.

---

# Exercice 17 — Bornes avant narrowing (sécuriser un cast)

```java
public class Exo17 {
    public static void main(String[] args) {
        int n = 130;

        if (n >= Byte.MIN_VALUE && n <= Byte.MAX_VALUE) {
            byte b = (byte) n;
            System.out.println("byte = " + b);
        } else {
            System.out.println("byte: hors bornes pour n=" + n);
        }

        if (n >= Short.MIN_VALUE && n <= Short.MAX_VALUE) {
            short s = (short) n;
            System.out.println("short = " + s);
        } else {
            System.out.println("short: hors bornes pour n=" + n);
        }
    }
}
```

**Explications détaillées :**

* Vérifier les **bornes** avant un cast **évite les overflows silencieux** (résultats surprenants).
* Bonne pratique surtout aux **interfaces** où les entrées sont externes/variables.

---

# Exercice 18 — Mini-benchmark précision

```java
public class Exo18 {
    public static void main(String[] args) {
        // Somme 1..1_000_000 en int (overflow)
        int sumInt = 0;
        for (int k = 1; k <= 1_000_000; k++) sumInt += k;
        System.out.println("Somme int (overflow) = " + sumInt);

        // Somme 1..1_000_000 en long (correct)
        long sumLong = 0L;
        for (int k = 1; k <= 1_000_000; k++) sumLong += k;
        System.out.println("Somme long (ok) = " + sumLong + " (attendu 500000500000)");

        // Accumulation double
        double d = 0.0;
        for (int k = 0; k < 1_000_000; k++) d += 1e-6;
        System.out.println("Somme double (1e-6 * 1e6) = " + d + " (attendu 1.0)");
    }
}
```

**Explications détaillées :**

* La somme 1..1,000,000 = **500 000 500 000** dépasse `int` (`≈2.1e9`) → **overflow** (valeur incorrecte).
* `long` (64 bits) convient.
* En `double`, `1e-6` n’est pas toujours représentable exactement → possible écart minime autour de 1.0.

---

# Exercice 19 — Opérateurs logiques composés

```java
public class Exo19 {
    public static void main(String[] args) {
        int age = 22;
        double revenuAnnuel = 30000.0;
        boolean estResident = true;
        boolean aCasier = false;

        boolean condAge = age >= 18;
        boolean condResid = estResident;
        boolean condRevenu = revenuAnnuel >= 25000.0;
        boolean condCasier = !aCasier;

        boolean eligible = condAge && condResid && condRevenu && condCasier;

        System.out.println("condAge=" + condAge + ", condResid=" + condResid
                + ", condRevenu=" + condRevenu + ", condCasier=" + condCasier);
        System.out.println("Éligible = " + eligible);
    }
}
```

**Explications détaillées :**

* Décomposer en **conditions intermédiaires** rend la logique **lisible** et **testable**.
* `!aCasier` inverse la condition (on exige **pas** de casier).

---

# Exercice 20 — Lecture de code (prédire la sortie)

```java
public class Exo20 {
    public static void main(String[] args) {
        int a = 5, b = 2;
        System.out.println(a / b);          // 1) 2 (division entière)
        System.out.println(a / (double)b);  // 2) 2.5

        int x = 10;
        System.out.println(++x); // 3) 11
        System.out.println(x++); // 4) 11 (puis x devient 12)
        System.out.println(x);   // 5) 12

        String s1 = "Ja" + "va";           // plié en "Java" (constante, internée)
        String s2 = new String("Java");     // nouvel objet
        System.out.println(s1 == s2);       // 6) false (références différentes)
        System.out.println(s1.equals(s2));  // 7) true (contenu égal)

        System.out.println( (true || f()) && (false || t()) ); // 8) 't()' (appel), 9) true
    }
    static boolean f() { System.out.println("f()"); return false; }
    static boolean t() { System.out.println("t()"); return true; }
}
```






---

## Exercice 5 — Strings (références) : pool, `==` vs `equals` — Explication détaillée

`String` est un **type référence** immuable, interné lorsqu’il provient d’un **littéral** : la JVM peut réutiliser la même instance pour `'Java'` répété. `new String("Java")` force la création d’un **nouvel objet** sur le tas, avec une **adresse différente** bien que le contenu soit identique. L’opérateur `==` compare les **références** (identité), donc dépend du pool/interning ; `equals` compare les **séquences de caractères** (contenu), ce qu’on veut 99% du temps. Mémo : `a == b` n’est pas un test “texte égal”, mais “même objet”. Pour forcer l’interning d’une `String` construite à l’exécution, on peut appeler `intern()`, mais c’est rarement nécessaire en pratique.

---

## Exercice 6 — Déclaration, initialisation, constantes — Explication détaillée

Les **constantes** `final` expriment un contrat : la valeur ne changera pas après initialisation, ce qui améliore lisibilité, **sécurité** (pas de modification accidentelle) et optimisation potentielle par la JVM/JIT. Nommer les constantes en **MAJUSCULES\_SNAKE\_CASE** (ex. `TAUX_TPS`) est une convention qui renforce l’intention. Évitez les **littéraux magiques** dispersés : centraliser les taux, offsets physiques, tailles de buffers, etc. L’usage de `printf`/`format` assure un rendu cohérent (ex. 2 décimales pour les montants) et évite les surprises avec la concaténation implicite. Enfin, privilégier des **noms parlants** (ex. `salaireAnnuel`) réduit la charge cognitive et les erreurs.

---

## Exercice 7 — Opérateurs arithmétiques & d’assignation — Explication détaillée

La **division entière** tronque systématiquement la partie décimale (pas d’arrondi), d’où `17/5 == 3`. L’opérateur `%` (modulo) est crucial pour les décompositions (monnaie, horloges), les **tests de parité** (`x%2==0`) ou les pas circulaires (indices d’anneaux). Les opérateurs combinés (`+=`, `*=`, etc.) n’ajoutent pas de sémantique mais clarifient la **mutation** de la variable et évitent la répétition. Attention avec l’ordre : `x *= 2 + 1` n’est pas `x *= 2; x += 1;` mais `x = x * (2 + 1)`. Enfin, surveiller les **overflows** silencieux en `int` lors de multiplications : préférer `long` si le résultat peut croître.

---

## Exercice 8 — Pré/Post-incrémentation — Explication détaillée

`++i` (pré-incrément) modifie **puis** renvoie, `i++` (post-incrément) renvoie **puis** modifie : la différence apparaît dans les **expressions composées**. Dans `i++ + ++i`, l’ordre d’évaluation à gauche-droite importe : on lit la valeur **avant** incrément pour `i++`, puis on incrémente **avant** lecture pour `++i`. Mélanger pré/post incréments dans la même expression réduit la lisibilité et favorise les bugs ; préférez des **lignes séparées**. Les effets de bord sont encore plus piégeux en présence d’appels de méthodes (surtout si des arguments sont modifiés). Idéalement, maintenez les expressions **pures** et séparez calculs et mutations.

---

## Exercice 9 — Comparaisons & booléens — Explication détaillée

Les opérateurs de comparaison produisent **toujours** des booléens, chaînables avec `&&`, `||` et `!`. Le **court-circuit** évite d’évaluer la droite si le résultat est déjà déterminé par la gauche (`false && …` ou `true || …`), ce qui est utile pour protéger des appels **coûteux** ou **dangereux** (`obj != null && obj.isReady()`). Factoriser en conditions intermédiaires (`condAge`, `condResid`) améliore la **testabilité** (on imprime chaque sous-résultat) et le débogage. Évitez de coder des conditions **trop denses** : la décomposition montre la logique métier et simplifie les corrections. Enfin, rappelez que `=` n’est pas `==` (affectation vs comparaison) — une source classique d’erreurs dans d’autres langages, moins en Java car le typage aide.

---

## Exercice 10 — Table de vérité mini — Explication détaillée

Générer la table systématiquement renforce l’intuition sur `&&`, `||` et `!`. Retenez : `&&` n’est `true` que si **tous** les opérandes sont `true`, `||` est `true` si **au moins un** l’est, et `!` inverse la vérité. Cette base se généralise à des expressions complexes ; on peut raisonner par **algebra booléenne** (De Morgan : `!(A && B) == !A || !B`). Dans la pratique, sachez choisir entre court-circuit (`&&`/`||`) et opérateurs sans court-circuit (`&`/`|`) : ces derniers évaluent **toujours** la droite (utile parfois avec des flags binaires). Enfin, rappelez que l’ordre d’évaluation en Java est **défini** (gauche→droite), ce qui évite certains pièges présents dans d’autres langages.

---

## Exercice 11 — IMC — Explication détaillée

L’IMC est sensible aux **arrondis** : arrondir à l’affichage (et non dans le calcul) limite l’erreur. Les **seuils** sont des conventions cliniques ; l’objectif ici est la manipulation de `double`, pas la validité médicale stricte. Attention aux conversions d’unités : si la taille était en **centimètres**, on diviserait d’abord par 100 ; sinon l’IMC serait 10 000× trop grand. Pour mettre au propre l’arrondi, `BigDecimal` avec un `RoundingMode` explicite garantit un comportement déterministe. En général, `double` suffit pour des calculs d’exercices, tout en rappelant que les décimaux binaires restent **approchés**.

---

## Exercice 12 — Rendu de monnaie (cents sûrs) — Explication détaillée

Les `double` introduisent des erreurs binaires (ex. 0.1 ≠ exact), ce qui peut faire échouer des **comparaisons** et **décompositions** monétaires. Travailler en **cents (`int`)**, après un `Math.round` unique, garantit l’intégrité des montants pendant les divisions/modulos. L’ordre des **dénominations** (du plus grand au plus petit) permet une stratégie gloutonne optimale dans un système de monnaie **canonique** (comme CAD/EUR/US). Documenter la table `{valeur → libellé}` évite toute ambiguïté de lecture. Pour des systèmes complexes (rendu minimal sous contraintes), on passerait à des algorithmes de programmation dynamique, mais c’est surdimensionné ici.

---

## Exercice 13 — Convertisseur texte↔nombres — Explication détaillée

`Integer.parseInt` et `Double.parseDouble` supposent que la chaîne est **bien formée** selon la locale US (point décimal). Une chaîne invalide lève `NumberFormatException`, qu’il faut **attraper** pour garder un programme robuste (message à l’utilisateur, valeur par défaut, etc.). L’inverse `String.valueOf` est **totalement sûr** (il ne lève pas d’exception). En contexte international, préférez `NumberFormat`/`DecimalFormat` pour respecter la **locale** (virgules, espaces insécables). Ne confondez pas `toString()` d’un objet et `String.valueOf(x)` : le second gère aussi les **primitifs** et `null` (renvoie `"null"`).

---

## Exercice 14 — Calculatrice formatée — Explication détaillée

`System.out.printf` utilise des **spécificateurs** C-like : `%.2f` = flottant sur 2 décimales, `%n` = saut de ligne portable. Pour des sorties alignées, `%-10.2f` aligne à gauche sur 10 colonnes par exemple. Attention à la **division** : `a/b` entre `double` fait une division flottante, mais `int/int` bascule en division entière (convertissez explicitement au besoin). Évitez d’additionner des `String` et nombres dans des boucles intempestives (création d’objets) ; une `StringBuilder` ou `printf` est souvent plus performant et clair. Enfin, pour des devises, combinez formatage avec la **locale** (ex. `NumberFormat.getCurrencyInstance(locale)`).

---

## Exercice 15 — Températures & constantes — Explication détaillée

Les conversions utilisent des **constantes physiques** (`273.15`) ; les mettre en `final` évite les fautes de frappe et harmonise tout le code. Soyez attentif à la **priorité** des opérateurs : `C * 9.0 / 5.0 + 32.0` est lisible et non soumis à la division entière. En changeant d’unité (Kelvin↔Celsius), on n’applique que des **translations** (offsets), ce qui se prête à l’extraction de fonctions pures. Pour l’affichage, fixer **1 décimale** est un compromis : suffisant pour des températures au quotidien, sans prétendre à la mesure scientifique. Sur un TP, demander un **arrondi explicite** (Math.round / format) vaut mieux que de laisser le défaut.

---

## Exercice 16 — Identifiant simple (char & règles) — Explication détaillée

Valider caractère par caractère renforce la compréhension de `char`/`Character` et évite de “magier” des regex. `Character.isUpperCase` et `Character.isDigit` sont **Unicode-aware**, donc plus sûrs que des tests `c >= 'A' && c <= 'Z'`. Penser à **vérifier la longueur** d’abord évite des `StringIndexOutOfBoundsException`. Pour des formats plus stricts, on ajouterait des **règles d’entreprise** (ex. pas de séquences “0000”, blacklist de lettres ambiguës). La même technique se généralise à des validateurs d’IBAN, d’identifiants étudiants, ou des “slugs” de fichiers.

---

## Exercice 17 — Bornes avant narrowing — Explication détaillée

Contrôler les bornes avant cast réduit à zéro le risque d’**overflow silencieux**, qui est l’un des bugs les plus coûteux à diagnostiquer. Cela documente l’intention : on **accepte** de perdre de l’information uniquement si la valeur est dans l’intervalle prévu. Dans des API, on renverra idéalement un **statut** (boolean/Optional/Try) plutôt que de caster aveuglément. Pour des conversions massives (fichiers, flux), on mesure d’abord la **distribution** des valeurs pour choisir un type cible pertinent. Enfin, souvenez-vous : `short`/`byte` ne sont pas “plus rapides” que `int` sur la JVM moderne ; choisissez-les pour la **contrainte métier**, pas pour des optimisations spéculatives.

---

## Exercice 18 — Mini-benchmark précision — Explication détaillée

La somme 1..1 000 000 vaut **500 000 500 000**, qui dépasse `Integer.MAX_VALUE` (**2 147 483 647**) : on obtient une valeur **wrap-around** (overflow) sans exception — d’où l’intérêt de planifier le **type du résultat** (`long`). Avec `double`, accumuler des micro-décimales (1e-6) expose l’**erreur d’arrondi** ; le total peut être légèrement < ou > 1.0 selon l’ordre d’addition. Les algorithmes **compensés** (Kahan, Neumaier) réduisent cette erreur lors d’accumulations massives. Pour des besoins financiers, préférez `BigDecimal` (avec `MathContext` et `RoundingMode`). En synthèse : **anticipez** la plage et la précision et testez des cas limites.

---

## Exercice 19 — Opérateurs logiques composés — Explication détaillée

Exprimer les conditions en variables nommées (`condAge`, `condRevenu`) rend le code **auto-documenté**, facilite les logs (“quelle condition a échoué ?”) et favorise les tests unitaires ciblés. L’ordre des tests peut aussi être un **levier de performance** si certains sont coûteux (I/O, base de données) : placez-les à droite dans un `&&` pour profiter du court-circuit. Les booléens négatifs (`!aCasier`) sont parfois moins lisibles — envisager de renommer la variable (`aCasier` → `aCasierJudiciaire`) ou inverser la logique côté donnée. Gardez un **seuil** en constante (`MIN_REVENUS`) plutôt que litéral magique. Enfin, évitez d’imbriquer trop profondément : décomposer en fonctions lisibles.

---

## Exercice 20 — Lecture de code (prédire la sortie) — Explication détaillée

La prédiction correcte repose sur quatre points : **division entière** (`5/2 == 2`), **promotion** en double (`a/(double)b`), **ordre d’évaluation** et **pré/post-incrément**, puis **identité vs égalité** des `String` et **court-circuit** logique. Les appels `f()` et `t()` montrent visuellement le court-circuit : `true || f()` n’appelle pas `f()`, tandis que `false || t()` appelle `t()`. La construction de `s1` par concaténation de **littéraux** peut être internée au compile-time, alors que `new String("Java")` crée un objet distinct en mémoire. L’**ordre** (`++x` avant affichage, `x++` après) explique la séquence `11`, `11`, `12`. Savoir dérouler ces règles mentalement est crucial pour analyser des expressions plus longues et repérer les effets de bord.


**Sortie attendue (ligne par ligne) :**

```
1) 2
2) 2.5
3) 11
4) 11
5) 12
6) false
7) true
8) t()
9) true
```

**Explications détaillées :**

* `true || f()` **court-circuite** `f()` (non appelé).
* `false || t()` appelle `t()` → affiche `t()`, retourne `true`.
* `true && true` → `true` est finalement imprimé.


