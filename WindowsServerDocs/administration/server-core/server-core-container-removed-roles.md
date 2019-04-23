---
title: I ruoli, servizi ruolo e funzionalità non in Server Core contenitori - Windows Server, versione 1803
description: Informazioni sui ruoli e funzionalità che è stata rimossa dall'immagine del contenitore di Server Core per Windows Server.
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 05/07/2018
ms.openlocfilehash: 0ad574a04ba7ecd235f1825bd25c247a1565edf6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873782"
---
# <a name="roles-role-services-and-features-not-in-server-core-containers---windows-server-version-1803"></a>I ruoli, servizi ruolo e funzionalità non in Server Core contenitori - Windows Server, versione 1803

> Si applica a: Windows Server, versione 1803

In Windows Server, versione 1803, abbiamo [ridotta la dimensione complessiva dell'immagine del contenitore di base del Server per **1.58 GB**](https://blogs.technet.microsoft.com/virtualization/2018/01/22/a-smaller-windows-server-core-container-with-better-application-compatibility/). Il modo in cui è stata eseguita questa operazione è ottimizzando l'architettura e la rimozione di elementi non sono necessarie un [contenitore di Server Core](https://docs.microsoft.com/virtualization/windowscontainers/about/). Alcuni sono cose che non ha funzionato in contenitori, alcuni hanno ruoli e funzionalità che non è stato utilizzato. 

> [!IMPORTANT]
> Queste informazioni dal Server Core è stato rimosso **contenitore** delle immagini, non [Core Server stesso](server-core-roles-and-services.md). 

Ecco l'elenco completo di funzionalità e ruoli rimossi dall'immagine del contenitore di Server Core:

<div style='font-size:9.0pt'>

<br>ADCertificateServicesRole
<br>AuthManager
<br>BitLocker-utilità
<br>BitLocker
<br>BITS
<br>BITSExtensions-Upload
<br>CCFFilter
<br>CertificateEnrollmentPolicyServer
<br>CertificateEnrollmentServer
<br>CertificateServices
<br>ClientForNFS-Infrastructure
<br>Contenitori
<br>CoreFileServer
<br>DataCenterBridging-LLDP-Tools
<br>DataCenterBridging
<br>Deduplicazione-Core
<br>DeviceHealthAttestationService
<br>DFSN-Server
<br>DFSR-Infrastructure-ServerEdition
<br>DirectoryServices-ADAM
<br>DirectoryServices-DomainController
<br>DiskIo-QoS
<br>EnhancedStorage
<br>FailoverCluster-AdminPak
<br>FailoverCluster-AutomationServer
<br>FailoverCluster-CmdInterface
<br>FailoverCluster-FullServer
<br>FailoverCluster-PowerShell
<br>File-Services
<br>FileServerVSSAgent
<br>FRS-Infrastructure
<br>FSRM-Infrastructure-Services
<br>FSRM-Infrastructure
<br>HardenedFabricEncryptionTask
<br>HostGuardian
<br>HostGuardianService-Package
<br>IdentityServer-SecurityTokenService
<br>IPAMClientFeature
<br>IPAMServerFeature
<br>iSCSITargetServer-PowerShell
<br>iSCSITargetServer
<br>iSCSITargetStorageProviders
<br>iSNS_Service
<br>Gestione licenze
<br>LightweightServer
<br>Microsoft-Hyper-V-Management-client
<br>Microsoft-Hyper-V-Offline
<br>Microsoft-Hyper-V-Online
<br>Microsoft-Hyper-V
<br>Microsoft-Windows-FCI-Client-Package
<br>Microsoft-Windows-GroupPolicy-ServerAdminTools-Update
<br>Microsoft-Windows-Subsystem-Linux
<br>Infrastruttura di MSRDC
<br>MultipathIo
<br>NetworkController
<br>NetworkControllerTools
<br>NetworkDeviceEnrollmentServices
<br>NetworkLoadBalancingFullServer
<br>NetworkVirtualization
<br>OnlineRevocationServices
<br>P2P-PnrpOnly
<br>PeerDist
<br>Stampa-Client-interfaccia utente grafica
<br>Printing-LPDPrintService
<br>Stampa-Server-Foundation-funzionalità
<br>Printing-Server-Role
<br>QWAVE
<br>RasRoutingProtocols
<br>Remote-Desktop-Services
<br>RemoteAccess
<br>RemoteAccessMgmtTools
<br>RemoteAccessPowerShell
<br>RemoteAccessServer
<br>ResumeKeyFilter
<br>RightsManagementServices-Role
<br>RightsManagementServices
<br>RMS-Federation
<br>SBMgr-UI
<br>ServerCore-Drivers-General-WOW64
<br>ServerCore-driver-generale
<br>ServerForNFS-Infrastructure
<br>ServerManager-Core-RSAT-Feature-Tools
<br>ServerMediaFoundation
<br>ServerMigration
<br>SessionDirectory
<br>SetupAndBootEventCollection
<br>ShieldedVMToolsAdminPack
<br>SMB1Protocol-Server
<br>SmbDirect
<br>SMBHashGeneration
<br>SmbWitness
<br>SNMP
<br>SoftwareLoadBalancer
<br>Storage-Replica-AdminPack
<br>Replica di archiviazione
<br>Cmdlet di PSH TPM
<br>UpdateServices-Database
<br>UpdateServices-Services
<br>UpdateServices-WidDatabase
<br>UpdateServices
<br>VmHostAgent
<br>VolumeActivation-Full-Role
<br>Web-Application-Proxy
<br>WebAccess
<br>WebEnrollmentServices
<br>Windows-Defender
<br>WindowsServerBackup
<br>WindowsStorageManagementService
<br>WINSRuntime
<br>WMISnmpProvider
<br>WorkFolders-Server
<br>WSS-Product-Package

</div>