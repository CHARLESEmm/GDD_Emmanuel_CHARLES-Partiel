# GDD_Emmanuel_CHARLES-Partiel
Nouveau projet en gestion de données 


La structure de base de données pour l'hôtel, selon moi, devrait inclure des tables pour les clients, les réservations, les chambres,
les employés, y compris les cadres, les factures et les services en plus. Cela permettra à l'hôtel de collecter des informations importantes
pour améliorer les services proposés aux clients et de s'adapter aux tendances du marché pour rester compétitif.
Vous trouverez mon schéma UML dans le fichier intitulé "thehotelschema.png". La base de données serait organisée de la manière suivante :

Je commence par la table "hôtel", cette table contient les informations globales sur l'hôtel (nom, numéro, adresse, nombre d'employés, nombre de chambres, services et capacité totale).

La table "employés" contient les informations sur les employés de l'hôtel, telles que leur nom, adresse et poste, ainsi que s'ils ont un manager.
.
			use thehotel
 
				CREATE TABLE employes (
  				 id INT PRIMARY KEY,
    				nom VARCHAR(255) NOT NULL,
   				 prenom VARCHAR(255) NOT NULL,
   				 email VARCHAR(255) NOT NULL,
   				 numero_de_telephone VARCHAR(255) NOT NULL,
   				 Hire_date DATE NOT NULL,
   				 commission_pct INT NOT NULL,
    				salaire DECIMAL(10,2) NOT NULL,
    				departement_id INT,
   				 poste INT NOT null, 
    				FOREIGN KEY (departement_id) REFERENCES thehotel.employes(id)
				);



La table "clients" contient des informations sur les clients de l'hôtel, telles que leur nom, adresse et numéro de téléphone. 
Il existe également un lien avec la table "réservations" pour indiquer les réservations effectuées par un client donné.

				CREATE TABLE client (
  			      id INT PRIMARY KEY,
    				nom VARCHAR(20) NOT NULL,
    				prenom VARCHAR(20) NOT NULL,
    				adresse VARCHAR(50),
    				email VARCHAR(30) NOT NULL,
    				numero_de_telephone VARCHAR(255) NOT NULL,
    				paiement VARCHAR(50),
    				historique_reservation VARCHAR(120)
				);


La table "chambre" contient tout ce qu'il faut savoir sur une chambre d'hotel (numero,tarifs, sa disponibilité).
 
---- 				CREATE TABLE chambre (
    				id INT PRIMARY KEY,
    				numero_chambre INT NOT NULL,
    				id_hotel INT NOT NULL,
    				id_reservation INT NOT NULL,
    				etage INT NOT NULL,
    				type VARCHAR(20),
    				prix DECIMAL(10,2) NOT NULL,
    				statut ENUM('Occupée','Libre','En attente') NOT NULL;
    				FOREIGN KEY (ID_hotel) REFERENCES hotel(ID)
    
				);








La table "services" contient les informations sur les services offerts par l'hôtel, comme leur nom et leur prix.

				CREATE TABLE services (
    				id INT PRIMARY KEY,
    				nom VARCHAR(50),
   				 description TEXT NOT NULL,
    				tarif DECIMAL (10,2) NOT NULL,
    				id_reservation INT NOT NULL,
   				 department_id INT,
    				FOREIGN KEY (department_id) REFERENCES departement(department_id)
    
				);


La table "département" contient les informations sur les départements de l'hôtel, comme leur nom et leur responsable.
				
				CREATE TABLE departement (
    				department_id INT PRIMARY KEY,
    				nom VARCHAR(50) NOT NULL,
   				 id_hotel INT NOT NULL,
    				manager_id INT NOT NULL,
    				FOREIGN KEY (manager_id) REFERENCES employes(employes_id),
   				 FOREIGN KEY (id_hotel) REFERENCES hotel(id_hotel)
				);




Table "réservation" relie les clients aux chambres qu'ils ont réservées, avec des informations comme la date d'arrivée et de départ.

				CREATE TABLE reservation (
    				reservation_id INT PRIMARY KEY,
    				client_id INT NOT NULL,
    				chambre_id INT NOT NULL,
    				employe_id INT NOT NULL,
    				date_debut DATE NOT NULL,
    				date_fin DATE NOT NULL,
    				nombre_personne INT NOT NULL,
    				total FLOAT NOT NULL,
    				commentaire VARCHAR(255),
    				statut ENUM('Confirmée','Annulée','En attente') NOT NULL,
    				FOREIGN KEY (client_id) REFERENCES client(client_id),
   				 FOREIGN KEY (chambre_id) REFERENCES chambre(chambre_id),
    				FOREIGN KEY (employe_id) REFERENCES employes(employe_id)
				);


la table "factures" contient les informations sur les factures des clients, comme leur montant total et la date.
				CREATE TABLE factures (
    				facture_id INT PRIMARY KEY,
    				date_facture DATE NOT NULL,
   				 montant DECIMAL(10,2) NOT NULL,
    				client_id INT NOT NULL,
    				employe_id INT NOT NULL,
    				service_id INT NOT NULL,
    				chambre_id INT NOT NULL,
    				FOREIGN KEY (client_id) REFERENCES clients(client_id),
    				FOREIGN KEY (employe_id) REFERENCES employes(employe_id),
    				FOREIGN KEY (service_id) REFERENCES services(service_id),
    				FOREIGN KEY (chambre_id) REFERENCES chambres(chambre_id)
				);

Une fois fini je vais remplir les tables de données afin d'interoger la base mais avant tout sa je vous montre quelques requettes sql 
pour inserer des données dans la base.


debut de l'exemple 
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

use thehotel

INSERT INTO the_hotel (id, name, address,phone_number,email,capacity,chambre_dispo,reserved_rooms,number_of_employees)
VALUES (1, 'THEHOTEL', '7 rue de demange, 75002 Paris', '0123856759', 'thehotelsupport@gmail.com', '500', '250','250',105);

INSERT INTO chambre (id, numero_chambre, id_reservation, etage, type, prix, statut)
VALUES (9, 203, 9, 2, 'Double', 70, 'Libre');

INSERT INTO client (ID, nom, prenom, adresse, numero_de_telephone , email,paiement, historique_reservation) 
VALUES 
(1, 'Dupont', 'Jacques', '1 rue de la Paix', '0123456789', 'jacques.dupont@gmail.com', 'Carte de crédit', 'Réservation du 25/05/2022 au 27/05/2022'),

INSERT INTO reservation (reservation_id, client_id, chambre_id, employe_id, date_debut, date_fin, nombre_personne, total, commentaire) VALUES 
(11, 1, 203, 1, '2022-03-01', '2022-03-05', 2, 150, 'Première réservation,incroyable!!'),

INSERT INTO employes (id, nom, prenom, email, numero_de_telephone, hire_date, commission_pct, salaire, departement_id, poste) VALUES
(1, 'John', 'Smith', 'john.smith@example.com', '06-12-34-56-78', '2022-01-01', 0.1, 50000, 1, 'Manager'),

INSERT INTO factures (facture_id, date_facture, montant, client_id, employe_id, service_id, chambre_id, reservation_id)
VALUES(1, '2022-01-01', 100.50, 1, 1, 1, 1, 1), (2, '2022-01-02', 120.75, 2, 2, 2, 2, 2), (3, '2022-01-03', 150.25, 3, 3, 3, 3, 3),
(4, '2022-01-04', 75.50, 4, 4, 4, 4, 4),(5, '2022-01-05', 200.00, 5, 5, 5, 5, 5),(6, '2022-01-06', 250.00, 6, 6, 6, 6, 6),
(7, '2022-01-07', 300.00, 7, 7, 7, 7, 7),(8, '2022-01-08', 350.00, 8, 8, 8, 8, 8),(9, '2022-01-09', 400.00, 9, 9, 9, 9, 9);

INSERT INTO services (id, nom, description, tarif, reservation_id, department_id)
VALUES (1, 'Petit déjeuner buffet', 'Un petit déjeuner buffet complet comprenant des produits frais et locaux', 15.00, 1, 1),
(2, 'Service de chambre', 'Service de chambre disponible 24h/24 et 7j/7', 20.00, 2, 2),
(3, 'Piscine extérieure', 'Piscine extérieure ouverte en saison', 10.00, 3, 3),
(4, 'Salle de fitness', 'Salle de fitness équipée de machines de cardio et de musculation', 8.00, 4, 4),
(5, 'Service de blanchisserie', 'Service de blanchisserie professionnel disponible pour les clients', 5.00, 5, 5);


INSERT INTO departement (department_id, nom, id_hotel, manager_id) VALUES (1, 'Réception', 1, 1);

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
fin de l'example




Requette Sql pour interoger la base de l'hotel:

	- un client en particulier en utilisant son nom et prenom par exemple, je veux des informations sur le client "Petit Paul".

      		SELECT id, adresse, email, numero_de_telephone FROM client
				WHERE nom = 'Petit' AND prenom = 'Paul';

     En utilisant son ID exemple [5].
			
			SELECT * FROM client WHERE id ='5';

     /*Afficher le nombre de réservations annulées:*/

		SELECT COUNT(*) FROM reservation WHERE statut='Annulée';

     -Afficher les chambres libres triées par numéro de chambre:

		SELECT * FROM chambre WHERE statut='Libre' ORDER BY numero_chambre;

     - Afficher le nom et le prénom des clients ayant effectué une réservation avec un total supérieur à 100:
    	      
            SELECT client.nom, client.prenom FROM client
		JOIN reservation ON client.id = reservation.client_id
		WHERE reservation.total > 100;

	- Sélectionner les noms et prénoms de tous les employés:
			
		SELECT nom, prenom FROM employes;

	-Sélectionner le nombre total d'employés:

		SELECT COUNT(*) FROM employes;
	
	-Sélectionner les employés triés par nom décroissant :

		SELECT * FROM employes ORDER BY nom DESC;

	-Sélectionner les employés qui ont une commission supérieure à 5%:

		SELECT * FROM employes WHERE commission_pct > '5';

	-Sélectionner les noms des services :

		SELECT nom FROM services;

	- Sélectionner le tarif moyen des services :

		SELECT AVG(tarif) FROM services;

	-Sélectionner le nombre total de services :

		SELECT COUNT(*) FROM services;	

	- Sélectionner les services qui appartiennent au département 4 :

		SELECT * FROM services WHERE department_id = 4;

	-sélectionner les employés et les services qu'ils gèrent:

		SELECT employes.*, services.*
            FROM employes
            JOIN departement ON employes.departement_id = departement.department_id
            JOIN services ON departement.department_id = services.department_id;

	-Afficher le nombres de  reservations.

		SELECT COUNT(*) FROM reservation WHERE statut = 'Confirmée';

	-Afficher les informations des chambres qui sont actuellement occupées:

		SELECT numero_chambre, type, etage, statut FROM chambre WHERE statut = 'Occupée';
