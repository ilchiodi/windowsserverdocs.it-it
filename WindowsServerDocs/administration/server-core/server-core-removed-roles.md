---
title: Ruoli, servizi ruolo e funzionalità non in Windows Server - Server Core
description: Informazioni sui ruoli e funzionalità non è inclusa nell'opzione di installazione Server Core di Windows Server.
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 308bc8a5d25e2ec67438f0ee03cbfce6f7411ca2
ms.sourcegitcommit: 4b9b21ca1f366388a78ead7413cb581f2b23d4c6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/09/2018
ms.locfileid: "2604789"
---
# Ruoli, servizi ruolo e funzionalità non in Windows Server - Server Core

> Si applica a: Windows Server (Channel semi-annuale) e Windows Server 2016

Ruoli, servizi ruolo e funzionalità seguenti sono stati rimossi dall'opzione di installazione Server Core di Windows Server. Utilizzare queste informazioni per determinare se l'opzione Server Core può essere utilizzato per l'ambiente.

> [!NOTE]
> È inoltre possibile visualizzare un elenco di ruoli, servizi ruolo e funzionalità che [sono inclusi nel Server Core](server-core-roles-and-services.md). È un elenco di grande dimensioni, in modo per ottenere risultati migliori, eseguire una ricerca tale elenco per il ruolo specifico o la funzionalità che ti interessano.

## Ruoli Server Core

- Fax
- MultiPointServerRole
- NPAS
- WDS

## Servizi ruolo non nel Server Core
Si noti che alcuni servizi ruolo di Desktop remoto sono inclusi nel Server Core (Broker di connessione, licenze, Host di virtualizzazione), ma non ad altri utenti sono (Gateway, Host sessione Desktop remoto, Web Access).

- Server di analisi di stampa
- Stampa Internet
- Gateway di servizi desktop remoto
- Server di desktop remoto RDS
- Accesso di Web RDS
- Console di gestione Web
- Console Web-Lgcy-Mgmt
- Distribuzione di WDS
- Trasporto WDS *(prima di Windows Server versione 1803)*

## Funzionalità non nei Server Core

- BIT-IIS-est
- BitLocker NetworkUnlock
- Riproduci diretto
- Client di stampa Internet
- Monitor porta LPR
- Multicast MSMQ
- CMAK
- Assistenza remota
- REMOTA DEL SMTP
- Remota del-Feature-Tools-BitLocker-RemoteAdminTool
- Server di Bits remota del
- BILANCIAMENTO CARICO DI RETE REMOTA DEL
- REMOTA DEL SNMP
- REMOTA DEL WINS
- Hyper-V-Tools
- Strumenti di RDS remota del
- Gateway di RDS remota del
- Remota del-RDS-Licensing-diagnosi-interfaccia utente
- Interfaccia licenze RDS
- Interfaccia utente UpdateServices
- REMOTA DEL DISTRIBUTOR
- Remota del-Distributor-Mgmt
- Remota del risponditore Online
- REMOTA DEL ADRMS
- Remota del Fax
- Servizi di File remota del
- Remota del-DFS-Mgmt-Ezio
- Remota del-Gestione risorse file server-Mgmt
- Amministrazione di NFS remota del
- REMOTA DEL NPAS
- Servizi di stampa remota del
- Strumenti di VA remota del
- WDS AdminPack
- Server SMTP
- Client TFTP
- Redirector WebDAV
- Biometrica Framework
- Interfaccia grafica Defender di Windows
- Windows Identity-Foundation
- PowerShell ISE
- Servizio di ricerca
- IFilter TIFF di Windows
- Reti wireless
- XPS Viewer

