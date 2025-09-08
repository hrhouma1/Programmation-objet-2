# Exemple"

- Je vous propose **un exemple simple et complet “de A à Z” uniquement sur les *interfaces***.




## `Main.java` — Interfaces faciles + 20 classes d’exemples

```java
// Main.java
// Un seul fichier : interfaces simples + 20 classes indépendantes + Main.
// Compile : javac Main.java
// Exécute : java Main

// =====================
// 0) INTERFACES SIMPLES
// =====================

interface Affichable {
    void afficherInfo();
}

interface Payant {
    double prixHT();
    // Méthode par défaut : ajoute 5% de taxe (ex. TPS simplifiée)
    default double prixTTC() { return prixHT() * 1.05; }
    // Méthode statique utilitaire
    static double ttc(double ht) { return ht * 1.05; }
}

interface Deplacable {
    void deplacer(); // ex: rouler, marcher, etc.
}

interface Soignable {
    void soigner(); // ex: hôpital soigne, médecin soigne
}

interface Vendable {
    void vendre(int n); // vend n unités
}

interface Rechargeable {
    void recharger(); // ex: téléphone, voiture électrique, avion, ordi
}

interface Volant {
    void decoller();
    void atterrir();
}

interface Stockable {
    int getStock();
    void ajouterStock(int n);
    void retirerStock(int n);
}


// =====================
// 1) CLASSES D'EXEMPLE
// =====================

// 1) Etudiant
class Etudiant implements Affichable {
    String nom = "Alice";
    int age = 20;
    String programme = "Informatique";
    @Override public void afficherInfo() {
        System.out.println("Etudiant: " + nom + " (" + age + " ans), " + programme);
    }
}

// 2) Professeur
class Professeur implements Affichable {
    String nom = "Dr. Martin";
    String matiere = "Mathématiques";
    @Override public void afficherInfo() {
        System.out.println("Professeur: " + nom + ", matière=" + matiere);
    }
}

// 3) Hopital
class Hopital implements Affichable, Soignable {
    String nom = "Hôpital Général";
    int patients = 100;
    @Override public void soigner() { System.out.println(nom + " soigne un patient."); patients++; }
    @Override public void afficherInfo() { System.out.println("Hopital: " + nom + ", patients=" + patients); }
}

// 4) Voiture
class Voiture implements Affichable, Deplacable, Rechargeable, Vendable {
    String marque = "Toyota";
    String modele = "Corolla";
    int vitesse = 0;
    int stock = 3;     // pour l'exemple de Vendable
    boolean batterie = false; // simulons une hybride à recharger
    @Override public void deplacer() { vitesse += 10; System.out.println("La voiture roule à " + vitesse + " km/h."); }
    @Override public void recharger() { batterie = true; System.out.println("Voiture rechargée."); }
    @Override public void vendre(int n) { if (stock >= n) { stock -= n; System.out.println("Vendu " + n + " voiture(s)."); } }
    @Override public void afficherInfo() { System.out.println("Voiture: " + marque + " " + modele + " (stock=" + stock + ")"); }
}

// 5) Medecin
class Medecin implements Affichable, Soignable {
    String nom = "Dr. Leblanc";
    String specialite = "Cardiologie";
    @Override public void soigner() { System.out.println(nom + " soigne un patient."); }
    @Override public void afficherInfo() { System.out.println("Médecin: " + nom + " (" + specialite + ")"); }
}

// 6) Infirmier
class Infirmier implements Affichable, Soignable {
    String nom = "Julie";
    String service = "Urgence";
    @Override public void soigner() { System.out.println(nom + " administre des soins au service " + service + "."); }
    @Override public void afficherInfo() { System.out.println("Infirmier: " + nom + ", service=" + service); }
}

// 7) Patient
class Patient implements Affichable {
    String nom = "Marc";
    String dossier = "DOS-555";
    @Override public void afficherInfo() { System.out.println("Patient: " + nom + " (" + dossier + ")"); }
}

// 8) Livre
class Livre implements Affichable, Payant, Vendable, Stockable {
    String titre = "Java Facile";
    double prix = 30.0;
    int stock = 10;
    @Override public double prixHT() { return prix; }
    @Override public void vendre(int n) { if (stock >= n) { stock -= n; System.out.println("Vendu " + n + " livre(s)."); } }
    @Override public int getStock() { return stock; }
    @Override public void ajouterStock(int n) { stock += n; }
    @Override public void retirerStock(int n) { stock = Math.max(0, stock - n); }
    @Override public void afficherInfo() { System.out.println("Livre: " + titre + " - " + prix + " $ HT (stock=" + stock + ")"); }
}

// 9) Bibliotheque
class Bibliotheque implements Affichable {
    String nom = "Biblio Centrale";
    @Override public void afficherInfo() { System.out.println("Bibliothèque: " + nom); }
}

// 10) Animal
class Animal implements Affichable, Deplacable {
    String espece = "Chien";
    String nom = "Rex";
    @Override public void deplacer() { System.out.println(nom + " marche."); }
    @Override public void afficherInfo() { System.out.println("Animal: " + espece + " (" + nom + ")"); }
}

// 11) Maison
class Maison implements Affichable {
    String adresse = "12 Rue des Pins";
    int pieces = 5;
    @Override public void afficherInfo() { System.out.println("Maison: " + adresse + " (" + pieces + " pièces)"); }
}

// 12) Ecole
class Ecole implements Affichable {
    String nom = "École Poly";
    @Override public void afficherInfo() { System.out.println("École: " + nom); }
}

// 13) Magasin
class Magasin implements Affichable, Vendable, Stockable {
    String nom = "TechShop";
    int stock = 100;
    @Override public void vendre(int n) { if (stock >= n) { stock -= n; System.out.println("Magasin : vendu " + n + " article(s)."); } }
    @Override public int getStock() { return stock; }
    @Override public void ajouterStock(int n) { stock += n; }
    @Override public void retirerStock(int n) { stock = Math.max(0, stock - n); }
    @Override public void afficherInfo() { System.out.println("Magasin: " + nom + " (stock=" + stock + ")"); }
}

// 14) Produit
class Produit implements Affichable, Payant, Vendable, Stockable {
    String nom = "Casque Audio";
    double prix = 79.99;
    int stock = 50;
    @Override public double prixHT() { return prix; }
    @Override public void vendre(int n) { if (stock >= n) { stock -= n; System.out.println("Produit : vendu " + n + " unité(s)."); } }
    @Override public int getStock() { return stock; }
    @Override public void ajouterStock(int n) { stock += n; }
    @Override public void retirerStock(int n) { stock = Math.max(0, stock - n); }
    @Override public void afficherInfo() { System.out.println("Produit: " + nom + " - " + prix + " $ HT (stock=" + stock + ")"); }
}

// 15) Ordinateur
class Ordinateur implements Affichable, Rechargeable, Payant {
    String modele = "ThinkPad";
    double prix = 1200.0;
    boolean charge = false;
    @Override public void recharger() { charge = true; System.out.println("Ordinateur rechargé."); }
    @Override public double prixHT() { return prix; }
    @Override public void afficherInfo() { System.out.println("Ordinateur: " + modele + " - " + prix + " $ HT"); }
}

// 16) Telephone
class Telephone implements Affichable, Rechargeable, Payant {
    String modele = "S22";
    double prix = 999.0;
    boolean charge = true;
    @Override public void recharger() { charge = true; System.out.println("Téléphone rechargé."); }
    @Override public double prixHT() { return prix; }
    @Override public void afficherInfo() { System.out.println("Téléphone: " + modele + " - " + prix + " $ HT"); }
}

// 17) Avion
class Avion implements Affichable, Volant, Deplacable, Rechargeable {
    String modele = "A320";
    boolean enVol = false;
    @Override public void decoller() { enVol = true; System.out.println("Avion décolle."); }
    @Override public void atterrir() { enVol = false; System.out.println("Avion atterrit."); }
    @Override public void deplacer() { System.out.println("Avion roule sur le tarmac."); }
    @Override public void recharger() { System.out.println("Avion ravitaillé/énergie prête."); }
    @Override public void afficherInfo() { System.out.println("Avion: " + modele + " (enVol=" + enVol + ")"); }
}

// 18) Banque
class Banque implements Affichable {
    String nom = "Banque Centrale";
    @Override public void afficherInfo() { System.out.println("Banque: " + nom); }
}

// 19) ServiceNettoyage (ex. service Payant)
class ServiceNettoyage implements Affichable, Payant {
    String nom = "Nettoyage Express";
    double tarif = 80.0;
    @Override public double prixHT() { return tarif; }
    @Override public void afficherInfo() { System.out.println("Service: " + nom + " - " + tarif + " $ HT"); }
}

// 20) Cinema
class Cinema implements Affichable, Vendable, Stockable {
    String nom = "Cinéma Lux";
    int stockBillets = 200;
    @Override public void vendre(int n) { 
        if (stockBillets >= n) { stockBillets -= n; System.out.println("Billets vendus: " + n); }
    }
    @Override public int getStock() { return stockBillets; }
    @Override public void ajouterStock(int n) { stockBillets += n; }
    @Override public void retirerStock(int n) { stockBillets = Math.max(0, stockBillets - n); }
    @Override public void afficherInfo() { System.out.println("Cinéma: " + nom + " (billets restants=" + stockBillets + ")"); }
}


// =====================
// 2) PROGRAMME PRINCIPAL
// =====================

public class Main {
    public static void main(String[] args) {
        System.out.println("=== Démo Interfaces (simple et complète) ===");

        // A) Instances variées
        Etudiant e = new Etudiant();
        Professeur p = new Professeur();
        Hopital h = new Hopital();
        Voiture v = new Voiture();
        Medecin m = new Medecin();
        Infirmier inf = new Infirmier();
        Patient pat = new Patient();
        Livre li = new Livre();
        Bibliotheque biblio = new Bibliotheque();
        Animal ani = new Animal();
        Maison ms = new Maison();
        Ecole ec = new Ecole();
        Magasin mg = new Magasin();
        Produit pr = new Produit();
        Ordinateur ordi = new Ordinateur();
        Telephone tel = new Telephone();
        Avion av = new Avion();
        Banque bk = new Banque();
        ServiceNettoyage sn = new ServiceNettoyage();
        Cinema cn = new Cinema();

        // B) 1. Polymorphisme par INTERFACE: Affichable
        System.out.println("\n-- POLYMORPHISME: Affichable --");
        Affichable[] affichables = {
            e,p,h,v,m,inf,pat,li,biblio,ani,ms,ec,mg,pr,ordi,tel,av,bk,sn,cn
        };
        for (Affichable a : affichables) a.afficherInfo();

        // C) 2. Méthodes spécifiques à certaines interfaces
        System.out.println("\n-- Deplacable / Rechargeable / Volant / Soignable --");
        v.deplacer();
        ani.deplacer();
        av.deplacer(); av.decoller(); av.atterrir();
        tel.recharger();
        ordi.recharger();
        h.soigner();
        m.soigner();
        inf.soigner();

        // D) 3. Vendable + Stockable (simples)
        System.out.println("\n-- Vendable & Stockable --");
        li.vendre(2);  // vend 2 livres
        pr.vendre(5);  // vend 5 produits
        cn.vendre(30); // vend 30 billets
        mg.vendre(10); // vend 10 articles

        // E) 4. Payant : default prixTTC() + static ttc()
        System.out.println("\n-- Payant (default + static) --");
        Payant[] payants = { li, pr, ordi, tel, sn };
        double totalHT = 0, totalTTC = 0;
        for (Payant x : payants) {
            totalHT  += x.prixHT();
            totalTTC += x.prixTTC(); // appelle la méthode par défaut
        }
        System.out.println("Total HT = " + totalHT);
        System.out.println("Total TTC (default) = " + totalTTC);
        System.out.println("Total TTC (static util) = " + Payant.ttc(totalHT));

        System.out.println("\n=== Fin de la démo ===");
    }
}
```



## Comment utiliser

1. Sauvegarde ce bloc dans un fichier **`Main.java`**.
2. Compile :

```bash
javac Main.java
```

3. Exécute :

```bash
java Main
```



## Ce que vous allez apprendre : 

* **Polymorphisme** via `Affichable` : un **même tableau d’interface** contient 20 types différents, et chaque objet répond à `afficherInfo()` selon **sa propre** classe.
* **Plusieurs interfaces par classe** : par exemple `Voiture` implémente `Affichable`, `Deplacable`, `Rechargeable`, `Vendable`.
* **Méthode `default`** (dans `Payant`) : `prixTTC()` fonctionne **sans** être réécrite dans chaque classe payante.
* **Méthode `static`** (dans `Payant`) : `Payant.ttc(...)` calcule la taxe **sans instance**.
* **Interfaces spécialisées** : `Vendable`, `Stockable`, `Soignable`, `Deplacable`, `Volant`, `Rechargeable`… faciles à comprendre et à tester séparément.



## Mini-exercices (à faire dans `main` pour s’entraîner)

1. Ajoute une classe `Snack` qui implémente `Affichable`, `Payant`, `Vendable`, `Stockable` (prix unitaire 3.5\$, stock 40).
2. Dans `Main`, crée un `Snack`, affiche ses infos, vends 3 unités, affiche **HT/TTC**.
3. Ajoute une interface `Reservable` avec `void reserver(int n)`. Fais que `Cinema` implémente aussi `Reservable` et réserve 10 billets.
4. Crée un tableau `Deplacable[]` contenant `Voiture`, `Animal`, `Avion` et appelle `deplacer()` sur chaque élément.
5. Change la taxe du `Payant.prixTTC()` (par exemple 15%) : fais une **seconde interface `PayantQC`** qui étend `Payant` et **redéfinit** la méthode `default prixTTC()` à 1.14975. Fais une classe `ProduitQC` qui l’implémente, et compare les résultats avec `Produit`.

