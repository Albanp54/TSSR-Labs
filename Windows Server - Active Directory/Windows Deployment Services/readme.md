# Windows Deployment Services

## Objectif

Mettre en place un serveur de déploiement Windows permettant de gérer de déployer des systèmes d'exploitations à partir d'une image master préparée pour homogénéiser et simplifier les déploiements.

## Environnement

* Windows Server 2022 avec les rôles DHCP et WDS installés
* Client Windows 10
* Poste Windows 10 pour créer l'image Master
* VirtualBox

## Configuration du serveur

J'ai tout d'abord installé les rôles **WDS** et **DHCP** sur le serveur.
Pour le stockage des fichiers relatifs à WDS, j'ai ajouté un disque de stockage supplémentaire nommé W:Images.
Disposant d'un domaine Active Directory, le serveur a été intégré au domaine.
Les images seront stockées dans le dossier RemoteInstall, créé sur le disque de stockage préalablement créé.
J'ai choisi de répondre à tous les postes clients connus ou inconnus sous réserve de l'approbation de l'administrateur.

## DHCP

Après avoir configuré l'étendue DHCP, j'ai activé la prise en charge des clients BOOTP pour que les machines clientes puissent démarrer sur le réseau et récupérer les images.

## Image de démarrage et image de capture

Pour capturer l'image master et la rendre déployable via WDS, il est nécessaire d'avoir une image de démarrage. C'est grâce à elle que l'on peut créer une image de capture, qui sera utilisée ensuite pour capturer le système installé sur le poste master et l'enregistrer sous forme d'une image d'installation.

Pour ce faire, j'ai insérer dans le lecteur optique du serveur l'ISO d'un Windows 10 pour en extraire le fichier **boot.wim** pour créer l'image de démarrage.
En faisant un **clic droit -> Créer une image de capture** sur l'image de démarrage, j'ai créé l'image de capture.

<img width="929" height="201" alt="capture et demarrage" src="https://github.com/user-attachments/assets/18b66da1-b300-4cb4-a49b-85f88055adb5" />

## Image master

A partir de la VM Windows 10 master, j'ai réalisé les différentes mises à jour systèmes et installé les différents logiciels (ici le navigateur Chrome) avant la capture.

<img width="892" height="689" alt="Mise à jour Windows" src="https://github.com/user-attachments/assets/eb4cf856-0b8a-4d64-893f-46c879a4a075" />

<img width="1030" height="354" alt="Google Chrome MAster" src="https://github.com/user-attachments/assets/d1fdaeef-6611-467c-9644-97448d050f02" />
<br/>
<br/>
Une fois les mise à jour terminées et les logiciels installés, j'ai utilisé l'outil Windows **Sysprep** permettant de généraliser l'image en supprimant les information spécifiques de la machine.

J'ai donc utilisé la commande **sysprep.exe /generalize /oobe /shutdown**.

* /generalize : Supprime les informations spécifiques à la machine, notamment le SID (Security Identifier). 
* /oobe : Lance l'expérience de première utilisation (Out-of-Box Experience) au prochain démarrage. 
* /shutdown : Éteint l'ordinateur une fois le processus terminé, permettant la capture de l'image.

<img width="838" height="246" alt="SysPrep exec" src="https://github.com/user-attachments/assets/fc2e18ef-4763-482e-8dcc-08b178fd677c" />
<br/>
<br/>
Ceci étant fait, j'ai modifié les options d'amorçage de la machine master pour qu'elle démarre sur le réseau et utilise l'image de capture sur le serveur WDS lors du démarrage.
L'assistant de capture s'ouvre alors pour permettre la capture du lecteur contenant le système d'exploitation.
<br/>
<br/>
<img width="525" height="478" alt="Capture de l&#39;image master" src="https://github.com/user-attachments/assets/9918772c-2719-4afe-a8ad-ab64fad213ca" />
<br/>
<br/>
Attribution du nouvel emplacement de stockage de l'image d'installation sur le serveur WDS : 
<br/>
<img width="515" height="468" alt="Auth serv WDS" src="https://github.com/user-attachments/assets/b8bf9be4-48be-483e-946a-769d0431038d" />

<img width="929" height="201" alt="capture et demarrage" src="https://github.com/user-attachments/assets/922358de-4d47-49d2-aabf-c581a7806cd0" />


## Test depuis une machine vierge

Pour terminé, j'ai créé la machine virtuelle cliente sur laquelle j'ai simplement ajouter cette machine dans le même réseau que le serveur et modifier l'option d'amorçage pour qu'elle démarre sur le réseau.
Après quelques secondes, la VM télécharge le fichier boot.wim depuis le serveur WDS : 
<br/><br/>
<img width="1023" height="215" alt="Boot du client sur reseau" src="https://github.com/user-attachments/assets/bf80c652-aacd-4b1f-8438-b9b241177640" />
<br/>
<br/>
Puis l'assistant d'installation se lance et je peux choisir mon image d'installation personnalisée : 
<br/><br/>
<img width="647" height="485" alt="choix de l&#39;os client" src="https://github.com/user-attachments/assets/96e7bccb-fa70-43fc-8db9-11c1f884a21c" />
<br/><br/>
Une fois le déploiement terminé, j'ai pu constater que tout avait bien fonctionné : 
<br/><br/>
<img width="1031" height="770" alt="vm cliente prete a l&#39;emploi post installation" src="https://github.com/user-attachments/assets/55ffbad9-de2e-4eb7-a844-aa0b17b785b6" />




