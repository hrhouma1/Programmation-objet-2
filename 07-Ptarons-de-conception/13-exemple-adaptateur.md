# 01- DÉFINITION

Le **patron de conception Adaptateur** sert à faire collaborer deux classes qui ne sont pas compatibles parce qu’elles n’ont pas la même interface.
L’adaptateur joue le rôle de **traducteur** : il reçoit les appels dans le format attendu par le client, puis les transforme pour appeler les méthodes réelles d’un autre objet.
On l’utilise quand on veut réutiliser une bibliothèque ou une classe existante sans pouvoir (ou vouloir) modifier son code.
Ainsi, le client continue à travailler avec **une interface simple et stable**, pendant que l’adaptateur gère tous les détails de conversion ou de mapping.
Ce patron permet d’intégrer progressivement de nouveaux systèmes ou de vieilles API dans une architecture sans casser le code existant.



# 02- ÉNONCÉ DE L'EXERCICE

On souhaite concevoir un système de **lecture audio** simple pour une application Java.
L’application a été conçue autour d’une interface moderne :

```java
interface LecteurAudio {
    void jouer(String fichierAudio);
}
```

Toute la logique de l’application utilise cette interface, par exemple :

```java
lecteur.jouer("musique.mp3");
```

Problème :
L’entreprise possède déjà une **ancienne bibliothèque de lecture MP3** qui expose une classe :

```java
class AncienLecteurMp3 {
    void playFile(String chemin, String format) { ... }
}
```

Cette classe est considérée comme **non modifiable** (code legacy, bibliothèque externe).
Les signatures ne sont pas les mêmes, mais on veut **réutiliser** ce vieux lecteur sans réécrire toute l’application.

L’objectif est d’appliquer le **patron de conception Adaptateur** :

> La classe `AdaptateurLecteur` implémente l’interface moderne `LecteurAudio`,
> mais **délègue** la vraie lecture à `AncienLecteurMp3` en traduisant l’appel.

L’utilisateur pourra :

* Utiliser un nouveau lecteur moderne (`NouveauLecteurSimple`) qui implémente directement `LecteurAudio`,
* Utiliser un **adaptateur** (`AdaptateurLecteur`) pour exploiter `AncienLecteurMp3` comme si c’était un `LecteurAudio`,
* Changer de lecteur sans modifier le code appelant (`LecteurAudio lecteur = ...`).

---

# 02 - Résumé

| Élément                | Rôle                                                                               |
| ---------------------- | ---------------------------------------------------------------------------------- |
| `LecteurAudio`         | Interface cible utilisée par l’application.                                        |
| `NouveauLecteurSimple` | Implémentation moderne de `LecteurAudio`.                                          |
| `AncienLecteurMp3`     | Classe existante (legacy) avec une API différente.                                 |
| `AdaptateurLecteur`    | Classe **adaptateur** qui implémente `LecteurAudio` et utilise `AncienLecteurMp3`. |
| Ligne clé              | `ancienLecteur.playFile(fichierAudio, "mp3");` → c’est **l’adaptation**.           |

> Ainsi, le code client continue d’utiliser `LecteurAudio` sans connaître `AncienLecteurMp3`.
> L’**Adaptateur** fait le lien entre l’interface moderne et l’API existante.

---

# 03 - Correction

```java
// ===== Interface cible =====
interface LecteurAudio {
    void jouer(String fichierAudio);
}
```

```java
// ===== Implémentation moderne =====
class NouveauLecteurSimple implements LecteurAudio {
    @Override
    public void jouer(String fichierAudio) {
        System.out.println("[NouveauLecteurSimple] Lecture du fichier : " + fichierAudio);
    }
}
```

```java
// ===== Ancienne bibliothèque (API existante) =====
class AncienLecteurMp3 {

    // On suppose que cette API ne peut pas être modifiée
    public void playFile(String chemin, String format) {
        System.out.println("[AncienLecteurMp3] Lecture de " + chemin + " au format " + format);
    }
}
```

```java
// ===== ADAPTATEUR =====
class AdaptateurLecteur implements LecteurAudio {

    private final AncienLecteurMp3 ancienLecteur;

    public AdaptateurLecteur(AncienLecteurMp3 ancienLecteur) {
        this.ancienLecteur = ancienLecteur;
    }

    @Override
    public void jouer(String fichierAudio) {
        System.out.println("AdaptateurLecteur : adaptation de l'appel jouer(" + fichierAudio + ")");
        // *** ADAPTATION ici ***
        // L'application pense appeler un LecteurAudio moderne,
        // mais l'adaptateur traduit vers l'API de l'ancien lecteur.
        ancienLecteur.playFile(fichierAudio, "mp3");  // <-- ADAPTATION
    }
}
```

```java
// ===== Programme principal =====
public class Main {
    public static void main(String[] args) {

        // 1. Utilisation du lecteur moderne
        LecteurAudio lecteur1 = new NouveauLecteurSimple();
        lecteur1.jouer("nouveau_titre.mp3");
        System.out.println();

        // 2. Utilisation de l'ancien lecteur via un ADAPTATEUR
        AncienLecteurMp3 ancien = new AncienLecteurMp3();
        LecteurAudio lecteur2 = new AdaptateurLecteur(ancien);

        // Pour le code client, c'est toujours un LecteurAudio
        lecteur2.jouer("ancien_titre.mp3");
    }
}
```

---

# 04 - Explication claire

* `LecteurAudio` est l’**interface cible** : tout le reste de l’application parle ce langage.
* `AncienLecteurMp3` est la **classe existante** (l’API legacy) que l’on veut réutiliser sans la modifier.
* `AdaptateurLecteur` est la **classe adaptateur** :

  * Elle implémente `LecteurAudio` pour respecter le contrat attendu,
  * Elle contient un `AncienLecteurMp3` en interne,
  * Elle **traduit** l’appel `jouer(fichierAudio)` en `playFile(fichierAudio, "mp3")`.

La ligne clé est :

```java
ancienLecteur.playFile(fichierAudio, "mp3");
```

C’est à cet endroit précis que se fait l’**adaptation** :
Le client appelle une méthode moderne (`jouer`), l’adaptateur appelle la méthode de l’API legacy (`playFile`) avec les bons paramètres.

En changeant l’implémentation concrète :

```java
LecteurAudio lecteur = new NouveauLecteurSimple();
// ou
LecteurAudio lecteur = new AdaptateurLecteur(new AncienLecteurMp3());
```

on peut basculer entre le nouveau lecteur et l’ancien système adapté **sans toucher au reste du code**.

---

# 05 - Adapté et Adaptateur

**Interface cible :** `LecteurAudio`
→ C’est l’interface que le code client connaît et utilise partout.

**Adapté (Adaptee) :** `AncienLecteurMp3`
→ C’est la classe existante, avec une API différente, que l’on veut intégrer.

**Adaptateur :** `AdaptateurLecteur`
→ C’est la classe qui implémente l’interface cible (`LecteurAudio`) et qui, en interne, délègue le travail à `AncienLecteurMp3` en traduisant les appels.

**Ligne où l’adaptation a lieu :**

```java
ancienLecteur.playFile(fichierAudio, "mp3");
```

Ici, `AdaptateurLecteur` ne fait pas réellement la lecture audio lui-même :
il **adapte** la requête du client à l’API de `AncienLecteurMp3` et laisse ce dernier exécuter le travail.

