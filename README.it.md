# Apex Fintech: Hardening delle Infrastrutture Cloud-Native e Laboratorio di Monitoraggio SOC

[![Language: EN](https://img.shields.io/badge/Language-English-blue.svg)](README.md)

## 1. Sintesi Esecutiva e Business Case
**Apex Fintech** è una scale-up moderna e snella che gestisce micro-transazioni, token di sessione attivi e dati finanziari sensibili degli utenti. Operando secondo la filosofia delle tech company native, l'infrastruttura si concentra sull'ottimizzazione delle spese operative (**OpEx**) attraverso soluzioni open-source di livello aziendale e ambienti virtuali ibridi, eliminando completamente i costi fissi delle licenze software proprietarie (**CapEx**).

L'architettura aziendale si affida a sistemi Linux per i carichi di produzione ad alte prestazioni, affiancati da ambienti Microsoft centralizzati per la gestione dell'identità aziendale.

## 2. Topologia dell'Infrastruttura e Segmentazione Multi-Zona
Per garantire un rigoroso isolamento dei privilegi e impedire agli attori di minaccia di effettuare movimenti laterali, l'infrastruttura è segmentata in tre reti distinte gestite da un firewall perimetrale:

*   **WAN (Zona di Audit Esterna):** Rappresenta l'internet pubblico. Ospita l'istanza dell'auditor/attaccante con **Kali Linux** per eseguire test di sicurezza controllati e validare le regole di rilevamento.
*   **LAN Corporate (Dominio di Identità):** Dedicata alle operazioni interne. Contiene il cuore dell'identità aziendale (**Windows Server con Active Directory**) e la workstation del dipendente (**Windows 10/11 Pro**), che funge da vettore principale per le simulazioni di social engineering.
*   **DMZ / Rete Isolata di Produzione:** Una zona blindata che ospita il **Database Core transazionale su Ubuntu Server (solo riga di comando)**. Questa macchina non ha un'esposizione diretta a internet.
*   **Rete di Monitoraggio SOC:** Una sottorete isolata che ospita il **SIEM Wazuh (Ubuntu Server)**, incaricato di raccogliere i flussi di log cifrati da tutti gli endpoint e i server aziendali.
*   ## 3. Matrice dei Target e Allocazione Risorse (Host: 32 GB RAM)
La configurazione hardware ad alte prestazioni dell'host consente l'esecuzione fluida e simultanea dell'intera topologia aziendale, garantendo la massima stabilità durante i carichi di simulazione.

| Nome VM | Sistema Operativo | Ruolo Operativo | RAM Allocata | Interfacce di Rete (VirtualBox) |
| :--- | :--- | :--- | :--- | :--- |
| **pfSense-GW** | FreeBSD / pfSense | Firewall Perimetrale / Router | 1 GB | Adattatore 1: Bridge (WAN)<br>Adattatore 2: Rete Interna 1 (LAN)<br>Adattatore 3: Rete Interna 2 (Prod) |
| **Wazuh-SOC** | Ubuntu Server LTS (CLI) | SIEM / Analizzatore Centrale SOC | 4 GB | Rete Interna 1 (LAN Corporate) |
| **DC-WinServer**| Windows Server | Domain Controller Identità (AD) | 4 GB | Rete Interna 1 (LAN Corporate) |
| **Client-Win10**| Windows 10/11 Pro | Workstation Aziendale (Target) | 4 GB | Rete Interna 1 (LAN Corporate) |
| **Fintech-DB**  | Ubuntu Server LTS (CLI) | Database Transazionale Core (Target)| 2 GB | Rete Interna 2 (Prod Zone) |
| **Auditor-Kali**| Kali Linux | Assurance & Security Auditing | 2 GB | Scheda con bridge (Simulazione WAN Esterna)|

---

## 4. Tabella di Marcia del Progetto e Fasi di Implementazione
Questo deployment tecnico è strutturato in quattro fasi distinte per riflettere i flussi di lavoro standard dell'ingegneria aziendale:

*   **Fase 1: Architettura e Provisioning di Rete** -> Configurazione del gateway pfSense, applicazione di regole rigide del firewall e gestione DHCP/DNS per le zone isolate.
*   **Fase 2: Configurazione Identità ed Endpoint** -> Installazione di Windows Server, configurazione della Foresta Active Directory e aggiunta del client (Workstation Windows 10/11) al dominio.
*   **Fase 3: Hardening della Produzione e Integrazione SIEM** -> Configurazione del server Database su Ubuntu, installazione del SIEM centrale Wazuh e deployment degli agenti di monitoraggio su tutti gli endpoint.
*   **Fase 4: Security Assurance e Simulazione delle Minacce** -> Esecuzione di test controllati tramite Kali Linux per convalidare il rilevamento delle regole, l'integrità dei log e i sistemi di alert per l'incident response.
