---
title: Configurare una stazione a schermo diviso
description: Viene descritto come configurare servizi MultiPoint in modo che due utenti possono condividere un singolo sistema
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 35d1d434-79b2-4e0a-835f-d94ed8ee6b21
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: b2d01df6175eee99fd1374cd5af270cbcc764ca8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849942"
---
# <a name="set-up-a-split-screen-station"></a>Configurare una stazione a schermo diviso
È possibile configurare una stazione con schermo diviso in modo che due utenti possono usare contemporaneamente il sistema.

Qualsiasi monitor con risoluzione minima immesso è 1200 x 720, quando si è connessi a una stazione che supporta la funzionalità con schermo diviso, può essere diviso in due stazioni. Dopo una stazione è stata divisa, desktop in cui è stato visualizzato il monitoraggio passa nella metà sinistra dello schermo e una nuova stazione viene visualizzata nella parte destra della schermata. Per completare la creazione nuova stazione, è necessario eseguire il mapping di una tastiera, mouse e hub USB per la stazione. Dopo che la stazione è stata divisa, un utente può connettersi alla stazione sulla sinistra mentre un altro utente si connette alla stazione sulla destra.  
  
Stazioni con schermo diviso offrono alcuni vantaggi:  
  
-   È possibile ridurre costi e lo spazio da tenere conto di altri utenti in un sistema MultiPoint Services.  
  
-   Due utenti possono collaborare tra loro, side-by-side, in un progetto.  
  
-   Un utente di Dashboard MultiPoint possa illustrare una procedura su una stazione mentre uno studente segue su altra stazione.  
  
La figura seguente mostra un sistema MultiPoint Services con una stazione con schermo split (sulla destra).  
  
![Workstation con schermo diviso](./media/WMS_diagram3.gif)  
   
## <a name="requirements-for-a-split-screen-station"></a>Requisiti per una stazione divisa nuovamente sullo schermo  
Per creare una stazione con schermo diviso, il monitoraggio e stazione deve soddisfare questi requisiti:  
  
-   Il monitoraggio deve avere una risoluzione di 1200 x720 o versione successiva.  
  
-   Se si usa un client USB-over-Ethernet zero, rivolgersi al fornitore dell'hardware per sapere se sono supportate stazioni con schermo diviso. Molti dispositivi client USB-over-Ethernet zero presentano limitazioni che impediscono la propria configurazione come stazioni con schermo diviso.  
  
## <a name="setting-up-a-split-screen-station"></a>Configurare una stazione con schermo diviso  
Usare le procedure seguenti per aggiungere un secondo hub di stazione con schermo diviso e quindi suddividere la stazione di MultiPoint Services. Nella procedura finale viene illustrato come restituire una stazione con schermo diviso una stazione singola.  
  
> [!NOTE]  
> Quando si divide una stazione, la sessione attiva nella stazione viene sospesa. L'utente deve accedere alla stazione per riprendere il lavoro dopo che si verifica la divisione.  
  
**Per aggiungere un secondo hub con tastiera e mouse:**  
  
1.  Collegare un hub USB a una porta USB aperta nel computer, come illustrato nella figura seguente.  
  
    ![Immagine di connessione hub USB di MultiPoint server](./media/WMSUSBHubConnection.gif)  
  
2.  Collegare una tastiera e mouse all'hub USB.  
  
    ![Immagine delle connessioni del dispositivo di input dell'hub USB](./media/WMSUSBDeviceConnection.gif)  
  
3.  Connettere le periferiche aggiuntive, ad esempio le cuffie all'hub USB.  
  
4.  Se si utilizza un hub esternamente spenta, collegare il cavo di alimentazione dell'hub a una presa.  
  
**Per dividere una stazione:**  
  
1.  In Gestione MultiPoint, fare clic sui **stazioni** scheda.  
  
2.  Sotto **stazione**, fare clic sul nome della stazione da dividere.  
  
3.  Sotto **attività degli elementi selezionati**, fare clic su **Split station**.  
  
    La schermata originale viene spostato nella metà sinistra dello schermo e schermata della nuova stazione viene creata nella metà destra dello stesso monitor.  
  
4.  Creare la nuova stazione, premere la lettera specificata sulla tastiera appena aggiunta come indicato quando il **creare una stazione di MultiPoint Server** schermata viene visualizzata nella parte destra della finestra di monitoraggio.  
  
Dopo una stazione è stata divisa, un utente può accedere alla stazione a sinistra mentre un altro utente accede alla stazione sulla destra.  
  
**Per restituire una stazione divisa nuovamente una stazione singola:**  
  
1.  In Gestione MultiPoint, fare clic sui **stazioni** scheda.  
  
2.  Sotto **stazione**, fare clic sul nome della stazione che si desidera non diviso.  
  
3.  Sotto **attività degli elementi selezionati**, fare clic su **Unsplit station**.