---
title: Ruoli, servizi ruolo e funzionalità non inclusi nei contenitori di Server Core-Windows Server, versione 1803
description: Informazioni sui ruoli e le funzionalità che sono state rimosse dall'immagine del contenitore di Server Core per Windows Server.
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 05/07/2018
ms.openlocfilehash: 41b5a9ac32066f1b2a41de84f66b9be79252c336
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383406"
---
# <a name="roles-role-services-and-features-not-in-server-core-containers---windows-server-version-1803"></a>Ruoli, servizi ruolo e funzionalità non inclusi nei contenitori di Server Core-Windows Server, versione 1803

> Si applica a: Windows Server, versione 1803

In Windows Server versione 1803 sono state [ridotte le dimensioni complessive dell'immagine del contenitore di server core a **1,58 GB**](https://blogs.technet.microsoft.com/virtualization/2018/01/22/a-smaller-windows-server-core-container-with-better-application-compatibility/). Per eseguire questa operazione, è possibile ottimizzare l'architettura e rimuovere gli elementi che non sono necessari in un [contenitore](https://docs.microsoft.com/virtualization/windowscontainers/about/)dei componenti di base del server. Alcuni erano elementi che non funzionavano nei contenitori, alcuni sono i ruoli e le funzionalità che non sono state usate da nessuno. 

> [!IMPORTANT]
> Questi elementi sono stati rimossi dall'immagine del **contenitore** Server Core, non da [Server Core](server-core-roles-and-services.md). 

Ecco l'elenco completo delle funzionalità e dei ruoli rimossi dall'immagine del contenitore di Server Core:

<div style='font-size:9.0pt'>

<br>ADCertificateServicesRole
<br>AuthManager
<br>Utilità BitLocker
<br>BitLocker
<br>BITS
<br>BITSExtensions-caricamento
<br>CCFFilter
<br>CertificateEnrollmentPolicyServer
<br>CertificateEnrollmentServer
<br>ServiziCertificati
<br>ClientForNFS-infrastruttura
<br>Contenitori
<br>CoreFileServer
<br>DataCenterBridging-LLDP-Tools
<br>DataCenterBridging
<br>Deduplicazione-Core
<br>DeviceHealthAttestationService
<br>DFSN-server
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
<br>Servizi file
<br>FileServerVSSAgent
<br>FRS-infrastruttura
<br>FSRM-Infrastructure-Services
<br>FSRM-infrastruttura
<br>HardenedFabricEncryptionTask
<br>HostGuardian
<br>HostGuardianService-pacchetto
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
<br>Microsoft-Hyper-V-offline
<br>Microsoft-Hyper-V-online
<br>Microsoft-Hyper-V
<br>Microsoft-Windows-FCI-client-package
<br>Microsoft-Windows-GroupPolicy-ServerAdminTools-Update
<br>Microsoft-Windows-sottosistema-Linux
<br>MSRDC-infrastruttura
<br>MultipathIo
<br>NetworkController
<br>NetworkControllerTools
<br>NetworkDeviceEnrollmentServices
<br>NetworkLoadBalancingFullServer
<br>NetworkVirtualization
<br>OnlineRevocationServices
<br>P2P-PnrpOnly
<br>PeerDist
<br>Stampa-client-GUI
<br>Stampa-LPDPrintService
<br>Printing-Server-Foundation-features
<br>Printing-Server-Role
<br>QWAVE
<br>RasRoutingProtocols
<br>Servizi Desktop remoto
<br>RemoteAccess
<br>RemoteAccessMgmtTools
<br>RemoteAccessPowerShell
<br>RemoteAccessServer
<br>ResumeKeyFilter
<br>RightsManagementServices-ruolo
<br>RightsManagementServices
<br>RMS-Federazione
<br>SBMgr-interfaccia utente
<br>ServerCore-drivers-General-WOW64
<br>ServerCore-drivers-generale
<br>ServerForNFS-infrastruttura
<br>ServerManager-Core-strumenti di amministrazione delle funzionalità
<br>ServerMediaFoundation
<br>ServerMigration
<br>SessionDirectory
<br>SetupAndBootEventCollection
<br>ShieldedVMToolsAdminPack
<br>SMB1Protocol-server
<br>SmbDirect
<br>SMBHashGeneration
<br>SmbWitness
<br>SNMP
<br>SoftwareLoadBalancer
<br>Storage-replica-Adminpak
<br>Archiviazione-replica
<br>TPM-PSH-cmdlet
<br>UpdateServices-database
<br>UpdateServices-servizi
<br>UpdateServices-WidDatabase
<br>UpdateServices
<br>VmHostAgent
<br>VolumeActivation-Full-Role
<br>Web-application-proxy
<br>WebAccess
<br>WebEnrollmentServices
<br>Windows-Defender
<br>WindowsServerBackup
<br>WindowsStorageManagementService
<br>WINSRuntime
<br>WMISnmpProvider
<br>Cartellelavoro-server
<br>WSS-pacchetto prodotto

</div>