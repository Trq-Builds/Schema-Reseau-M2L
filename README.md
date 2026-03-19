```mermaid
graph LR
    subgraph Internet
        WAN["☁️ WAN<br/>$172.16.0.0/16$"]
    end

    WAN -- "DHCP" --> FW[("🛡️ Stormshield")]

    subgraph DMZ_Zone [Zone DMZ]
        FW -- "dmz (ip statique)<br/>$192.168.0.0/24$" --> DMZ_Srv["🖥️ Serveurs DMZ"]
    end

    FW -- "$172.18.0.0/16$" --> R1(("🔀 Routeur Cisco<br/>.1"))
    
    R1 -- "VLAN 5<br/>$172.17.0.0/16$" --> SW["🔌 Switch L3<br/>.254"]

    subgraph LAN_Segmentation [Segmentation LAN]
        direction TB
        SW --- V1["🆔 VLAN 1<br/>$192.168.1.0/24$"]
        SW --- V2["🆔 VLAN 2<br/>$10.0.0.0/8$"]
        SW --- V3["🆔 VLAN 3<br/>$172.18.0.0/16$"]
        SW --- V4["🗄️ VLAN 4 (Serveurs)<br/>$192.168.0.0/24$"]
    end

    %% Styles
    style WAN fill:#f9f,stroke:#333,stroke-width:2px
    style FW fill:#f66,stroke:#333,stroke-width:2px
    style R1 fill:#69f,stroke:#333,stroke-width:2px
    style SW fill:#9f9,stroke:#333,stroke-width:2px
    style V4 fill:#dfd,stroke:#333
    ```
    
