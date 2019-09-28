---
title: Ruoli, servizi ruolo e funzionalità inclusi in Windows Server-Server Core
description: Quali ruoli e funzionalità sono inclusi nell'opzione di installazione dei componenti di base del server di Windows Server?
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 7b5d5d5ad38b1b03e409c26485860f43799f1322
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383327"
---
# <a name="roles-role-services-and-features-included-in-windows-server---server-core"></a>Ruoli, servizi ruolo e funzionalità inclusi in Windows Server-Server Core

> Si applica a: Windows Server 2019, Windows Server 2016 e Windows Server (canale semestrale)

Si tratta in genere di [ciò che *non* si trova in Server Core](server-core-removed-roles.md) . ora si proverà a usare un approccio diverso e si dirà cosa è *incluso* e se *per impostazione predefinita viene installato*un elemento. I ruoli, i servizi ruolo e le funzionalità seguenti sono disponibili *nell'* opzione di installazione dei componenti di base del server di Windows Server. Usare queste informazioni per determinare se l'opzione dei componenti di base del server funziona per l'ambiente. Poiché si tratta di un elenco di grandi dimensioni, provare a cercare il ruolo o la funzionalità specifica a cui si è interessati. se la ricerca non restituisce ciò che si sta cercando, non è inclusa in Server Core.

Ad esempio, se si cerca "host sessione Desktop remoto", non sarà possibile trovarlo in questa pagina. Questo perché l'host sessione Desktop remoto non è incluso nell'immagine dei componenti di base del server.

Tenere presente che è [sempre possibile esaminare](server-core-removed-roles.md) ciò che *non* è incluso. Si tratta solo di un modo diverso per esaminare gli elementi.

## <a name="roles-included-in-server-core"></a>Ruoli inclusi in Server Core
L'opzione di installazione dei componenti di base del server include i ruoli del server seguenti.

| Role                                            | Nome                           | Installato per impostazione predefinita? |
|-------------------------------------------------|--------------------------------|-----------------------|
| Servizi certificati Active Directory           | Certificato AD                 | N                     |
| Servizi di dominio di Active Directory                | Servizi di dominio Active Directory             | N                     |
| Active Directory Federation Services            | ADFS-Federazione                | N                     |
| Active Directory Lightweight Directory Services | ADLDS                          | N                     |
| Active Directory Rights Management Services     | ADRMS                          | N                     |
| Attestazione dell'integrità dei dispositivi                       | DeviceHealthAttestationService | N                     |
| Server DHCP                                     | DHCP                           | N                     |
| Server DNS                                      | DNS                            | N                     |
| Servizi file e archiviazione                       | FileAndStorage-servizi        | Y                     |
| Servizio Sorveglianza host                           | HostGuardianServiceRole        | N                     |
| Hyper-V                                         | Hyper-V                        | N                     |
| Servizi di stampa e digitalizzazione                     | Servizi di stampa                 | N                     |
| Accesso remoto                                   | RemoteAccess                   | N                     |
| Servizi Desktop remoto                         | Servizi Desktop remoto        | N                     |
| Servizi di attivazione contratti multilicenza                      | VolumeActivation               | N                     |
| Server Web IIS                                  | Server Web                     | N                     |
| Esperienza Windows Server Essentials            | ServerEssentialsRole           | N                     |
| Windows Server Update Services                  | UpdateServices                 | N                     |

## <a name="role-services-included-in-server-core"></a>Servizi ruolo inclusi in Server Core
L'opzione di installazione dei componenti di base del server include i servizi ruolo seguenti.

| Role                                  | Servizio ruolo                                                   | Nome                    | Installato per impostazione predefinita? |
|---------------------------------------|----------------------------------------------------------------|-------------------------|-----------------------|
| Servizi certificati Active Directory | Autorità di certificazione                                        | ADC-CERT-Authority     | N                     |
|                                       | Servizio Web di informazioni sulle registrazioni di certificati                      | ADC-registra-Web-pol     | N                     |
|                                       | Servizio Web di registrazione certificati                             | ADC-registra-Web-SVC     | N                     |
|                                       | Registrazione Web Autorità di certificazione                         | ADC-registrazione Web     | N                     |
|                                       | Servizio Registrazione dispositivi di rete                              | ADC-registrazione dispositivo  | N                     |
|                                       | Risponditore online                                               | ADC-online-CERT        | N                     |
| Active Directory Rights Management    | Server AD RMS (Active Directory Rights Management Services)                      | ADRMS-server            | N                     |
|                                       | Supporto federazione identità                                    | ADRMS-identità          | N                     |
| Servizi file e archiviazione             | Servizi file e iSCSI                                        | Servizi file           | N                     |
|                                       | File Server                                                    | FS-FileServer           | N                     |
|                                       | BranchCache per file di rete                                  | FS-BranchCache          | N                     |
|                                       | Deduplicazione dati                                             | FS-Deduplicazione dati   | N                     |
|                                       | Spazi dei nomi DFS                                                 | FS-DFS-spazio dei nomi        | N                     |
|                                       | Replica DFS                                                | FS-replica DFS      | N                     |
|                                       | Gestione risorse file server                                   | FS-Resource-Manager     | N                     |
|                                       | Servizio agente File Server VSS                                  | FS-VSS-Agent            | N                     |
|                                       | Server di destinazione iSCSI                                            | iSCSITarget-server      | N                     |
|                                       | Provider di archiviazione di destinazione iSCSI (provider hardware VDS e VSS) | iSCSITarget-VSS-VDS     | N                     |
|                                       | Server per NFS                                                 | FS-NFS-servizio          | N                     |
|                                       | Cartelle di lavoro                                                   | FS-SyncShareService     | N                     |
|                                       | Servizi di archiviazione                                               | Servizi di archiviazione        | Y                     |
| Servizi di stampa e digitalizzazione           | Server di stampa                                                   | Server di stampa            | N                     |
|                                       | Servizio LPD                                                    | Print-LPD-Service       | N                     |
| Accesso remoto                         | DirectAccess e VPN (RAS)                                     | DirectAccess-VPN        | N                     |
|                                       | Routing                                                        | Routing                 | N                     |
|                                       | Proxy applicazione Web                                          | Web-application-proxy   | N                     |
| Servizi Desktop remoto               | Gestore connessione Desktop remoto                               | Connessione Servizi Desktop remoto   | N                     |
|                                       | Servizio licenze Desktop remoto                                       | Gestione licenze Servizi Desktop remoto           | N                     |
|                                       | Host di virtualizzazione Desktop remoto                             | Virtualizzazione RDS      | N                     |
| Server Web (IIS)                      | Web Server                                                     | Web-webserver           | N                     |
|                                       | Funzionalità HTTP comuni                                           | Web-comune-http         | N                     |
|                                       | Documento predefinito                                               | Web-predefinito-doc         | N                     |
|                                       | Esplorazione directory                                             | Esplorazione del Web-dir        | N                     |
|                                       | Errori HTTP                                                    | Web-http-errori         | N                     |
|                                       | Contenuto statico                                                 | Web-statico-contenuto      | N                     |
|                                       | Reindirizzamento HTTP                                               | Web-http-Reindirizzamento       | N                     |
|                                       | Pubblicazione WebDAV                                              | Web-DAV-Publishing      | N                     |
|                                       | Integrità e diagnostica                                         | Web-integrità              | N                     |
|                                       | Registrazione HTTP                                                   | Registrazione HTTP Web        | N                     |
|                                       | Registrazione personalizzata                                                 | Web-Custom-Logging      | N                     |
|                                       | Strumenti di registrazione                                                  | Web-Log-Libraries       | N                     |
|                                       | Registrazione ODBC                                                   | Web-ODBC-registrazione        | N                     |
|                                       | Monitoraggio richieste                                              | Web-Request-monitor     | N                     |
|                                       | Traccia                                                        | Web-http-Tracing        | N                     |
|                                       | Prestazioni                                                    | Prestazioni Web         | N                     |
|                                       | Compressione contenuto statico                                     | Web-Stat-Compression    | N                     |
|                                       | Compressione contenuto dinamico                                    | Web-Dyn-Compression     | N                     |
|                                       | Security                                                       | Sicurezza Web            | N                     |
|                                       | Request Filtering                                              | Filtro Web           | N                     |
|                                       | Autenticazione di base                                           | Web-Basic-Auth          | N                     |
|                                       | Supporto certificato SSL centralizzato                            | CertProvider Web        | N                     |
|                                       | Autenticazione mapping certificati client                      | Web-client-auth         | N                     |
|                                       | Autenticazione del digest                                          | Web-Digest-Auth         | N                     |
|                                       | Autenticazione mapping certificati client IIS                  | Web-CERT-auth           | N                     |
|                                       | Restrizioni per IP e domini                                     | Web-IP-sicurezza         | N                     |
|                                       | Autorizzazione URL                                              | Web-URL-auth            | N                     |
|                                       | Autenticazione di Windows                                         | Web-Windows-autenticazione        | N                     |
|                                       | Sviluppo di applicazioni                                        | Web-App-dev             | N                     |
|                                       | Estendibilità .NET 3,5                                         | Web-NET-EXT             | N                     |
|                                       | Estendibilità .NET 4,6                                         | Web-NET-Ext45           | N                     |
|                                       | Inizializzazione dell'applicazione                                     | AppInit Web             | N                     |
|                                       | ASP                                                            | Web-ASP                 | N                     |
|                                       | ASP.NET 3,5                                                    | Web-ASP-NET             | N                     |
|                                       | ASP.NET 4,6                                                    | Web-ASP-Net45           | N                     |
|                                       | CGI                                                            | Web-CGI                 | N                     |
|                                       | Estensioni ISAPI                                               | Web-ISAPI-Ext           | N                     |
|                                       | ISAPI Filters                                                  | Web-ISAPI-filtro        | N                     |
|                                       | Server Side Includes                                           | Web-inclusioni            | N                     |
|                                       | Protocollo WebSocket                                             | Web-WebSocket          | N                     |
|                                       | Server FTP                                                     | Web-FTP-Server          | N                     |
|                                       | Servizio FTP                                                    | Web-FTP-Service         | N                     |
|                                       | Estendibilità FTP                                              | Web-FTP-EXT             | N                     |
|                                       | Strumenti di gestione                                               | Web-Mgmt-Tools          | N                     |
|                                       | Compatibilità gestione IIS 6                                 | Web-Mgmt-compat         | N                     |
|                                       | Compatibilità metabase IIS 6                                   | Web-metabase            | N                     |
|                                       | Strumenti di script di IIS 6                                          | Web-Lgcy-scripting      | N                     |
|                                       | Compatibilità WMI con IIS 6                                        | Web-WMI                 | N                     |
|                                       | Strumenti e script di gestione IIS                               | Script Web-strumenti     | N                     |
|                                       | Servizio di gestione                                             | Web-Mgmt-Service        | N                     |
| Windows Server Update Services        | Connettività WID                                               | UpdateServices-WidDB    | N                     |
|                                       | Servizi WSUS                                                  | UpdateServices-servizi | N                     |
|                                       | Connettività SQL Server                                        | UpdateServices-DB       | N                     |

## <a name="features-included-in-server-core"></a>Funzionalità incluse in Server Core
L'opzione di installazione dei componenti di base del server include le funzionalità seguenti.

| Funzionalità                                                | Nome                               | Installato per impostazione predefinita? |
|--------------------------------------------------------|------------------------------------|-----------------------|
| Funzionalità di .NET Framework 3,5                            | NET-Framework-funzionalità             | N                     |
| .NET Framework 3,5 (include .NET 2,0 e 3,0)       | NET-Framework-Core                 | rimosso             |
| Attivazione HTTP                                        | NET-HTTP-Activation                | N                     |
| Attivazione non HTTP                                    | NET-non-HTTP-Activ                 | N                     |
| Funzionalità di .NET Framework 4,6                            | NET-Framework-45-funzionalità          | Y                     |
| .NET framework 4.6                                     | NET-Framework-45-Core              | Y                     |
| ASP.NET 4,6                                            | NET-Framework-45-ASPNET            | N                     |
| Servizi WCF                                           | NET-WCF-Services45                 | Y                     |
| Attivazione HTTP                                        | NET-WCF-HTTP-Activation45          | N                     |
| Attivazione di Accodamento messaggi (MSMQ)                      | NET-WCF-MSMQ-Activation45          | N                     |
| Attivazione named pipe                                  | NET-WCF-pipe-Activation45          | N                     |
| Attivazione TCP                                         | NET-WCF-TCP-Activation45           | N                     |
| Condivisione delle porte TCP                                       | NET-WCF-TCP-PortSharing45          | Y                     |
| Servizio trasferimento intelligente in background (BITS)         | BITS                               | N                     |
| Compact Server                                         | BITS-Compact-Server                | N                     |
| Crittografia unità BitLocker                             | BitLocker                          | N                     |
| BranchCache                                            | BranchCache                        | N                     |
| Client per NFS                                         | NFS-client                         | N                     |
| Contenitori                                             | Contenitori                         | N                     |
| Data Center Bridging                                   | Data Center-Bridging               | N                     |
| Archiviazione avanzata                                       | EnhancedStorage                    | N                     |
| Clustering di failover                                    | Failover-clustering                | N                     |
| Gestione Criteri di gruppo                                | Console Gestione Criteri di gruppo                               | N                     |
| QoS (Quality of Service) I/O                                 | DiskIo-QoS                         | N                     |
| HWC (Hostable Web Core) di IIS                                  | WHC Web                            | N                     |
| Server di Gestione indirizzi IP                    | Gestione indirizzi IP                               | N                     |
| Servizio server iSNS                                    | ISNS                               | N                     |
| Estensione IIS OData gestione                         | ManagementOdata                    | N                     |
| Media Foundation                                       | Server-Media-Foundation            | N                     |
| Accodamento messaggi                                        | MSMQ                               | N                     |
| Servizi di Accodamento messaggi                               | MSMQ-servizi                      | N                     |
| Server di Accodamento messaggi                                 | Server MSMQ                        | N                     |
| Integrazione servizio directory                          | MSMQ-directory                     | N                     |
| Supporto HTTP                                           | MSMQ-HTTP-supporto                  | N                     |
| Trigger di Accodamento messaggi                               | MSMQ-trigger                      | N                     |
| Servizio di routing                                        | Routing MSMQ                       | N                     |
| Proxy DCOM di Accodamento messaggi                             | MSMQ-DCOM                          | N                     |
| Multipath I/O                                          | Percorsi multipli-IO                       | N                     |
| MultiPoint Connector                                   | Connettore MultiPoint               | N                     |
| Servizi MultiPoint Connector                          | MultiPoint-Connector-Services      | N                     |
| Gestione MultiPoint e Dashboard MultiPoint            | MultiPoint-Tools                   | N                     |
| Bilanciamento carico di rete                                 | NLB                                | N                     |
| Protocollo PNRP (Peer Name Resolution Protocol)                          | PNRP                               | N                     |
| Servizio audio/video Windows di qualità                 | qWave                              | N                     |
| Compressione differenziale remota                        | RDC                                | N                     |
| Strumenti di amministrazione server remoto                     | Strumenti di amministrazione remota del server                               | N                     |
| Strumenti di amministrazione funzionalità                           | Strumenti di amministrazione remota del server-funzionalità                 | N                     |
| Utilità di amministrazione di Crittografia unità BitLocker  | Strumenti di amministrazione remota del server-funzionalità-BitLocker       | N                     |
| Strumenti LLDP DataCenterBridging                          | Strumenti di amministrazione remota del server-DataCenterBridging-LLDP | N                     |
| Strumenti per Clustering di failover                              | Amministrazione remota del cluster                    | N                     |
| Modulo cluster di failover per Windows PowerShell         | RSAT-Clustering-PowerShell         | N                     |
| Server di automazione del cluster di failover                     | Amministrazione remota del cluster-AutomationServer   | N                     |
| Interfaccia del comando cluster di failover                     | Amministrazione remota del cluster-CmdInterface       | N                     |
| Client di gestione indirizzi IP                    | Gestione indirizzi IP-client-Feature                | N                     |
| Strumenti di VM schermate                                      | Strumenti di amministrazione remota del Server-VM-schermate             | N                     |
| Modulo replica di archiviazione per Windows PowerShell          | Amministrazione remota di archiviazione               | N                     |
| Strumenti di amministrazione ruoli                              | Strumenti di ruolo-strumenti di amministrazione remota                    | N                     |
| Strumenti per Servizi di dominio Active Directory e AD LDS                                 | Strumenti di amministrazione remota del server                      | N                     |
| Modulo di Active Directory per Windows PowerShell         | Amministrazione remota di Active Directory-PowerShell                 | N                     |
| Strumenti di Servizi di dominio Active Directory                                            | STRUMENTI DI AMMINISTRAZIONE REMOTA DEL SERVER-AGGIUNTA                          | N                     |
| Centro di amministrazione di Active Directory                 | Amministrazione remota di Active Directory-AdminCenter                | N                     |
| Snap-in e strumenti da riga di comando di Servizi di dominio Active Directory                  | Strumenti di amministrazione remota del server                    | N                     |
| AD LDS snap-in e strumenti da riga di comando                 | STRUMENTI DI AMMINISTRAZIONE REMOTA (ADLDS)                         | N                     |
| Strumenti di gestione Hyper-V                               | Strumenti di amministrazione remota del server-Hyper-V                 | N                     |
| Modulo Hyper-V per Windows PowerShell                  | Hyper-V-PowerShell                 | N                     |
| Strumenti di Windows Server Update Services                   | UpdateServices-strumenti di amministrazione remota                | N                     |
| API e cmdlet di PowerShell                             | UpdateServices-API                 | N                     |
| Strumenti server DHCP                                      | STRUMENTI DI AMMINISTRAZIONE REMOTA DEL SERVER                          | N                     |
| Strumenti server DNS                                       | Strumenti di amministrazione remota del server-DNS                    | N                     |
| Strumenti di gestione di accesso remoto                         | Strumenti di amministrazione remota (RemoteAccess)                  | N                     |
| Modulo Accesso remoto per Windows PowerShell            | Strumenti di amministrazione remota del server-RemoteAccess-PowerShell       | N                     |
| Proxy RPC su HTTP                                    | RPC-over-HTTP-proxy                | N                     |
| Raccolta eventi di configurazione e avvio                        | Set-and-boot-Event-Collection    | N                     |
| Servizi TCP/IP semplificati                                 | Simple-TCPIP                       | N                     |
| Supporto condivisione di file SMB 1.0/CIFS                      | FS-SMB1                            | Y                     |
| Limite larghezza di banda SMB                                    | FS-SMBBW                           | N                     |
| Servizio SNMP                                           | SNMP-servizio                       | N                     |
| Provider WMI SNMP                                      | SNMP-WMI-provider                  | N                     |
| Client Telnet                                          | Telnet-client                      | N                     |
| Strumenti di schermatura VM per la gestione dell'infrastruttura               | FabricShieldedTools                | N                     |
| Funzionalità di Windows Defender                              | Windows-Defender-features          | Y                     |
| Windows Defender                                       | Windows-Defender                   | Y                     |
| Database interno di Windows                              | Windows-interno-database          | N                     |
| Windows PowerShell                                     | PowerShellRoot                     | Y                     |
| Windows PowerShell 5,1                                 | PowerShell                         | Y                     |
| Motore di Windows PowerShell 2,0                          | PowerShell-V2                      | rimosso             |
| Servizio di configurazione dello stato desiderato di Windows PowerShell | DSC-servizio                        | N                     |
| Accesso Web di Windows PowerShell                          | WindowsPowerShellWebAccess         | N                     |
| Servizio Attivazione dei processi Windows                     | È STATO                                | N                     |
| Modello di processo                                          | Is-Process-Model                  | N                     |
| Ambiente .NET 3,5                                   | WAS-NET-environment                | N                     |
| API di configurazione                                     | WAS-Config-API                    | N                     |
| Windows Server Backup                                  | Windows-Server-backup              | N                     |
| Strumenti di migrazione per Windows Server                         | Migrazione                          | N                     |
| Gestione archiviazione basata su standard Windows             | WindowsStorageManagementService    | N                     |
| Estensione IIS di Gestione remota Windows                                    | WinRM-IIS-Ext                      | N                     |
| Server WINS                                            | WINS                               | N                     |
| Supporto di WoW64                                          | WoW64-supporto                      | Y                     |
|                                                        |                                    |                       |
