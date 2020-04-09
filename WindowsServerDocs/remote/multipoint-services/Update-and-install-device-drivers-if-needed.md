---
title: Aggiornare e installare i driver di dispositivo se necessario
description: Informazioni su come controllare e aggiornare i driver di dispositivo in MultiPoint Services
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 16be3ef9-a05b-4621-a431-5806b567e997
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 6d20aa80edeafa4311262a380cfd7aad65ae0315
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820604"
---
# <a name="update-and-install-device-drivers-if-needed"></a>Aggiornare e installare i driver di dispositivo se necessario
Se si usano client USB zero o periferiche che richiedono driver, è necessario installare i driver in questo momento. È inoltre consigliabile controllare **Device Manager** per tutti gli avvisi del driver e installare i driver per tali dispositivi.  
  
In genere, i driver più recenti sono necessari per i tipi di dispositivi seguenti:  
  
-   Client USB zero  
  
-   Client USB-over-Ethernet zero  
  
-   Controller del disco  
  
-   Schede di rete  
  
-   Controller audio  
  
-   Controller host USB

-   Schede grafiche


## <a name="to-check-for-driver-alerts-in-device-manager"></a>Per verificare la presenza di avvisi driver in Device Manager  
  
1.  Aprire la schermata Start.  
  
2.  Digitare **Gestione computer**, quindi fare clic su **Gestione computer** nei risultati.  
  
3.  Nell'albero della console di gestione computer fare clic su **Device Manager**.  
  
4.  Nei dispositivi di sistema a destra, verificare la presenza di avvisi driver che potrebbero influire su MultiPoint Server.  
  
## <a name="to-install-device-drivers-in-multipoint-manager"></a>Per installare i driver di dispositivo in Gestione MultiPoint  
  
1.  Per aprire Gestione MultiPoint, cercare "MultiPoint Manager" e quindi fare clic su **Gestione MultiPoint** nei risultati.  
  
2.  In Gestione MultiPoint fare clic sulla scheda **Home** , quindi fare clic su **passa alla modalità console**.  
  
3.  Per installare un driver di dispositivo, fare doppio clic sul file del driver e seguire le istruzioni per installare il driver.  
  
4.  Ripetere il passaggio precedente per installare tutti i driver necessari.  
  
    > [!NOTE]  
    > Se per un'installazione è necessario riavviare il computer, sarà necessario tornare alla modalità console prima di installare il driver successivo. MultiPoint Server viene sempre avviato in modalità stazione. Per passare alla modalità console, passare alla scheda **Home** in Gestione MultiPoint e fare clic su **passa alla modalità console**.