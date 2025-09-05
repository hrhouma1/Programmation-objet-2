
# Exercice 1 – Conversion implicite (Widening)

Écrivez un programme qui :

1. Déclare une variable `byte` contenant la valeur `50`.
2. Convertit cette valeur automatiquement en `short`, puis en `int`, puis en `double`.
3. Affiche chaque valeur avec un `System.out.println`.

**À la fin de l’exercice :** Expliquez pourquoi aucune erreur n’apparaît lors de ces conversions.

<br/>­

# Exercice 2 – Conversion explicite (Narrowing)

Écrivez un programme qui :

1. Déclare une variable `int` contenant la valeur `900`.
2. Convertit cette valeur en `byte` à l’aide d’un cast.
3. Affiche le résultat et comparez la valeur obtenue avec la valeur d’origine.

**À la fin de l’exercice :** Expliquez pourquoi la valeur finale n’est pas identique à la valeur d’origine.

<br/>

# Exercice 3 – Perte des décimales

Écrivez un programme qui :

1. Déclare une variable `double d = 12.75489785364;`.
2. Convertit ce `double` en `int` à l’aide d’un cast.
3. Affiche la valeur avant et après conversion.
4. Quesqu'il arrive si je convertit d en float ?  C'est quoi la nouvelle valeur obtenue?

**À la fin de l’exercice :** Expliquez pourquoi la partie décimale disparaît lors de la conversion.

<br/>

# Exercice 4 – Conversion avec char

Écrivez un programme qui :

1. Déclare une variable `char c = 'A';`.
2. Convertit ce caractère en `int` et affiche le code ASCII.
3. Convertit ce code ASCII en `byte` avec un cast, puis affiche le résultat.

**À la fin de l’exercice :** Expliquez pourquoi un caractère peut être converti en entier et ce que représente le nombre affiché.

<br/>


# Exercice 5 – Trouver l’erreur et corriger

On vous donne le code suivant :

```java
public class ExoCasting {
    public static void main(String[] args) {
        double d = 9.8;
        int i = d;   // ligne qui pose problème
        System.out.println("Valeur de i : " + i);
    }
}
```

1. Essayez de compiler ce programme.
2. Observez le message d’erreur du compilateur.
3. Expliquez pourquoi ce code ne fonctionne pas.
4. Proposez une correction en utilisant le bon mécanisme de conversion.

**À la fin de l’exercice :** Expliquez dans quels cas Java accepte une conversion automatique et dans quels cas un **casting explicite** est obligatoire.



<br/>

# Bonus (optionnel)

# Mini-application utilisateur

Écrivez un programme qui :

1. Demande à l’utilisateur de saisir un nombre décimal (`double`).
2. Convertit ce nombre en `int` puis en `byte`.
3. Affiche toutes les valeurs obtenues (originale, entière, tronquée).

**À la fin de l’exercice :** Expliquez les différences entre les trois valeurs affichées.

