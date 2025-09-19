# Lecture de fichiers texte en Java : De la précision minutieuse à la lecture rapide et globale


# Question: 

- Si je vous donne ce fichier texte, quelle serait votre façon de le lire ?



*"Java est un langage de programmation orienté objet, largement utilisé dans le monde entier. Créé en 1995, il permet de développer des applications robustes, performantes et multiplateformes. Aujourd'hui, il est l'un des langages les plus populaires pour le développement d'applications Web, mobiles et d'entreprise."*


> ###### Lecture de fichiers texte en Java : Entre Grand-mère et Génération Z  
> ###### Caractère par caractère, mot par mot, ligne par ligne, ou tout d’un coup ?

---
Maintenant, mettons en scène **4 personnes**, chacune avec sa propre façon de lire ce texte.  
---


#### **1. Grand-mère : La précision avant tout** 

```
J
a
v
a
 
e
s
t
 
u
n
 
l
a
n
g
...
```

#### **2. Le chercheur ou disons l'enquêteur : Focus sur les mots clés (Mot par mot)**

```
Java
est
un
langage
de
programmation
...
```


#### **3. L’ingénieur efficace : Un pas à la fois (Ligne par ligne)**


**Sortie :**
```
Java est un langage de programmation orienté objet, largement utilisé dans le monde entier.
Créé en 1995, il permet de développer des applications robustes, performantes et multiplateformes.
Aujourd'hui, il est l'un des langages les plus populaires pour le développement d'applications Web, mobiles et d'entreprise.
```



#### **4. Le jeune dynamique et pressé ou une personne hyperactive: Tout lire d’un coup (Tout le fichier)** - *Le survol rapide*

```
Java est un langage de programmation orienté objet, largement utilisé dans le monde entier. Créé en 1995, il permet de développer des applications robustes, performantes et multiplateformes. Aujourd'hui, il est l'un des langages les plus populaires pour le développement d'applications Web, mobiles et d'entreprise.
```



# Conclusion: 

- **Grand-mère :** `FileReader`  
- **Le chercheur :** `Scanner`  
- **L’ingénieur efficace :** `BufferedReader`  
- **Le jeune pressé :** `Files`  



| **Personnage**            | **Classe utilisée** | **Avantages**                                                | **Inconvénients**                                      |
|----------------------------|---------------------|------------------------------------------------------------|------------------------------------------------------|
| **Grand-mère**             | `FileReader`        | Très précis, lecture caractère par caractère. Elle ne manque aucun détails             | Lent, inefficace pour les fichiers volumineux.       |
| **Le chercheur**           | `Scanner`           | Lecture mot par mot, pratique pour des données formatées.  | Pas fait pour lire rapidement de gros fichiers.      |
| **L’ingénieur efficace**   | `BufferedReader`    | Rapide, lecture ligne par ligne avec tampon.              | Ne lit que ligne par ligne, ignore les détails dans les lignes. |
| **Le jeune pressé**        | `Files`             | Le survol rapide ou lecture complète en mémoire, très rapide.                 | Peut dépasser la mémoire si le fichier est trop gros. |



----------------------
----------------------
----------------------
# Annexe : Code
----------------------

# **1. Grand-mère : La précision avant tout (Caractère par caractère)**

**Blague :**  
*"Ma grand-mère dit toujours : 'Si tu veux vraiment tout comprendre, lis chaque lettre. Chaque virgule a son importance.' Alors elle lit... lentement, très lentement !"*

**Comment elle fait ?**  
Elle lit **caractère par caractère**, déchiffrant chaque lettre comme si elle traduisait un texte antique.

**Code Java :**
```java
import java.io.FileReader;

public class GrandMere {
    public static void main(String[] args) throws Exception {
        FileReader reader = new FileReader("texte.txt");
        int ch;
        while ((ch = reader.read()) != -1) { // Lire caractère par caractère
            System.out.print((char) ch); // Afficher chaque lettre
        }
        reader.close();
    }
}
```

**Sortie (lue lentement) :**
```
J
a
v
a
 
e
s
t
 
u
n
 
l
a
n
g
...
```

**Avantage :** Elle ne manque **aucun détail**.  
**Inconvénient :** Elle met des heures pour lire un paragraphe !  

---

# **2. Le chercheur : Focus sur les mots clés (Mot par mot)**

**Blague :**  
*"Le chercheur est concentré, il saute les mots inutiles comme 'est' ou 'un', et va directement chercher les mots importants : 'Java', 'langage', 'applications'. Pour lui, la précision, c'est savoir quoi chercher."*

**Comment il fait ?**  
Il lit **mot par mot**, en se concentrant uniquement sur les informations importantes.

**Code Java :**
```java
import java.io.File;
import java.util.Scanner;

public class Chercheur {
    public static void main(String[] args) throws Exception {
        Scanner scanner = new Scanner(new File("texte.txt"));
        while (scanner.hasNext()) { // Lire mot par mot
            System.out.println(scanner.next()); // Afficher chaque mot
        }
        scanner.close();
    }
}
```

**Sortie :**
```
Java
est
un
langage
de
programmation
...
```

**Avantage :** Il trouve les **informations clés** rapidement.  
**Inconvénient :** Il **perd le contexte global** en sautant certains mots.  

---

# **3. L’ingénieur efficace : Un pas à la fois (Ligne par ligne)**

**Blague :**  
*"L’ingénieur ne perd pas son temps. Il traite chaque ligne comme une tâche séparée : une ligne, un message clair, et il passe à la suivante. 'Pourquoi lire lettre par lettre quand on peut avancer rapidement ?' dit-il."*

**Comment il fait ?**  
Il lit **ligne par ligne**, en traitant chaque ligne comme une unité d’information.

**Code Java :**
```java
import java.io.BufferedReader;
import java.io.FileReader;

public class Ingenieur {
    public static void main(String[] args) throws Exception {
        BufferedReader reader = new BufferedReader(new FileReader("texte.txt"));
        String line;
        while ((line = reader.readLine()) != null) { // Lire ligne par ligne
            System.out.println(line); // Afficher chaque ligne
        }
        reader.close();
    }
}
```

**Sortie :**
```
Java est un langage de programmation orienté objet, largement utilisé dans le monde entier.
Créé en 1995, il permet de développer des applications robustes, performantes et multiplateformes.
Aujourd'hui, il est l'un des langages les plus populaires pour le développement d'applications Web, mobiles et d'entreprise.
```

**Avantage :** Il lit **rapidement** tout en gardant une **bonne compréhension**.  
**Inconvénient :** Certains **détails subtils peuvent lui échapper**.  

---

# **4. Le jeune hyperactif : Tout lire d’un coup (Tout le fichier)**

**Blague :**  
*"Le jeune dit : 'Pourquoi perdre du temps ? Lis tout d’un coup et on passe à autre chose !' Mais il ne réalise pas qu’en allant trop vite, il peut rater des détails."*

**Comment il fait ?**  
Il charge **tout le fichier en mémoire** pour tout lire immédiatement.

**Code Java :**
```java
import java.nio.file.Files;
import java.nio.file.Paths;

public class Jeune {
    public static void main(String[] args) throws Exception {
        String content = Files.readString(Paths.get("texte.txt")); // Charger tout le contenu
        System.out.println(content); // Afficher tout
    }
}
```

**Sortie :**
```
Java est un langage de programmation orienté objet, largement utilisé dans le monde entier. Créé en 1995, il permet de développer des applications robustes, performantes et multiplateformes. Aujourd'hui, il est l'un des langages les plus populaires pour le développement d'applications Web, mobiles et d'entreprise.
```

**Avantage :** **Ultra rapide**, il obtient immédiatement une vue d’ensemble.  
**Inconvénient :** Il **perd les détails** ou risque de surcharger la mémoire pour de gros fichiers.  

---

### **Résumé avec humour :**

| **Personnage**         | **Méthode utilisée**            | **Approche**                                      | **Avantage**                              | **Inconvénient**                           |
|-------------------------|---------------------------------|--------------------------------------------------|------------------------------------------|---------------------------------------------|
| **Grand-mère**          | `FileReader.read()`            | Lire **caractère par caractère**                | Ultra précis, rien n’échappe.            | Très lent pour de gros fichiers.           |
| **Le chercheur**        | `Scanner.next()`               | Lire **mot par mot**                            | Rapide pour trouver des mots-clés.       | Perd le contexte global.                   |
| **L’ingénieur**         | `BufferedReader.readLine()`    | Lire **ligne par ligne**                        | Rapide et efficace pour du contenu clair.| Peut manquer des détails subtils.          |
| **Une personne hyperactive** | `Files.readString()`           | Lire **tout d’un coup**                         | Ultra rapide, vision globale immédiate.  | Perte de détails et problème avec gros fichiers. |

---

# **Conclusion :**

*"Alors, que vous soyez une grand-mère patiente, un chercheur méticuleux, un ingénieur efficace ou une personne hyperactive, Java a une méthode de lecture adaptée à votre style. Mais rappelez-vous : chaque méthode a ses avantages et ses inconvénients... choisissez bien !"* 🚀
