# 📘 Exercices – Visibilité et Paquetages en Java

## Partie 1 – Visibilité des classes et membres

### Exercice 1 – Attributs privés et publics

1. Crée une classe `Personne` avec deux attributs :

   * `nom` (public)
   * `age` (private)
2. Ajoute une méthode `afficherInfo()` qui affiche les deux attributs.
3. Depuis une classe `Main`, essaie d’accéder directement à `nom` et à `age`.

   * Que se passe-t-il ? Pourquoi ?



### Exercice 2 – Méthodes protégées

1. Crée une classe `Animal` avec une méthode protégée `manger()`.
2. Crée une sous-classe `Chien` qui hérite de `Animal`.
3. Dans `Chien`, appelle `manger()` et explique pourquoi cela fonctionne.
4. Essaie d’appeler `manger()` depuis `Main`. Est-ce possible ?



### Exercice 3 – Package-private (visibilité par défaut)

1. Crée deux classes `A` et `B` dans le même package `com.test`.

   * `A` contient une méthode **sans modificateur** `saluer()`.
   * `B` essaie d’appeler `saluer()`.
2. Déplace `B` dans un autre package (`com.autre`) et réessaie.

   * Que constates-tu ?



### Exercice 4 – Résumé des niveaux de visibilité

Complète ce tableau en testant avec ton code :

| Tentative d’accès | Même classe | Même package | Sous-classe | Classe extérieure |
| ----------------- | ----------- | ------------ | ----------- | ----------------- |
| private           |             |              |             |                   |
| (package)         |             |              |             |                   |
| protected         |             |              |             |                   |
| public            |             |              |             |                   |



## Partie 2 – Paquetages (Packages)

### Exercice 5 – Organisation en packages

1. Crée l’arborescence suivante :

   ```
   src/
    └── com/
         └── banque/
              ├── Compte.java
              └── Client.java
   ```
2. Dans `Compte.java`, ajoute un attribut `solde` et une méthode `afficherSolde()`.
3. Dans `Client.java`, crée un client avec un compte.
4. Ajoute une classe `Main` dans un autre package `com.app` qui importe `com.banque.*` et utilise `Client` et `Compte`.



### Exercice 6 – Conflit de noms

1. Crée deux packages :

   ```
   com.animaux
   com.voitures
   ```

   * Dans `com.animaux`, crée une classe `Chien`.
   * Dans `com.voitures`, crée une classe `Chien`.
2. Dans `Main`, importe les deux et montre comment résoudre le conflit de nom avec le **nom qualifié complet** (`com.animaux.Chien`, `com.voitures.Chien`).



### Exercice 7 – Accès entre packages

1. Crée un package `com.util` contenant une classe `Outils` avec :

   * une méthode publique `afficherMessage()`
   * une méthode protégée `calculer()`
   * une méthode sans modificateur `helper()`
2. Depuis un autre package `com.app`, essaie d’accéder à chacune des méthodes.

   * Note lesquelles sont accessibles et pourquoi.


