# 01 – ÉNONCÉ

On veut concevoir un petit système capable de **décompresser des fichiers** selon **leur format**.

* Si le fichier est en **`.zip`** → on utilise **WinZip** (ou équivalent).
* Si le fichier est en **`.rar`** → on utilise **WinRar** (ou équivalent).

Le point important :
👉 **l’application ne doit pas être bourrée de `if/else` ou `switch` partout** pour choisir l’algorithme.
On veut appliquer le **patron de conception Strategy** :

> On définit une **famille d’algorithmes** (ici : décompresser ZIP, décompresser RAR),
> on les **encapsule** derrière une **interface commune**,
> et on permet au **contexte** de les **interchanger dynamiquement**.

Concrètement, on veut :

1. Une interface `DecompressionStrategy` qui décrit **“comment on décompresse”**.
2. Deux stratégies concrètes : `ZipDecompressionStrategy` et `RarDecompressionStrategy`.
3. Un **contexte** `FileDecompressor` qui **ne fait pas le travail lui-même**, mais **délègue à la stratégie** choisie.
4. Le client (ex. `main`) choisit la bonne stratégie selon le fichier, **sans modifier** le contexte.

---

# 02 – Résumé

| Élément                    | Rôle                                                                 |
| -------------------------- | -------------------------------------------------------------------- |
| `DecompressionStrategy`    | **Interface Strategy** : décrit l’opération `decompress(...)`.       |
| `ZipDecompressionStrategy` | Stratégie concrète pour `.zip` (WinZip).                             |
| `RarDecompressionStrategy` | Stratégie concrète pour `.rar` (WinRar).                             |
| `FileDecompressor`         | **Context** : tient une stratégie et l’invoque.                      |
| Ligne clé                  | `strategy.decompress(filePath);` → c’est là que le **Strategy agit** |

> **Idée clef** : on ne change pas le code du contexte pour ajouter un nouveau format, on ajoute une **nouvelle stratégie**.

---

# 03 – Correction (code complet)

```java
// ===== 1) L'interface STRATEGY =====
public interface DecompressionStrategy {
    void decompress(String filePath);
}
```

```java
// ===== 2) Stratégie concrète pour les .zip =====
public class ZipDecompressionStrategy implements DecompressionStrategy {
    @Override
    public void decompress(String filePath) {
        // Ici on simule seulement
        System.out.println("[ZIP] Décompression de " + filePath + " avec WinZip...");
        // Dans un vrai système : appel à une lib ZIP
    }
}
```

```java
// ===== 3) Stratégie concrète pour les .rar =====
public class RarDecompressionStrategy implements DecompressionStrategy {
    @Override
    public void decompress(String filePath) {
        // Simulation
        System.out.println("[RAR] Décompression de " + filePath + " avec WinRar...");
        // Dans un vrai système : appel à une lib RAR
    }
}
```

```java
// ===== 4) CONTEXTE : utilise une stratégie, mais ne connaît PAS les détails =====
public class FileDecompressor {

    // Stratégie actuelle
    private DecompressionStrategy strategy;

    // On peut imposer une stratégie au départ
    public FileDecompressor(DecompressionStrategy strategy) {
        this.strategy = strategy;
    }

    // On peut aussi la changer à chaud
    public void setStrategy(DecompressionStrategy strategy) {
        this.strategy = strategy;
    }

    // Méthode métier : décompresser un fichier
    public void decompress(String filePath) {
        if (strategy == null) {
            throw new IllegalStateException("Aucune stratégie de décompression définie !");
        }
        // *** STRATEGY ICI ***
        strategy.decompress(filePath);
    }
}
```

Pour éviter de faire des `if (endsWith(".zip")) ...` partout dans le code client, on peut ajouter une **petite fabrique** (ce n’est pas obligatoire dans Strategy, mais ça rend l’exemple pédagogique) :

```java
// ===== 5) Petite factory pour choisir la bonne stratégie selon l'extension =====
public class DecompressionStrategyFactory {

    public static DecompressionStrategy forFile(String filePath) {
        if (filePath == null) {
            throw new IllegalArgumentException("Nom de fichier nul");
        }
        String lower = filePath.toLowerCase();
        if (lower.endsWith(".zip")) {
            return new ZipDecompressionStrategy();
        } else if (lower.endsWith(".rar")) {
            return new RarDecompressionStrategy();
        } else {
            throw new UnsupportedOperationException("Format non supporté : " + filePath);
        }
    }
}
```

Et enfin le **programme principal** :

```java
public class Main {
    public static void main(String[] args) {

        String f1 = "backup-2025-10-31.zip";
        String f2 = "cours-informatique.rar";

        // 1. On choisit la bonne stratégie selon le fichier
        DecompressionStrategy s1 = DecompressionStrategyFactory.forFile(f1);
        DecompressionStrategy s2 = DecompressionStrategyFactory.forFile(f2);

        // 2. On crée un contexte avec une première stratégie
        FileDecompressor decompressor = new FileDecompressor(s1);
        decompressor.decompress(f1); // --> utilisera la stratégie ZIP

        // 3. Plus tard : on change la stratégie
        decompressor.setStrategy(s2);
        decompressor.decompress(f2); // --> utilisera la stratégie RAR
    }
}
```

**Sortie attendue (simulation)** :

```text
[ZIP] Décompression de backup-2025-10-31.zip avec WinZip...
[RAR] Décompression de cours-informatique.rar avec WinRar...
```

---

# 04 – Explication claire

* Dans **Strategy**, le **contexte** (`FileDecompressor`) **ne met pas de `if`/`else`** pour savoir *comment* décompresser.

* Il dit juste :

  ```java
  strategy.decompress(filePath);
  ```

  → c’est **la ligne-clé** (comme dans ta délégation).

* Le choix de **quelle** stratégie utiliser (ZIP, RAR, autre) est déplacé **en dehors** du contexte :

  * soit dans le **client** (`main`) ;
  * soit dans une **factory** (comme `DecompressionStrategyFactory` ci-dessus) ;
  * soit dans l’UI (boutons “décompresser en ZIP”, “décompresser en RAR”).

Résultat :

1. **Famille d’algorithmes** : toutes les classes qui implémentent `DecompressionStrategy`.
2. **Interchangeables** : on peut faire `setStrategy(...)` à chaud.
3. **Évolutif** : tu ajoutes `TarGzDecompressionStrategy`, tu ne touches PAS au contexte.

C’est exactement ce que tu décrivais dans ton contexte :

> “Pas besoin de faire de `if else` ou `switch case` à l’intérieur de Context”.

---

# 05 – Context & Strategy (diagramme verbal)

* **Context** : `FileDecompressor`

  * garde une référence vers **1 strategy** (`DecompressionStrategy`)
  * expose une méthode métier : `decompress(String filePath)`
  * délègue le travail concret à la stratégie

* **Strategy (interface)** : `DecompressionStrategy`

  * définit la **forme** de l’algorithme : `void decompress(String filePath);`

* **Concrètes** :

  * `ZipDecompressionStrategy`
  * `RarDecompressionStrategy`
  * (facile d’ajouter) `TarGzDecompressionStrategy`, `SevenZipDecompressionStrategy`, etc.

* **Client** :

  * choisit la stratégie
  * la donne au contexte
  * appelle le contexte normalement

