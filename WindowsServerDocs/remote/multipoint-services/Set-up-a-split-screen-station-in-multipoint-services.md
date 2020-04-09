---
title: Configurare una stazione a schermo diviso
description: Viene descritto come configurare i servizi MultiPoint in modo che due utenti possano condividere un singolo sistema
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 35d1d434-79b2-4e0a-835f-d94ed8ee6b21
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 006422614b87cd331dd157218ac910b6f7af1602
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853914"
---
# <a name="set-up-a-split-screen-station"></a>Configurare una stazione a schermo diviso
È possibile configurare una stazione a schermo intero in modo che due utenti possano usare simultaneamente il sistema.

Qualsiasi monitoraggio con una risoluzione minima di 1200 X720, quando si è connessi a una stazione che supporta la funzionalità Split screen, può essere suddiviso in due stazioni. Dopo la suddivisione di una stazione, il desktop visualizzato dal monitor si sposta sulla metà sinistra dello schermo e viene visualizzata una nuova stazione nella metà destra dello schermo. Per completare la creazione della nuova stazione, sarà necessario eseguire il mapping di una tastiera, un mouse e un hub USB alla stazione. Dopo che la stazione è stata divisa, un utente può connettersi alla stazione sulla sinistra mentre un altro utente si connette alla stazione sulla destra.  
  
Le stazioni a schermo condiviso presentano diversi vantaggi:  
  
-   Per ridurre i costi e lo spazio, è possibile ospitare più utenti in un sistema MultiPoint Services.  
  
-   Due utenti possono collaborare insieme, affiancato, in un progetto.  
  
-   Un utente di Dashboard MultiPoint può dimostrare una procedura in una stazione mentre uno studente segue l'altra stazione.  
  
La figura seguente mostra un sistema MultiPoint Services con una stazione Split Screen (a destra).  
  
![Workstation con schermo diviso](./media/WMS_diagram3.gif)  
   
## <a name="requirements-for-a-split-screen-station"></a>Requisiti per una stazione Split screen  
Per creare una stazione a schermo diviso, il monitoraggio e la stazione devono soddisfare i requisiti seguenti:  
  
-   Il monitoraggio deve avere una risoluzione di 1200 X720 o superiore.  
  
-   Se si usa un client USB-over-Ethernet zero, rivolgersi al fornitore dell'hardware per verificare se sono supportate le stazioni a schermo diviso. Molti dispositivi client USB-over-Ethernet zero presentano limitazioni che ne impediscono la configurazione come stazioni a schermo diviso.  
  
## <a name="setting-up-a-split-screen-station"></a>Configurazione di una stazione a schermo diviso  
Usare le procedure seguenti per aggiungere un secondo hub per una stazione a schermo diviso e quindi dividere la stazione in MultiPoint Services. Nella procedura finale viene illustrato come restituire una stazione a schermo diviso a una singola stazione.  
  
> [!NOTE]  
> Quando si divide una stazione, la sessione attiva nella stazione viene sospesa. È necessario che l'utente acceda nuovamente alla stazione per riprendere il lavoro dopo aver eseguito la suddivisione.  
  
**Per aggiungere un secondo hub con la tastiera e il mouse:**  
  
1.  Connettere un hub USB a una porta USB aperta nel computer, come illustrato nella figura seguente.  
  
    ![Immagine della connessione hub USB di MultiPoint Server](./media/WMSUSBHubConnection.gif)  
  
2.  Connettere una tastiera e un mouse all'hub USB.  
  
    ![Immagine delle connessioni del dispositivo di input dell'hub USB](./media/WMSUSBDeviceConnection.gif)  
  
3.  Connettere le periferiche aggiuntive, ad esempio le cuffie, all'hub USB.  
  
4.  Se si utilizza un hub esternamente spenta, collegare il cavo di alimentazione dell'hub a una presa.  
  
**Per dividere una stazione:**  
  
1.  In Gestione MultiPoint fare clic sulla scheda **stazioni** .  
  
2.  In **stazione**fare clic sul nome della stazione che si desidera dividere.  
  
3.  In **attività elemento selezionato**fare clic su **Dividi stazione**.  
  
    La schermata originale viene spostata sulla metà sinistra del monitoraggio e viene creata una nuova schermata della stazione sulla metà destra dello stesso monitor.  
  
4.  Creare la nuova stazione premendo la lettera specificata sulla tastiera appena aggiunta, come indicato quando nella metà destra del monitor viene visualizzata la schermata **creare una stazione MultiPoint Server** .  
  
Dopo la suddivisione di una stazione, un utente può accedere alla stazione a sinistra mentre un altro utente accede alla stazione corretta.  
  
**Per restituire una stazione di divisione a un'unica stazione:**  
  
1.  In Gestione MultiPoint fare clic sulla scheda **stazioni** .  
  
2.  In **stazione**fare clic sul nome della stazione che si vuole separare.  
  
3.  In **attività elemento selezionato**fare clic su **unsplit Station**.