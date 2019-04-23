---
title: I ruoli, servizi ruolo e funzionalità incluse in Windows Server - Server Core
description: Quali ruoli e funzionalità sono inclusi nell'opzione di installazione Server Core di Windows Server?
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 65729eb68c9590fd6316f5650be48f33c19c926d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859292"
---
# <a name="roles-role-services-and-features-included-in-windows-server---server-core"></a>I ruoli, servizi ruolo e funzionalità incluse in Windows Server - Server Core

> Si applica a: Windows Server (canale semestrale) e Windows Server 2016

Parleremo in genere [cosa del *non* in Server Core](server-core-removed-roles.md) : ora dobbiamo provare un approccio diverso e stabilire cosa del *incluso* e se qualcosa è *installati per impostazione predefinita*. I seguenti ruoli, servizi ruolo e funzionalità siano *in* l'opzione di installazione Server Core di Windows Server. Usare queste informazioni per determinare se l'opzione Server Core funziona per l'ambiente. Poiché si tratta di un elenco di grandi dimensioni, prendere in considerazione la ricerca per i ruoli o funzionalità specifiche si è interessati - se la ricerca non restituisce ciò che sta cercando, non è incluso in Server Core.

Ad esempio, se esegue una ricerca per "Host sessione Desktop remoto" - è non trovarlo in questa pagina. Ciò avviene perché l'Host sessione Desktop remoto non è incluso nell'immagine Server Core.

Tenere presente che è possibile [sempre a somigliare](server-core-removed-roles.md) con quale del *non* incluso. Si tratta semplicemente un modo diverso a guardare le cose.

## <a name="roles-included-in-server-core"></a>Ruoli inclusi in Server Core
L'opzione di installazione Server Core include i seguenti ruoli del server.

| Ruolo                                            | Nome                           | Installato per impostazione predefinita? |
|-------------------------------------------------|--------------------------------|-----------------------|
| Servizi certificati Active Directory           | Certificato di AD                 | N                     |
| Servizi di dominio di Active Directory                | AD-Domain-Services             | N                     |
| Active Directory Federation Services            | ADFS-Federation                | N                     |
| Active Directory Lightweight Directory Services | ADLDS                          | N                     |
| Active Directory Rights Management Services     | ADRMS                          | N                     |
| Attestazione dell'integrità dei dispositivi                       | DeviceHealthAttestationService | N                     |
| Server DHCP                                     | DHCP                           | N                     |
| Server DNS                                      | DNS                            | N                     |
| Servizi file e archiviazione                       | FileAndStorage-Services        | Y                     |
| Servizio Sorveglianza host                           | HostGuardianServiceRole        | N                     |
| Hyper-V                                         | Hyper-V                        | N                     |
| Servizi di stampa e digitalizzazione                     | Print-Services                 | N                     |
| Accesso remoto                                   | RemoteAccess                   | N                     |
| Servizi Desktop remoto                         | Remote-Desktop-Services        | N                     |
| Servizi di attivazione contratti multilicenza                      | VolumeActivation               | N                     |
| Server Web IIS                                  | Web-Server                     | N                     |
| Esperienza Windows Server Essentials            | ServerEssentialsRole           | N                     |
| Windows Server Update Services                  | UpdateServices                 | N                     |

## <a name="role-services-included-in-server-core"></a>Servizi di ruolo inclusi in Server Core
L'opzione di installazione Server Core include i seguenti servizi ruolo.

| Ruolo                                  | Servizio ruolo                                                   | Nome                    | Installato per impostazione predefinita? |
|---------------------------------------|----------------------------------------------------------------|-------------------------|-----------------------|
| Servizi certificati Active Directory | Autorità di certificazione                                        | Autorità di certificazione di servizi certificati Active Directory     | N                     |
|                                       | Servizio Web di informazioni sulle registrazioni di certificati                      | Servizi certificati Active Directory-registrare-Web-Pol     | N                     |
|                                       | Servizio Web di registrazione certificati                             | ADCS-Enroll-Web-Svc     | N                     |
|                                       | Registrazione Web Autorità di certificazione                         | Servizi certificati Active Directory-Web-registrazione     | N                     |
|                                       | Servizio Registrazione dispositivi di rete                              | Servizi certificati Active Directory-dispositivi-registrazione  | N                     |
|                                       | Risponditore online                                               | ADCS-Online-Cert        | N                     |
| Active Directory Rights Management    | Server AD RMS (Active Directory Rights Management Services)                      | ADRMS-Server            | N                     |
|                                       | Supporto federazione identità                                    | ADRMS-Identity          | N                     |
| Servizi file e archiviazione             | Servizi file e iSCSI                                        | File-Services           | N                     |
|                                       | File Server                                                    | FS-FileServer           | N                     |
|                                       | BranchCache per file di rete                                  | FS-BranchCache          | N                     |
|                                       | Deduplicazione dati                                             | Deduplicazione dati di ADFS   | N                     |
|                                       | Spazi dei nomi DFS                                                 | FS-DFS-Namespace        | N                     |
|                                       | Replica DFS                                                | FS-DFS-Replication      | N                     |
|                                       | Gestione risorse file server                                   | FS-Resource-Manager     | N                     |
|                                       | Servizio agente File Server VSS                                  | FS-VSS-Agent            | N                     |
|                                       | Server di destinazione iSCSI                                            | iSCSITarget-Server      | N                     |
|                                       | Provider di archiviazione di destinazione (provider hardware VDS e VSS) iSCSI | iSCSITarget-VSS-VDS     | N                     |
|                                       | Server per NFS                                                 | FS-NFS-Service          | N                     |
|                                       | Cartelle di lavoro                                                   | FS-SyncShareService     | N                     |
|                                       | Servizi di archiviazione                                               | Storage-Services        | Y                     |
| Servizi di stampa e digitalizzazione           | Server di stampa                                                   | Server di stampa            | N                     |
|                                       | Servizio LPD                                                    | Print-LPD-Service       | N                     |
| Accesso remoto                         | DirectAccess e VPN (RAS)                                     | DirectAccess-VPN        | N                     |
|                                       | Routing                                                        | Routing                 | N                     |
|                                       | Proxy applicazione Web                                          | Web-Application-Proxy   | N                     |
| Servizi Desktop remoto               | Gestore connessione Desktop remoto                               | Broker di connessione Servizi Desktop remoto   | N                     |
|                                       | Servizio licenze Desktop remoto                                       | Gestione delle licenze di servizi desktop remoto           | N                     |
|                                       | Host di virtualizzazione Desktop remoto                             | RDS-Virtualization      | N                     |
| Server Web (IIS)                      | Web Server                                                     | Web-WebServer           | N                     |
|                                       | Funzionalità HTTP comuni                                           | Web-Common-Http         | N                     |
|                                       | Documento predefinito                                               | Web-Default-Doc         | N                     |
|                                       | Esplorazione directory                                             | Web-Dir-Browsing        | N                     |
|                                       | Errori HTTP                                                    | Web-Http-Errors         | N                     |
|                                       | Contenuto statico                                                 | Web-Static-Content      | N                     |
|                                       | Reindirizzamento HTTP                                               | Web-Http-Redirect       | N                     |
|                                       | Pubblicazioni WebDAV                                              | Pubblicazione sul Web DAV      | N                     |
|                                       | Integrità e diagnostica                                         | Web-Health              | N                     |
|                                       | Registrazione HTTP                                                   | Web-Http-Logging        | N                     |
|                                       | Registrazioni personalizzate                                                 | Web-personalizzato-Logging      | N                     |
|                                       | Strumenti di registrazione                                                  | Web-Log-Libraries       | N                     |
|                                       | Registrazione ODBC                                                   | Web-ODBC-Logging        | N                     |
|                                       | Monitoraggio richieste                                              | Web-Request-Monitor     | N                     |
|                                       | Traccia                                                        | Web-Http-Tracing        | N                     |
|                                       | Prestazioni                                                    | Web-Performance         | N                     |
|                                       | Compressione contenuto statico                                     | Web-Stat-Compression    | N                     |
|                                       | Compressione contenuto dinamico                                    | Web-Dyn-Compression     | N                     |
|                                       | Sicurezza                                                       | Web-Security            | N                     |
|                                       | Request Filtering                                              | Web-Filtering           | N                     |
|                                       | Autenticazione di base                                           | Web-Basic-Auth          | N                     |
|                                       | Supporto certificati SSL centralizzati                            | Web-CertProvider        | N                     |
|                                       | Autenticazione Mapping certificati client                      | Web-Client-Auth         | N                     |
|                                       | Autenticazione del digest                                          | Web-Digest-Auth         | N                     |
|                                       | Autenticazione Mapping certificati Client IIS                  | Web-Cert-Auth           | N                     |
|                                       | Restrizioni per IP e dominio                                     | Web-IP-Security         | N                     |
|                                       | Autorizzazione URL                                              | Web-Url-Auth            | N                     |
|                                       | Autenticazione di Windows                                         | Web-Windows-Auth        | N                     |
|                                       | Sviluppo di applicazioni                                        | Web-App-Dev             | N                     |
|                                       | Estendibilità .NET 3.5                                         | Web-Net-Ext             | N                     |
|                                       | Estendibilità .NET 4.6                                         | Web-Net-Ext45           | N                     |
|                                       | Inizializzazione dell'applicazione                                     | Web-AppInit             | N                     |
|                                       | ASP                                                            | Web-ASP                 | N                     |
|                                       | ASP.NET 3.5                                                    | Web-Asp: Net             | N                     |
|                                       | ASP.NET 4.6                                                    | Web-Asp-Net45           | N                     |
|                                       | CGI                                                            | Web-CGI                 | N                     |
|                                       | Estensioni ISAPI                                               | Web-ISAPI-Ext           | N                     |
|                                       | ISAPI Filters                                                  | Web-ISAPI-Filter        | N                     |
|                                       | Server Side Includes                                           | Web include            | N                     |
|                                       | Protocollo WebSocket                                             | Web-WebSockets          | N                     |
|                                       | Server FTP                                                     | Web-Ftp-Server          | N                     |
|                                       | Servizio FTP                                                    | Web-Ftp-Service         | N                     |
|                                       | Estendibilità FTP                                              | Web-Ftp-Ext             | N                     |
|                                       | Strumenti di gestione                                               | Web-Mgmt-Tools          | N                     |
|                                       | Compatibilità gestione IIS 6                                 | Web-Mgmt-compatibilità         | N                     |
|                                       | Compatibilità Metabase IIS 6                                   | Web-Metabase            | N                     |
|                                       | Strumenti di Scripting 6 di IIS                                          | Web Lgcy-creazione di script      | N                     |
|                                       | Compatibilità WMI IIS 6                                        | Web-WMI                 | N                     |
|                                       | Strumenti e script di gestione IIS                               | Web--strumenti di script     | N                     |
|                                       | Servizio di gestione                                             | Web-Mgmt-Service        | N                     |
| Windows Server Update Services        | Connettività di database interno di Windows                                               | UpdateServices-WidDB    | N                     |
|                                       | Servizi WSUS                                                  | UpdateServices-Services | N                     |
|                                       | Connettività di SQL Server                                        | UpdateServices-DB       | N                     |

## <a name="features-included-in-server-core"></a>Funzionalità incluse in Server Core
L'opzione di installazione Server Core include le funzionalità seguenti.

| Funzionalità                                                | Nome                               | Installato per impostazione predefinita? |
|--------------------------------------------------------|------------------------------------|-----------------------|
| Funzionalità di .NET framework 3.5                            | Funzionalità di NET Framework             | N                     |
| .NET framework 3.5 (include .NET 2.0 e 3.0)       | NET-Framework-Core                 | (rimossi)             |
| Attivazione HTTP                                        | NET-HTTP-Activation                | N                     |
| Attivazione non HTTP                                    | NET-Non-HTTP-Activ                 | N                     |
| Funzionalità di .NET framework 4.6                            | 45-funzionalità-NET-Framework          | Y                     |
| .NET framework 4.6                                     | NET-Framework-45-Core              | Y                     |
| ASP.NET 4.6                                            | NET-Framework-45-ASPNET            | N                     |
| Servizi WCF                                           | NET-WCF-Services45                 | Y                     |
| Attivazione HTTP                                        | NET-WCF-HTTP-Activation45          | N                     |
| Attivazione Accodamento messaggi (MSMQ)                      | NET-WCF-MSMQ-Activation45          | N                     |
| Attivazione Named Pipe                                  | NET-WCF-Pipe-Activation45          | N                     |
| Attivazione TCP                                         | NET-WCF-TCP-Activation45           | N                     |
| Condivisione delle porte TCP                                       | NET-WCF-TCP-PortSharing45          | Y                     |
| Servizio trasferimento intelligente in background (BITS)         | BITS                               | N                     |
| Compact Server                                         | BITS-Compact-Server                | N                     |
| Crittografia unità BitLocker                             | BitLocker                          | N                     |
| BranchCache                                            | BranchCache                        | N                     |
| Client per NFS                                         | NFS-Client                         | N                     |
| Contenitori                                             | Contenitori                         | N                     |
| Data Center Bridging                                   | Data-Center-Bridging               | N                     |
| Archiviazione avanzata                                       | EnhancedStorage                    | N                     |
| Clustering di failover                                    | Clustering di failover                | N                     |
| Gestione Criteri di gruppo                                | Console Gestione Criteri di gruppo                               | N                     |
| QoS (Quality of Service) I/O                                 | DiskIo-QoS                         | N                     |
| HWC (Hostable Web Core) di IIS                                  | Web-WHC                            | N                     |
| Server di Gestione indirizzi IP                    | Gestione indirizzi IP                               | N                     |
| Servizio server iSNS                                    | ISNS                               | N                     |
| Estensione IIS OData gestione                         | ManagementOdata                    | N                     |
| Media Foundation                                       | Server-Media-Foundation            | N                     |
| Accodamento messaggi                                        | MSMQ                               | N                     |
| Servizi Accodamento messaggi                               | MSMQ-Services                      | N                     |
| Server di Accodamento messaggi                                 | MSMQ-Server                        | N                     |
| Integrazione servizio directory                          | MSMQ-Directory                     | N                     |
| Supporto HTTP                                           | Supporto HTTP MSMQ                  | N                     |
| Trigger Accodamento messaggi                               | MSMQ-Triggers                      | N                     |
| Servizio di routing                                        | MSMQ-Routing                       | N                     |
| Proxy DCOM di Accodamento messaggi                             | MSMQ-DCOM                          | N                     |
| Multipath I/O                                          | Multipath i/o                       | N                     |
| MultiPoint Connector                                   | MultiPoint-Connector               | N                     |
| Connettore multiPoint Services                          | MultiPoint-Connector-Services      | N                     |
| Selezionare Gestione multiPoint e MultiPoint Dashboard            | MultiPoint-Tools                   | N                     |
| Bilanciamento carico di rete                                 | NLB                                | N                     |
| Protocollo PNRP (Peer Name Resolution Protocol)                          | PNRP                               | N                     |
| Servizio audio/video Windows di qualità                 | qWave                              | N                     |
| Compressione differenziale remota                        | RDC                                | N                     |
| Strumenti di amministrazione server remoto                     | Strumenti di amministrazione remota del server                               | N                     |
| Strumenti di amministrazione funzionalità                           | RSAT-Feature-Tools                 | N                     |
| Utilità di amministrazione di crittografia unità BitLocker  | RSAT-Feature-Tools-BitLocker       | N                     |
| Strumenti LLDP DataCenterBridging                          | RSAT-DataCenterBridging-LLDP-Tools | N                     |
| Strumenti per Clustering di failover                              | RSAT-Clustering                    | N                     |
| Modulo Cluster di failover per Windows PowerShell         | RSAT-Clustering-PowerShell         | N                     |
| Server di automazione di Cluster di failover                     | RSAT-Clustering-AutomationServer   | N                     |
| Interfaccia di comando Cluster di failover                     | RSAT-Clustering-CmdInterface       | N                     |
| Client di gestione indirizzi IP indirizzo IP                    | IPAM-Client-Feature                | N                     |
| Strumenti VM schermata                                      | RSAT-Shielded-VM-Tools             | N                     |
| Modulo Replica di archiviazione per Windows PowerShell          | RSAT-Storage-Replica               | N                     |
| Strumenti di amministrazione ruoli                              | RSAT-Role-Tools                    | N                     |
| Strumenti per Servizi di dominio Active Directory e AD LDS                                 | RSAT-AD-Tools                      | N                     |
| Modulo di Active Directory per Windows PowerShell         | RSAT-AD-PowerShell                 | N                     |
| Strumenti di Servizi di dominio Active Directory                                            | RSAT-ADDS                          | N                     |
| Centro di amministrazione di Active Directory                 | RSAT-AD-AdminCenter                | N                     |
| Snap-in e strumenti da riga di comando di Servizi di dominio Active Directory                  | Amministrazione remota del server-aggiunge-Tools                    | N                     |
| Strumenti da riga di comando e gli Snap-in AD LDS                 | RSAT-ADLDS                         | N                     |
| Strumenti di gestione Hyper-V                               | RSAT-Hyper-V-Tools                 | N                     |
| Modulo Hyper-V per Windows PowerShell                  | Hyper-V-PowerShell                 | N                     |
| Strumenti di Windows Server Update Services                   | UpdateServices-RSAT                | N                     |
| Cmdlet di PowerShell e API                             | UpdateServices-API                 | N                     |
| Strumenti per Server DHCP                                      | RSAT-DHCP                          | N                     |
| Strumenti per Server DNS                                       | RSAT-DNS-Server                    | N                     |
| Strumenti di gestione accesso remoto                         | RSAT-RemoteAccess                  | N                     |
| Modulo Accesso remoto per Windows PowerShell            | RSAT-RemoteAccess-PowerShell       | N                     |
| Proxy RPC su HTTP                                    | RPC-over-HTTP-Proxy                | N                     |
| Raccolta eventi di configurazione e avvio                        | Il programma di installazione-e-Boot-eventi-raccolta    | N                     |
| Servizi TCP/IP semplificati                                 | Simple-TCPIP                       | N                     |
| Supporto condivisione di file SMB 1.0/CIFS                      | FS-SMB1                            | Y                     |
| Limite larghezza di banda SMB                                    | FS-SMBBW                           | N                     |
| Servizio SNMP                                           | SNMP-Service                       | N                     |
| Provider WMI per SNMP                                      | SNMP-WMI-Provider                  | N                     |
| Client Telnet                                          | Telnet-Client                      | N                     |
| Strumenti di schermatura VM per la gestione dell'infrastruttura               | FabricShieldedTools                | N                     |
| Funzionalità di Windows Defender                              | Windows-Defender-Features          | Y                     |
| Windows Defender                                       | Windows-Defender                   | Y                     |
| Database interno di Windows                              | Windows Internal Database          | N                     |
| Windows PowerShell                                     | PowerShellRoot                     | Y                     |
| Windows PowerShell 5.1                                 | PowerShell                         | Y                     |
| Windows PowerShell 2.0 Engine                          | PowerShell-V2                      | (rimossi)             |
| Windows PowerShell Desired State Configuration Service | DSC-Service                        | N                     |
| Accesso Web di Windows PowerShell                          | WindowsPowerShellWebAccess         | N                     |
| Servizio Attivazione dei processi Windows                     | WAS                                | N                     |
| Modello di processo                                          | Modello di processo è stato                  | N                     |
| Ambiente .NET 3.5                                   | È stata-NET-Environment                | N                     |
| API di configurazione                                     | WAS-Config-APIs                    | N                     |
| Windows Server Backup                                  | Windows-Server-Backup              | N                     |
| Strumenti di migrazione per Windows Server                         | Migrazione                          | N                     |
| Gestione archiviazione basata su standard Windows             | WindowsStorageManagementService    | N                     |
| Estensione IIS di Gestione remota Windows                                    | WinRM-IIS-Ext                      | N                     |
| Server WINS                                            | WINS                               | N                     |
| Supporto di WoW64                                          | Supporto di WoW64                      | Y                     |
|                                                        |                                    |                       |
