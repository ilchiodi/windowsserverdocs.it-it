---
title: Aggiornare e installare i driver di dispositivo se necessario
description: Informazioni su come controllare e aggiornare i driver di dispositivo in servizi MultiPoint
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16be3ef9-a05b-4621-a431-5806b567e997
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 66477634e06df217656876b084ae37be8cb311c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829132"
---
# <a name="update-and-install-device-drivers-if-needed"></a>Aggiornare e installare i driver di dispositivo se necessario
Se si usa i client USB zero o periferiche che hanno bisogno di driver, è necessario installare i driver in questo momento. È consigliabile anche controllare **Gestione dispositivi** per eventuali avvisi di driver e installazione dei driver per tali dispositivi.  
  
In generale, i driver più recenti sono necessari per tipi di dispositivo seguenti:  
  
-   Client USB zero  
  
-   Client USB-over-Ethernet zero  
  
-   Controller del disco  
  
-   Schede di rete  
  
-   Controller audio  
  
-   Controller host USB

-   Schede grafiche


## <a name="to-check-for-driver-alerts-in-device-manager"></a>Controllare gli avvisi di driver in Gestione dispositivi  
  
1.  Aprire la schermata Start.  
  
2.  Tipo di **Gestione computer**, quindi fare clic su **Gestione Computer** nei risultati.  
  
3.  Nell'albero della console Gestione Computer, fare clic su **Gestione dispositivi**.  
  
4.  In periferiche di sistema a destra, controllare gli avvisi di driver che possono influire sul MultiPoint Server.  
  
## <a name="to-install-device-drivers-in-multipoint-manager"></a>Per installare i driver di dispositivo nella console di gestione MultiPoint  
  
1.  Per aprire Gestione MultiPoint, cercare "MultiPoint Manager" e quindi fare clic su **MultiPoint Manager** nei risultati.  
  
2.  Selezionare Gestione MultiPoint, fare clic sui **casa** scheda e quindi fare clic su **passare alla modalità console**.  
  
3.  Per installare un driver di dispositivo, fare doppio clic sul file di driver e seguire le istruzioni per installare il driver.  
  
4.  Ripetere il passaggio precedente per installare tutti i driver necessari.  
  
    > [!NOTE]  
    > Se un'installazione richiede un riavvio del computer, è necessario tornare alla modalità console prima di installare il driver successivo. MultiPoint server viene sempre avviato in modalità stazione. Per passare alla modalità console, passare al **casa** scheda di gestione MultiPoint e fare clic su **passare alla modalità console**.