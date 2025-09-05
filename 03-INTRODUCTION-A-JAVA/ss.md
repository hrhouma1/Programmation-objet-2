

# ÉVALUATION JAVA — HÉRITAGE & ENCAPSULATION

Durée conseillée : 90–120 min — Barème total : 100 pts

## Objectifs d’apprentissage

* Distinguer `private` et `protected` dans une hiérarchie.
* Appliquer encapsulation via getters/setters + validations.
* Utiliser héritage, `abstract`, `@Override`, `super`, `final`.
* Pratiquer polymorphisme (up/downcasting, `instanceof`) et interface.
* Expliquer vos choix (petites réponses écrites).

## Règles

* Conservez la **structure en packages** ci-dessous.
* **Commentez** toute ligne volontairement erronée (test d’accès/`final`) et **expliquez en 1–2 phrases**.
* Répondez **dans les zones “Réponse : …”** (ci-dessous) **ET/OU** directement en commentaires dans le code.
* Compiler/exécuter souvent.


### `src/app/Main.java`

```java
package app;

import school.*;
import hr.*;
import transport.*;
import bank.*;
import books.*;
import util.*;

public class Main {
    public static void main(String[] args) {
        System.out.println("=== DEMO DÉPART ===");

        Personne e = new Etudiant("Alice", 20, "E123", 85.5);
        Personne p = new Professeur("Dr. Martin", 45, "P001", "Maths", 75000);
        e.afficherInfo();
        p.afficherInfo();

        Employe med = new Medecin("Dr. Leblanc", 50, "MD-01", 80.0, 40, "Cardiologie");
        Employe inf = new Infirmier("Julie", 32, "INF-9", 40.0, 36, true);
        System.out.println("Salaire médecin = " + med.calculerSalaire());
        System.out.println("Salaire infirmier = " + inf.calculerSalaire());

        Vehicule v1 = new Voiture("Toyota", "Corolla", "Noir");
        Vehicule v2 = new Avion("AirMonde", "A320");
        v1.accelerer(15); v1.freiner(5); v1.klaxonner();
        v2.accelerer(100); ((Avion)v2).monter(1000);
        v1.afficherEtat(); v2.afficherEtat();

        CompteBancaire cb = new CompteBancaire("Sofia", "CB-001", 1500.0, 500.0);
        cb.retirer(200); cb.deposer(300); cb.afficher();
        CompteBancaire epargne = new CompteEpargne("Sam", "CE-777", 1200.0, 300.0, 0.02);
        epargne.retirer(350); ((CompteEpargne)epargne).appliquerInterets(); epargne.afficher();

        Livre papier = new Livre("Java Facile", "A. Dupont", 300);
        Livre num = new LivreNumerique("Java Pro", "B. Martin", 420, 12.5);
        papier.afficherDetails(); num.afficherDetails();

        // protected dans même projet (via util)
        Registre.afficherIdInterne((Personne) e);

        System.out.println("=== FIN DEMO ===");
    }
}
```

### `src/school/Personne.java`

```java
package school;

public abstract class Personne {
    private String nom;
    private int age;
    protected String idInterne; // accessible aux sous-classes et même package

    public Personne(String nom, int age, String idInterne) {
        setNom(nom); setAge(age);
        this.idInterne = idInterne;
    }

    public final String getNom() { return nom; }
    public final void setNom(String nom) {
        if (nom == null || nom.isBlank()) throw new IllegalArgumentException("nom invalide");
        this.nom = nom.trim();
    }
    public final int getAge() { return age; }
    public final void setAge(int age) {
        if (age < 0) throw new IllegalArgumentException("age invalide");
        this.age = age;
    }

    public abstract String role();

    public void afficherInfo() {
        System.out.println(role() + " - " + getNom() + " (" + getAge() + " ans, id=" + idInterne + ")");
    }

    protected void debugInterne() {
        System.out.println("[DEBUG] " + role() + " id=" + idInterne);
    }
}
```

### `src/school/Etudiant.java`

```java
package school;

public class Etudiant extends Personne {
    private double moyenne;

    public Etudiant(String nom, int age, String id, double moyenne) {
        super(nom, age, id);
        setMoyenne(moyenne);
    }

    public double getMoyenne() { return moyenne; }
    public void setMoyenne(double m) {
        if (m < 0 || m > 100) throw new IllegalArgumentException("moyenne invalide");
        this.moyenne = m;
    }

    @Override public String role() { return "Étudiant"; }

    public void etudier() {
        System.out.println(getNom() + " étudie (idInterne=" + idInterne + ")");
    }
    public void utiliserDebug() { debugInterne(); }
}
```

### `src/school/Professeur.java`

```java
package school;

public class Professeur extends Personne {
    private String matiere;
    protected double salaire;

    public Professeur(String nom, int age, String id, String matiere, double salaire) {
        super(nom, age, id);
        setMatiere(matiere); setSalaire(salaire);
    }

    public String getMatiere() { return matiere; }
    public void setMatiere(String m) {
        if (m == null || m.isBlank()) throw new IllegalArgumentException("matière invalide");
        this.matiere = m.trim();
    }
    public double getSalaire() { return salaire; }
    public void setSalaire(double s) {
        if (s < 0) throw new IllegalArgumentException("salaire invalide");
        this.salaire = s;
    }

    @Override public String role() { return "Professeur"; }
    public void enseigner() { System.out.println(getNom() + " enseigne " + matiere); }
}
```

### `src/hr/Payable.java`

```java
package hr;
public interface Payable { double calculerSalaire(); }
```

### `src/hr/Employe.java`

```java
package hr;

import school.Personne;

public abstract class Employe extends Personne implements Payable {
    protected double tauxHoraire;
    private int heures;

    public Employe(String nom, int age, String id, double tauxHoraire, int heures) {
        super(nom, age, id);
        setTauxHoraire(tauxHoraire); setHeures(heures);
    }

    public final void setTauxHoraire(double t) {
        if (t <= 0 || t > 200) throw new IllegalArgumentException("taux horaire invalide");
        this.tauxHoraire = t;
    }
    public final int getHeures() { return heures; }
    public final void setHeures(int h) {
        if (h < 0) throw new IllegalArgumentException("heures invalides");
        this.heures = h;
    }

    @Override public double calculerSalaire() { return tauxHoraire * getHeures(); }
    public final int primeAnciennete(int ans) { return ans * 50; }
}
```

### `src/hr/Medecin.java`

```java
package hr;

public class Medecin extends Employe {
    private String specialite;

    public Medecin(String nom, int age, String id, double taux, int heures, String specialite) {
        super(nom, age, id, taux, heures);
        setSpecialite(specialite);
    }
    public String getSpecialite() { return specialite; }
    public void setSpecialite(String s) {
        if (s == null || s.isBlank()) throw new IllegalArgumentException("spécialité invalide");
        this.specialite = s.trim();
    }
    @Override public String role() { return "Médecin"; }
    @Override public double calculerSalaire() { return super.calculerSalaire() + 500; }
    public void soigner() { System.out.println("Médecin soigne un patient"); }
}
```

### `src/hr/Infirmier.java`

```java
package hr;

public class Infirmier extends Employe {
    private boolean deNuit;
    public Infirmier(String nom, int age, String id, double taux, int heures, boolean deNuit) {
        super(nom, age, id, taux, heures);
        this.deNuit = deNuit;
    }
    public boolean isDeNuit() { return deNuit; }
    @Override public String role() { return "Infirmier"; }
    @Override public double calculerSalaire() {
        double base = super.calculerSalaire();
        return deNuit ? base * 1.2 : base;
    }
    public void soigner() { System.out.println("Infirmier soigne un patient"); }
}
```

### `src/transport/Vehicule.java`

```java
package transport;

public abstract class Vehicule {
    private String marque;
    private String modele;
    protected int vitesse;

    public Vehicule(String marque, String modele) { setMarque(marque); setModele(modele); }

    public final String getMarque() { return marque; }
    public final void setMarque(String m) {
        if (m == null || m.isBlank()) throw new IllegalArgumentException("marque invalide");
        this.marque = m.trim();
    }
    public final String getModele() { return modele; }
    public final void setModele(String m) {
        if (m == null || m.isBlank()) throw new IllegalArgumentException("modèle invalide");
        this.modele = m.trim();
    }

    public void accelerer(int dv) { vitesse += Math.max(0, dv); }
    public void freiner(int dv) { vitesse = Math.max(0, vitesse - Math.max(0, dv)); }
    public final void klaxonner() { System.out.println("BEEP!"); }

    public void afficherEtat() {
        System.out.println(getClass().getSimpleName() + " " + getMarque() + " " + getModele() + " (v=" + vitesse + ")");
    }
}
```

### `src/transport/Voiture.java`

```java
package transport;

public class Voiture extends Vehicule {
    private String couleur;
    public Voiture(String marque, String modele, String couleur) {
        super(marque, modele);
        setCouleur(couleur);
    }
    public String getCouleur() { return couleur; }
    public void setCouleur(String c) {
        if (c == null || c.isBlank()) throw new IllegalArgumentException("couleur invalide");
        this.couleur = c.trim();
    }
    @Override public void accelerer(int dv) {
        super.accelerer(dv);
        if (vitesse > 200) vitesse = 200;
    }
}
```

### `src/transport/Avion.java`

```java
package transport;

public class Avion extends Vehicule {
    protected int altitude;
    public Avion(String marque, String modele) { super(marque, modele); }
    public void monter(int da) { altitude += Math.max(0, da); }
    public void descendre(int da) { altitude = Math.max(0, altitude - Math.max(0, da)); }
    @Override public void freiner(int dv) {
        super.freiner(dv);
        if (dv > 50) altitude = Math.max(0, altitude - 100);
    }
    @Override public void afficherEtat() {
        System.out.println("Avion " + getMarque() + " " + getModele() + " (v=" + vitesse + ", alt=" + altitude + ")");
    }
}
```

### `src/bank/CompteBancaire.java`

```java
package bank;

public class CompteBancaire {
    private String titulaire;
    private String numero;
    private double solde;
    protected double plafondRetrait;

    public CompteBancaire(String titulaire, String numero, double solde, double plafondRetrait) {
        setTitulaire(titulaire); setNumero(numero);
        this.solde = Math.max(0, solde);
        this.plafondRetrait = Math.max(0, plafondRetrait);
    }

    public final String getTitulaire() { return titulaire; }
    public final void setTitulaire(String t) {
        if (t == null || t.isBlank()) throw new IllegalArgumentException("titulaire invalide");
        this.titulaire = t.trim();
    }
    public final String getNumero() { return numero; }
    public final void setNumero(String n) {
        if (n == null || n.isBlank()) throw new IllegalArgumentException("numéro invalide");
        this.numero = n.trim();
    }
    public final double getSolde() { return solde; }

    public void deposer(double m) { if (m > 0) solde += m; }
    public boolean retirer(double m) {
        if (m <= 0 || m > plafondRetrait || m > solde) return false;
        solde -= m; return true;
    }
    protected void crediter(double m) { if (m > 0) solde += m; }

    public void afficher() {
        System.out.println("Compte " + numero + " - " + titulaire + " | solde=" + solde);
    }
}
```

### `src/bank/CompteEpargne.java`

```java
package bank;

public class CompteEpargne extends CompteBancaire {
    private double tauxInteret;
    public CompteEpargne(String titulaire, String numero, double solde, double plafond, double taux) {
        super(titulaire, numero, solde, plafond);
        setTauxInteret(taux);
    }
    public void setTauxInteret(double t) {
        if (t < 0) throw new IllegalArgumentException("taux invalide");
        this.tauxInteret = t;
    }
    @Override public boolean retirer(double m) {
        if (m > (plafondRetrait / 2)) return false;
        boolean ok = super.retirer(m);
        if (ok && m > 100) crediter(-2); // frais
        return ok;
    }
    public void appliquerInterets() { crediter(getSolde() * tauxInteret); }
}
```

### `src/books/Livre.java`

```java
package books;

public class Livre {
    private String titre;
    private String auteur;
    private int pages;

    public Livre(String titre, String auteur, int pages) {
        setTitre(titre); setAuteur(auteur); setPages(pages);
    }
    public void setTitre(String t) {
        if (t == null || t.isBlank()) throw new IllegalArgumentException("titre invalide");
        this.titre = t.trim();
    }
    public void setAuteur(String a) {
        if (a == null || a.isBlank()) throw new IllegalArgumentException("auteur invalide");
        this.auteur = a.trim();
    }
    public void setPages(int p) {
        if (p <= 0 || p > 2000) throw new IllegalArgumentException("pages invalides");
        this.pages = p;
    }
    public void afficherDetails() {
        System.out.println("Livre papier : " + titre + " - " + auteur + " (" + pages + "p)");
    }
}
```

### `src/books/LivreNumerique.java`

```java
package books;

public class LivreNumerique extends Livre {
    private double tailleMo;
    public LivreNumerique(String titre, String auteur, int pages, double tailleMo) {
        super(titre, auteur, pages);
        setTailleMo(tailleMo);
    }
    public void setTailleMo(double t) {
        if (t <= 0) throw new IllegalArgumentException("taille invalide");
        this.tailleMo = t;
    }
    @Override public void afficherDetails() {
        System.out.println("eBook : (taille ~ " + tailleMo + " Mo)");
        super.afficherDetails();
    }
}
```

### `src/util/Registre.java`

```java
package util;

import school.Personne;

public class Registre {
    public static void afficherIdInterne(Personne p) {
        System.out.println("[Registre] idInterne=" + p.idInterne);
    }
}
```

---

## EXERCICES — “FAIS ÇA…” (avec espaces pour répondre)

> Indiquez vos réponses sous chaque item. Quand on vous demande de **provoquer une erreur**, commentez la/les ligne(s) fautives et **expliquez en 1–2 phrases**.

### A) Encapsulation & accès (20 pts)

1. **Lecture seule de `moyenne`** dans `Etudiant` : supprimez le setter, adaptez si besoin.
   *Preuve (extrait de code/erreur de compilation si on tente de modifier) + explication :*
   Réponse :
   ………………………………………………………………………………………………
   ………………………………………………………………………………………………

2. Dans `Professeur`, passez `salaire` de `protected` à **`private`** et utilisez getters/setters.
   *Expliquez pourquoi une sous-classe n’y accède plus directement :*
   Réponse :
   ………………………………………………………………………………………………
   ………………………………………………………………………………………………

3. Dans `Main`, tentez d’accéder directement à `cb.solde`. Doit échouer. **Commentez** la ligne.
   *Message/raison (private) :*
   Réponse :
   ………………………………………………………………………………………………
   ………………………………………………………………………………………………

4. Créez `util/Audit.java` lisant `idInterne` d’une `Personne`. Expliquez pourquoi c’est autorisé.
   Réponse :
   ………………………………………………………………………………………………
   ………………………………………………………………………………………………

5. Passez `idInterne` en **`private`** dans `Personne`. Montrez que `Audit` ne compile plus.
   *Différence `protected` vs `private` :*
   Réponse :
   ………………………………………………………………………………………………
   ………………………………………………………………………………………………

---

### B) Héritage, override, super, final (20 pts)

6. Créez `school/Assistant.java` héritant de `Professeur`, surchargez `enseigner()` (“TD dirigé”).
   *Preuve d’appel polymorphe depuis `Main` :*
   Réponse :
   ………………………………………………………………………………………………

7. Marquez `accelerer` **`final`** dans `Vehicule`. Essayez de la surcharger dans `Voiture` (échec attendu).
   *Collez la ligne commentée + expliquez :*
   Réponse :
   ………………………………………………………………………………………………

8. Créez `transport/Moto.java` (hérite de `Vehicule`) + booléen `abs`. Testez accélération/freinage.
   *Extrait de code + sortie attendue :*
   Réponse :
   ………………………………………………………………………………………………

9. Dans `Avion.freiner(int)`, si `dv > 50`, diminuez aussi `altitude` de 100 (déjà présent : vérifiez).
   *Capture/sortie démontrant l’effet :*
   Réponse :
   ………………………………………………………………………………………………

10. Dans `LivreNumerique.afficherDetails()`, expliquez l’intérêt d’appeler `super.afficherDetails()`.
    Réponse :
    ………………………………………………………………………………………………

---

### C) Polymorphisme, up/downcasting (20 pts)

11. Placez `Etudiant`, `Professeur`, `Medecin`, `Infirmier` dans un `Personne[]` et bouclez `afficherInfo()`.
    *Extrait + brève explication “late binding” :*
    Réponse :
    ………………………………………………………………………………………………

12. Appelez `etudier()` **seulement** pour les `Etudiant` (via `instanceof`).
    *Extrait + sortie :*
    Réponse :
    ………………………………………………………………………………………………

13. **Upcasting** : `Personne px = new Medecin(...); px.afficherInfo();`
    *Pourquoi la version “Médecin” est appelée ?*
    Réponse :
    ………………………………………………………………………………………………

14. **Downcasting** légal vers `Medecin` puis appel `calculerSalaire()`. Ajoutez un downcasting **illégal** (attrapez `ClassCastException`) et commentez.
    *Extrait + explication :*
    Réponse :
    ………………………………………………………………………………………………

15. Créez une interface `Soignant { void soigner(); }`, faites-la implémenter par `Medecin` et `Infirmier`. Parcourez une liste de `Soignant` et appelez `soigner()`.
    *Extrait + sortie :*
    Réponse :
    ………………………………………………………………………………………………

---

### D) Conception & validations (20 pts)

16. Montrez un test qui **rejette** `tauxHoraire > 200` (try/catch dans `Main`).
    *Extrait + message :*
    Réponse :
    ………………………………………………………………………………………………

17. Prouvez que `CompteEpargne` **refuse** un retrait `> plafondRetrait/2`.
    *Extrait + résultat :*
    Réponse :
    ………………………………………………………………………………………………

18. Ajoutez `toString()` (au choix : dans `Personne` ou surcharges) et comparez avec `afficherInfo()`.
    *Extrait + différence d’usage :*
    Réponse :
    ………………………………………………………………………………………………

19. Déclenchez l’exception `pages` > 2000 dans `Livre`.
    *Extrait + message :*
    Réponse :
    ………………………………………………………………………………………………

20. En 3–4 phrases : **quand** mettre `final` sur une méthode publique ?
    Réponse :
    ………………………………………………………………………………………………
    ………………………………………………………………………………………………

---

### E) Protected en profondeur (10 pts)

21. Créez `ChefDeService extends Medecin` lisant `idInterne` pour un log interne. Montrez que l’accès fonctionne en sous-classe.
    *Extrait :*
    Réponse :
    ………………………………………………………………………………………………

22. Expliquez la règle **protected + autre package** (accès uniquement via héritage et instance courante).
    Réponse :
    ………………………………………………………………………………………………



### F) Questions rapides (10 pts)

23. Encapsulation **vs** Abstraction (3–4 lignes, exemples du code).
    Réponse :
    ………………………………………………………………………………………………

24. Pourquoi **éviter les champs publics** ? Donnez 3 raisons.
    Réponse :
    ………………………………………………………………………………………………

25. 2 raisons d’utiliser `abstract` plutôt qu’une classe concrète.
    Réponse :
    ………………………………………………………………………………………………

26. 2 cas où **interface** est préférable à une classe de base.
    Réponse :
    ………………………………………………………………………………………………



## Rendu & barème (indicatif)

* A–D : 70 pts (preuves + explications concises).
* E : 10 pts.
* F : 10 pts.
* +10 pts bonus si code propre (nommage, `@Override`, messages clairs), et si **toutes** les erreurs attendues sont bien **commentées** + justifiées.


### Astuces pédagogiques

* Compiler souvent ; isolez chaque modif.
* Toujours **valider** en getter/setter (invariants).
* `@Override` partout où vous surchargez.
* Pour les tests d’échec, utilisez `try/catch` et imprimez un message clair.

