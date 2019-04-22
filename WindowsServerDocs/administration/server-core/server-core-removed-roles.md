---
title: I ruoli, servizi ruolo e funzionalità non in Windows Server - Server Core
description: Informazioni sui ruoli e funzionalità non incluse nell'opzione di installazione Server Core per Windows Server.
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 308bc8a5d25e2ec67438f0ee03cbfce6f7411ca2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825532"
---
# <a name="roles-role-services-and-features-not-in-windows-server---server-core"></a>I ruoli, servizi ruolo e funzionalità non in Windows Server - Server Core

> Si applica a: Windows Server (canale semestrale) e Windows Server 2016

I ruoli, servizi ruolo e funzionalità seguenti sono stati rimossi dall'opzione di installazione Server Core di Windows Server. Usare queste informazioni per determinare se l'opzione Server Core funziona per l'ambiente.

> [!NOTE]
> È anche possibile visualizzare un elenco dei ruoli, servizi ruolo e funzionalità [sono inclusi in Server Core](server-core-roles-and-services.md). È un elenco di dimensioni molto grande, pertanto, per ottenere risultati ottimali, eseguire la ricerca tale elenco per i ruoli o funzionalità specifiche si è interessati.

## <a name="roles-not-in-server-core"></a>Ruoli non in Server Core

- Fax
- MultiPointServerRole
- NPAS
- WDS

## <a name="role-services-not-in-server-core"></a>Servizi di ruolo non in Server Core
Si noti che alcuni servizi ruolo Desktop remoto sono inclusi in Server Core (connessione Broker, gestione delle licenze, Host di virtualizzazione), ma non tutte (Host sessione Desktop remoto, Gateway Web Access).

- Server di digitalizzazione di stampa
- Stampa Internet
- RDS-Gateway
- RDS-RD-Server
- RDS-Web-Access
- Web-Mgmt-Console
- Web-Lgcy-Mgmt-Console
- Distribuzione di WDS
- Servizi di distribuzione Windows: trasporto *(prima versione 1803 per Windows Server)*

## <a name="features-not-in-server-core"></a>Funzionalità non in Server Core

- BITS-IIS-Ext
- BitLocker-NetworkUnlock
- Direct-Play
- Internet-Print-Client
- LPR-Port-Monitor
- Multicasting di MSMQ
- CMAK
- Remote-Assistance
- RSAT-SMTP
- RSAT-Feature-Tools-BitLocker-RemoteAdminTool
- RSAT-Bits-Server
- RSAT-NLB
- RSAT-SNMP
- AMMINISTRAZIONE REMOTA DEL SERVER WINS
- Hyper-V-Tools
- Strumenti di amministrazione remota del server-Servizi Desktop remoto
- RSAT-RDS-Gateway
- Amministrazione remota del server Servizi Desktop remoto-Licensing-diagnosi-dell'interfaccia utente
- Servizi Desktop remoto-Licensing-interfaccia utente
- UpdateServices-UI
- RSAT-ADCS
- RSAT-ADCS-Mgmt
- RSAT-Online-Responder
- RSAT-ADRMS
- RSAT-Fax
- RSAT-File-Services
- RSAT-DFS-Mgmt-Con
- RSAT-FSRM-Mgmt
- RSAT-NFS-Admin
- RSAT-NPAS
- RSAT-Print-Services
- RSAT-VA-Tools
- WDS-AdminPack
- SMTP-Server
- TFTP-Client
- WebDAV-Redirector
- Biometric-Framework
- Windows-Defender-Gui
- Windows-Identity-Foundation
- PowerShell-ISE
- Servizio di ricerca
- Windows-TIFF-IFilter
- Reti wireless
- XPS-Viewer

