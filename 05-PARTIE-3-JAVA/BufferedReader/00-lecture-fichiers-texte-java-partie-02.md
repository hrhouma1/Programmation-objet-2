# <a id="top"></a>Lecture de Fichiers Texte en Java - partie 02

---

## **Plan de la présentation**
1. [Introduction](#01---introduction)
2. [Classes principales pour lire des fichiers texte](#02---classes-principales-pour-lire-des-fichiers-texte)
3. [Étapes pour lire un fichier texte](#03---étapes-pour-lire-un-fichier-texte)
4. [Exemple : Lecture ligne par ligne avec `BufferedReader`](#04---exemple--lecture-ligne-par-ligne-avec-bufferedreader)
5. [Comparaison des méthodes](#05---comparaison-des-méthodes)
6. [Exemple avancé : Compter les mots dans un fichier](#06---exemple-avancé--compter-les-mots-dans-un-fichier)
7. [Conclusion](#07---conclusion)

---

# <a id="01---introduction"></a>01 - Introduction
La lecture de fichiers texte en Java est une tâche essentielle dans le développement logiciel. Elle permet de :
- Traiter des données provenant de fichiers externes.
- Lire des journaux système ou des fichiers de configuration.
- Analyser et manipuler du texte pour diverses applications.



[Applications](01-applications.md)





[⬆️ Retour à la table des matières](#top)

---

# <a id="02---classes-principales-pour-lire-des-fichiers-texte"></a>02 - Classes principales pour lire des fichiers texte



Java offre plusieurs classes pour lire des fichiers texte, chacune ayant des spécificités adaptées à différents cas d'usage. 

---

# **1. BufferedReader**
- **Spécificité :**
  - Permet de lire un fichier **ligne par ligne**.
  - Utilise un **tampon (buffer)** pour améliorer les performances, particulièrement pour les fichiers volumineux.
  - Très efficace pour des lectures séquentielles de texte.

- **Quand l'utiliser :**
  - Lorsque vous travaillez avec des fichiers volumineux et que vous voulez lire les données ligne par ligne.
  - Si vous souhaitez optimiser les performances en limitant le nombre d'opérations d'entrée/sortie.

- **Quand éviter :**
  - Si vous avez besoin de traiter des données plus complexes ou formatées (mots, chiffres spécifiques). ==> [inconvénients](04-BufferedvsScanner.md)
  - Si la lecture aléatoire est nécessaire.

- **Exemple :**
  ```java
  BufferedReader reader = new BufferedReader(new FileReader("file.txt"));
  String line;
  while ((line = reader.readLine()) != null) {
      System.out.println(line);
  }
  reader.close();
  ```

[BufferedReader](02-BufferedReader.md)

---

### **2. Scanner**
- **Spécificité :**
  - Lit ligne par ligne ou mot à mot ==> [démo](07-scanner-mot-a-mot.md)
  - Idéal pour lire des **données formatées**, comme des mots, des chiffres, ou des lignes.
  - Fournit des méthodes pratiques comme `nextLine()`, `nextInt()`, etc.

- **Quand l'utiliser :**
  - Si vous devez analyser ou extraire des parties spécifiques des données (par exemple, lire des chiffres ou des mots séparément).
  - Si vous lisez des fichiers relativement petits.

- **Quand éviter :**
  - Pour des fichiers très volumineux, car `Scanner` est moins performant en raison de l'analyse (parsing) qu'il effectue.
  - Si vous avez juste besoin de lire des lignes rapidement sans traitement complexe.

- **Exemple :**
  ```java
  Scanner scanner = new Scanner(new File("file.txt"));
  while (scanner.hasNextLine()) {
      System.out.println(scanner.nextLine());
  }
  scanner.close();
  ```

---

### **3. Files (Java NIO)**
- **Spécificité :**
  - Partie de l'API NIO, qui propose des méthodes modernes et concises.
  - Permet de lire tout le contenu d'un fichier en une seule opération.
  - Fonctionne avec des objets `Path` pour manipuler les fichiers.

- **Quand l'utiliser :**
  - Pour des fichiers de taille **modérée à petite**, car le contenu est chargé entièrement en mémoire.
  - Si vous préférez une syntaxe moderne et concise.

- **Quand éviter :**
  - Pour des fichiers très volumineux, car tout le contenu est chargé en mémoire.
  - Si vous avez besoin de lire les fichiers ligne par ligne.

- **Exemple :**
  ```java
  List<String> lines = Files.readAllLines(Paths.get("file.txt"));
  for (String line : lines) {
      System.out.println(line);
  }
  ```

---

### **4. FileReader**
- **Spécificité :**
  - Classe de base pour lire des **caractères** depuis un fichier.
  - Ne gère pas le buffering, donc chaque lecture est directement effectuée sur le disque, ce qui peut être moins performant.

- **Quand l'utiliser :**
  - Pour des fichiers **petits** ou si vous avez besoin d'un contrôle basique sur la lecture caractère par caractère.
  - Si aucune performance particulière n'est nécessaire.

- **Quand éviter :**
  - Pour des fichiers volumineux, car il n'est pas optimisé (pas de buffering).
  - Si vous avez besoin de lire des lignes ou des données formatées.

- **Exemple :**
  ```java
  FileReader reader = new FileReader("file.txt");
  int ch;
  while ((ch = reader.read()) != -1) {
      System.out.print((char) ch);
  }
  reader.close();
  ```

---

### **Comparaison rapide**
| **Classe**       | **Avantage principal**                     | **Inconvénient**                    | **Cas d'utilisation**                     |
|-------------------|-------------------------------------------|--------------------------------------|-------------------------------------------|
| **BufferedReader** | Lecture rapide ligne par ligne            | Moins intuitif pour des données formatées | Fichiers volumineux, lectures séquentielles |
| **Scanner**        | Lecture de données formatées              | Moins performant pour gros fichiers  | Extraction de données formatées           |
| **Files**          | Lecture moderne et concise (tout en mémoire) | Charge tout en mémoire              | Fichiers de petite à taille modérée       |
| **FileReader**     | Simple pour lire caractère par caractère  | Pas optimisé (pas de buffer)         | Fichiers petits, contrôle simple          |

En résumé, choisissez la classe en fonction de vos besoins spécifiques :  
- **BufferedReader** pour des performances sur des fichiers volumineux.  
- **Scanner** pour des données formatées.  
- **Files** pour la concision et des fichiers de taille moyenne.  
- **FileReader** pour des tâches très simples ou si vous souhaitez un contrôle basique.

















[Comparaison](03-comparaison.md)

[⬆️ Retour à la table des matières](#top)

---

# <a id="03---étapes-pour-lire-un-fichier-texte"></a>03 - Étapes pour lire un fichier texte

1. Importer les bibliothèques nécessaires.
2. Localiser le fichier via son chemin.
3. Utiliser une classe adaptée pour lire le fichier.
4. Gérer les exceptions avec `try-catch`.
5. Fermer les ressources pour libérer la mémoire.

[⬆️ Retour à la table des matières](#top)

---

# <a id="04---exemple--lecture-ligne-par-ligne-avec-bufferedreader"></a>04 - Exemple : Lecture ligne par ligne avec `BufferedReader`

Voici un exemple simple pour lire un fichier texte ligne par ligne :

```java
import java.io.*;

public class FileReadingExample {
    public static void main(String[] args) {
        try (BufferedReader br = new BufferedReader(new FileReader("example.txt"))) {
            String line;
            while ((line = br.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            System.err.println("Erreur de lecture : " + e.getMessage());
        }
    }
}
```

### Points clés :
- **Pourquoi `BufferedReader` ?** : Lecture rapide et efficace.
- **Méthode `readLine()`** : Lit ligne par ligne jusqu'à atteindre la fin du fichier.
- **Gestion des exceptions :** Le bloc `try-with-resources` libère automatiquement les ressources.

[⬆️ Retour à la table des matières](#top)

---

# <a id="05---comparaison-des-méthodes"></a>05 - Comparaison des méthodes

| Classe           | Avantages                        | Limitations                   |
|------------------|----------------------------------|-------------------------------|
| `BufferedReader` | Rapide pour des fichiers volumineux. | Nécessite un `FileReader`.    |
| `Scanner`        | Idéal pour extraire des données spécifiques. | Plus lent pour des gros fichiers. |
| `Files`          | Lecture simple en une seule ligne de code. | Charge tout en mémoire (attention aux gros fichiers). |

[Comparaison](03-comparaison.md)


[⬆️ Retour à la table des matières](#top)

---

# <a id="06---exemple-avancé--compter-les-mots-dans-un-fichier"></a>06 - Exemple avancé : Compter les mots dans un fichier

#### Je vous présente un exemple pour lire un fichier et compter le nombre total de mots :

```java
import java.io.*;

public class WordCountExample {
    public static void main(String[] args) {
        try (BufferedReader br = new BufferedReader(new FileReader("example.txt"))) {
            int wordCount = 0;
            String line;
            while ((line = br.readLine()) != null) {
                wordCount += line.split("\\s+").length; // Compter les mots dans chaque ligne
            }
            System.out.println("Nombre total de mots : " + wordCount);
        } catch (IOException e) {
            System.err.println("Erreur : " + e.getMessage());
        }
    }
}
```

### Points clés :
- **`split("\\s+")`** : Découpe les lignes en mots en se basant sur les espaces.
- **Addition ligne par ligne** : Accumule le nombre de mots.

[⬆️ Retour à la table des matières](#top)

---

# <a id="07---conclusion"></a>07 - Conclusion

- Java propose plusieurs options pour lire des fichiers texte en fonction du contexte.
- **`BufferedReader`** est souvent le choix optimal pour une lecture ligne par ligne.
- Toujours gérer les exceptions avec des blocs `try-catch` ou `try-with-resources`.
- Adapter la méthode en fonction des besoins (taille du fichier, données spécifiques, etc.).

[⬆️ Retour à la table des matières](#top)

---

## Questions ?

N’hésitez pas à poser des questions ou demander des clarifications sur les concepts et exemples abordés !

[⬆️ Retour à la table des matières](#top)


--------------
--------------
--------------
# Annexe : Imports nécessaires 


---

#### **1. BufferedReader**
```java
// Permet de lire un fichier ligne par ligne avec un tampon pour améliorer les performances
import java.io.BufferedReader;

// Fournit une classe pour lire des fichiers caractère par caractère ou ligne par ligne
import java.io.FileReader;

// Nécessaire pour gérer les exceptions liées aux opérations d'entrée/sortie
import java.io.IOException;
```

---

#### **2. Scanner**
```java
// Fournit la classe Scanner pour lire et analyser du texte à partir de fichiers ou d'autres sources
import java.util.Scanner;

// Nécessaire pour ouvrir et représenter le fichier à lire
import java.io.File;

// Gère les exceptions si le fichier n'existe pas ou ne peut pas être lu
import java.io.FileNotFoundException;
```

---

#### **3. Files (Java NIO)**  
```java
// Fournit des méthodes modernes pour lire tout le contenu d'un fichier, comme readAllLines()
import java.nio.file.Files;

// Utilisé pour manipuler les chemins de fichiers dans l'API NIO
import java.nio.file.Paths;

// Nécessaire pour gérer les exceptions lors des opérations de lecture ou d'écriture avec NIO
import java.io.IOException;

// Fournit une liste pour stocker les lignes lues à partir du fichier
import java.util.List;
```

---

#### **4. FileReader**
```java
// Fournit une classe simple pour lire des fichiers texte caractère par caractère
import java.io.FileReader;

// Nécessaire pour gérer les exceptions liées aux opérations d'entrée/sortie
import java.io.IOException;
```

---

#### **5. RandomAccessFile**
```java
// Fournit la classe RandomAccessFile pour lire ou écrire à des positions spécifiques dans un fichier
import java.io.RandomAccessFile;

// Nécessaire pour gérer les exceptions lors de l'accès ou de la manipulation du fichier
import java.io.IOException;
```

---

### **Commentaire général :**
- Les classes `java.io` sont utilisées pour les opérations classiques d'entrée/sortie.
- Les classes `java.nio.file` appartiennent à l'API NIO, qui offre des fonctionnalités modernes et efficaces pour manipuler les fichiers et répertoires.
- Les exceptions (`IOException`, `FileNotFoundException`) doivent toujours être gérées pour éviter des plantages en cas de problème (fichier introuvable, permissions insuffisantes, etc.).

---

#### **Exemple d'import complet :**

Si vous utilisez plusieurs approches dans le même programme, voici à quoi pourraient ressembler les imports regroupés :
```java
// Classes pour l'entrée/sortie classique
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.File;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.RandomAccessFile;

// Classes pour l'API NIO
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.List;

// Classe Scanner pour lire et analyser le texte
import java.util.Scanner;
```

