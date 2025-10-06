# **Examen 1 – Programmation Orientée Objet en Java**

**Date :** ………
**Durée :** 1 h 30
**À remettre :** un dossier `src/` compilable + un document Word ou .txt décrivant vos réponses et vos captures d’écran.


# **Partie 1 – Classe de base (10 points)**

### **Question 1 (10 pts)**

Créer une classe `Film` avec :

* Attributs privés : `titre` (String), `realisateur` (String), `duree` (int, en minutes)
* Un constructeur initialisant ces trois attributs
* Une méthode `afficher()` qui affiche :

**Exemple de sortie :**

```
Titre : Inception | Réalisateur : Christopher Nolan | Durée : 148 minutes
```

```java
// Film.java
...........................................................
...........................................................
...........................................................
...........................................................
```


<br/>

# **Partie 2 – Héritage et polymorphisme (30 points)**

### **Question 2 (10 pts)**

Créer une classe `Appareil` avec une méthode `utiliser()` qui affiche :

```
Utilisation d’un appareil générique
```

```java
// Appareil.java
...........................................................
...........................................................
...........................................................
```

# **Question 3 (10 pts)**

Créer deux classes qui héritent de `Appareil` :

* `Ordinateur` : `utiliser()` affiche :

  ```
  Utilisation d’un ordinateur portable
  ```
* `Smartphone` : `utiliser()` affiche :

  ```
  Utilisation d’un smartphone moderne
  ```

```java
// Ordinateur.java
...........................................................
...........................................................
...........................................................

// Smartphone.java
...........................................................
...........................................................
...........................................................
```

# **Question 4 (10 pts)**

Créer une fonction `demarrerUtilisation(Appareil a)` qui prend un objet `Appareil` (ou sous-classe) et appelle `utiliser()`.

**Exemple d’utilisation :**

```java
Appareil a1 = new Ordinateur();
Appareil a2 = new Smartphone();
demarrerUtilisation(a1);
demarrerUtilisation(a2);
```

**Sortie attendue :**

```
Utilisation d’un ordinateur portable  
Utilisation d’un smartphone moderne
```

```java
// AppMain.java
...........................................................
...........................................................
...........................................................
```



# **Partie 3 – Classes abstraites (20 points)**

# **Question 5 (5 pts)**

Créer une classe abstraite `Vehicule` contenant une méthode abstraite `double calculVitesse()`.

```java
// Vehicule.java
...........................................................
...........................................................
...........................................................
```

# **Question 6 (10 pts)**

Créer deux classes concrètes qui héritent de `Vehicule` :

* `Voiture` avec `distance` et `temps`
* `Moto` avec `distance` et `temps`
  Chacune implémente `calculVitesse()` (vitesse = distance / temps).

```java
// Voiture.java
...........................................................
...........................................................
...........................................................

// Moto.java
...........................................................
...........................................................
...........................................................
```

# **Question 7 (5 pts)**

Créer une fonction `afficherVitesse(Vehicule v)` qui affiche la vitesse, quelle que soit la classe :

**Exemple d’utilisation :**

```java
Vehicule v1 = new Voiture(150, 2);
Vehicule v2 = new Moto(100, 1);
afficherVitesse(v1);
afficherVitesse(v2);
```

**Sortie attendue :**

```
Vitesse : 75.0 km/h  
Vitesse : 100.0 km/h
```

```java
// TestVehicule.java
...........................................................
...........................................................
...........................................................
```



# **Partie 4 – Encapsulation (15 points)**

## **Question 8 (15 pts)**

Créer une classe `Thermometre` avec :

* Un attribut privé `temperature`
* Un getter `getTemperature()` et un setter qui interdit les températures < -273 (zéro absolu).

**Exemple d’utilisation :**

```java
Thermometre t = new Thermometre(20);
System.out.println(t.getTemperature());
t.setTemperature(30);
System.out.println(t.getTemperature());
t.setTemperature(-300);
```

**Sortie attendue :**

```
20  
30  
Erreur : température invalide (inférieure au zéro absolu).
```

```java
// Thermometre.java
...........................................................
...........................................................
...........................................................
```



## **Partie 5 – Méthodes supplémentaires (15 points)**

### **Question 9 (15 pts)**

Ajouter à `Thermometre` :

* `augmenter(double degre)` : augmente la température
* `diminuer(double degre)` : diminue la température seulement si ≥ -273, sinon afficher une erreur.

**Exemple d’utilisation :**

```java
Thermometre t = new Thermometre(10);
t.augmenter(15);
System.out.println(t.getTemperature());
t.diminuer(5);
System.out.println(t.getTemperature());
t.diminuer(300);
```

**Sortie attendue :**

```
25  
20  
Erreur : température invalide (inférieure au zéro absolu).
```

```java
// Thermometre.java (complément)
...........................................................
...........................................................
...........................................................
```



## **Partie 6 – Mise en œuvre complète (un peu plus difficile – 20 points)**

### **Question 10 (20 pts)**

Créer un mini-système contenant :

* Une classe abstraite `Utilisateur`
* Deux classes filles : `Admin` et `Invite`
* Un attribut privé `nom` avec getter/setter validant que le nom n’est pas vide
* Une méthode `connexion()` décorée avec un décorateur `logger` qui affiche "Connexion détectée" avant le message propre à chaque utilisateur.

**Exemple d’utilisation :**

```java
Admin admin = new Admin("Alice");
Invite invite = new Invite("Bob");
admin.connexion();
invite.connexion();
```

**Sortie attendue :**

```
Connexion détectée  
Admin Alice connecté  
Connexion détectée  
Invite Bob connecté
```

*(Astuce : en Java, le “décorateur” se fait avec une méthode statique qui appelle la méthode réelle et affiche un message avant.)*

```java
// Utilisateur.java
...........................................................
...........................................................
...........................................................

// Admin.java
...........................................................
...........................................................
...........................................................

// Invite.java
...........................................................
...........................................................
...........................................................

// Logger.java (utilitaire statique)
...........................................................
...........................................................
...........................................................
```



## **Consignes de rendu**

* Pour chaque question, copiez votre code dans un fichier Java correspondant.
* Fournissez dans votre rapport Word ou .txt :

  * le code complété,
  * les captures d’écran de l’exécution de chaque partie.
* Votre code doit compiler sans erreur et produire les sorties attendues.


