---
title: Ruoli, servizi ruolo e funzionalità non nelle versioni di contenitori - Windows Server, Server Core 1803
description: Informazioni sui ruoli e funzionalità che sono state rimosse dall'immagine contenitore Server Core di Windows Server.
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 05/07/2018
ms.openlocfilehash: 0ad574a04ba7ecd235f1825bd25c247a1565edf6
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/01/2018
ms.locfileid: "1859908"
---
# <a name="roles-role-services-and-features-not-in-server-core-containers---windows-server-version-1803"></a>Ruoli, servizi ruolo e funzionalità non nelle versioni di contenitori - Windows Server, Server Core 1803

> Si applica a: Windows Server, versione 1803

In Windows Server, versione 1803, sono state [ridotte dimensione complessiva dell'immagine del contenitore Server Core a **1.58 GB**](https://blogs.technet.microsoft.com/virtualization/2018/01/22/a-smaller-windows-server-core-container-with-better-application-compatibility/). Il modo in cui è stato fatto è per ottimizzare l'architettura e rimozione di elementi che non necessari in un [Server Core contenitore](https://docs.microsoft.com/virtualization/windowscontainers/about/). Alcuni aspetti che non funzionano in contenitori, alcuni erano ruoli e funzionalità che nessuno utilizza già. 

> [!IMPORTANT]
> Sono state rimosse queste dall'immagine **contenitore** Server Core, non [Server Core stesso](server-core-roles-and-services.md). 

Di seguito è riportato l'elenco completo delle funzionalità e ruoli rimossi dall'immagine contenitore Server Core:

<div style='font-size:9.0pt'>

<br>ADCertificateServicesRole
<br>Gestione autenticazione
<br>Utilità BitLocker
<br>BitLocker
<br>BIT
<br>Caricamento BITSExtensions
<br>CCFFilter
<br>CertificateEnrollmentPolicyServer
<br>CertificateEnrollmentServer
<br>ServiziCertificati
<br>Infrastruttura di ClientForNFS
<br>Contenitori
<br>CoreFileServer
<br>Strumenti di LLDP DataCenterBridging
<br>DataCenterBridging
<br>Dedup Core
<br>DeviceHealthAttestationService
<br>REPLICHI Server
<br>Che il servizio-Infrastructure-ServerEdition
<br>DirectoryServices ADAM
<br>DirectoryServices-DomainController
<br>DiskIo QoS
<br>EnhancedStorage
<br>AdminPak FailoverCluster
<br>FailoverCluster AutomationServer
<br>FailoverCluster CmdInterface
<br>FailoverCluster idealebackup
<br>FailoverCluster PowerShell
<br>Servizi file
<br>FileServerVSSAgent
<br>Infrastruttura di FRS
<br>Servizi di infrastruttura di gestione risorse file server
<br>Infrastruttura di gestione risorse file server
<br>HardenedFabricEncryptionTask
<br>HostGuardian
<br>Pacchetto di HostGuardianService
<br>IdentityServer SecurityTokenService
<br>IPAMClientFeature
<br>IPAMServerFeature
<br>iSCSITargetServer PowerShell
<br>iSCSITargetServer
<br>iSCSITargetStorageProviders
<br>iSNS_Service
<br>Licenze
<br>LightweightServer
<br>Hyper-V-gestione-i client di Microsoft
<br>Microsoft-Hyper-V-non in linea
<br>Microsoft-Hyper-V-Online
<br>Microsoft Hyper-V
<br>Microsoft Windows-FCI-Client-pacchetto
<br>Microsoft Windows-Affectation-ServerAdminTools-aggiornamento
<br>Microsoft Windows-sottosistema Linux
<br>Infrastruttura di MSRDC
<br>MultipathIo
<br>NetworkController
<br>NetworkControllerTools
<br>NetworkDeviceEnrollmentServices
<br>NetworkLoadBalancingFullServer
<br>NetworkVirtualization
<br>OnlineRevocationServices
<br>PnrpOnly P2P
<br>PeerDist
<br>Interfaccia grafica Client di stampa
<br>Stampa LPDPrintService
<br>Server di stampa-funzionalità di Foundation
<br>Stampa-Server-Role
<br>TALE PIATTAFORMA
<br>RasRoutingProtocols
<br>Servizi di Desktop remoto
<br>Accesso remoto
<br>RemoteAccessMgmtTools
<br>RemoteAccessPowerShell
<br>RemoteAccessServer
<br>ResumeKeyFilter
<br>Ruolo RightsManagementServices
<br>RightsManagementServices
<br>Federazione RMS
<br>Interfaccia utente SBMgr
<br>WOW64 ServerCore-driver-generale
<br>ServerCore driver generale
<br>Infrastruttura di ServerForNFS
<br>ServerManager-Core-remota del-Feature-Tools
<br>ServerMediaFoundation
<br>ServerMigration
<br>SessionDirectory a seconda che
<br>SetupAndBootEventCollection
<br>ShieldedVMToolsAdminPack
<br>SMB1Protocol Server
<br>SmbDirect
<br>SMBHashGeneration
<br>SmbWitness
<br>SNMP
<br>SoftwareLoadBalancer
<br>AdminPack di Replica spazio di archiviazione
<br>Replica di archiviazione
<br>Cmdlet di PSH GPC
<br>Database UpdateServices
<br>Servizi di UpdateServices
<br>UpdateServices WidDatabase
<br>UpdateServices
<br>VmHostAgent
<br>VolumeActivation-Full-Role
<br>Proxy dell'applicazione Web
<br>Terminal
<br>WebEnrollmentServices
<br>Windows Defender
<br>WindowsServerBackup
<br>WindowsStorageManagementService
<br>WINSRuntime
<br>WMISnmpProvider
<br>WorkFolders Server
<br>Pacchetto di prodotto WSS

</div>