
### **1. `File`**
- **Description :**  
  La classe **`File`** représente une **abstraction** d'un fichier ou d'un répertoire dans le système de fichiers. Elle est utilisée pour manipuler les chemins de fichiers, vérifier l'existence d'un fichier, créer/supprimer des fichiers, etc.  
  **`File`** ne lit ni n'écrit directement dans le fichier ; elle sert simplement à localiser et représenter le fichier.

- **Utilisation courante :**  
  - Vérifier si un fichier existe.
  - Obtenir des informations sur le fichier (taille, nom, chemin).
  - Créer ou supprimer des fichiers/répertoires.

- **Exemple :**
  ```java
  import java.io.File;

  public class FileExample {
      public static void main(String[] args) {
          File file = new File("file.txt");
          if (file.exists()) {
              System.out.println("Le fichier existe.");
              System.out.println("Nom : " + file.getName());
              System.out.println("Chemin absolu : " + file.getAbsolutePath());
          } else {
              System.out.println("Le fichier n'existe pas.");
          }
      }
  }
  ```

---

### **2. `FileReader`**
- **Description :**  
  La classe **`FileReader`** est conçue pour **lire le contenu d'un fichier** caractère par caractère. Elle est spécifiquement utilisée pour les fichiers texte et travaille directement avec les flux d'entrée (InputStream).

- **Utilisation courante :**  
  - Lire le contenu d'un fichier texte.
  - Utiliser en combinaison avec d'autres classes (comme **`BufferedReader`**) pour des lectures plus efficaces.

- **Exemple simple avec `FileReader` seul :**
  ```java
  import java.io.FileReader;
  import java.io.IOException;

  public class FileReaderExample {
      public static void main(String[] args) {
          try (FileReader fr = new FileReader("file.txt")) {
              int ch;
              while ((ch = fr.read()) != -1) { // Lire caractère par caractère
                  System.out.print((char) ch);
              }
          } catch (IOException e) {
              System.err.println("Erreur de lecture : " + e.getMessage());
          }
      }
  }
  ```

---

### **Différences principales entre `File` et `FileReader`**

| **Aspect**             | **File**                                             | **FileReader**                                      |
|-------------------------|-----------------------------------------------------|----------------------------------------------------|
| **Fonction principale** | Représenter un fichier ou un répertoire.            | Lire le contenu d’un fichier caractère par caractère. |
| **Lecture/Écriture ?**  | Non, uniquement une abstraction (pas d'I/O directe).| Oui, utilisé pour les opérations de lecture.        |
| **Manipulation ?**      | Vérifier l’existence, créer, supprimer, obtenir des informations (nom, chemin, taille, etc.). | Lire le contenu d’un fichier texte.                |
| **Combinaisons courantes** | Utilisé avec **Scanner** ou pour passer un chemin de fichier à d'autres classes. | Souvent combiné avec **BufferedReader** pour une lecture ligne par ligne. |
| **Exemple d'utilisation** | Vérifier si un fichier existe avant de l’ouvrir.   | Lire le contenu d'un fichier texte.                |

---

### **Exemple combiné : `File` avec `FileReader`**

Dans un cas pratique, on peut utiliser **`File`** pour localiser un fichier et **`FileReader`** pour lire son contenu :

```java
import java.io.File;
import java.io.FileReader;
import java.io.IOException;

public class CombinedExample {
    public static void main(String[] args) {
        File file = new File("file.txt");
        if (file.exists()) { // Vérifie si le fichier existe
            System.out.println("Fichier trouvé : " + file.getAbsolutePath());
            try (FileReader fr = new FileReader(file)) {
                int ch;
                while ((ch = fr.read()) != -1) {
                    System.out.print((char) ch); // Lire et afficher le contenu
                }
            } catch (IOException e) {
                System.err.println("Erreur de lecture : " + e.getMessage());
            }
        } else {
            System.out.println("Le fichier n'existe pas.");
        }
    }
}
```

---

### **Résumé :**
- **`File`** : Sert à localiser, gérer et obtenir des informations sur un fichier, mais **ne lit pas son contenu**.
- **`FileReader`** : Conçu pour lire le contenu d’un fichier texte caractère par caractère.  
Vous pouvez combiner les deux pour une gestion et une lecture plus efficaces.
