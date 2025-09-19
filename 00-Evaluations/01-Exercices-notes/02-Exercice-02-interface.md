# Exercice : Interfaces en Java (packages obligatoires)

## Objectifs

* Implémenter une interface et utiliser le polymorphisme via un tableau d’interfaces.
* Structurer un projet Java en **packages**.

## Consignes (obligatoires)



1. Crée une interface `Affichable` avec une méthode `void afficherInfo();`.
2. Implémente cette interface dans **deux classes différentes** (par exemple `Livre` et `Voiture`).

   * Chaque classe doit avoir quelques attributs simples (titre/prix pour un livre, marque/modèle pour une voiture).
   * La méthode `afficherInfo()` doit afficher les informations de l’objet.
3. Dans la classe `Main` :

   * Crée un tableau d’objets de type `Affichable` contenant tes deux objets.
   * Parcours le tableau avec une boucle et appelle `afficherInfo()` sur chaque élément.



* **Packages à créer :**

  * `tp.interfaces` → `Affichable` (et `Payant` pour la variante)
  * `tp.modeles` → `Livre`, `Voiture`
  * `tp.app` → `Main`
* **`Affichable`** : méthode `void afficherInfo();`
* **`Livre`** et **`Voiture`** implémentent `Affichable` (attributs simples, `afficherInfo()` affiche les infos).
* **`Main`** : créer un tableau `Affichable[]`, le parcourir et appeler `afficherInfo()` sur chaque élément.




### Variante (optionnelle)

* Interface `Payant` (méthode `double prixHT();`) dans `tp.interfaces`.
* `Livre` implémente aussi `Payant`.
* Dans `Main`, calculer et afficher la **somme des prix HT** des objets `Payant`.

## Captures d’écran (OBLIGATOIRES)

1. **Arborescence du projet** montrant les **packages** et fichiers :
   `src/tp/interfaces/*.java`, `src/tp/modeles/*.java`, `src/tp/app/Main.java`.
2. **Compilation et exécution réussies** (terminal visible) :

   * Exemple (si vous utilisez `src/` et `out/`) :

     ```
     javac -d out src/tp/interfaces/*.java src/tp/modeles/*.java src/tp/app/Main.java
     java -cp out tp.app.Main
     ```
3. **Sortie du programme** complète (affichage des objets, + total HT si variante).

> Formats acceptés : PNG/JPG. Noms suggérés : `01-arborescence.png`, `02-run.png`, `03-sortie.png`.

## Remise

* **Un seul DOCUMENT WORD, TXT ou PDF** nommé `Nom_Prenom_Interfaces_Java.pdf` contenant :

  1. **Captures d’écran** (obligatoires).
  2. **Annexes — Code** : **copiez-collez** l’intégralité de vos fichiers (voir intitulés ci-dessous).

## Annexes — Code (à coller dans le PDF)

* Annexe A — `src/tp/interfaces/Affichable.java`
* (Variante) Annexe B — `src/tp/interfaces/Payant.java`
* Annexe C — `src/tp/modeles/Livre.java`
* Annexe D — `src/tp/modeles/Voiture.java`
* Annexe E — `src/tp/app/Main.java`

## Checklist rapide

* [ ] **Packages** créés et respectés
* [ ] **Compilation + exécution** visibles en capture
* [ ] **Sortie** affichée (et total HT si variante)
* [ ] **Code complet** en **annexes**
