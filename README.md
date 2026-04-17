
# `  🌐  ` ︲ `  🗺️  `︲Shéma Réseau - Contexte M2L

<p align="center">
  <img src="https://img.shields.io/badge/Context-Ateliers_Professionnels-blue?style=for-the-badge">
  <img src="https://img.shields.io/badge/Formation-SISR-28A745?style=for-the-badge">
  <img src="https://img.shields.io/badge/Written_in-Markdown-2B7489?logo=markdown&style=for-the-badge">
  <img src="https://img.shields.io/badge/Made%20with-Mermaid-FF3670?logo=mermaid&style=for-the-badge">
</p>

-----

## ` ❔ `︲Contexte de ce Shéma : 

Le projet s'inscrit dans le cadre des **Ateliers Professionnels (AP)** de la formation **SISR**. L'objectif est de concevoir et de déployer l'intégralité de l'infrastructure réseau de la **Maison des Ligues de Lorraine (M2L)**. Ce montage servira de support technique pour l'examen final.

L'infrastructure est segmentée en quatre zones IP distinctes :

### 1. Interconnexion et Sécurité
* **Routeur :** Assure le routage entre les zones et la route par défaut vers l'extérieur.
* **Pare-feu :** Gère le filtrage entre les interfaces et le NAT côté WAN.
* **Réseau Externe :** Liaison via le LAN du lycée en `172.16.0.0/24`.

### 2. Zone M2L "Administration"
* **Contrôleur de domaine :** Serveur Microsoft pour la gestion centralisée.
* **Services Réseau :** Serveur DHCP et DNS pour l'adressage dynamique du parc. (Dans l'AD)
* **Poste Utilisateur :** Un client de test pour valider la connectivité.

### 3. Zone DMZ (Démilitarisée)
* **Serveur Web :** Installation sous Debian avec Apache.

-----

```mermaid
graph LR
    %% Définition du WAN sans subgraph
    WAN["WAN<br/>172.16.0.0/16"]

    WAN -- "DHCP <--> Port 1 (out)" --> FW[("Stormshield")]

    subgraph Management
    FW -- "Port 2 <--> Réseau" --> ADMIN["Admin<br/>172.30.0.0"]
    end

    subgraph LAME[Lame Serveur]
    NIC_DMZ["Carte réseau<br/>Haut à Droite (DMZ)"]
    NIC_LAN["Carte réseau<br/>Bas à Droite (LAN)"]
    
    APACHE["Serveur Apache<br/>192.168.2.1"]
    
    AD["Active Directory<br/>192.168.0.1"]
    GLPI["GLPI<br/>192.168.0.2"]
    FOG["FOG Server<br/>192.168.0.3"]
    end

    subgraph DMZ_Zone[Zone DMZ]
    FW -- "Port 3 <--> 192.168.2.0/24" --> NIC_DMZ
    NIC_DMZ --- APACHE
    end

    FW -- "Port 4 <--> Port 0/0/0<br/>Réseau: 172.19.0.2/16" --> R1(("Routeur Cisco<br/>.1"))
    
    R1 -- "Port 0/0/1 <--> Port Fa1/0/22<br/>VLAN 5<br/>" --> SW["Switch L3<br/>.254"]

    subgraph LAN_Segmentation[Segmentation LAN & Affectation des Ports]
    direction TB
    SW --- V1["VLAN 1<br/>192.168.1.0/24"]
    SW --- V2["VLAN 2<br/>10.0.0.0/8"]
    SW --- V3["VLAN 3<br/>172.18.0.0/16"]
    SW --- V4["VLAN 4 (LAN)<br/>192.168.0.0/24"]
    end

    V4 --- NIC_LAN
    NIC_LAN --- AD
    NIC_LAN --- GLPI
    NIC_LAN --- FOG

    %% Styles
    style WAN fill:#f9f,stroke:#333,stroke-width:2px
    style FW fill:#f66,stroke:#333,stroke-width:2px
    style R1 fill:#69f,stroke:#333,stroke-width:2px
    style SW fill:#9f9,stroke:#333,stroke-width:2px
    style V4 fill:#dfd,stroke:#333
    style ADMIN fill:#eee,stroke:#333,stroke-dasharray: 5 5
    style LAME fill:#fff2cc,stroke:#d6b656,stroke-width:2px
    style APACHE fill:#fff,stroke:#333
    style AD fill:#fff,stroke:#333
    style GLPI fill:#fff,stroke:#333
    style FOG fill:#fff,stroke:#333
```

---
