-------------------
# Décorateur
-------------------


- **`BufferedReader reader = new BufferedReader(new FileReader("file.txt"));`**, est un exemple de l'utilisation d'un **décorateur (decorator)** en programmation.
- Cela s'appelle un **"modèle de conception de décorateur"** ou simplement un **"décorateur"**.

---

### **Pourquoi "décorateur" ?**
Le **patron de conception (design pattern)** "Décorateur" permet d'ajouter des fonctionnalités à un objet existant (ici, un **`FileReader`**) de manière dynamique, sans modifier sa structure ou son comportement de base.

Dans cet exemple :
1. **`FileReader`** : Est utilisé pour lire un fichier caractère par caractère.
2. **`BufferedReader`** : "Décore" l'objet **`FileReader`** en ajoutant une fonctionnalité de **mise en tampon (buffering)**, qui améliore les performances en réduisant les accès directs au fichier.

---

### **Fonctionnement :**
- **`FileReader`** est l'objet de base qui fournit une méthode pour lire les caractères depuis un fichier.
- **`BufferedReader`** prend **`FileReader`** comme argument dans son constructeur et ajoute un tampon pour optimiser les lectures, permettant également de lire des lignes complètes grâce à la méthode **`readLine()`**.

---

### **Pourquoi utiliser un décorateur ici ?**
1. **Amélioration des performances :**
   - `BufferedReader` réduit les lectures répétées sur le disque en stockant des blocs de données en mémoire (tampon).
2. **Ajout de fonctionnalités :**
   - Par exemple, la méthode **`readLine()`** de `BufferedReader` n'existe pas dans `FileReader`.

---

### **Exemple sans décorateur :**
Lecture caractère par caractère avec **`FileReader`** seul :
```java
FileReader fileReader = new FileReader("file.txt");
int ch;
while ((ch = fileReader.read()) != -1) {
    System.out.print((char) ch);
}
fileReader.close();
```

### **Exemple avec décorateur :**
Lecture ligne par ligne en utilisant **`BufferedReader`** pour améliorer les performances :
```java
BufferedReader reader = new BufferedReader(new FileReader("file.txt"));
String line;
while ((line = reader.readLine()) != null) {
    System.out.println(line);
}
reader.close();
```

---

### **Résumé :**
- **Nom :** Décorateur (en anglais, **Decorator**).
- **Principe :** Ajouter des fonctionnalités supplémentaires à un objet existant sans le modifier.
- **Avantage dans cet exemple :** `BufferedReader` optimise la lecture et offre des fonctionnalités supplémentaires comme la lecture ligne par ligne.
