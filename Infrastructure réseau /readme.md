
# Infrastructure réseau Cisco

## Objectif

Mettre en place un petit réseau dont les postes sont séparés en plusieurs **VLAN**, permettre la communication entre les réseaux grâce au routage inter-VLAN et filtrer le trafic grâce aux **ACL**.

Je voulais également permettre l'administration à distance du serveur DHCP depuis le poste admin en utilisant le protocole **SSH**.

## Environnement

- Cisco Packet Tracer

## Mise en place du réseau

Le réseau a été séparé en trois VLAN :

- **VLAN 10** : Serveurs - Contient le serveur DHCP
- **VLAN 20** : IT - Contient le poste admin du service informatique
- **VLAN 30** : RH - Postes clients du service RH

<img width="1227" height="581" alt="INFRA" src="https://github.com/user-attachments/assets/38cb04b6-0000-4525-b818-3962f123d886" />

## VLAN et routage inter-VLAN

Les VLAN ont été configurés sur le commutateur et associés aux ports en fonction du réseau auquel appartient chaque poste.

Le but était de limiter les communications entre les différents réseaux et de réduire le domaine de diffusion.

Le lien entre le commutateur et le routeur R1 est configuré en mode **trunk**, permettant de transporter le trafic des VLAN 10, 20 et 30.

Pour cela, trois sous-interfaces ont été créées sur le routeur, une par VLAN, afin d'assurer le routage entre les différents réseaux.

**VLAN créés sur le commutateur**

<br>

<img width="902" height="209" alt="VLANs" src="https://github.com/user-attachments/assets/19392d97-af77-46bc-9755-62fb04592f48" />

<br><br>

**Sous-interfaces du routeur**

<br>

<img width="902" height="137" alt="sous interfaces" src="https://github.com/user-attachments/assets/2f12cdc7-8244-4cb5-8df8-956f41fbf175" />

## Contrôle des communications

J'ai mis en place des **ACL** pour contrôler les communications entre les différents VLAN.

Le but était d'empêcher les VLAN 20 et 30 de communiquer tout en permettant au poste admin du VLAN 20 d'administrer à distance le serveur DHCP du VLAN 10.

Il fallait également permettre aux postes du VLAN 30 de joindre le serveur DHCP dans le VLAN 10 afin qu'ils puissent récupérer leurs paramètres réseau.

<img width="682" height="220" alt="filtre vlan 30" src="https://github.com/user-attachments/assets/afa562d5-82ad-4f67-8901-d8d26e602e23" />


## Administration du serveur DHCP

J'ai configuré le serveur pour permettre son administration à distance par **SSH**.

<img width="687" height="446" alt="config ssh partie 1" src="https://github.com/user-attachments/assets/cbf6c0f6-fd9f-4312-8bf6-b7b127f29b87" />

<img width="511" height="21" alt="config ssh partie 2" src="https://github.com/user-attachments/assets/a4baf0d6-47a3-42d9-bfa5-26d6e4b54904" />

## Validation

J'ai réalisé plusieurs tests de connectivité pour vérifier que le routage fonctionne correctement et que les règles de filtrage s'appliquent.

- Communications entre les VLAN 20 et 30 bloquées
- Administration à distance du serveur DHCP depuis le poste admin
- Connexion SSH bloquée depuis le VLAN 30 vers le serveur DHCP
  
<br>

**Test Ping et connexion SSH depuis un poste du VLAN 30 vers le serveur DHCP**

Le ping vers le serveur DHCP fonctionne, mais la connexion SSH est bloquée par les ACL.

<br>

<img width="966" height="554" alt="test ping et ssh vers serveur dhcp" src="https://github.com/user-attachments/assets/ee1bd6f7-c13e-42c5-a43d-05fd621628dd" />

<br><br>

**Test Ping depuis un poste du VLAN 30 vers le poste admin du VLAN 20**

<br>

<img width="719" height="613" alt="test rh vers admin acl" src="https://github.com/user-attachments/assets/28d7dec8-bc8c-4db8-92ed-a3e8cdb4d3a9" />

<br><br>

**Test de connexion SSH depuis le poste admin du VLAN 20 vers le serveur DHCP**

<br>

<img width="968" height="548" alt="admin dhcp test" src="https://github.com/user-attachments/assets/6dd53256-a781-45b8-adf8-fe400efec41d" />
