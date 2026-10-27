# Infrastructure Active Directory

## Objectif

Mettre en place un serveur Windows permettant de gérer un domaine Active Directory avec les services AD-DS, DNS et DHCP. Le projet comprend aussi la gestion des utilisateurs, la mise en place de stratégies de groupe et la gestion des droits d'accès à des dossiers partagés.

## Environnement

* Windows Server 2022
* Windows 10
* VirtualBox

## Active Directory
J'ai tout d'abord installé et configuré les rôles **AD-DS**, **DHCP** et **DNS** sur le serveur avant de promouvoir ce dernier en tant que **Contrôleur de domaine** et de créer le domaine **studio.lan**.

<img width="757" height="405" alt="Nom Serv Domaine et IP" src="https://github.com/user-attachments/assets/ec757093-5222-4ffc-b31f-6d74d6fcca0a" />




À la racine du domaine, j'ai créé trois unités d'organisation afin de classer les objets selon leur type :

* Utilisateurs
* Ordinateurs
* Groupes

<img width="755" height="532" alt="arborescence users et groupe" src="https://github.com/user-attachments/assets/0910a0d1-92a6-4350-a9f3-8b43ebce19db" />


## Stratégies de groupe

Deux stratégies de groupe ont notamment été mises en place :

* **Mappage lecteur :** permet aux utilisateurs d'accéder au partage de fichiers depuis l'Explorateur de fichiers. Elle est appliquée à l'UO  « Utilisateurs ».

* **Bloquer Panneau de configuration :** empêche les utilisateurs du service Comptabilité d'accéder au Panneau de configuration et aux paramètres Windows.

<img width="368" height="364" alt="GPOs" src="https://github.com/user-attachments/assets/34ae51b3-697b-41f8-b7b8-944219ba92fc" />


## Partage de fichiers

Le partage contient deux dossiers de travail correspondant à deux services :

* Comptabilité
* Techniciens

Chaque dossier possède ses propres règles d'accès et permissions NTFS.

<img width="777" height="183" alt="Contenu Partage" src="https://github.com/user-attachments/assets/5a9664ad-8db2-49f2-9d3e-66490ee13b64" />


## Sécurité des dossiers

Les permissions NTFS sont configurées individuellement sur les dossiers afin de contrôler les accès selon les groupes de sécurité.

Dossier Comptabilité 

<img width="770" height="524" alt="Autorisation dossier Compta" src="https://github.com/user-attachments/assets/5971cdde-f437-4ba5-b6de-e3b0f3844657" />


Dossier Techniciens

<img width="769" height="524" alt="Autorisation dossier Techniciens" src="https://github.com/user-attachments/assets/db3de2ca-25d8-42f8-86b3-470c00fc597e" />


## Gestion des droits

J'ai employé la méthode **AGDLP** pour gérer les accès aux ressources et les permissions NTFS.

* **GG_Compta / GG_Techniciens** représentent les groupes correspondant aux fonctions métier.
* **DL_Partage_R** est le groupe de sécurité permettant l'accès au partage.
* **DL_Compta_RW** permet l'accès au dossier Comptabilité avec les permissions NTFS de lecture et d'écriture.
* **DL_Techniciens_RW** permet l'accès au dossier Techniciens avec les permissions NTFS de lecture et d'écriture.

<img width="758" height="532" alt="Groupes" src="https://github.com/user-attachments/assets/049a6d93-34db-4f09-9b56-4a71cfeb6197" />


## DHCP

J'ai configuré une étendue DHCP pour distribuer automatiquement les paramètres réseau aux postes du domaine.

<img width="1025" height="294" alt="DHCP COnfig" src="https://github.com/user-attachments/assets/bc05bc2e-5fa9-4764-ac21-40eba178a71f" />


## Validation

### Distribution des paramètres réseau

Test de l'attribution automatique des paramètres réseau par le serveur DHCP sur un poste client.

<img width="981" height="513" alt="config reseau poste client" src="https://github.com/user-attachments/assets/a420f9fd-4c66-4ac6-8786-b966300d63f3" />


### Application des stratégies de groupe

Test de l'application des stratégies de groupe sur un poste client.

<img width="795" height="417" alt="GPOs appliquées" src="https://github.com/user-attachments/assets/abc1a887-69b0-4a68-99f4-b9de56b7fdd7" />


### Blocage du Panneau de configuration

Depuis le poste client, j'ai tenté d'ouvrir le Panneau de configuration avec la commande `control`.

L’accès est refusé et un message d’erreur apparaît à l’écran.

<img width="559" height="136" alt="Blocage panneau test" src="https://github.com/user-attachments/assets/dd9cb051-c005-485c-a893-69df810baf2b" />


### Accès au partage

Test de l'accès au partage depuis un compte utilisateur appartenant au service Comptabilité.

<img width="792" height="361" alt="partage test" src="https://github.com/user-attachments/assets/05076e1f-115d-4a10-ba38-b5681fb26019" />


### Création d'un fichier

Création d'un fichier dans le dossier Comptabilité depuis un compte utilisateur du service Comptabilité afin de vérifier les permissions d'écriture.

<img width="783" height="357" alt="Test création fichier dans compta" src="https://github.com/user-attachments/assets/110490b7-a244-4bc7-8e21-c48537c2ac6f" />


