### Années d’apparition des classes Java pour la lecture des fichiers

| **Personnage**            | **Classe utilisée** | **Année d’apparition** | **Contexte d'introduction**                                   |
|----------------------------|---------------------|-------------------------|----------------------------------------------------------------|
| **Grand-mère**             | `FileReader`        | 1996                   | Introduit dans **Java 1.0**, utilisé pour lire des caractères. |
| **Le chercheur**           | `Scanner`           | 2004                   | Introduit dans **Java 5**, pratique pour lire des données formatées. |
| **L’ingénieur efficace**   | `BufferedReader`    | 1996                   | Introduit dans **Java 1.0**, optimisé avec un tampon pour les gros fichiers. |
| **Le jeune pressé**        | `Files`             | 2011                   | Introduit dans **Java 7** (NIO.2), moderne pour la gestion des fichiers. NIO= Non-blocking I/O (I/O non bloquant) utilise les buffers |

---

### Détails complémentaires
1. **`FileReader` (1996) :**  
   - Conçu pour lire des fichiers **caractère par caractère**.
   - Fait partie des classes fondamentales de la première version de Java.

2. **`Scanner` (2004) :**  
   - Ajouté avec **Java 5**, lors de l'introduction des fonctionnalités modernes comme les génériques.
   - Permet une lecture facile des **données formatées**.

3. **`BufferedReader` (1996) :**  
   - Présent depuis **Java 1.0**, il optimise la lecture en utilisant un **tampon**, rendant les lectures ligne par ligne plus rapides et efficaces.

4. **`Files` (2011) :**  
   - Arrivé avec **Java 7**, dans le cadre de l'API **NIO.2**, pour une manipulation moderne et performante des fichiers et chemins.

--- 

**En résumé :**  
Java offre des outils de lecture adaptés à tous les besoins, avec des évolutions significatives depuis **1996** pour passer de la précision minutieuse (`FileReader`) à des lectures modernes et globales (`Files`). 🚀
