# maroua-oufik-hajar-oularbi
Application Magasin
import java.util.*;
import java.io.*;

public class ApplicationMagasin {
    static String[] noms=new String[100];
    static double[] prix=new double[100];
    static int[] quantite=new int[1500];
    static int nbrProduits=0;
    static int maxStock=1500;
    static Scanner scanner=new Scanner(System.in);

    public static void main(String[] args) {
        chargerStock();
        int choix;
        do{
            afficherMenu();
                System.out.print("Veuillez entrer votre choix :");
                choix=scanner.nextInt();
                switch(choix){
                    case 1:ajouterProduit();break;
                    case 2:modifierPrix();break;
                    case 3:effectuerVente();break;
                    case 4:afficherStock();break;
                    case 5:rechercherProduit();break;
                    case 6:sauvegarderStock();break;
                    case 7:System.out.println("Au revoir!"); sauvegarderStock(); break;
                    default:System.out.println("Choix invalide!");
                }
        }while(choix>7 && choix <0);

    }
    static void afficherMenu(){
        System.out.println("\n********* MENU D'APPLICATION MAGASIN *********");
        System.out.println("1. Ajouter un produit");
        System.out.println("2. Modifier le prix d'un produit ");
        System.out.println("3. Vendre le produit");
        System.out.println("4. Voir stock des produit");
        System.out.println("5. Rechercher d'un produit");
        System.out.println("6. Sauvegarder un produit");
        System.out.println("7. Quitter l'application ");
    }

    static void chargerStock() {
        try {
            File file=new File("stock.txt");
            if(!file.exists()) return;

            Scanner fileScan=new Scanner(file);
            while(fileScan.hasNextLine() && nbrProduits < maxStock) {
                String ligne = fileScan.nextLine();
                String[] parts = ligne.split(";");
                if(parts.length==3) {
                    noms[nbrProduits] = parts[0];
                    prix[nbrProduits] = Double.parseDouble(parts[1]);
                    quantite[nbrProduits] = Integer.parseInt(parts[2]);
                    nbrProduits++;
                }
            }
            fileScan.close();
            System.out.println("Stock chargé!");
        } catch(Exception e) {
            System.out.println("Pas de fichier stock trouvé");
        }
    }

    static void ajouterProduit(){
        if(nbrProduits>=maxStock){
            System.out.println("Pardon . Le stock est plein!");
            return;
        }

        System.out.print("Veuillez entrer le nom de votre produit : ");
        noms[nbrProduits]=scanner.nextLine();

        System.out.print("Veuillez entrer le prix de votre produit : ");
        prix[nbrProduits]=scanner.nextDouble();

        System.out.print("Veuillez entrer la quantité de votre produit : ");
        quantite[nbrProduits]=scanner.nextInt();
        scanner.nextLine();

        nbrProduits++;
        System.out.println("Opération bien éffectuée . Votre Produit est ajouté!");
    }

    static void modifierPrix(){
        System.out.print("Veuillez entrer le nom du produit: ");
        String nom=scanner.nextLine();
        for (int i = 0; i < nbrProduits; i++) {
            if (noms[i].equalsIgnoreCase(nom)) {
                System.out.print("Veuillez entrer votre nouveau prix : ");
                prix[i]=scanner.nextDouble();
                scanner.nextLine();
                System.out.println("Opération bien effectuée ! Le prix de votre produit est changé ! ");
            }else{
                System.out.println("Pardon ! Produit recherché n'existe pas !");
            }
        }
    }

    static void effectuerVente() {
        System.out.print("Veuillez entrer le nom du produit: ");
        String nom = scanner.nextLine();
        for (int i = 0; i < nbrProduits; i++) {
            if (noms[i].equalsIgnoreCase(nom)) {
                System.out.print("Veuillez entrer le quantité voulue du produit: ");
                int qtt = scanner.nextInt();
                scanner.nextLine();
                if (qtt > quantite[i]) {
                    System.out.println("Le stock est insuffisant! ");
                } else {
                    double prixtotal = prix[i] * qtt;
                    double remise = 0;
                    System.out.println("\n=== TICKET ===");
                    System.out.println("Nom de Produit: " + noms[i]);
                    System.out.println("Prix unitaire : " + prix[i] + " DHs");
                    System.out.println("Quantité de stock: " + qtt);
                    System.out.println("PrixTotal: " + prixtotal + " DHs");
                    if (prixtotal > 1000) {
                        remise = prixtotal * 0.1;
                        prixtotal = prixtotal - remise;
                        System.out.println("Vous avez une remise 10%: -" + remise + " DHs");
                    }
                    System.out.println("Le prix total à payer est : " + prixtotal + " DHs");
                    quantite[i] = quantite[i] - qtt;
                }
            } else {
                System.out.println("Votre produit n'existe pas !");
            }
        }
    }
    static void afficherStock(){
        System.out.println("\n VOTRE STOCK ACTUEL");
        if(nbrProduits==0) {
            System.out.println("Pas de produit");
            return;
        }
        for(int i=0;i<nbrProduits;i++){
            System.out.println(noms[i]+" |"+prix[i]+"DHs |"+quantite[i]+" pièces");
        }
    }

    static void rechercherProduit() {
        System.out.print("Veuillez entrer le nom de produit à chercher:");
        String nom = scanner.nextLine();
        for (int i = 0; i < nbrProduits; i++) {
            if (noms[i].equalsIgnoreCase(nom)) {
                System.out.println("Produit Recherché est : " + noms[i] + " | Prix: " + prix[i] + " | Quantité De Stock: " + quantite[i]);
            } else {
                System.out.println("Pardon . Votre produit n'existe pas dans le stock !");
            }
        }
    }

    static void sauvegarderStock(){
        try{
            FileWriter flWr=new FileWriter("stock.txt");
            for(int i=0; i<nbrProduits;i++) {
                flWr.write(noms[i] + ";" + prix[i] + ";" + quantite[i] + "\n");
            }
            flWr.close();
            System.out.println("Opération effectuée ! Votre stock est sauvegardé!");
        } catch(Exception e) {
            System.out.println("Vous avez une erreur de sauvegarde !");
        }
    }





}
