---
title: Configurare una stazione di RDP-over-LAN connessa in MultiPoint Services
description: Informazioni su come configurare un sistema RDP-over-LAN in MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 60e1a025-c2fb-4708-a3ff-c44c223a3224
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 8d2f1644918f1a581c1bcab181cd084e12c6b576
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843702"
---
# <a name="set-up-an-rdp-over-lan-connected-station-in-multipoint-services"></a>Configurare una stazione di RDP-over-LAN connessa in MultiPoint Services
Una stazione di RDP-over-LAN connessa è un thin client, un desktop tradizionali o un computer portatile che si connette ai servizi MultiPoint in una rete locale (LAN) tramite Remote Desktop Protocol (RDP). Per ulteriori informazioni su questo e altri tipi di espansione, vedere [MultiPoint stazioni](MultiPoint-services-Stations.md).  
  
## <a name="to-set-up-a-multipoint-station-using-a-computer-or-thin-client-on-a-lan"></a>Per configurare una stazione MultiPoint utilizzando un computer o un thin client in una LAN  
  
1.  Accendere il computer che esegue MultiPoint Services.  
  
2.  Assicurarsi che il computer MultiPoint Server sia connesso alla LAN da uno switch, router o altro dispositivo di rete e ha un indirizzo IP corretto. (Un indirizzo IP che inizia con 169.254 (un indirizzo APIPA) potrebbe indicare un problema con la connessione di rete LAN o che il server DHCP non è raggiungibile o non funziona correttamente).  
  
3.  Connettersi al computer client o un thin client alla rete LAN.  
  
4.  Attivare i computer client o un thin client.  
  
5.  Nel computer client o thin client, avviare connessione Desktop remoto o un'applicazione equivalente e immettere il nome o indirizzo IP del computer che esegue MultiPoint Services.

## <a name="set-up-a-windows-10-device-for-remote-management-by-using-connector-services"></a>Configurare un dispositivo Windows 10 per la gestione remota con i servizi del connettore
Qualsiasi PC o un computer portatile che eseguono Windows 10 può essere gestito in remoto, purché:
- sono stati abilitati i servizi del connettore  
- la macchina è stata aggiunta ai computer gestiti nel server MultiPoint.  

Seguire questi passaggi per abilitare il connettore MultiPoint in un computer che eseguono Windows 10:

1. Nella casella di ricerca, digitare "Windows attivazione o disattivazione delle funzionalità" e selezionare il risultato della ricerca appropriata. 

2. Nell'elenco delle funzionalità abilitare **connettore MultiPoint**. Abilita servizi del connettore MultiPoint che sono necessari per gestire il dispositivo. 

Nel server MultiPoint:
1. Aprire Gestione MultiPoint e selezionare **aggiungere o rimuovere personal computer** oppure **Add o remove MultiPoint Services**.

2. Selezionare i computer remoti da gestire e fare clic su **OK**.  Richiederà di credenziali di amministratore sui computer remoti.  Al termine, il computer remoto verrà visualizzato nella scheda home di gestione MultiPoint.

Quando set di Dashboard Manager può monitorare correttamente agli utenti che lavorano sul dispositivo gestito.

> [!IMPORTANT]  
> Quando il monitoraggio gestito administratrive di dispositivi Windows 10 gli utenti non possono essere monitorati, ad eccezione di server le impostazioni sono state modificate di conseguenza. Vedere [modificare le impostazioni del Server](Edit-Server-Settings.md)