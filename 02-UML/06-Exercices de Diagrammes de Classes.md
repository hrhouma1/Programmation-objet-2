# 1. Exercices de Diagrammes de Classes

### Exercice 1 — Bibliothèque

* Conçois un diagramme de classes pour une **bibliothèque en ligne** :

  * Classes possibles : `Livre`, `Auteur`, `Utilisateur`, `Emprunt`.
  * Relations à modéliser : un livre a un auteur, un utilisateur peut emprunter plusieurs livres, un emprunt relie un utilisateur à un livre avec une date de retour.

### Exercice 2 — Application de Livraison

* Conçois un diagramme de classes pour une **application de livraison** :

  * Classes possibles : `Commande`, `Client`, `Livreur`, `Produit`.
  * Relations : un client peut passer plusieurs commandes, une commande contient plusieurs produits, un livreur est assigné à une commande.

---

# 2. Exercices de Diagrammes de Cas d’Utilisation

### Exercice 1 — Guichet Automatique Bancaire (GAB)

* Système : guichet automatique.
* Acteurs : `Client`, `Banque`.
* Cas d’utilisation à représenter :

  * Retirer de l’argent
  * Consulter le solde
  * Déposer de l’argent
  * Vérifier le solde auprès de la banque

### Exercice 2 — Plateforme de Streaming

* Système : application de streaming vidéo.
* Acteurs : `Utilisateur`, `Administrateur`.
* Cas d’utilisation à représenter :

  * S’inscrire
  * Se connecter
  * Regarder une vidéo
  * Gérer la bibliothèque de vidéos (admin)

---

# 3. Exercices de Diagrammes de Séquence

### Exercice 1 — Achat en ligne

* Scénario : un utilisateur achète un produit.
* Objets : `Utilisateur`, `SiteWeb`, `Panier`, `Système de Paiement`.
* Étapes :

  * L’utilisateur ajoute un produit au panier.
  * Le site demande la validation du paiement.
  * Le système de paiement confirme.
  * Le site confirme la commande à l’utilisateur.

### Exercice 2 — Réservation de Vol

* Scénario : un client réserve un vol via une agence en ligne.
* Objets : `Client`, `Agence`, `CompagnieAérienne`, `SystèmePaiement`.
* Étapes :

  * Le client demande un vol à l’agence.
  * L’agence contacte la compagnie aérienne.
  * La compagnie propose les vols disponibles.
  * Le client choisit un vol et paie via le système de paiement.
  * Confirmation envoyée au client.
