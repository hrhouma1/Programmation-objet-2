### **Lecture avec `Scanner` : Mot à mot ou ligne par ligne**

1. **Lire mot à mot :**  
   Utilisez la méthode **`next()`** pour lire un mot à la fois.  
   ```java
   Scanner scanner = new Scanner(new File("example.txt"));
   while (scanner.hasNext()) { // Vérifie s'il reste un mot
       String word = scanner.next(); // Lit le prochain mot
       System.out.println(word); // Affiche le mot
   }
   scanner.close();
   ```

2. **Lire ligne par ligne :**  
   Utilisez la méthode **`nextLine()`** pour lire une ligne entière à la fois.  
   ```java
   Scanner scanner = new Scanner(new File("example.txt"));
   while (scanner.hasNextLine()) { // Vérifie s'il reste une ligne
       String line = scanner.nextLine(); // Lit la prochaine ligne
       System.out.println(line); // Affiche la ligne
   }
   scanner.close();
   ```

---

### **Quand utiliser quelle méthode ?**
- **`next()`** : Si vous voulez traiter le fichier **mot par mot** (par exemple, analyser les mots séparément).  
- **`nextLine()`** : Si vous voulez traiter des données **ligne par ligne** (par exemple, lire des phrases ou analyser des lignes entières).  

