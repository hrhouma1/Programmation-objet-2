
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



















<br/>
<br/>

# Grille d’évaluation 

**Consignes de remise :**

* Remettez **un seul fichier .docx** contenant vos **captures d’écran** (code, compilation, exécution) **et vos explications**.
* **Nom de fichier** : `Nom_Prenom_ConversionsTypes_Java.docx`
* **Captures obligatoires** (par exercice) :

  1. Code source, 2) Compilation/exécution en console, 3) Éventuel message d’erreur (Ex. 5).
* **Explications** : 5–8 lignes à la fin de chaque exercice (voir gabarit).

## Barème par exercice (20 % chacun)

> Total partie obligatoire = **100 %**.
> **Bonus optionnel** = +20 % (peut porter la note finale jusqu’à 120 %; l’enseignant peut caper à 100 % si nécessaire).

### Exercice 1 – Widening (20 %)

* Code correct et compile (widening `byte→short→int→double`) : **8 pts**
* Sorties console claires (toutes les valeurs affichées) : **4 pts**
* Captures conformes (code + exécution) : **3 pts**
* Explication (pourquoi pas d’erreur, notion d’implicite) : **5 pts**

### Exercice 2 – Narrowing (20 %)

* Code correct et compile (`int 900 → byte` avec cast) : **8 pts**
* Sorties console (comparaison avant/après) : **4 pts**
* Captures conformes (code + exécution) : **3 pts**
* Explication (débordement/ perte d’info et raison) : **5 pts**

### Exercice 3 – Perte des décimales (20 %)

* Code correct (`double → int` avec cast) : **6 pts**
* Conversion **double → float** et nouvelle valeur affichée : **4 pts**
* Sorties console (avant/après + float) : **3 pts**
* Captures conformes (code + exécution) : **2 pts**
* Explication (troncature vers int, précision float vs double) : **5 pts**

### Exercice 4 – `char` ↔ entiers (20 %)

* Code correct (`char 'A' → int` puis `int → byte` avec cast) : **8 pts**
* Sorties console (char, code int/ASCII, byte) : **4 pts**
* Captures conformes (code + exécution) : **3 pts**
* Explication (représentation numérique d’un char, codage) : **5 pts**

### Exercice 5 – Erreur & correction (20 %)

* **Capture du message d’erreur** lors de la compilation du code fourni : **6 pts**
* Correction correcte (cast explicite `double → int`) + exécution : **6 pts**
* Captures conformes (erreur, code corrigé, exécution) : **3 pts**
* Explication (quand widening implicite vs narrowing explicite) : **5 pts**

## Bonus – Mini-application (optionnel, +20 %)

* Lecture utilisateur (`double`) robuste (gestion simple d’entrée) : **10 pts**
* Conversions (`double → int → byte`) et affichage des 3 valeurs : **6 pts**
* Explication claire des différences (originale/entière/tronquée) : **4 pts**



## Règles & pénalités

* **Absence de captures** demandées pour un exercice : **–50 %** sur l’exercice.
* Code qui **ne compile pas** et aucune preuve d’exécution : **maximum 50 %** si l’analyse/justification est pertinente.
* **Plagiat / copie** : 0 à l’exercice (ou à l’évaluation), selon la politique du cours.
* **Clarté** (mise en page, sections nommées, noms de fichiers) : **–5 %** global si non respecté.

