---
title: Ruoli, servizi ruolo e funzionalità non in Windows Server-Server Core
description: Informazioni sui ruoli e le funzionalità non inclusi nell'opzione di installazione dei componenti di base del server per Windows Server.
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: ce5107e8e0ab573df7588428db65c8b223cf1f13
ms.sourcegitcommit: 216d97ad843d59f12bf0b563b4192b75f66c7742
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/24/2019
ms.locfileid: "68476510"
---
# <a name="roles-role-services-and-features-not-in-windows-server---server-core"></a>Ruoli, servizi ruolo e funzionalità non in Windows Server-Server Core

> Si applica a Windows Server 2019, Windows Server 2016 e Windows Server (canale semestrale)

I ruoli, i servizi ruolo e le funzionalità seguenti sono stati rimossi dall'opzione di installazione dei componenti di base del server di Windows Server. Usare queste informazioni per determinare se l'opzione dei componenti di base del server funziona per l'ambiente.

> [!NOTE]
> È anche possibile visualizzare un elenco dei ruoli, dei servizi ruolo e delle funzionalità [inclusi in Server Core](server-core-roles-and-services.md). Si tratta di un elenco di dimensioni molto elevate. per ottenere risultati ottimali, eseguire ricerche nell'elenco per il ruolo o la funzionalità specifica a cui si è interessati.

## <a name="roles-not-in-server-core"></a>Ruoli non in Server Core

- Fax
- MultiPointServerRole
- NPAS
- WDS

## <a name="role-services-not-in-server-core"></a>Servizi ruolo non in Server Core
Si noti che alcuni Desktop remoto Servizi ruolo sono inclusi in Server Core (Connection Broker, Licensing, Virtualization Host), mentre altri non lo sono (gateway, host sessione Desktop remoto, Accesso Web).

- Print-Scan-Server
- Stampa-Internet
- Gateway RDS
- RDS-RD-server
- Accesso Web RDS
- Web-Mgmt-Console
- Web-Lgcy-Mgmt-Console
- WDS-distribuzione
- WDS-trasporto *(prima di Windows Server versione 1803)*

## <a name="features-not-in-server-core"></a>Funzionalità non incluse in Server Core

- BITS-IIS-Ext
- BitLocker-NetworkUnlock
- Riproduzione diretta
- Internet-Print-client
- LPR-porta-monitoraggio
- MSMQ-multicast
- CMAK
- Assistenza remota
- STRUMENTI DI AMMINISTRAZIONE REMOTA (SMTP)
- Strumenti di amministrazione remota del server-funzionalità-BitLocker-RemoteAdminTool
- Strumenti di amministrazione remota del server
- AMMINISTRAZIONE REMOTA DI SERVER-NLB
- STRUMENTI DI AMMINISTRAZIONE REMOTA DEL SERVER-SNMP
- STRUMENTI DI AMMINISTRAZIONE REMOTA DEL SERVER-WINS
- Strumenti di Hyper-V
- Strumenti di amministrazione remota del server
- Strumenti di amministrazione remota del gateway
- Amministrazione remota Servizi Desktop remoto-gestione licenze-diagnosi-interfaccia utente
- Servizi Desktop remoto-gestione licenze-interfaccia utente
- UpdateServices-interfaccia utente
- STRUMENTI DI AMMINISTRAZIONE REMOTA-ADC
- Amministrazione remota del server-ADC-Mgmt
- Strumenti di amministrazione remota-online-risponditore
- STRUMENTI DI AMMINISTRAZIONE REMOTA DEL SERVER-ADRMS
- Strumenti di amministrazione remota-fax
- Strumenti di amministrazione remota dei servizi file
- Strumenti di amministrazione remota-DFS-Mgmt-con
- Strumenti di amministrazione remota-FSRM-Mgmt
- Strumenti di amministrazione remota del server-NFS
- STRUMENTI DI AMMINISTRAZIONE REMOTA (NPAS)
- Servizi di stampa-amministrazione
- Strumenti di amministrazione remota del server
- WDS-Adminpak
- Server SMTP
- TFTP-client
- WebDAV-redirector
- Biometrico-Framework
- Windows-Defender-GUI
- Windows-Identity-Foundation
- PowerShell-ISE
- Servizio di ricerca
- Windows-TIFF-IFilter
- Rete wireless
- XPS-Visualizzatore

