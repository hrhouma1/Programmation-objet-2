# <a id="top"></a>Lecture de Fichiers Texte en Java - partie 01


---

### **Introduction avec une blague :**

*"Vous savez, lire un fichier en programmation, c'est comme lire un livre à votre façon. Moi, je lis vite, ma grand-mère lit lentement. Elle dit : *‘Chaque lettre compte !’*. Mais moi, je saute directement aux mots importants ou je vais carrément à la fin pour tout lire d’un coup ! Alors, grand-mère, c'est comme ça qu'un programme fait aussi, il s'adapte !"*

---

### **Comparaison avec une situation réelle :**

Imaginez que vous lisez **un livre de recettes** dans votre cuisine :

---

#### **1. Lettre par lettre :**  
C’est comme si vous appreniez une langue étrangère. Vous lisez lentement, chaque lettre compte.  
**Exemple réel :** Votre grand-mère essaie de comprendre une recette italienne mot à mot, mais commence par déchiffrer les lettres :  
- *"M... a... r... g... h... e... r... i... t... a... Ah ! Margherita, une pizza !"*

**Code Java correspondant :**
```java
import java.io.FileReader;
import java.io.IOException;

public class CharByChar {
    public static void main(String[] args) {
        try (FileReader reader = new FileReader("recette.txt")) {
            int ch;
            while ((ch = reader.read()) != -1) { // Lire lettre par lettre
                System.out.print((char) ch);   // Afficher chaque caractère
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

---

#### **2. Mot par mot :**  
C’est comme lire les **ingrédients clés**. Vous sautez les détails inutiles et cherchez des mots spécifiques.  
**Exemple réel :** Vous repérez rapidement *"tomates"*, *"basilic"*, et *"mozzarella"* pour savoir ce que vous devez acheter.

**Code Java correspondant :**
```java
import java.io.File;
import java.util.Scanner;

public class WordByWord {
    public static void main(String[] args) {
        try (Scanner scanner = new Scanner(new File("recette.txt"))) {
            while (scanner.hasNext()) { // Lire mot par mot
                String word = scanner.next();
                System.out.println(word); // Afficher chaque mot
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

---

#### **3. Ligne par ligne :**  
C’est comme lire chaque étape d’une recette, une par une, pour bien comprendre les instructions.  
**Exemple réel :** Vous lisez :  
- *Étape 1 : Préchauffer le four.*  
- *Étape 2 : Étaler la pâte à pizza.*  
- *Étape 3 : Ajouter la sauce tomate et les ingrédients.*  

**Code Java correspondant :**
```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class LineByLine {
    public static void main(String[] args) {
        try (BufferedReader reader = new BufferedReader(new FileReader("recette.txt"))) {
            String line;
            while ((line = reader.readLine()) != null) { // Lire ligne par ligne
                System.out.println(line); // Afficher chaque ligne
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

---

#### **4. Tout lire d’un coup :**  
C’est comme prendre une **photocopie entière** de la recette et la consulter quand vous en avez besoin.  
**Exemple réel :** Vous photocopiez le livre de cuisine et l’emportez chez vous pour le lire tranquillement.

**Code Java correspondant :**
```java
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.List;

public class ReadAllAtOnce {
    public static void main(String[] args) {
        try {
            List<String> lines = Files.readAllLines(Paths.get("recette.txt"));
            for (String line : lines) {
                System.out.println(line); // Afficher tout le contenu
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

---

### **Résumé avec humour :**

- **Lettre par lettre :** Comme grand-mère qui apprend l'italien. (*M...a...r...g...h...e...r...i...t...a*)  
- **Mot par mot :** Comme chercher les ingrédients clés pour la pizza (*tomates*, *mozzarella*).  
- **Ligne par ligne :** Lire chaque étape d’une recette pour ne pas brûler la pizza.  
- **Tout d’un coup :** Comme photocopier la recette entière et la consulter plus tard.

**Phrase finale pour impressionner :**  
*"En programmation comme en cuisine, tout dépend de la recette... et de votre patience !"* 🍕
