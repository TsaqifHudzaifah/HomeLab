# HomeLab
Custom SIEM lab built with Wazuh, Windows Server, and Kali Linux for hands-on security monitoring and threat research.

```mermaid
graph TD
    subgraph Outside ["Outside / Untrusted Network (VMware NAT)"]
        Attacker["🥷 Kali Linux<br/>(Attacker VM)"]
    end

    subgraph Boundary ["Isolation Boundary"]
        Router["Virtual Switch / Gateway"]
    end

    subgraph Inside ["Corporate LAN Segment (Corp-Network)"]
        PC01["💻 Windows 11 Enterprise<br/>(PC01 Endpoint)"]
        DC01["🗄️ Windows Server 2022<br/>(DC01 Domain Controller)"]
        SIEM["🧠 Ubuntu Server 24.04<br/>(Wazuh SIEM / Port 1514)"]

        PC01 -->|Telemetry / Sysmon| SIEM
        DC01 -->|Telemetry / Sysmon| SIEM
        PC01 -.->|Domain Joined| DC01
    end

    Attacker -->|Simulated Attacks| Boundary
    Boundary --> PC01
    Boundary --> DC01
