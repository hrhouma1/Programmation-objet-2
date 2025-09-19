# Exercice : Pourquoi utilise-t-on la méthode scanner.close() ?

  ```java
  Scanner scanner = new Scanner(new File("file.txt"));
  while (scanner.hasNextLine()) {
      System.out.println(scanner.nextLine());
  }
  scanner.close();
  ```


Dans cet exemple, l’appel à **`scanner.close()`** ne ferme pas une connexion réseau ou une autre ressource distante, mais il **libère les ressources associées à l'objet `Scanner`**, notamment celles liées au fichier ouvert.

### **Pourquoi `scanner.close()` est nécessaire ?**
Lorsque vous utilisez un **Scanner** pour lire un fichier (comme dans l'exemple), il ouvre un **flux d'entrée (InputStream)** sur le fichier. L'appel à **`close()`** :
- **Ferme le flux d'entrée sous-jacent** (ici, un flux basé sur le fichier).
- Libère les ressources système associées, comme les descripteurs de fichiers.

Si vous oubliez de fermer le scanner :
- Le fichier peut rester verrouillé dans certains systèmes, empêchant d'autres programmes ou opérations de le modifier ou de le supprimer.
- Cela peut provoquer une **fuite de ressources** (le système maintient un descripteur inutilement ouvert).

---

### **Ce qui est réellement fermé ici**
Dans cet exemple :
```java
Scanner scanner = new Scanner(new File("file.txt"));
while (scanner.hasNextLine()) {
    System.out.println(scanner.nextLine());
}
scanner.close();
```
L’appel à **`scanner.close()`** va :
1. Fermer l’objet **`FileInputStream`** que le `Scanner` utilise en interne pour lire le fichier `file.txt`.
2. Libérer toutes les ressources système associées à la lecture du fichier.

---

### **Bonne pratique**
Il est toujours recommandé de fermer un `Scanner` ou tout autre flux de lecture/écriture une fois qu'il n'est plus utilisé. Une alternative moderne serait d'utiliser un **bloc `try-with-resources`**, qui ferme automatiquement le scanner :
```java
try (Scanner scanner = new Scanner(new File("file.txt"))) {
    while (scanner.hasNextLine()) {
        System.out.println(scanner.nextLine());
    }
} // Pas besoin d'appeler close(), le bloc gère la fermeture automatiquement.
```

### **En résumé**
Dans ce cas, **`scanner.close()`** ferme le fichier ouvert en arrière-plan, libérant ainsi le descripteur du fichier et garantissant que les ressources système sont disponibles pour d'autres usages.
