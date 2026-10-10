# HomeLab
**Project Objective:** 
	To develop and deploy a fully monitored enterprise network environment to simulate real-world cyber attacks and engineering-based detections (Purple Teaming).

```mermaid
graph TD
    subgraph Outside ["Outside / Untrusted Network (VMware NAT)"]
        Attacker["Kali Linux<br/>(Attacker VM)"]
    end

    subgraph Boundary ["Isolation Boundary"]
        Router["Virtual Switch / Gateway"]
    end

    subgraph Inside ["Corporate LAN Segment (Corp-Network)"]
        PC01["Windows 11 Enterprise<br/>(PC01 Endpoint)"]
        DC01["Windows Server 2022<br/>(DC01 Domain Controller)"]
        SIEM["Ubuntu Server 24.04<br/>(Wazuh SIEM / Port 1514)"]

        PC01 -->|Telemetry / Sysmon| SIEM
        DC01 -->|Telemetry / Sysmon| SIEM
        PC01 -.->|Domain Joined| DC01
    end

    Attacker -->|Simulated Attacks| Boundary
    Boundary --> PC01
    Boundary --> DC01
### 1. Hardware & Virtualization Layer
- **Host Machine:** HP OMEN 16 (64GB RAM, Intel i7 Gen 14th).
- **Hypervisor:** VMware Workstation Pro.
- **Network Topology:** 
	Implemented a **Segmented LAN** architecture using VMware LAN Segments to isolate the corporate network from the host machine and the public internet.
### 2. The "Victim" Network (Active Directory)
- **Domain Controller (DC01):** 
	Deployed Windows Server 2022. Configured as a Forest Root Domain (Active Directory Domain Services).
- **Endpoint (PC01):** 
	Deployed Windows 11 Enterprise.
	Performed a **Domain Join** to link the workstation to DC01.
* **Telemetry Layer:**
		Installed **Microsoft Sysmon** on all Windows hosts.
		Applied the **SwiftOnSecurity configuration** to filter noise and capture high-fidelity events (Process Creation, Network Connections, Registry modifications).
### 3. The "SOC" Infrastructure (SIEM)
- **SIEM Platform:** Wazuh (Open-source XDR/SIEM).
- **Server Host:** Ubuntu Server 24.04 LTS (Headless).
- **Implementation:** Performed a successful All-in-One deployment of the Wazuh Indexer, Manager, and Dashboard.
- **Data Pipeline:** Configured the ossec.conf files on Windows agents to bridge **Sysmon Operational logs** into the Wazuh manager for real-time analysis.
