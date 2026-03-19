# M2L

```graph LR
    subgraph Internet
        WAN["☁️ WAN<br/>$172.16.0.0/16$"]
    end

    WAN -- "DHCP (Interface 'out')" --> FW[("🛡️ Stormshield")]

    subgraph DMZ_Zone [Zone DMZ]
        FW -- "IP Statique<br/>$192.168.0.0/24$" --> DMZ["🖥️ Serveurs DMZ"]
    end

    FW -- "$172.18.0.0/16$" --> R1(("🔀 Routeur Cisco"))
    
    R1 -- "$172.17.0.0/16$" --> SW["🔌 Switch L3"]

    subgraph LAN [Réseau Local]
        SW --- PC1["💻 Clients"]
        SW --- PC2["🖨️ Imprimantes"]
    end

    %% Styles
    style WAN fill:#f9f,stroke:#333,stroke-width:2px
    style FW fill:#f66,stroke:#333,stroke-width:2px
    style R1 fill:#69f,stroke:#333,stroke-width:2px
    style SW fill:#9f9,stroke:#333,stroke-width:2px
    ```
