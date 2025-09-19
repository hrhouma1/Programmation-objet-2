

### **1. Classes et Objets**
- **Classe** : Modèle ou blueprint qui définit les propriétés (attributs) et comportements (méthodes) des objets.
- **Objet** : Instance d'une classe.

**Exemple :**

```java
class Animal {
    String nom;
    void manger() {
        System.out.println(nom + " mange.");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal chat = new Animal();
        chat.nom = "Chat";
        chat.manger(); // Affiche : Chat mange.
    }
}
```

---

### **2. Encapsulation**
- Regrouper les données (attributs) et les méthodes qui les manipulent dans une même classe.
- Utiliser des modificateurs d'accès (**private**, **public**, **protected**) pour protéger les données.

**Exemple :**

```java
class CompteBancaire {
    private double solde;

    public double getSolde() {
        return solde;
    }

    public void deposer(double montant) {
        if (montant > 0) {
            solde += montant;
        }
    }
}

public class Main {
    public static void main(String[] args) {
        CompteBancaire compte = new CompteBancaire();
        compte.deposer(100);
        System.out.println("Solde : " + compte.getSolde());
    }
}
```

---

### **3. Héritage**
- Permet à une classe (classe enfant) d'hériter des propriétés et méthodes d'une autre classe (classe parent).

**Exemple :**

```java
class Animal {
    void dormir() {
        System.out.println("L'animal dort.");
    }
}

class Chat extends Animal {
    void miauler() {
        System.out.println("Le chat miaule.");
    }
}

public class Main {
    public static void main(String[] args) {
        Chat chat = new Chat();
        chat.dormir(); // Hérité de Animal
        chat.miauler();
    }
}
```

---

### **4. Polymorphisme**
- **Polymorphisme statique (surcharge)** : Plusieurs méthodes avec le même nom mais des paramètres différents.
- **Polymorphisme dynamique (redéfinition)** : Une méthode de la classe enfant remplace celle de la classe parent.

**Exemple de surcharge :**

```java
class Calculateur {
    int addition(int a, int b) {
        return a + b;
    }
    double addition(double a, double b) {
        return a + b;
    }
}

public class Main {
    public static void main(String[] args) {
        Calculateur calc = new Calculateur();
        System.out.println(calc.addition(2, 3));       // 5
        System.out.println(calc.addition(2.5, 3.5));   // 6.0
    }
}
```

**Exemple de redéfinition :**

```java
class Animal {
    void parler() {
        System.out.println("L'animal fait un bruit.");
    }
}

class Chien extends Animal {
    @Override
    void parler() {
        System.out.println("Le chien aboie.");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal animal = new Chien();
        animal.parler(); // Affiche : Le chien aboie.
    }
}
```

---

### **5. Abstraction**
- Utiliser des **classes abstraites** ou des **interfaces** pour définir des comportements sans implémentation complète.
- Une classe abstraite peut contenir des méthodes avec ou sans corps.
- Une interface définit des méthodes sans corps (toutes les méthodes sont abstraites par défaut).

**Exemple de classe abstraite :**

```java
abstract class Animal {
    abstract void parler();
}

class Chien extends Animal {
    void parler() {
        System.out.println("Le chien aboie.");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal animal = new Chien();
        animal.parler();
    }
}
```

**Exemple d'interface :**

```java
interface Vehicule {
    void demarrer();
}

class Voiture implements Vehicule {
    public void demarrer() {
        System.out.println("La voiture démarre.");
    }
}

public class Main {
    public static void main(String[] args) {
        Vehicule voiture = new Voiture();
        voiture.demarrer();
    }
}
```

---

### **6. Interface vs Classe abstraite**
| **Caractéristique**             | **Classe abstraite**      | **Interface**         |
|----------------------------------|---------------------------|------------------------|
| Héritage multiple                | Non (héritage unique)     | Oui (plusieurs interfaces) |
| Méthodes concrètes (avec corps)  | Oui                       | Non (avant Java 8)    |
| Accès aux attributs              | Oui                       | Non                   |

---

### **7. Concepts supplémentaires**
- **Constructeurs** : Méthodes spéciales utilisées pour initialiser un objet.
- **Static** : Les attributs/méthodes statiques appartiennent à la classe, pas aux objets.
- **Final** : Utilisé pour empêcher l'héritage ou la redéfinition de méthodes.
- **Packages** : Permettent d'organiser les classes.
- **Exception Handling** : Gestion des erreurs avec `try`, `catch`, et `finally`.

**Exemple d'exception :**

```java
public class Main {
    public static void main(String[] args) {
        try {
            int resultat = 10 / 0;
        } catch (ArithmeticException e) {
            System.out.println("Erreur : Division par zéro !");
        }
    }
}
```

