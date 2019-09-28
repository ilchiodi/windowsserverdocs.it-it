---
title: Ruoli, servizi ruolo e funzionalità non in Windows Server-Server Core
description: Informazioni sui ruoli e le funzionalità non inclusi nell'opzione di installazione dei componenti di base del server per Windows Server.
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: ce8fd0edc426b673f873717a27e6045e3476170f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383360"
---
# <a name="roles-role-services-and-features-not-in-windows-server---server-core"></a>Ruoli, servizi ruolo e funzionalità non in Windows Server-Server Core

> Si applica a: Windows Server 2019, Windows Server 2016 e Windows Server (canale semestrale)

I ruoli, i servizi ruolo e le funzionalità seguenti sono stati rimossi dall'opzione di installazione dei componenti di base del server di Windows Server. Usare queste informazioni per determinare se l'opzione dei componenti di base del server funziona per l'ambiente.

> [!NOTE]
> È anche possibile visualizzare un elenco dei ruoli, dei servizi ruolo e delle funzionalità [inclusi in Server Core](server-core-roles-and-services.md). Si tratta di un elenco di dimensioni molto elevate. per ottenere risultati ottimali, eseguire ricerche nell'elenco per il ruolo o la funzionalità specifica a cui si è interessati.

## <a name="roles-not-in-server-core"></a>Ruoli non in Server Core

- Server fax (**Fax**)
- Servizi MultiPoint (**MultiPointServerRole**)
- Servizi di accesso e criteri di rete (**npas**)
- Servizi di distribuzione Windows (**WDS**) *(prima di windows Server versione 1803)*

## <a name="role-services-not-in-server-core"></a>Servizi ruolo non in Server Core
Si noti che alcuni Desktop remoto Servizi ruolo sono inclusi in Server Core (Connection Broker, Licensing, Virtualization Host), mentre altri non lo sono (gateway, host sessione Desktop remoto, Accesso Web).

- Servizi di stampa e digitalizzazione \ Server Digitalizzazione distribuita (**Print-Scan-Server**)
- Servizi di stampa e digitalizzazione \ stampa Internet (**Print-Internet**)
- Servizi Desktop remoto \ Desktop remoto Gateway (**RDS-Gateway**)
- Servizi Desktop remoto \ host sessione Desktop remoto (**RDS-Rd-server**)
- Servizi Desktop remoto \ Desktop remoto Accesso Web (Servizi Desktop remoto-**accesso Web**)
- Server Web del servizio ruolo (IIS) \ strumenti di gestione \ console di gestione IIS (**Web-Mgmt-Console**)
- Server Web del servizio ruolo (IIS) \ strumenti di gestione \ compatibilità di gestione con IIS 6 \ console di gestione IIS 6 (**Web-Lgcy-Mgmt-Console**)
- Servizi di distribuzione Windows \ server di distribuzione (**WDS-Deployment**)
- Servizi di distribuzione Windows \ server trasporto (**WDS-Transport**) *(prima di Windows Server versione 1803)*

## <a name="features-not-in-server-core"></a>Funzionalità non incluse in Server Core
- Servizio trasferimento intelligente in background (BITS) \ IIS Server Extension (**bits-IIS-Ext**)
- Sblocco di rete BitLocker (**BitLocker-NetworkUnlock**)
- Riproduzione diretta (**Direct-Play**)
- Client di stampa Internet (**Internet-Print-client**)
- Monitoraggio porta LPR (**LPR-Port-Monitor**)
- Accodamento messaggi \ servizi di Accodamento messaggi \ supporto multicast (**MSMQ-multicast**)
- Connection Manager Administration Kit (**CMAK**) RAS
- Assistenza remota (**assistenza remota**)
- Strumenti**di amministrazione di**strumenti di amministrazione remota del server \ funzionalità \ strumenti server SMTP (strumenti di amministrazione SMTP)
- Strumenti di amministrazione di Strumenti di amministrazione remota del server \ funzionalità \ Crittografia unità BitLocker Administration Utilities \ Crittografia unità BitLocker Tools (strumenti di amministrazione remota del server-**funzionalità-BitLocker-RemoteAdminTool**)
- Strumenti di amministrazione di Strumenti di amministrazione remota del server \ funzionalità \ BITS Server Extensions Tools (strumenti**di amministrazione remota-BITS-Server**)
- Strumenti di amministrazione di Strumenti di amministrazione remota del server \ funzionalità \ strumenti di bilanciamento carico**di rete (amministrazione remota**)
- Strumenti di amministrazione di Strumenti di amministrazione remota del server \ funzionalità \ SNMP Tools (strumenti**di amministrazione remota**)
- Strumenti**di amministrazione di**strumenti di amministrazione remota del server \ funzionalità \ strumenti server WINS (amministrazione remota)
- Strumenti di amministrazione remota del server \ Role Administration Tools \ Hyper-V Management Tools \ Hyper-V GUI Management Tools (**Hyper-v-Tools**)
- Strumenti di amministrazione ruoli di Strumenti di amministrazione remota del server \ Servizi Desktop remoto Tools \ Desktop remoto Gateway Tools (strumenti**di amministrazione remota**)
- Strumenti di amministrazione ruoli di Strumenti di amministrazione remota del server \ Servizi Desktop remoto Tools \ Desktop remoto Gateway Tools (strumenti**di amministrazione remota**)
- Strumenti di amministrazione remota del server \ Role Administration Tools \ Servizi Desktop remoto Tools \ Desktop remoto Licensing Diagnostics Tools (**amministrazione remota**dei servizi)
- Strumenti di amministrazione ruoli di Strumenti di amministrazione remota del server \ Servizi Desktop remoto Tools \ Desktop remoto Licensing Tools (**RDS-Licensing-UI**)
- Strumenti di amministrazione remota del server \ Role Administration Tools \ Windows Server Update Services Tools \ User Interface Management Console (**UpdateServices-UI**)
- Strumenti di amministrazione ruoli di Strumenti di amministrazione remota del server \ Active Directory strumenti per Servizi certificati (amministrazione**remota**dei certificati)
- Strumenti di amministrazione ruoli di Strumenti di amministrazione remota del server \ Active Directory strumenti di Servizi certificati \ strumenti di gestione dell'autorità di certificazione (strumenti**di amministrazione remota-ADC-Mgmt**)
- Strumenti di amministrazione ruoli di Strumenti di amministrazione remota del server \ Active Directory strumenti**di**Servizi certificati \ strumenti risponditore online
- Strumenti di amministrazione di Strumenti di amministrazione remota del server \ ruoli \ Active Directory Rights Management Services Tools (strumenti**di amministrazione remota**)
- Strumenti di amministrazione remota del server \ Role Administration Tools \ Fax Server Tools (strumenti**di amministrazione remota**)
- Strumenti di amministrazione ruoli di Strumenti di amministrazione remota del server \ strumenti per servizi file (Servizi file **-** Server)
- Strumenti di amministrazione remota del server \ Role Administration Tools \ file Services Tools \ DFS Management Tools (strumenti di amministrazione DFS **-DFS-Mgmt-con**)
- Strumenti di amministrazione remota del server \ Role Administration Tools \ file Services Tools \ file server Gestione risorse Tools (**netFSRM-Mgmt**)
- Strumenti di amministrazione ruoli di Strumenti di amministrazione remota del server \ strumenti Servizi file \ servizi per strumenti di gestione del file System di rete (amministrazione**remota**dei file)
- Strumenti di amministrazione ruoli di Strumenti di amministrazione remota del server \ servizi di accesso e criteri di rete (**npas**)
- Strumenti di amministrazione dei ruoli Strumenti di amministrazione remota del server \ Servizi di stampa e digitalizzazione Tools (strumenti**di amministrazione remota**)
- Strumenti di amministrazione ruoli di Strumenti di amministrazione remota del server \ Volume Activation Tools (strumenti**di**attivazione contratti multilicenza)
- Strumenti di amministrazione remota del server \ strumenti di amministrazione ruoli \ strumenti di servizi di distribuzione Windows (**WDS-Adminpak**)
- Servizi TCP/IP semplici (**Simple-Tcpip**)
- Server SMTP (**server SMTP**)
- Client TFTP (**TFTP-client**)
- Redirector WebDAV (**WebDAV-redirector**)
- Windows Biometric Framework (**biometrico-Framework**)
- Funzionalità di Windows Defender \ GUI per Windows Defender (**Windows-Defender-GUI**)
- Windows Identity Foundation 3,5 (**Windows-Identity-Foundation**)
- Windows PowerShell \ Windows PowerShell ISE (**PowerShell-ISE**)
- Windows servizio di ricerca (**servizio di ricerca**)
- IFilter TIFF di Windows (**Windows-TIFF-IFilter**)
- Servizio LAN wireless (**wireless-rete**)
- Visualizzatore XPS (**XPS-Viewer**)
