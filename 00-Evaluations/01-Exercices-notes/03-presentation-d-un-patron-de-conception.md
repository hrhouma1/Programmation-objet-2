# Évaluation – Patron de conception (présentation individuelle)

<br/>

# 1 -  Objectif

Vous devez présenter un patron de conception de votre choix en suivant la même méthodologie vue en classe.

Le patron choisi doit être un patron reconnu en génie logiciel.
Le patron choisi ne doit pas faire partie des patrons déjà traités ensemble.

Patrons interdits :

* Singleton
* Visiteur (Visitor)
* Délégation
* Itérateur (Iterator)

Vous devez choisir un autre patron (exemples possibles : Strategy, Adapter, Factory Method, Observer, Decorator, Facade, Command, State, etc.).


<br/>

# 2 -  Livrable attendu

Vous devez produire un court document (PDF, Word ou équivalent) ou une courte présentation pptx. Le contenu doit respecter exactement la structure suivante :

### 1. Problème à résoudre (contexte)

Expliquez :

* Quel est le problème concret que l’on rencontre dans un projet logiciel réel.
* Pourquoi une solution directe / naïve serait difficile à maintenir, à faire évoluer ou à réutiliser.

Forme attendue : "On veut faire X, mais sans patron on se retrouve avec Y problème."

Cette section doit être rédigée avec vos mots.

### 2. Intention du patron (rôle du patron)

Expliquez l’idée centrale du patron :

* Qu’est-ce que ce patron permet de faire.
* En une phrase claire : "Ce patron permet de ____ pour pouvoir _____."

Cette phrase doit être courte et compréhensible pour quelqu’un qui n’a pas travaillé sur votre exemple.

### 3. Structure des classes / rôles

Décrivez les rôles principaux du patron :

* Quelles sont les classes ou interfaces importantes.
* Qui dépend de qui.
* Qui connaît qui.

Vous pouvez :

* décrire cela en texte (Classe A utilise Interface B, etc.),
* ou dessiner un petit schéma ASCII.

Exemple de style attendu (générique) :

* Une interface commune.
* Plusieurs implémentations concrètes.
* Une classe qui utilise uniquement l’interface, sans connaître les implémentations concrètes.

L’objectif est de montrer l’architecture, pas de produire un diagramme UML complet.

### 4. Exemple de code Java

Fournissez un extrait minimal mais cohérent de code Java qui démontre le patron.

Exigences obligatoires :

* Le code doit contenir au moins deux classes ou interfaces liées au patron.
* Le code doit inclure une méthode `main` (ou équivalent) qui montre comment on utilise le patron.
* Le code doit être commenté en français pour expliquer le rôle de chaque classe.

Ce que l’on s’attend à voir :

* Une interface ou classe abstraite.
* Une ou deux implémentations concrètes.
* Une classe cliente qui les utilise.
* Un `main` qui appelle tout et montre le résultat.

Le code doit être lisible. Le pseudo-code incomplet n’est pas accepté.

### 5. Avantages du patron

Listez deux ou trois avantages concrets, par exemple :

* Possibilité d’ajouter de nouveaux comportements sans modifier le code existant.
* Séparation claire des responsabilités.
* Réduction du couplage direct entre deux composants.
* Code plus testable ou plus réutilisable.

Les avantages doivent être reliés à votre exemple, pas juste recopiés d’Internet.

### 6. Limites / quand éviter ce patron

Donnez au moins une limite réelle, par exemple :

* Complexité ajoutée dans un projet simple.
* Multiplication du nombre de classes / fichiers.
* Lecture et débogage moins directs.

Une réponse du type "il n’y a pas vraiment de limite" n’est pas acceptée.

### 7. Cas d’usage concret (application réaliste)

Décrivez un exemple de scénario où ce patron serait pertinent, par exemple :

* Application interactive
* Traitement de données
* Système transactionnel
* Gestion d’actions ou de commandes
* Gestion de notifications
* Jeu, simulation, moteur d’état
* Interface modulaire, etc.

Le cas doit être réaliste sur le plan logiciel.


<br/>


# 3 - Règles

* Travail individuel.
* Maximum : 2 pages de texte, plus votre code. (Le code ne compte pas dans la limite de 2 pages.)
* Vous devez respecter les 7 sections ci-dessus dans l’ordre.
* Le patron choisi ne doit pas être :

  * Singleton
  * Visiteur (Visitor)
  * Délégation
  * Itérateur (Iterator)

Plagiat ou utilisation d’un patron interdit : note 0.

<br/>

# 5 - Grille d’évaluation (20 points)

| Critère                                                       | Points |
| ------------------------------------------------------------- | ------ |
| 1. Problème à résoudre (section 1) clair, concret, crédible   | /3     |
| 2. Intention du patron (section 2) formulée correctement      | /2     |
| 3. Structure et rôles des classes (section 3) compréhensibles | /3     |
| 4. Qualité du code Java (section 4)                           | /6     |
| • Code cohérent avec le patron choisi                         |        |
| • Présence de commentaires en français                        |        |
| • Démonstration d’utilisation (`main`)                        |        |
| 5. Avantages (section 5)                                      | /2     |
| 6. Limites / quand ne pas l’utiliser (section 6)              | /2     |
| 7. Cas d’usage concret et réaliste (section 7)                | /2     |

Total : /20

Pénalité possible (jusqu’à -40% de la note finale) si :

* Le patron présenté fait partie des patrons interdits.
* Le code ne correspond pas au patron annoncé.
* Le travail ne respecte pas la structure imposée (sections 1 à 7).
