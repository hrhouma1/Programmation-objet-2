# 01- ÉNONCÉ

On souhaite concevoir une classe **`ChannelIterator`** pour **parcourir les émissions TV** disponibles via un **contrôleur à distance**.
Le contrôleur connaît plusieurs **chaînes** (`Chaine`), chacune ayant une grille d’**émissions** (`Emission`).
L’objectif est d’appliquer le **patron *Iterator*** :

> Le client ne connaît **ni** la structure interne des chaînes **ni** comment les émissions sont stockées.
> Il obtient un **itérateur** qui lui permet de **parcourir** séquentiellement les émissions.

L’utilisateur pourra :

* Créer quelques chaînes et leurs émissions,
* Créer un **contrôleur** qui agrège ces chaînes,
* Obtenir un **itérateur** (`ChannelIterator`) pour parcourir **toutes les émissions** simplement (ex. avec un `for-each` via un `Iterable`).

<br/>

# 02 - Résumé

| Élément              | Rôle                                                                         |
| -------------------- | ---------------------------------------------------------------------------- |
| `Emission`           | Modèle d’une émission (titre, heure début/fin).                              |
| `Chaine`             | Contient une liste d’émissions.                                              |
| `ControleurDistance` | Agrégat : connaît toutes les chaînes et expose un `Iterable`.                |
| `ChannelIterator`    | **Itérateur** qui parcourt les émissions **sans** exposer la structure.      |
| Ligne clé            | `return new ChannelIterator(this);` → le contrôleur **fournit** l’itérateur. |

> Le client consomme l’API de parcours **sans dépendre** des détails internes (`List`, ordre, etc.).

<br/>

# 03 - Correction

```java
import java.time.LocalTime;

class Emission {
    final String titre;
    final LocalTime debut, fin;

    Emission(String titre, LocalTime debut, LocalTime fin) {
        this.titre = titre; this.debut = debut; this.fin = fin;
    }

    @Override public String toString() {
        return titre + " [" + debut + "-" + fin + "]";
    }
}
```

```java
import java.util.ArrayList;
import java.util.List;

class Chaine {
    final int numero;
    final String nom;
    final List<Emission> grille = new ArrayList<>();

    Chaine(int numero, String nom) { this.numero = numero; this.nom = nom; }

    Chaine ajouter(Emission e) { grille.add(e); return this; }
}
```

```java
import java.util.Iterator;
import java.util.List;
import java.util.NoSuchElementException;

/** Itérateur qui parcourt, chaîne par chaîne, toutes les émissions. */
class ChannelIterator implements Iterator<Emission> {
    private final List<Chaine> chaines;
    private int idxChaine = 0;
    private int idxEmission = 0;

    ChannelIterator(List<Chaine> chaines) { this.chaines = chaines; advanceToNextValid(); }

    private void advanceToNextValid() {
        while (idxChaine < chaines.size() &&
               idxEmission >= chaines.get(idxChaine).grille.size()) {
            idxChaine++;
            idxEmission = 0;
        }
    }

    @Override public boolean hasNext() {
        return idxChaine < chaines.size();
    }

    @Override public Emission next() {
        if (!hasNext()) throw new NoSuchElementException();
        Emission e = chaines.get(idxChaine).grille.get(idxEmission++);
        advanceToNextValid();
        return e;
    }
}
```

```java
import java.util.List;

/** Agrégat : connaît les chaînes et fournit un Iterable<Emission>. */
class ControleurDistance {
    private final List<Chaine> chaines;

    ControleurDistance(List<Chaine> chaines) { this.chaines = chaines; }

    /** Permet l'usage d'un for-each via Iterable. */
    public Iterable<Emission> emissions() {
        return () -> new ChannelIterator(chaines);
    }
}
```

```java
import java.time.LocalTime;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        Chaine c1 = new Chaine(1, "News24")
            .ajouter(new Emission("Morning Brief",  LocalTime.of(6, 0),  LocalTime.of(9, 0)))
            .ajouter(new Emission("Midday Live",    LocalTime.of(12, 0), LocalTime.of(13, 0)));

        Chaine c2 = new Chaine(2, "Cinema+")
            .ajouter(new Emission("Classic 1",      LocalTime.of(8, 0),  LocalTime.of(10, 0)))
            .ajouter(new Emission("Classic 2",      LocalTime.of(20, 0), LocalTime.of(22, 0)));

        ControleurDistance remote = new ControleurDistance(List.of(c1, c2));

        // ✅ Parcours simple, sans connaître la structure interne
        for (Emission e : remote.emissions()) {
            System.out.println(e);
        }
    }
}
```

<br/>

# 04 - Explication claire

* **But de l’Iterator** : offrir un **parcours séquentiel** standard (via `Iterator<T>`) sans révéler **comment** les émissions sont stockées (plusieurs listes par chaîne, etc.).
* **Ligne clé** :

  ```java
  return () -> new ChannelIterator(chaines);
  ```

  👉 Le **contrôleur** fournit un **itérateur** ; le client peut faire un `for-each` sans dépendre des détails internes.
* L’itérateur gère deux indices (`idxChaine`, `idxEmission`) et avance proprement d’une émission à l’autre, puis d’une chaîne à l’autre.


<br/>

# 05 - Itérateur & Agrégat

**Agrégat :** `ControleurDistance`
→ centralise les chaînes et **fournit** un `Iterable<Emission>`.

**Itérateur :** `ChannelIterator`
→ encapsule la **mécanique de parcours** (indices, contrôles de fin, exceptions), ce qui **simplifie** le code client.

<br/>

💡 **Extensions possibles (faciles)**

* Ajouter `emissionsTrieesParHeure()` : fusionner toutes les grilles puis trier.
* Ajouter `emissionsEnCours(LocalTime now)` : filtrer celles actives maintenant.
* Créer un `Iterable<Emission>` alternatif (ordre inverse, par genre, etc.).
