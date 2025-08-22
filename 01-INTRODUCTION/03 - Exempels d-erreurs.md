# 1) Erreurs de compilation (syntax/typage/import)

## 1.1 Oubli de point-virgule

**Code fautif**

```java
public class Ex1 {
    public static void main(String[] args) {
        System.out.println("Bonjour")  // ; manquant
    }
}
```

**Message typique**

```
error: ';' expected
        System.out.println("Bonjour")
                                     ^
```

**Correction**

```java
System.out.println("Bonjour");
```



## 1.2 Accolade fermante manquante

**Code fautif**

```java
public class Ex2 {
    public static void main(String[] args) {
        System.out.println("OK");
} // <-- une accolade manque
```

**Message**

```
error: reached end of file while parsing
```

**Correction**
Fermer toutes les `{}` correctement.



## 1.3 Mauvais nom de fichier / classe publique

**Code fautif (fichier Hello.java)**

```java
public class Salut {   // nom de classe ≠ nom de fichier
    public static void main(String[] args) {}
}
```

**Message**

```
error: class Salut is public, should be declared in a file named Salut.java
```

**Correction**
Renommer le fichier en `Salut.java` **ou** la classe en `Hello`.



## 1.4 Identificateur inconnu (“cannot find symbol”)

**Code fautif**

```java
public class Ex4 {
    public static void main(String[] args) {
        System.out.println(message); // variable non définie
    }
}
```

**Message**

```
error: cannot find symbol
System.out.println(message);
                   ^
  symbol:   variable message
  location: class Ex4
```

**Correction**

```java
String message = "Hello";
System.out.println(message);
```



## 1.5 Conflit de types (type mismatch)

**Code fautif**

```java
public class Ex5 {
    public static void main(String[] args) {
        int age = "vingt"; // mauvais type
    }
}
```

**Message**

```
error: incompatible types: String cannot be converted to int
```

**Correction**

```java
int age = 20;
```



## 1.6 Import manquant

**Code fautif**

```java
public class Ex6 {
    public static void main(String[] args) {
        ArrayList<String> l = new ArrayList<>();
    }
}
```

**Message**

```
error: cannot find symbol
  symbol:   class ArrayList
```

**Correction**

```java
import java.util.ArrayList;

public class Ex6 {
    public static void main(String[] args) {
        ArrayList<String> l = new ArrayList<>();
    }
}
```



## 1.7 Mauvaise signature de `main`

**Code fautif**

```java
public class Ex7 {
    public static void main(String arg) { // devrait être String[]
        System.out.println("OK");
    }
}
```

**Compilation** : OK, **exécution** :

```
Error: Main method not found in class Ex7, please define the main method as:
public static void main(String[] args)
```

**Correction**

```java
public static void main(String[] args) { ... }
```



# 2) Erreurs à l’exécution (runtime)

## 2.1 Division par zéro (ArithmeticException)

**Code fautif**

```java
public class Ex8 {
    public static void main(String[] args) {
        int x = 10 / 0;
        System.out.println(x);
    }
}
```

**Message à l’exécution**

```
Exception in thread "main" java.lang.ArithmeticException: / by zero
    at Ex8.main(Ex8.java:3)
```

**Correction**

```java
int d = 0;
if (d != 0) System.out.println(10 / d);
else System.out.println("Denominateur nul");
```



## 2.2 NullPointerException (très fréquent)

**Code fautif**

```java
public class Ex9 {
    static class Etudiant { String nom; }
    public static void main(String[] args) {
        Etudiant e = null;
        System.out.println(e.nom.toUpperCase()); // e == null
    }
}
```

**Message**

```
Exception in thread "main" java.lang.NullPointerException
    at Ex9.main(Ex9.java:5)
```

**Correction (exemples)**

```java
Etudiant e = new Etudiant();
e.nom = "Alice";
if (e.nom != null) System.out.println(e.nom.toUpperCase());
```



## 2.3 Débordement de tableau (ArrayIndexOutOfBoundsException)

**Code fautif**

```java
public class Ex10 {
    public static void main(String[] args) {
        int[] t = {1,2,3};
        System.out.println(t[3]); // indices 0..2 seulement
    }
}
```

**Message**

```
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: Index 3 out of bounds for length 3
```

**Correction**

```java
System.out.println(t[t.length - 1]); // dernier élément
```



## 2.4 Erreur logique (compile & run OK, résultat faux)

**Code fautif**

```java
public class Ex11 {
    static int somme(int a, int b) { return a - b; } // bug logique
    public static void main(String[] args) {
        System.out.println(somme(2, 3)); // attendu: 5
    }
}
```

**Symptôme** : pas d’exception, mais résultat incorrect.
**Correction**

```java
static int somme(int a, int b) { return a + b; }
```

**Méthode pour trouver** : tests unitaires, assertions, débogueur.


# 3) Compiler & exécuter (ligne de commande)

1. Sauver le fichier `Ex1.java`
2. Ouvrir un terminal dans le dossier

```bash
javac Ex1.java         # compilation -> génère Ex1.class
java Ex1               # exécution (sans .class ni .java)
```

Projets multi-fichiers :

```bash
javac -d bin src/*.java          # compile tout vers /bin
java -cp bin com.moncours.App    # -cp = classpath
```

Messages utiles :

* `error:` → erreur de compilation (fichier/ligne/colonne indiqués).
* `Exception in thread "main"` → erreur à l’exécution (stack trace montre la ligne précise).



# 4) Compiler, exécuter et **déboguer dans Eclipse**

## 4.1 Créer et exécuter

* **File → New → Java Project**
* **New → Class** (cocher “public static void main” si besoin)
* **Run ▶** (ou `Ctrl+F11`) pour exécuter.
* Problèmes visibles dans l’onglet **Problems** (double-cliquer pour aller à la ligne).

## 4.2 Débogage pas-à-pas

1. **Toggle Breakpoint** : clic dans la marge à gauche d’une ligne (ou `Ctrl+Shift+B`).
2. **Debug ▶** (ou `F11`) pour lancer en mode Debug.
3. Contrôles :

   * **Step Into (F5)** : entrer dans la méthode appelée.
   * **Step Over (F6)** : exécuter la ligne sans entrer dans les méthodes.
   * **Step Return (F7)** : finir la méthode courante et revenir à l’appelant.
4. Vues utiles :

   * **Variables** : valeurs locales en temps réel.
   * **Expressions** : ajouter une expression à surveiller (`Add Watch`).
   * **Breakpoints** : lister/activer/désactiver des points d’arrêt.
5. **Console** : affiche la sortie et la **stack trace** (emplacement exact de l’exception).

## 4.3 Méthode rapide pour traquer un `NullPointerException`

* Lire la ligne indiquée dans la stack trace (ex. `Ex9.java:5`).
* Inspecter à gauche de chaque `.` quel objet peut être `null`.
* En debug : survoler la variable ou regarder la vue **Variables**.
* Ajouter des **garde-fous** :

  ```java
  Objects.requireNonNull(obj, "obj ne doit pas être null");
  ```

  ou

  ```java
  if (obj == null) return; // ou lever une exception claire
  ```



# 5) Mini-TP “Trouver & corriger”

Donnez ces fichiers tels quels (avec bugs), demandez :

1. Compiler → lire le message.
2. Expliquer l’erreur en une phrase.
3. Corriger → recompiler → exécuter.
4. (Option) Poser un **point d’arrêt** et suivre l’exécution.

* **TP-A (syntaxe)** : Ex1, Ex2
* **TP-B (import/typage)** : Ex5, Ex6
* **TP-C (runtime)** : Ex8, Ex9, Ex10
* **TP-D (logique)** : Ex11 (+ écrire un test rapide)


# 6) Bonus : assertions & mini-tests (détection d’erreurs logiques)

```java
public class Maths {
    static int somme(int a, int b) { return a + b; }
    public static void main(String[] args) {
        assert somme(2,3) == 5 : "Somme cassée !";
        System.out.println("Tests OK");
    }
}
```

**Exécuter avec assertions activées :**

```bash
java -ea Maths
```

Si l’assertion échoue, Java stoppe et indique la ligne fautive.

