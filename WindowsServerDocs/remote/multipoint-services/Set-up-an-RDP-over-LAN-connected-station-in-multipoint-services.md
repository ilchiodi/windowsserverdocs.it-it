---
title: Configurare una stazione connessa RDP-over-LAN in MultiPoint Services
description: Informazioni su come configurare un sistema RDP-over-LAN in MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 60e1a025-c2fb-4708-a3ff-c44c223a3224
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: d0d63a75d3ef6e042d44df0ecf4cc08973e859a6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394998"
---
# <a name="set-up-an-rdp-over-lan-connected-station-in-multipoint-services"></a>Configurare una stazione connessa RDP-over-LAN in MultiPoint Services
Una stazione connessa RDP-over-LAN è un thin client, un desktop tradizionale o un computer portatile che si connette a MultiPoint Services in una rete locale (LAN) usando il Remote Desktop Protocol (RDP). Per ulteriori informazioni su questo e altri tipi di espansione, vedere [MultiPoint stazioni](MultiPoint-services-Stations.md).  
  
## <a name="to-set-up-a-multipoint-station-using-a-computer-or-thin-client-on-a-lan"></a>Per configurare una stazione MultiPoint usando un computer o thin client in una LAN  
  
1.  Accendere il computer che esegue MultiPoint Services.  
  
2.  Verificare che il computer Server MultiPoint sia connesso alla LAN da un commutire, un router o un altro dispositivo di rete e disponga di un indirizzo IP appropriato. Un indirizzo IP che inizia con 169,254 (indirizzo APIPA) potrebbe indicare che si è verificato un problema con la connessione LAN o che il server DHCP non è raggiungibile o non funziona correttamente.  
  
3.  Connettere il computer client o thin client alla LAN.  
  
4.  Accendere il computer client o thin client.  
  
5.  Nel computer client o thin client, avviare Connessione Desktop remoto o un'applicazione equivalente e immettere il nome o l'indirizzo IP del computer in cui è in esecuzione MultiPoint Services.

## <a name="set-up-a-windows-10-device-for-remote-management-by-using-connector-services"></a>Configurare un dispositivo Windows 10 per la gestione remota usando i servizi connettore
Qualsiasi PC o computer portatile che esegue Windows 10 può essere gestito in remoto, purché:
- i servizi del connettore sono stati abilitati  
- il computer è stato aggiunto ai computer gestiti nel server MultiPoint.  

Nel computer che esegue Windows 10 seguire questa procedura per abilitare MultiPoint Connector:

1. Nella casella di ricerca digitare "attivazione o disattivazione delle funzionalità Windows" e selezionare il risultato della ricerca appropriato. 

2. Nell'elenco delle funzionalità abilitare **MultiPoint Connector**. In questo modo verranno abilitati i servizi MultiPoint Connector necessari per gestire il dispositivo. 

Sul server MultiPoint:
1. Aprire Gestione MultiPoint e selezionare **Aggiungi o Rimuovi personal computer** oppure **Aggiungi o Rimuovi servizi multipoint**.

2. Selezionare i computer remoti che si desidera gestire e fare clic su **OK**.  Verranno richieste le credenziali di amministratore nei computer remoti.  Al termine, i computer remoti vengono visualizzati nella scheda Home di gestione MultiPoint.

Quando la gestione Dashboard è stata impostata correttamente, è possibile monitorare gli utenti che lavorano sul dispositivo gestito.

> [!IMPORTANT]  
> Quando si esegue il monitoraggio di dispositivi Windows 10 gestiti administratrive gli utenti non possono essere monitorati, ad eccezione delle impostazioni del server modificate di conseguenza. Vedere [modificare le impostazioni del server](Edit-Server-Settings.md)