## Exemple d’exercice Java : 20 classes simples et indépendantes

### 1. Main.java

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("=== Programme principal démarré ===");

        Etudiant e = new Etudiant();
        e.afficherInfo();

        Professeur p = new Professeur();
        p.afficherInfo();

        Hopital h = new Hopital();
        h.afficherInfo();

        Voiture v = new Voiture();
        v.afficherInfo();

        System.out.println("=== Programme terminé ===");
    }
}
```



### 2. Exemple de classes



```java
// Main.java
// Un seul fichier contenant 20 classes indépendantes + la classe Main (porte d'entrée).
// Compile: javac Main.java
// Exécute : java Main

public class Main {
    public static void main(String[] args) {
        System.out.println("=== Programme principal démarré ===");

        Etudiant e = new Etudiant(); e.afficherInfo();
        Professeur p = new Professeur(); p.afficherInfo();
        Hopital h = new Hopital(); h.afficherInfo();
        Voiture v = new Voiture(); v.afficherInfo();
        Medecin m = new Medecin(); m.afficherInfo();
        Infirmier inf = new Infirmier(); inf.afficherInfo();
        Patient pat = new Patient(); pat.afficherInfo();
        Livre li = new Livre(); li.afficherInfo();
        Bibliotheque biblio = new Bibliotheque(); biblio.afficherInfo();
        Animal ani = new Animal(); ani.afficherInfo();
        Maison ms = new Maison(); ms.afficherInfo();
        Ecole ec = new Ecole(); ec.afficherInfo();
        Magasin mg = new Magasin(); mg.afficherInfo();
        Produit pr = new Produit(); pr.afficherInfo();
        Ordinateur ordi = new Ordinateur(); ordi.afficherInfo();
        Telephone tel = new Telephone(); tel.afficherInfo();
        Avion av = new Avion(); av.afficherInfo();
        Banque bk = new Banque(); bk.afficherInfo();
        CompteBancaire cb = new CompteBancaire(); cb.afficherInfo();
        Cinema cn = new Cinema(); cn.afficherInfo();

        System.out.println("=== Programme terminé ===");
    }
}

// 1) Etudiant
class Etudiant {
    String nom = "Alice";
    int age = 20;
    String programme = "Informatique";
    double moyenne = 85.5;
    String matricule = "E123";
    String email = "alice@mail.com";
    boolean inscrit = true;
    int credits = 30;
    String adresse = "123 Rue Principale";
    String telephone = "514-123-4567";

    void etudier(){ System.out.println(nom + " étudie."); }
    void dormir(){ System.out.println(nom + " dort."); }
    void manger(){ System.out.println(nom + " mange."); }
    void assisterCours(){ System.out.println(nom + " assiste au cours."); }
    void faireExamen(){ System.out.println(nom + " fait un examen."); }
    void sInscrire(){ inscrit = true; }
    void seDesinscrire(){ inscrit = false; }
    void envoyerEmail(){ System.out.println("Email envoyé à " + email); }
    void payerFrais(){ System.out.println(nom + " a payé ses frais."); }
    void afficherInfo(){ System.out.println("Etudiant: " + nom + ", prog=" + programme + ", moyenne=" + moyenne); }
}

// 2) Professeur
class Professeur {
    String nom = "Dr. Martin";
    int age = 45;
    String matiere = "Mathématiques";
    double salaire = 75000;
    String bureau = "B-201";
    String email = "martin@college.com";
    int experience = 20;
    boolean permanent = true;
    String departement = "Sciences";
    String telephone = "514-555-9876";

    void enseigner(){ System.out.println(nom + " enseigne " + matiere); }
    void corrigerExamens(){ System.out.println(nom + " corrige les examens."); }
    void organiserCours(){ System.out.println(nom + " organise un cours."); }
    void donnerTP(){ System.out.println(nom + " donne un TP."); }
    void faireRecherche(){ System.out.println(nom + " fait de la recherche."); }
    void assisterReunion(){ System.out.println(nom + " assiste à une réunion."); }
    void recevoirEtudiant(){ System.out.println(nom + " reçoit un étudiant."); }
    void envoyerEmail(){ System.out.println("Email envoyé à " + email); }
    void prendreConge(){ System.out.println(nom + " prend un congé."); }
    void afficherInfo(){ System.out.println("Professeur: " + nom + ", matière=" + matiere); }
}

// 3) Hopital
class Hopital {
    String nom = "Hopital Général";
    String adresse = "456 Avenue Santé";
    int capacite = 500;
    int medecins = 200;
    int infirmiers = 400;
    int patients = 350;
    boolean ouvert = true;
    String directeur = "Dr. Dupont";
    String telephone = "514-999-0000";
    String specialite = "Urgence";

    void admettrePatient(){ patients++; }
    void soignerPatient(){ System.out.println("Un patient est soigné."); }
    void libererPatient(){ if(patients>0) patients--; }
    void embaucherMedecin(){ medecins++; }
    void licencierMedecin(){ if(medecins>0) medecins--; }
    void ouvrirUrgence(){ ouvert = true; }
    void fermerUrgence(){ ouvert = false; }
    void faireRapport(){ System.out.println("Rapport annuel de l’hôpital."); }
    void appelerDirecteur(){ System.out.println("Appel au directeur " + directeur); }
    void afficherInfo(){ System.out.println("Hopital: " + nom + " (" + capacite + " lits)"); }
}

// 4) Voiture
class Voiture {
    String marque = "Toyota";
    String modele = "Corolla";
    int annee = 2022;
    String couleur = "Noir";
    int vitesse = 0;
    String immatriculation = "XYZ-123";
    boolean electrique = false;
    String proprietaire = "Paul";
    int carburant = 50; // litres
    int kilometrage = 10000;

    void demarrer(){ System.out.println(marque + " démarre."); }
    void arreter(){ vitesse = 0; System.out.println(marque + " s’arrête."); }
    void accelerer(){ vitesse += 10; }
    void freiner(){ if(vitesse>=10) vitesse -= 10; }
    void fairePlein(){ carburant = 50; }
    void rouler(){ kilometrage += 10; carburant = Math.max(0, carburant-1); }
    void klaxonner(){ System.out.println("BEEP BEEP!"); }
    void changerCouleur(String c){ couleur = c; }
    void changerProprietaire(String p){ proprietaire = p; }
    void afficherInfo(){ System.out.println("Voiture: " + marque + " " + modele + " (" + annee + ")"); }
}

// 5) Medecin
class Medecin {
    String nom = "Dr. Leblanc";
    String specialite = "Cardiologie";
    int experience = 15;
    String licence = "MD-98765";
    String email = "leblanc@hopital.com";
    String telephone = "514-444-2222";
    boolean enGarde = false;
    int patientsSuivis = 120;
    String hopital = "HG";
    double salaire = 120000;

    void diagnostiquer(){ System.out.println(nom + " pose un diagnostic."); }
    void prescrire(){ System.out.println(nom + " prescrit un traitement."); }
    void operer(){ System.out.println(nom + " réalise une opération."); }
    void consulter(){ System.out.println(nom + " fait une consultation."); }
    void prendreGarde(){ enGarde = true; }
    void finirGarde(){ enGarde = false; }
    void augmenterPatients(){ patientsSuivis++; }
    void diminuerPatients(){ if(patientsSuivis>0) patientsSuivis--; }
    void envoyerEmail(){ System.out.println("Email envoyé à " + email); }
    void afficherInfo(){ System.out.println("Médecin: " + nom + " (" + specialite + ")"); }
}

// 6) Infirmier
class Infirmier {
    String nom = "Julie";
    int age = 32;
    String service = "Urgence";
    String matricule = "INF-202";
    String telephone = "514-333-1111";
    String email = "julie@hopital.com";
    boolean deNuit = true;
    int patients = 12;
    int heuresSemaine = 36;
    String chef = "Chef Valérie";

    void soigner(){ System.out.println(nom + " soigne un patient."); }
    void preparerSalle(){ System.out.println(nom + " prépare une salle."); }
    void prendreTension(){ System.out.println("Tension prise."); }
    void changerService(String s){ service = s; }
    void commencerNuit(){ deNuit = true; }
    void finirNuit(){ deNuit = false; }
    void augmenterPatients(){ patients++; }
    void diminuerPatients(){ if(patients>0) patients--; }
    void envoyerEmail(){ System.out.println("Email à " + email); }
    void afficherInfo(){ System.out.println("Infirmier: " + nom + ", service=" + service); }
}

// 7) Patient
class Patient {
    String nom = "Marc";
    int age = 60;
    String numeroDossier = "DOS-555";
    double temperature = 36.7;
    int rythmeCardiaque = 72;
    String symptome = "Fatigue";
    boolean hospitalise = true;
    String chambre = "A-12";
    String assurance = "RAMQ";
    String contact = "514-000-2222";

    void prendreMedicaments(){ System.out.println(nom + " prend ses médicaments."); }
    void mesurerTemperature(){ System.out.println("Temp: " + temperature); }
    void mesurerPouls(){ System.out.println("Pouls: " + rythmeCardiaque); }
    void seReposer(){ System.out.println(nom + " se repose."); }
    void subirExamen(){ System.out.println(nom + " subit un examen."); }
    void admettre(){ hospitalise = true; }
    void liberer(){ hospitalise = false; }
    void changerChambre(String c){ chambre = c; }
    void appelerContact(){ System.out.println("Appel au contact: " + contact); }
    void afficherInfo(){ System.out.println("Patient: " + nom + " (" + numeroDossier + ")"); }
}

// 8) Livre
class Livre {
    String titre = "Java Facile";
    String auteur = "A. Dupont";
    String isbn = "978-0-00-000000-0";
    int pages = 300;
    int annee = 2021;
    String editeur = "TechBooks";
    String langue = "FR";
    boolean disponible = true;
    String categorie = "Programmation";
    int emprunts = 0;

    void emprunter(){ if(disponible){ disponible=false; emprunts++; } }
    void retourner(){ disponible = true; }
    void changerCategorie(String c){ categorie = c; }
    void ajouterPages(int p){ pages += p; }
    void retirerPages(int p){ pages = Math.max(1, pages - p); }
    void reimprimer(){ annee++; }
    void marquerIndispo(){ disponible = false; }
    void marquerDispo(){ disponible = true; }
    void afficherTitre(){ System.out.println("Titre: " + titre); }
    void afficherInfo(){ System.out.println("Livre: " + titre + " de " + auteur); }
}

// 9) Bibliotheque
class Bibliotheque {
    String nom = "Biblio Centrale";
    String adresse = "1 Place du Savoir";
    String telephone = "514-101-2020";
    String email = "contact@biblio.com";
    int capacite = 100000;
    int nbMembres = 5000;
    String horaire = "9h-21h";
    boolean ouverte = true;
    String directeur = "Mme Roy";
    int nbEmployes = 30;

    void ouvrir(){ ouverte = true; }
    void fermer(){ ouverte = false; }
    void inscrireMembre(){ nbMembres++; }
    void radierMembre(){ if(nbMembres>0) nbMembres--; }
    void embaucher(){ nbEmployes++; }
    void licencier(){ if(nbEmployes>0) nbEmployes--; }
    void agrandir(){ capacite += 1000; }
    void reduire(){ capacite = Math.max(0, capacite - 1000); }
    void mettreAJourHoraire(String h){ horaire = h; }
    void afficherInfo(){ System.out.println("Bibliothèque: " + nom + " (" + capacite + " livres)"); }
}

// 10) Animal
class Animal {
    String espece = "Chien";
    String nom = "Rex";
    int age = 3;
    double poids = 12.5;
    String couleur = "Marron";
    boolean vacciné = true;
    String proprietaire = "Sam";
    String puce = "CHIP-001";
    String alimentation = "Croquettes";
    String veterinaire = "Clinique Veto";

    void manger(){ System.out.println(nom + " mange."); }
    void dormir(){ System.out.println(nom + " dort."); }
    void aboyer(){ System.out.println(nom + " aboie."); }
    void jouer(){ System.out.println(nom + " joue."); }
    void sePeser(){ System.out.println("Poids: " + poids + " kg"); }
    void vacciner(){ vacciné = true; }
    void changerProprio(String p){ proprietaire = p; }
    void changerAlim(String a){ alimentation = a; }
    void consulterVeto(){ System.out.println(nom + " va chez le vétérinaire."); }
    void afficherInfo(){ System.out.println("Animal: " + espece + " nommé " + nom); }
}

// 11) Maison
class Maison {
    String adresse = "12 Rue des Pins";
    int pieces = 5;
    double surface = 120.0;
    String proprietaire = "Nadia";
    boolean jardin = true;
    boolean garage = false;
    int etages = 2;
    int annee = 2005;
    String chauffage = "Gaz";
    String toit = "Tuile";

    void renover(){ annee = 2025; }
    void ajouterPiece(){ pieces++; }
    void enleverPiece(){ if(pieces>1) pieces--; }
    void agrandir(double m2){ surface += m2; }
    void reduire(double m2){ surface = Math.max(0, surface - m2); }
    void changerProprio(String p){ proprietaire = p; }
    void basculerGarage(){ garage = !garage; }
    void basculerJardin(){ jardin = !jardin; }
    void changerChauffage(String c){ chauffage = c; }
    void afficherInfo(){ System.out.println("Maison: " + adresse + ", " + pieces + " pièces"); }
}

// 12) Ecole
class Ecole {
    String nom = "École Poly";
    String adresse = "100 Avenue du Savoir";
    String directeur = "Mme Belle";
    int eleves = 1200;
    int profs = 90;
    int salles = 50;
    boolean ouverte = true;
    String telephone = "514-777-8888";
    String email = "info@ecole.com";
    String niveau = "Secondaire";

    void ouvrir(){ ouverte = true; }
    void fermer(){ ouverte = false; }
    void inscrire(){ eleves++; }
    void desinscrire(){ if(eleves>0) eleves--; }
    void embaucher(){ profs++; }
    void licencier(){ if(profs>0) profs--; }
    void construireSalle(){ salles++; }
    void detruireSalle(){ if(salles>0) salles--; }
    void changerNiveau(String n){ niveau = n; }
    void afficherInfo(){ System.out.println("École: " + nom + " (" + niveau + ")"); }
}

// 13) Magasin
class Magasin {
    String nom = "TechShop";
    String adresse = "Centre Ville";
    String categorie = "Électronique";
    String proprietaire = "Omar";
    int employes = 8;
    boolean ouvert = true;
    double chiffreAffaires = 0.0;
    String telephone = "514-123-9090";
    String email = "contact@techshop.com";
    int stock = 1000;

    void ouvrir(){ ouvert = true; }
    void fermer(){ ouvert = false; }
    void vendre(int n){ if(stock>=n){ stock -= n; chiffreAffaires += n*100; } }
    void recevoir(int n){ stock += n; }
    void embaucher(){ employes++; }
    void licencier(){ if(employes>0) employes--; }
    void changerCategorie(String c){ categorie = c; }
    void changerProprio(String p){ proprietaire = p; }
    void inventaire(){ System.out.println("Stock: " + stock); }
    void afficherInfo(){ System.out.println("Magasin: " + nom + " (" + categorie + ")"); }
}

// 14) Produit
class Produit {
    String nom = "Casque Audio";
    String reference = "CA-100";
    String categorie = "Audio";
    double prix = 79.99;
    int stock = 50;
    double poids = 0.3;
    String couleur = "Noir";
    String marque = "Sonix";
    boolean actif = true;
    int ventes = 0;

    void activer(){ actif = true; }
    void desactiver(){ actif = false; }
    void augmenterPrix(double p){ prix += p; }
    void baisserPrix(double p){ prix = Math.max(0, prix - p); }
    void vendre(int n){ if(stock>=n){ stock -= n; ventes += n; } }
    void reappro(int n){ stock += n; }
    void recolorer(String c){ couleur = c; }
    void rebrander(String m){ marque = m; }
    void renommer(String n){ nom = n; }
    void afficherInfo(){ System.out.println("Produit: " + nom + " - " + prix + "€"); }
}

// 15) Ordinateur
class Ordinateur {
    String marque = "Lenovo";
    String modele = "ThinkPad";
    String cpu = "i7";
    int ramGo = 16;
    int stockageGo = 512;
    String gpu = "Intel";
    String os = "Windows";
    double batterie = 100.0;
    boolean allume = false;
    String utilisateur = "Guest";

    void allumer(){ allume = true; }
    void eteindre(){ allume = false; }
    void charger(){ batterie = 100.0; }
    void decharger(){ batterie = Math.max(0, batterie - 10); }
    void installerOS(String s){ os = s; }
    void ajouterRAM(int g){ ramGo += g; }
    void ajouterStockage(int g){ stockageGo += g; }
    void changerUser(String u){ utilisateur = u; }
    void afficherConfig(){ System.out.println(cpu + ", " + ramGo + "Go, " + stockageGo + "Go"); }
    void afficherInfo(){ System.out.println("Ordinateur: " + marque + " " + modele); }
}

// 16) Telephone
class Telephone {
    String marque = "Samsung";
    String modele = "S22";
    String os = "Android";
    String numero = "514-222-3333";
    int stockageGo = 256;
    int ramGo = 8;
    double batterie = 85.0;
    boolean allume = true;
    String operateur = "CarrierX";
    String proprietaire = "Lea";

    void appeler(String n){ System.out.println("Appel vers " + n); }
    void envoyerSMS(String n){ System.out.println("SMS à " + n); }
    void eteindre(){ allume = false; }
    void allumer(){ allume = true; }
    void charger(){ batterie = 100.0; }
    void installerApp(String app){ System.out.println("App installée: " + app); }
    void desinstallerApp(String app){ System.out.println("App désinstallée: " + app); }
    void changerOperateur(String op){ operateur = op; }
    void changerProprio(String p){ proprietaire = p; }
    void afficherInfo(){ System.out.println("Téléphone: " + marque + " " + modele); }
}

// 17) Avion
class Avion {
    String compagnie = "AirMonde";
    String modele = "A320";
    int sieges = 180;
    int carburant = 100; // %
    int altitude = 0;
    int vitesse = 0;
    String immatriculation = "AM-123";
    String base = "YUL";
    boolean enVol = false;
    String pilote = "Cap. Hugo";

    void decoller(){ enVol = true; altitude = 1000; vitesse = 300; }
    void atterrir(){ enVol = false; altitude = 0; vitesse = 0; }
    void monter(int m){ altitude += m; }
    void descendre(int m){ altitude = Math.max(0, altitude - m); }
    void accelerer(int v){ vitesse += v; }
    void ralentir(int v){ vitesse = Math.max(0, vitesse - v); }
    void ravitailler(){ carburant = 100; }
    void consommer(){ carburant = Math.max(0, carburant - 5); }
    void changerPilote(String p){ pilote = p; }
    void afficherInfo(){ System.out.println("Avion: " + modele + " " + immatriculation); }
}

// 18) Banque
class Banque {
    String nom = "Banque Centrale";
    String adresse = "10 Rue Finance";
    String telephone = "514-909-8080";
    String email = "info@banquecentrale.com";
    int agences = 25;
    int clients = 100000;
    double actifs = 1_000_000_000;
    boolean ouverte = true;
    String directeur = "M. Durand";
    String pays = "Canada";

    void ouvrir(){ ouverte = true; }
    void fermer(){ ouverte = false; }
    void ouvrirAgence(){ agences++; }
    void fermerAgence(){ if(agences>0) agences--; }
    void ajouterClient(){ clients++; }
    void retirerClient(){ if(clients>0) clients--; }
    void augmenterActifs(double x){ actifs += x; }
    void diminuerActifs(double x){ actifs = Math.max(0, actifs - x); }
    void changerDirecteur(String d){ directeur = d; }
    void afficherInfo(){ System.out.println("Banque: " + nom + " (" + agences + " agences)"); }
}

// 19) CompteBancaire
class CompteBancaire {
    String titulaire = "Sofia";
    String numero = "CB-001";
    String devise = "CAD";
    double solde = 1500.0;
    double plafondRetrait = 500.0;
    boolean actif = true;
    String type = "Chèque";
    String banque = "Banque Centrale";
    String iban = "CA00-1234";
    String swift = "BCNCCA";

    void deposer(double m){ if(m>0) solde += m; }
    void retirer(double m){ if(m>0 && m<=plafondRetrait && m<=solde) solde -= m; }
    void activer(){ actif = true; }
    void desactiver(){ actif = false; }
    void changerPlafond(double p){ plafondRetrait = p; }
    void changerType(String t){ type = t; }
    void changerDevise(String d){ devise = d; }
    void changerBanque(String b){ banque = b; }
    void afficherSolde(){ System.out.println("Solde: " + solde + " " + devise); }
    void afficherInfo(){ System.out.println("Compte: " + numero + " - " + titulaire); }
}

// 20) Cinema
class Cinema {
    String nom = "Cinéma Lux";
    String adresse = "Boulevard Central";
    int salles = 8;
    int siegesParSalle = 150;
    boolean ouvert = true;
    String directeur = "Mme Lenoir";
    String telephone = "514-606-7070";
    String email = "contact@cinemalux.com";
    double recettes = 0.0;
    String filmVedette = "Blockbuster X";

    void ouvrir(){ ouvert = true; }
    void fermer(){ ouvert = false; }
    void projeter(){ System.out.println("Projection de " + filmVedette); }
    void vendreBillets(int n){ recettes += n * 12.0; }
    void changerFilm(String f){ filmVedette = f; }
    void ajouterSalle(){ salles++; }
    void retirerSalle(){ if(salles>0) salles--; }
    void promo(){ System.out.println("Promo du mercredi!"); }
    void afficherRecettes(){ System.out.println("Recettes: " + recettes); }
    void afficherInfo(){ System.out.println("Cinéma: " + nom + " (" + salles + " salles)"); }
}
```

### Comment utiliser

1. Sauvegarde tout ce bloc dans un fichier `Main.java`.
2. Compile :

```bash
javac Main.java
```

3. Exécute :

```bash
java Main
```


