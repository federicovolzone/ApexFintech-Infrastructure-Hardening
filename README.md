# Apex Fintech: Cloud-Native Infrastructure Hardening & SOC Monitoring Lab

[![Language: IT](https://img.shields.io/badge/Language-Italiano-green.svg)](README.it.md)

## 1. Executive Summary & Business Case
**Apex Fintech** is a modern, agile scale-up managing micro-transactions, active session tokens, and sensitive financial user data. Operating under a dynamic tech company philosophy, the infrastructure focuses on optimizing operational expenditures (**OpEx**) through corporate open-source solutions and smart hybrid virtual environments, completely eliminating restrictive upfront software licensing fees (**CapEx**).

The core business architecture relies heavily on Linux systems for high-performance production workloads, paired with centralized Microsoft environments for corporate identity management. 

## 2. Infrastructure Topology & Multi-Zone Segmentation
To guarantee rigorous privilege isolation and stop threat actors from performing lateral movement, the infrastructure is segmented into three distinct networks managed by a perimeter firewall:

*   **WAN (External Audit Zone):** Represents the public internet. It hosts the auditor/threat actor instance running **Kali Linux** to execute controlled assurance assessments and rule validations.
*   **LAN Corporate (Identity Domain):** Dedicated to internal operations. It contains the enterprise identity core (**Windows Server running Active Directory**) and the employee workstation (**Windows 10/11 Pro**), which acts as the main vector for social engineering simulations.
*   **DMZ / Production isolated network:** A hardened zone hosting the transaction **Database Core on Ubuntu Server (CLI-only)**. This machine has zero direct internet exposure.
*   **SOC Monitoring Network:** An isolated subnet hosting the **Wazuh SIEM (Ubuntu Server)**, tasked with collecting encrypted log streams from all corporate endpoints and servers.
## 3. Target Matrix & Resource Allocation (Host: 32 GB RAM)
The high-performance host hardware configuration allows for smooth, simultaneous execution of the entire enterprise topology, ensuring maximum fluid simulation workloads.

| VM Name | Operating System | Operational Role | Allocated RAM | Network Interfaces (VirtualBox) |
| :--- | :--- | :--- | :--- | :--- |
| **pfSense-GW** | FreeBSD / pfSense | Perimeter Firewall / Router | 1 GB | Adapter 1: Bridge (WAN)<br>Adapter 2: Internal Network 1 (LAN)<br>Adapter 3: Internal Network 2 (Prod) |
| **Wazuh-SOC** | Ubuntu Server LTS (CLI) | SIEM / Central SOC Analyzer | 4 GB | Internal Network 1 (LAN Corporate) |
| **DC-WinServer**| Windows Server | Identity Domain Controller (AD) | 4 GB | Internal Network 1 (LAN Corporate) |
| **Client-Win10**| Windows 10/11 Pro | Corporate Workstation (Target) | 4 GB | Internal Network 1 (LAN Corporate) |
| **Fintech-DB**  | Ubuntu Server LTS (CLI) | Core Transaction Database (Target)| 2 GB | Internal Network 2 (Prod Zone) |
| **Auditor-Kali**| Kali Linux | Assurance & Security Auditing | 2 GB | Bridged Adapter (External WAN Simulation)|

---

## 4. Project Roadmap & Implementation Phases
This technical deployment is structured into four distinct execution layers to mirror standard corporate engineering workflows:

*   **Phase 1: Architecture & Network Provisioning** -> Deployment of the pfSense gateway, strict firewall rule enforcement, and DHCP/DNS configuration for isolated zones.
*   **Phase 2: Identity & Endpoint Setup** -> Provisioning of Windows Server, Active Directory Forest deployment, and client onboarding (Windows 10/11 Workstation).
*   **Phase 3: Production Hardening & SIEM Integration** -> Deployment of the Ubuntu Database server, installation of the central Wazuh SIEM, and deployment of security monitoring agents across all endpoints.
*   **Phase 4: Security Assurance & Threat Simulation** -> Execution of controlled testing using Kali Linux to validate rule detection, log forwarding integrity, and incident response alerting capabilities.
