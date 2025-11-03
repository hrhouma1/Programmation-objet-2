# STRATEGY - SYNOPSIS

* **Problème** : j’ai **plusieurs façons** de faire la même chose.

  * ex. décompresser en **ZIP**
  * ou décompresser en **RAR**
  * ou afficher un graphique **barres** ou **lignes**
  * ou calculer un **rabais étudiant** ou **rabais entreprise**

* **Ce que je ne veux PAS** : écrire partout

  ```java
  if (type == "ZIP") { ... }
  else if (type == "RAR") { ... }
  else if (...)
  ```

  parce que ça devient mal organisé, long, et à chaque nouveau cas je dois rouvrir le fichier.

* **Strategy**, c’est l’idée suivante :

  > “Je vais **définir une forme** de l’algorithme (une interface),
  > et **brancher** dessus la bonne version au bon moment.”

  Donc :

  1. Je définis **la forme** :

     ```java
     interface DecompressionStrategy {
         void decompress(String file);
     }
     ```
  2. Je fais **une classe par façon de faire** :

     * `ZipDecompressionStrategy`
     * `RarDecompressionStrategy`
  3. Dans mon code principal, je dis juste :

     ```java
     strategy.decompress(file);
     ```

     → je ne sais pas **comment** ça marche, je sais juste que “c’est une stratégie”.

* **Image** :
  C’est comme **une prise électrique** (le contexte) et **plusieurs chargeurs** (les stratégies).
  La prise dit : “donnez-moi un chargeur compatible”, et ensuite elle l’utilise.
  Elle ne réécrit pas son code pour chaque marque de chargeur.

* **Phrase à retenir** :

  > Strategy = **je ne change pas le code qui utilise l’algorithme, je change juste l’algorithme.**

<br/>

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

<br/>

# 02 – Résumé

| Élément                    | Rôle                                                                 |
| -------------------------- | -------------------------------------------------------------------- |
| `DecompressionStrategy`    | **Interface Strategy** : décrit l’opération `decompress(...)`.       |
| `ZipDecompressionStrategy` | Stratégie concrète pour `.zip` (WinZip).                             |
| `RarDecompressionStrategy` | Stratégie concrète pour `.rar` (WinRar).                             |
| `FileDecompressor`         | **Context** : tient une stratégie et l’invoque.                      |
| Ligne clé                  | `strategy.decompress(filePath);` → c’est là que le **Strategy agit** |

> **Idée clef** : on ne change pas le code du contexte pour ajouter un nouveau format, on ajoute une **nouvelle stratégie**.

<br/>

# 03 – Correction (code complet)


```java
// Fichier: App.java
public class App {

    // 1) STRATEGY : la "forme" de l'algorithme
    interface DecompressionStrategy {
        void decompress(String filePath);
    }
```

```java
    // 2) Stratégie .zip
    static class ZipDecompressionStrategy implements DecompressionStrategy {
        @Override
        public void decompress(String filePath) {
            System.out.println("[ZIP] Décompression de " + filePath + " avec WinZip...");
            // Appel lib ZIP réel ici si besoin
        }
    }
```

```java
    // 3) Stratégie .rar
    static class RarDecompressionStrategy implements DecompressionStrategy {
        @Override
        public void decompress(String filePath) {
            System.out.println("[RAR] Décompression de " + filePath + " avec WinRar...");
            // Appel lib RAR réel ici si besoin
        }
    }
```
```java
    // 4) CONTEXTE : ne connaît pas les détails, délègue à la stratégie
    static class FileDecompressor {
        private DecompressionStrategy strategy;

        public FileDecompressor(DecompressionStrategy strategy) {
            this.strategy = strategy;
        }

        public void setStrategy(DecompressionStrategy strategy) {
            this.strategy = strategy;
        }

        public void decompress(String filePath) {
            if (strategy == null) {
                throw new IllegalStateException("Aucune stratégie de décompression définie !");
            }
            strategy.decompress(filePath); // <<< STRATEGY ici
        }
    }

    // 5) Démo minimale : choix "local" de la stratégie (2 if), sans Factory
    public static void main(String[] args) {
        String f1 = "backup-2025-10-31.zip";
        String f2 = "cours-informatique.rar";

        DecompressionStrategy s1 = chooseStrategy(f1);
        DecompressionStrategy s2 = chooseStrategy(f2);

        FileDecompressor decompressor = new FileDecompressor(s1);
        decompressor.decompress(f1);      // -> ZIP

        decompressor.setStrategy(s2);
        decompressor.decompress(f2);      // -> RAR
    }

    // Helper **local** (pas un patron Factory) : centralise 2 if au même endroit
    private static DecompressionStrategy chooseStrategy(String filePath) {
        String lower = filePath.toLowerCase();
        if (lower.endsWith(".zip")) return new ZipDecompressionStrategy();
        if (lower.endsWith(".rar")) return new RarDecompressionStrategy();
        throw new UnsupportedOperationException("Format non supporté : " + filePath);
    }
}
```


**Sortie attendue (simulation)** :

```text
[ZIP] Décompression de backup-2025-10-31.zip avec WinZip...
[RAR] Décompression de cours-informatique.rar avec WinRar...
```

> **Pourquoi c’est simple :** le **Context** reste propre (`strategy.decompress(file)`),
> et tu centralises un tout petit choix (2 `if`) dans une seule méthode.

<br/>

### 03 bis – (Facultatif, à ignorer pour le moment) Mini-Factory (plus tard dans le cours)

> **Tu peux ignorer cette section.** Elle n’est là que pour montrer l’évolution naturelle si, un jour, tu veux **retirer** même ces deux `if` du `main`.

```java
// Option FACULTATIVE: petite Factory (peut être dans le même fichier)
class DecompressionStrategyFactory {
    public static App.DecompressionStrategy forFile(String filePath) {
        String lower = filePath.toLowerCase();
        if (lower.endsWith(".zip")) return new App.ZipDecompressionStrategy();
        if (lower.endsWith(".rar")) return new App.RarDecompressionStrategy();
        throw new UnsupportedOperationException("Format non supporté : " + filePath);
    }
}
```

<br/>

# 04 – Explication claire

* Dans **Strategy**, le **contexte** (`FileDecompressor`) **ne met pas de `if/else`** pour savoir *comment* décompresser.

* Il dit juste :

  ```java
  strategy.decompress(filePath);
  ```

  → c’est **la ligne-clé** (comme dans ta délégation).

* **Ici**, le choix de **quelle** stratégie utiliser (ZIP, RAR) est fait **localement** par `chooseStrategy(...)` (deux `if` au même endroit, pas de Factory).
  Si un jour tu veux éliminer ces `if`, tu pourras **remplacer** `chooseStrategy` par la **Factory facultative** ci-dessus — **sans toucher** au Context.

Résultat :

1. **Famille d’algorithmes** : toutes les classes qui implémentent `DecompressionStrategy`.
2. **Interchangeables** : on peut faire `setStrategy(...)` à chaud.
3. **Évolutif** : tu ajoutes `TarGzDecompressionStrategy`, tu **ne touches pas** à `FileDecompressor` (seulement `chooseStrategy` → 1 ligne).

<br/>

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

  * choisit la stratégie (ici via `chooseStrategy(...)`)
  * la donne au contexte
  * appelle le contexte normalement


<br/>


### Tous le code

```java
// Fichier: App.java
public class App {

    // 1) STRATEGY: forme de l'algorithme
    interface DecompressionStrategy {
        void decompress(String filePath);
    }

    // 2) Stratégie .zip
    static class ZipDecompressionStrategy implements DecompressionStrategy {
        @Override
        public void decompress(String filePath) {
            System.out.println("[ZIP] Décompression de " + filePath + " avec WinZip...");
            // Ici tu appellerais la lib ZIP réelle
        }
    }

    // 3) Stratégie .rar
    static class RarDecompressionStrategy implements DecompressionStrategy {
        @Override
        public void decompress(String filePath) {
            System.out.println("[RAR] Décompression de " + filePath + " avec WinRar...");
            // Ici tu appellerais la lib RAR réelle
        }
    }

    // 4) CONTEXTE: ne connaît pas les détails, délègue à la stratégie
    static class FileDecompressor {
        private DecompressionStrategy strategy;

        public FileDecompressor(DecompressionStrategy strategy) {
            this.strategy = strategy;
        }

        public void setStrategy(DecompressionStrategy strategy) {
            this.strategy = strategy;
        }

        public void decompress(String filePath) {
            if (strategy == null) {
                throw new IllegalStateException("Aucune stratégie de décompression définie !");
            }
            strategy.decompress(filePath); // <-- STRATEGY ici
        }
    }

    // 5) Demo minimale: on choisit la stratégie avec 2 if (pas de Factory)
    public static void main(String[] args) {
        String f1 = "backup-2025-10-31.zip";
        String f2 = "cours-informatique.rar";

        // Choix simple (et centralisé) de la stratégie
        DecompressionStrategy s1 = chooseStrategy(f1);
        DecompressionStrategy s2 = chooseStrategy(f2);

        FileDecompressor decompressor = new FileDecompressor(s1);
        decompressor.decompress(f1);      // -> ZIP

        decompressor.setStrategy(s2);
        decompressor.decompress(f2);      // -> RAR
    }

    // Petit helper local (pas une “Factory” au sens patron) : juste 2 if
    private static DecompressionStrategy chooseStrategy(String filePath) {
        String lower = filePath.toLowerCase();
        if (lower.endsWith(".zip")) return new ZipDecompressionStrategy();
        if (lower.endsWith(".rar")) return new RarDecompressionStrategy();
        throw new UnsupportedOperationException("Format non supporté : " + filePath);
    }
}
```


