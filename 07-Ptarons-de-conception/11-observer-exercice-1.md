# Exercice — Netflix : alerte “nouvelle saison” (*Emily in Paris*)

## Problème

* Des utilisateurs veulent être **prévenus automatiquement** quand **Emily in Paris** publie une **nouvelle saison**.
* Ils doivent pouvoir **s’abonner** et **se désabonner** librement.
* À l’annonce d’une saison, **seuls les abonnés** de cette série reçoivent la notification.

## Objectif

* Implémenter le **pattern Observer** :

  * **Sujet** : `Serie` (*Emily in Paris*), gère la liste d’abonnés et **notifie**.
  * **Observateur** : `Utilisateur`, **réagit** à la notification.
* API minimale attendue :

  * `abonner(Utilisateur)`, `desabonner(Utilisateur)`, `annoncerNouvelleSaison(int numero)`.

## Scénario à reproduire (console)

1. Créer `Serie emily = new Serie("Emily in Paris")`
2. Créer 3 utilisateurs : `Alice`, `Bob`, `Chloé`
3. Abonner **Alice** et **Bob** (Chloé non-abonnée)
4. `emily.annoncerNouvelleSaison(4)` → notif **Alice**, **Bob**
5. **Bob se désabonne**
6. `emily.annoncerNouvelleSaison(5)` → notif **Alice** seulement

## Contraintes

* Sorties **console** uniquement (pas d’email/SMS réels).
* **Interfaces** pour découpler Sujet / Observateur.
* Code **lisible**, messages clairs.

---

## Mermaid — Diagramme de classes

```mermaid
classDiagram
  direction TB

  class Sujet {
    <<interface>>
    +abonner(Observateur o) void
    +desabonner(Observateur o) void
    +notifier(String message) void
  }

  class Observateur {
    <<interface>>
    +mettreAJour(String message) void
  }

  class Serie {
    -String nom
    -List~Observateur~ abonnes
    +Serie(String nom)
    +annoncerNouvelleSaison(int numero) void
    +abonner(Observateur o) void
    +desabonner(Observateur o) void
    +notifier(String message) void
  }

  class Utilisateur {
    -String nom
    +Utilisateur(String nom)
    +mettreAJour(String message) void
  }

  Sujet <|.. Serie
  Observateur <|.. Utilisateur
  Serie "1" o-- "0..*" Observateur : Netflix (*Emily in Paris*)
```

## Mermaid — Diagramme de séquence

```mermaid
sequenceDiagram
  autonumber
  participant App
  participant Serie as Serie("Emily in Paris") @ Netflix
  participant Alice as Utilisateur("Alice")
  participant Bob as Utilisateur("Bob")
  participant Chloe as Utilisateur("Chloé")

  App->>Serie: abonner(Alice)
  App->>Serie: abonner(Bob)

  App->>Serie: annoncerNouvelleSaison(4)
  Serie-->>Alice: "[Emily in Paris] S4 disponible !"
  Serie-->>Bob:   "[Emily in Paris] S4 disponible !"

  App->>Serie: desabonner(Bob)

  App->>Serie: annoncerNouvelleSaison(5)
  Serie-->>Alice: "[Emily in Paris] S5 disponible !"
```
