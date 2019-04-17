---
title: Ruoli, servizi ruolo e funzionalità incluse in Windows Server - Server Core
description: I ruoli e le funzionalità sono inclusi nell'opzione di installazione Server Core di Windows Server?
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 65729eb68c9590fd6316f5650be48f33c19c926d
ms.sourcegitcommit: 439edac95e1427086268cab469ed03b94db630da
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "2217741"
---
# <a name="roles-role-services-and-features-included-in-windows-server---server-core"></a>Ruoli, servizi ruolo e funzionalità incluse in Windows Server - Server Core

> Si applica a: Windows Server (Channel semi-annuale) e Windows Server 2016

Parliamo in genere [cosa del *non* in Server Core](server-core-removed-roles.md) - ora dobbiamo, per cercare un approccio diverso a spiegare elementi *inclusi* e indica se un elemento viene *installato per impostazione predefinita*. I seguenti ruoli, servizi ruolo e funzionalità presenti ** nell'opzione di installazione Server Core di Windows Server. Utilizzare queste informazioni per determinare se l'opzione Server Core può essere utilizzato per l'ambiente. Poiché si tratta di un elenco di grandi dimensioni, prendere in considerazione la ricerca del ruolo specifico o caratteristica si è interessati - se che la ricerca non restituisce ciò che sta cercando, non è incluso in Server Core.

Ad esempio, se esegue la ricerca per "Host sessione Desktop remoto" - è non risulti in questa pagina. Ciò avviene perché l'Host sessione Desktop remoto non è incluso nell'immagine Server Core.

Tenere presente che è possibile [controllare sempre](server-core-removed-roles.md) a cosa *non* è incluso. Si tratta semplicemente diversamente da quanto paragonare aspetti.

## <a name="roles-included-in-server-core"></a>Ruoli inclusi in Server Core
L'opzione di installazione Server Core include i seguenti ruoli del server.

| Ruolo                                            | Nome                           | Installato per impostazione predefinita. |
|-------------------------------------------------|--------------------------------|-----------------------|
| Servizi certificati Active Directory           | Certificato di Active Directory                 | N                     |
| Active Directory Domain Services                | Servizi di dominio Active Directory             | N                     |
| Active Directory Federation Services            | Federazione di ADFS                | N                     |
| Active Directory Lightweight Directory Services | ADLDS                          | N                     |
| Active Directory Rights Management Services     | ADRMS                          | N                     |
| Attestazione dell'integrità dei dispositivi                       | DeviceHealthAttestationService | N                     |
| Server DHCP                                     | DHCP                           | N                     |
| Server DNS                                      | DNS                            | N                     |
| Servizi file e archiviazione                       | Servizi di FileAndStorage        | Y                     |
| Servizio Sorveglianza host                           | HostGuardianServiceRole        | N                     |
| Hyper-V                                         | Hyper-V                        | N                     |
| Servizi di stampa e digitalizzazione                     | Servizi di stampa                 | N                     |
| Accesso remoto                                   | Accesso remoto                   | N                     |
| Servizi Desktop remoto                         | Servizi di Desktop remoto        | N                     |
| Servizi di attivazione contratti multilicenza                      | VolumeActivation               | N                     |
| Server Web IIS                                  | Server Web                     | N                     |
| Esperienza Windows Server Essentials            | ServerEssentialsRole           | N                     |
| Windows Server Update Services                  | UpdateServices                 | N                     |

## <a name="role-services-included-in-server-core"></a>Servizi ruolo inclusi nel Server Core
L'opzione di installazione Server Core include i seguenti servizi ruolo.

| Ruolo                                  | Servizio ruolo                                                   | Nome                    | Installato per impostazione predefinita. |
|---------------------------------------|----------------------------------------------------------------|-------------------------|-----------------------|
| Servizi certificati Active Directory | Autorità di certificazione                                        | Autorità di certificazione Distributor     | N                     |
|                                       | Servizio Web di registrazione di certificati                      | Distributor-registrazione-Web-Pol     | N                     |
|                                       | Servizio Web di registrazione certificati                             | Distributor-registrazione-Web-Svc     | N                     |
|                                       | Registrazione Web autorità di certificazione                         | Registrazione di Web Distributor     | N                     |
|                                       | Servizio di registrazione di dispositivi di rete                              | Registrazione di dispositivo Distributor  | N                     |
|                                       | Risponditore online                                               | Distributor Cert Online        | N                     |
| Active Directory Rights Management    | Server AD RMS (Active Directory Rights Management Services)                      | ADRMS Server            | N                     |
|                                       | Supporto di federazione delle identità                                    | ADRMS-Identity          | N                     |
| Servizi file e archiviazione             | Servizi file e iSCSI                                        | Servizi file           | N                     |
|                                       | File Server                                                    | FileServer FS           | N                     |
|                                       | BranchCache per file di rete                                  | Panoramica di ADFS          | N                     |
|                                       | Deduplicazione dati                                             | Deduplication di dati di ADFS   | N                     |
|                                       | Spazi dei nomi DFS                                                 | Namespace di DFS FS        | N                     |
|                                       | Replica DFS                                                | Replica di DFS FS      | N                     |
|                                       | Gestione risorse file server                                   | Gestione risorse di ADFS     | N                     |
|                                       | Servizio agente File Server VSS                                  | Agente di VSS ADFS            | N                     |
|                                       | iSCSI Target Server                                            | iSCSITarget Server      | N                     |
|                                       | iSCSI Provider di archiviazione di destinazione (provider hardware VDS e VSS) | iSCSITarget-VSS-VDS     | N                     |
|                                       | Server per NFS                                                 | Servizio di NFS ADFS          | N                     |
|                                       | Cartelle di lavoro                                                   | ADFS SyncShareService     | N                     |
|                                       | Servizi di archiviazione                                               | Servizi di archiviazione        | Y                     |
| Servizi di stampa e digitalizzazione           | Server di stampa                                                   | Server di stampa            | N                     |
|                                       | Servizio LPD                                                    | Servizio di stampa LPD       | N                     |
| Accesso remoto                         | DirectAccess e VPN (RAS)                                     | DirectAccess VPN        | N                     |
|                                       | Routing                                                        | Routing                 | N                     |
|                                       | Proxy applicazione Web                                          | Proxy dell'applicazione Web   | N                     |
| Servizi Desktop remoto               | Gestore connessione Desktop remoto                               | Broker di connessione RDS   | N                     |
|                                       | Licenze di Desktop remoto                                       | Licenze Servizi Desktop remoto           | N                     |
|                                       | Host di virtualizzazione Desktop remoto                             | Virtualizzazione di RDS      | N                     |
| Server Web (IIS)                      | Web Server                                                     | Web server Web           | N                     |
|                                       | Funzionalità HTTP comuni                                           | Web comune Http         | N                     |
|                                       | Documento predefinito                                               | Web-predefinito-Doc         | N                     |
|                                       | Esplorazione directory                                             | Esplorazione directory Web        | N                     |
|                                       | Errori HTTP                                                    | Errori di Http Web         | N                     |
|                                       | Contenuto statico                                                 | Contenuto statico Web      | N                     |
|                                       | Reindirizzamento HTTP                                               | Reindirizzamento di Http Web       | N                     |
|                                       | Pubblicazione WebDAV                                              | Pubblicazione sul Web DAV      | N                     |
|                                       | Integrità e diagnostica                                         | Health Web              | N                     |
|                                       | Registrazione HTTP                                                   | Registrazione di Http Web        | N                     |
|                                       | Registrazione personalizzati                                                 | Registrazione personalizzato Web      | N                     |
|                                       | Strumenti di registrazione                                                  | Librerie di registro Web       | N                     |
|                                       | Registrazione ODBC                                                   | Registrazione ODBC Web        | N                     |
|                                       | Monitor richieste                                              | Monitor di richieste Web     | N                     |
|                                       | Traccia                                                        | Traccia di Http Web        | N                     |
|                                       | Prestazioni                                                    | Prestazioni Web         | N                     |
|                                       | Compressione contenuto statico                                     | Compressione di Stat Web    | N                     |
|                                       | Compressione contenuto dinamico                                    | Compressione di Dyn Web     | N                     |
|                                       | Sicurezza                                                       | Protezione Web            | N                     |
|                                       | Filtro richieste                                              | Il filtro Web           | N                     |
|                                       | Autenticazione di base                                           | Autenticazione di base Web          | N                     |
|                                       | Supporto di certificati SSL centralizzato                            | CertProvider Web        | N                     |
|                                       | Autenticazione Mapping certificati client                      | Autenticazione di Client Web         | N                     |
|                                       | Autenticazione del digest                                          | Autenticazione del Digest di Web         | N                     |
|                                       | Autenticazione Mapping certificati Client IIS                  | Autenticazione di certificato Web           | N                     |
|                                       | IP e restrizioni del dominio                                     | Web IPSec         | N                     |
|                                       | L'autorizzazione dell'URL                                              | Autenticazione di Url Web            | N                     |
|                                       | Autenticazione di Windows                                         | Autenticazione di Windows Web        | N                     |
|                                       | Sviluppo di applicazioni                                        | Sviluppo di App Web             | N                     |
|                                       | Estendibilità .NET 3.5                                         | Net-Web-est             | N                     |
|                                       | Estendibilità .NET 4.6                                         | Net-Web-Ext45           | N                     |
|                                       | Inizializzazione dell'applicazione                                     | AppInit Web             | N                     |
|                                       | ASP                                                            | Web ASP                 | N                     |
|                                       | IN ASP.NET 3.5                                                    | Web Asp rete             | N                     |
|                                       | ASP.NET 4.6                                                    | Net45 di Asp Web           | N                     |
|                                       | CGI                                                            | CGI Web                 | N                     |
|                                       | Estensioni ISAPI                                               | Ext di ISAPI Web           | N                     |
|                                       | Filtri ISAPI                                                  | Filtro di ISAPI Web        | N                     |
|                                       | Inclusioni sul lato server                                           | Include Web            | N                     |
|                                       | Protocollo WebSocket                                             | WebSockets Web          | N                     |
|                                       | Server FTP                                                     | Server Ftp Web          | N                     |
|                                       | Servizio FTP                                                    | Servizio Web-Ftp         | N                     |
|                                       | Estendibilità FTP                                              | Web-Ftp-est             | N                     |
|                                       | Strumenti di gestione                                               | Strumenti di gestione Web          | N                     |
|                                       | Compatibilità gestione IIS6                                 | Web-Mgmt-Compat         | N                     |
|                                       | Compatibilità Metabase IIS 6                                   | Metabase Web            | N                     |
|                                       | Strumenti di Scripting 6 di IIS                                          | Microsoft Script Editor Lgcy      | N                     |
|                                       | Compatibilità con IIS 6 WMI                                        | Web WMI                 | N                     |
|                                       | Strumenti e script di gestione IIS                               | Strumenti di script Web     | N                     |
|                                       | Servizio di gestione                                             | Web-Mgmt-Service        | N                     |
| Windows Server Update Services        | Connettività di database interno di Windows                                               | UpdateServices WidDB    | N                     |
|                                       | Servizi WSUS                                                  | Servizi di UpdateServices | N                     |
|                                       | Connettività SQL Server                                        | UpdateServices DB       | N                     |

## <a name="features-included-in-server-core"></a>Caratteristiche incluse in Server Core
L'opzione di installazione Server Core include le funzionalità seguenti.

| Funzionalità                                                | Nome                               | Installato per impostazione predefinita. |
|--------------------------------------------------------|------------------------------------|-----------------------|
| Funzionalità di .NET framework 3.5                            | Funzionalità di NET Framework             | N                     |
| .NET framework 3.5 (include .NET 2.0 e 3.0)       | NET Framework Core                 | (rimuovere)             |
| Attivazione di HTTP                                        | Attivazione di HTTP NET                | N                     |
| Attivazione non HTTP                                    | NET-Non-HTTP-l'attivazione                 | N                     |
| Funzionalità di .NET framework 4.6                            | NET Framework-45 caratteristiche          | Y                     |
| .NET framework 4.6                                     | NET Framework-45 Core              | Y                     |
| ASP.NET 4.6                                            | NET Framework-45 ASPNET            | N                     |
| Servizi WCF                                           | NET-WCF-Services45                 | Y                     |
| Attivazione di HTTP                                        | NET-WCF-HTTP-Activation45          | N                     |
| Attivazione di Accodamento messaggi (MSMQ)                      | NET-WCF-MSMQ-Activation45          | N                     |
| Attivazione di Named Pipe                                  | NET-WCF-Pipe-Activation45          | N                     |
| Attivazione TCP                                         | NET-WCF-TCP-Activation45           | N                     |
| Porta TCP condivisione                                       | NET-WCF-TCP-PortSharing45          | Y                     |
| Servizio trasferimento intelligente in background (BITS)         | BIT                               | N                     |
| Compact Server                                         | Server di compattazione BITS                | N                     |
| Crittografia unità BitLocker                             | BitLocker                          | N                     |
| BranchCache                                            | BranchCache                        | N                     |
| Client per NFS                                         | Client NFS                         | N                     |
| Contenitori                                             | Contenitori                         | N                     |
| Data Center Bridging                                   | Il Bridging centro dati               | N                     |
| Archiviazione avanzata                                       | EnhancedStorage                    | N                     |
| Clustering di failover                                    | Clustering di failover                | N                     |
| Gestione Criteri di gruppo                                | GESTIONE CRITERI DI GRUPPO                               | N                     |
| QoS (Quality of Service) I/O                                 | DiskIo QoS                         | N                     |
| HWC (Hostable Web Core) di IIS                                  | WHC Web                            | N                     |
| Server di gestione (IPAM) indirizzo IP                    | Gestione indirizzi IP                               | N                     |
| Servizio server iSNS                                    | ISNS                               | N                     |
| Estensione IIS OData gestione                         | ManagementOdata                    | N                     |
| Media Foundation                                       | Server-Media-Foundation            | N                     |
| Accodamento messaggi                                        | MSMQ                               | N                     |
| Servizi di Accodamento messaggi                               | Servizi MSMQ                      | N                     |
| Server di Accodamento messaggi                                 | MSMQ-Server                        | N                     |
| Integrazione servizio directory                          | MSMQ-Directory                     | N                     |
| Supporto HTTP                                           | Supporto HTTP MSMQ                  | N                     |
| Trigger di Accodamento messaggi                               | Trigger MSMQ                      | N                     |
| Servizio di routing                                        | Routing MSMQ                       | N                     |
| Proxy DCOM Accodamento messaggi                             | MSMQ DCOM                          | N                     |
| Multipath I/O                                          | / O percorsi multipli                       | N                     |
| MultiPoint Connector                                   | Connettore multiPoint               | N                     |
| Servizi del connettore multiPoint                          | Servizi di connettore multiPoint      | N                     |
| Manager multipunto e video multipunto Dashboard            | Strumenti di multiPoint                   | N                     |
| Bilanciamento carico di rete                                 | BILANCIAMENTO CARICO DI RETE                                | N                     |
| Protocollo PNRP (Peer Name Resolution Protocol)                          | PNRP                               | N                     |
| Servizio audio/video Windows di qualità                 | tale piattaforma                              | N                     |
| Compressione differenziale remota                        | CONNESSIONE DESKTOP REMOTO                                | N                     |
| Strumenti di amministrazione remota del server                     | Strumenti di amministrazione remota del server                               | N                     |
| Strumenti di amministrazione funzionalità                           | Strumenti di funzionalità remota del                 | N                     |
| Utilità di amministrazione di crittografia unità BitLocker  | Remota del-Feature-Tools-BitLocker       | N                     |
| Strumenti DataCenterBridging LLDP                          | Strumenti LLDP di DataCenterBridging remota del | N                     |
| Strumenti Clustering di failover                              | Remota del cluster                    | N                     |
| Modulo di Cluster di failover per Windows PowerShell         | RSAT-Clustering-PowerShell         | N                     |
| Server di automazione di Cluster di failover                     | AutomationServer Clustering remota del   | N                     |
| Interfaccia di comando di Cluster di failover                     | CmdInterface Clustering remota del       | N                     |
| Client Management (IPAM) indirizzo IP                    | Funzionalità di Client IPAM                | N                     |
| Strumenti di schermata macchina virtuale                                      | Remota del-protette-VM-degli strumenti             | N                     |
| Modulo di Replica di archiviazione per Windows PowerShell          | Replica di archiviazione remota del               | N                     |
| Strumenti di amministrazione ruoli                              | Strumenti di ruoli remota del                    | N                     |
| Strumenti di AD LDS e servizi di dominio Active Directory                                 | Strumenti di Active Directory remota del                      | N                     |
| Modulo di Active Directory per Windows PowerShell         | Remota del AD PowerShell                 | N                     |
| Strumenti di servizi di Active Directory                                            | AGGIUNGE REMOTA DEL                          | N                     |
| Centro di amministrazione di Active Directory                 | AdminCenter di Active Directory remota del                | N                     |
| Snap-in di dominio Active Directory di Active Directory e gli strumenti da riga di comando                  | Strumenti aggiunge remota del                    | N                     |
| Strumenti da riga di comando e gli Snap-in AD LDS                 | REMOTA DEL ADLDS                         | N                     |
| Strumenti di gestione di Hyper-V                               | Remota del-Hyper-V-Tools                 | N                     |
| Modulo di Hyper-V per Windows PowerShell                  | Hyper-V-PowerShell                 | N                     |
| Strumenti di Windows Server Update Services                   | Remota del UpdateServices                | N                     |
| Cmdlet per le API e PowerShell                             | API UpdateServices                 | N                     |
| Strumenti Server DHCP                                      | REMOTA DEL DHCP                          | N                     |
| Strumenti Server DNS                                       | Remota del Server DNS                    | N                     |
| Strumenti di gestione accesso remoto                         | Accesso remoto remota del                  | N                     |
| Modulo di accesso remoto per Windows PowerShell            | Remota del accesso remoto PowerShell       | N                     |
| Proxy RPC su HTTP                                    | Proxy HTTP di failover RPC                | N                     |
| Raccolta eventi di configurazione e avvio                        | Il programma di installazione e-avvio-evento-raccolta    | N                     |
| Servizi TCP/IP semplificati                                 | Semplice TCPIP                       | N                     |
| Supporto condivisione di file SMB 1.0/CIFS                      | ADFS SMB1                            | Y                     |
| Limite larghezza di banda SMB                                    | ADFS SMBBW                           | N                     |
| Servizio SNMP                                           | Servizio SNMP                       | N                     |
| Provider WMI SNMP                                      | Provider WMI di SNMP                  | N                     |
| Client Telnet                                          | Client Telnet                      | N                     |
| Strumenti di schermatura VM per la gestione dell'infrastruttura               | FabricShieldedTools                | N                     |
| Funzionalità di Windows Defender                              | Funzionalità di Windows Defender          | Y                     |
| Windows Defender                                       | Windows Defender                   | Y                     |
| Database interno di Windows                              | Windows Internal Database          | N                     |
| Windows PowerShell                                     | PowerShellRoot                     | Y                     |
| Windows PowerShell 5.1                                 | PowerShell                         | Y                     |
| Modulo di gestione di Windows PowerShell 2.0                          | PowerShell V2                      | (rimuovere)             |
| Windows PowerShell lo si desidera servizio informazioni sullo stato configurazione | DSC-Service                        | N                     |
| Windows PowerShell Web Access                          | WindowsPowerShellWebAccess         | N                     |
| Servizio Attivazione dei processi Windows                     | È STATO                                | N                     |
| Modello del processo                                          | Modello di processo è stato                  | N                     |
| Ambiente .NET 3.5                                   | Ambiente di rete è stata                | N                     |
| API di configurazione                                     | API di configurazione è stato                    | N                     |
| Windows Server Backup                                  | Windows Server Backup              | N                     |
| Strumenti di migrazione per Windows Server                         | Migrazione                          | N                     |
| Gestione archiviazione basata su standard Windows             | WindowsStorageManagementService    | N                     |
| Estensione IIS di Gestione remota Windows                                    | Gestione remota Windows-IIS-est                      | N                     |
| Server WINS                                            | WINS                               | N                     |
| Supporto WoW64                                          | Supporto WoW64                      | Y                     |
|                                                        |                                    |                       |
