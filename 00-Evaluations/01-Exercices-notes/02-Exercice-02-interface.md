# Exercice : Interfaces en Java

## Objectifs

* Comprendre comment déclarer et implémenter une interface.
* Manipuler le polymorphisme en utilisant un tableau d’interfaces.



## Consignes

1. Crée une interface `Affichable` avec une méthode `void afficherInfo();`.
2. Implémente cette interface dans **deux classes différentes** (par exemple `Livre` et `Voiture`).

   * Chaque classe doit avoir quelques attributs simples (titre/prix pour un livre, marque/modèle pour une voiture).
   * La méthode `afficherInfo()` doit afficher les informations de l’objet.
3. Dans la classe `Main` :

   * Crée un tableau d’objets de type `Affichable` contenant tes deux objets.
   * Parcours le tableau avec une boucle et appelle `afficherInfo()` sur chaque élément.



## Variante (optionnelle)

1. Ajoute une deuxième interface `Payant` avec une méthode `double prixHT();`.
2. Fais en sorte que `Livre` implémente aussi `Payant`.
3. Dans `Main`, calcule la somme des prix HT de tous les objets qui sont `Payant`.

