# Comparaison entre les méthodes de lecture de texte

- *Je vous présente  les méthodes de lecture de texte avec une brève description de leur introduction dans l’écosystème Java, leurs avantages, et leurs limitations :*

| **Classe**        | **Introduction (année)**         | **Avantages**                         | **Limitations**                       |
|-------------------|----------------------------------|---------------------------------------|---------------------------------------|
| **`BufferedReader`** | Introduite en **Java 1.1** (1997) | - Rapide pour lire de gros fichiers grâce à un tampon en mémoire.<br>- Idéal pour la lecture ligne par ligne.<br>- Flexible et compatible avec d'autres flux d'entrée. | - Nécessite souvent un **FileReader** ou un autre flux comme source.<br>- Lecture basique : pas d'analyse avancée. |
| **`Scanner`**       | Introduite en **Java 5** (2004)   | - Facile à utiliser pour lire et analyser des données formatées (nombres, mots, etc.).<br>- Méthodes comme `nextInt()` ou `nextDouble()` pour lire différents types de données. | - Plus lent que `BufferedReader` pour des gros fichiers.<br>- Moins performant dans des contextes de lecture intensive. |
| **`Files`**         | Introduite en **Java 7** (2011)   | - API moderne et concise.<br>- Méthodes pratiques comme `Files.lines()` ou `Files.readAllLines()`.<br>- Lecture directe en mémoire avec une seule ligne de code. | - Charge tout le fichier en mémoire (non adapté pour des fichiers volumineux).<br>- Peut provoquer des erreurs **OutOfMemoryError** avec de très grands fichiers. |

---

### **Détails sur les introductions :**

#### **`BufferedReader` (Java 1.1 - 1997)**  
- Introduite avec la bibliothèque de flux (I/O) dans les premières versions de Java.
- Elle faisait partie des efforts pour fournir une lecture efficace et tamisée à partir de fichiers ou de flux.

#### **`Scanner` (Java 5 - 2004)**  
- Introduite dans le cadre de l’amélioration des fonctionnalités de manipulation des données au sein de Java.  
- Fournit des fonctionnalités analytiques comme la lecture et la conversion de types (`int`, `double`, etc.).

#### **`Files` (Java 7 - 2011)**  
- Fait partie de l’API **NIO.2 (New Input/Output)** introduite dans Java 7.
- Conçue pour moderniser les opérations I/O en introduisant des classes pratiques et idiomatiques comme `Path` et `Files`.

---

### **Comment choisir entre ces classes ?**

| **Cas d'utilisation**       | **Classe recommandée**              |
|-----------------------------|------------------------------------|
| Lire un fichier ligne par ligne pour analyser ou afficher le contenu. | `BufferedReader`                   |
| Extraire des données spécifiques comme des nombres ou des mots. | `Scanner`                          |
| Charger tout le contenu d'un fichier rapidement (petits fichiers). | `Files`                            |
| Manipuler des fichiers volumineux ou performants pour des systèmes modernes. | `BufferedReader` avec `FileReader` |

---

### **Résumé rapide :**
1. **`BufferedReader`** : Le plus ancien (1997) mais toujours performant pour des fichiers volumineux.
2. **`Scanner`** : Facile pour des fichiers formatés, introduit avec Java 5.
3. **`Files`** : Moderne, mais limité aux fichiers de petite taille, disponible depuis Java 7.
