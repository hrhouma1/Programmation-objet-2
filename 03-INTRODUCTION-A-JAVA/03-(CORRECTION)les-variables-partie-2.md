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

---

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

---

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

---

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

---

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


