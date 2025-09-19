# NIO ?

**NIO** est l’abréviation de **New Input/Output**.  

### **Contexte**
Java NIO fait référence à une API introduite dans **Java 1.4** pour améliorer les opérations d'entrée/sortie (Input/Output ou I/O) traditionnelles. Elle a été conçue pour être plus rapide, flexible et efficace que l'ancienne API **java.io**, en particulier pour les applications nécessitant des performances élevées ou la gestion de grandes quantités de données.

---

### **Principales caractéristiques de Java NIO :**
1. **Buffer-oriented (orienté buffers)**  
   - Contrairement à l'ancienne API I/O qui est **stream-oriented** (orientée flux), NIO utilise des **buffers** pour stocker les données lors des opérations d'entrée/sortie.
   - Les buffers permettent de lire et écrire des données de manière plus efficace.

2. **Non-blocking I/O (I/O non bloquant)**  
   - Permet de lire ou écrire des données sans bloquer le thread principal.
   - Très utile pour les serveurs ou les applications réseau où de nombreux clients doivent être gérés simultanément.

3. **Selectors (sélecteurs)**  
   - Offrent un moyen efficace de surveiller plusieurs canaux (channels) en même temps, utile pour gérer des connexions multiples avec un seul thread.

4. **File System Access**  
   - Fournit des fonctionnalités modernes pour interagir avec le système de fichiers, comme la lecture, l'écriture, la création et la suppression de fichiers et répertoires via les classes **Files** et **Paths**.

---

### **Exemple de classes principales dans Java NIO :**
1. **`java.nio.file.Files`**  
   - Utilisée pour manipuler les fichiers et répertoires de manière simple et concise.

2. **`java.nio.file.Paths`**  
   - Permet de créer des objets représentant les chemins de fichiers ou répertoires.

3. **`java.nio.channels`**  
   - Contient des classes comme `FileChannel`, `SocketChannel`, et `ServerSocketChannel` pour gérer des canaux non bloquants.

4. **`java.nio.ByteBuffer`**  
   - Utilisé pour stocker et manipuler des données dans des buffers.

---

### **Pourquoi Java NIO ?**
- Plus adapté pour les **applications réseau** ou les **applications nécessitant des performances élevées**.
- Simplifie les opérations courantes avec le système de fichiers.
- Permet une **gestion simultanée de plusieurs connexions** grâce aux sélecteurs.

En résumé, **Java NIO** est une API moderne qui surmonte plusieurs limitations de l'ancienne API I/O et est particulièrement adaptée pour les applications modernes nécessitant efficacité et scalabilité.
