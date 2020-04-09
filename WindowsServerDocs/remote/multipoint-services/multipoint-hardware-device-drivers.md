---
title: Raccogliere hardware e driver di dispositivo necessari per l'installazione
description: Informazioni sui driver che è necessario installare per servizi MultiPoint
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 4cf5fdbe-b871-4360-b003-d65ac43b491e
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: 57e47b357d5b6311c69cf54a74e3eaff7913da53
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858724"
---
# <a name="collect-hardware-and-device-drivers-needed-for-the-installation"></a>Raccogliere hardware e driver di dispositivo necessari per l'installazione
Prima di iniziare a distribuire il sistema MultiPoint Services, sarà necessario:  
  
-   **Componenti hardware per il server** : installare le schede video aggiuntive o altri componenti di sistema in questo momento.  
  
-   **Componenti hardware per le stazioni** : per informazioni sulle stazioni di pianificazione per l'ambiente in uso, vedere [selezione dell'hardware per il sistema MultiPoint Services](Selecting-Hardware-for-Your-MultiPoint-services-System.md).
-   **I driver più recenti per le schede video** : se il produttore OEM o del dispositivo non li ha forniti, sarà necessario scaricarli dal sito Web del produttore del dispositivo.  
  
-   **Driver USB zero client più recenti** : se si usano le stazioni USB zero client, è necessario installare i driver del client USB zero più recenti.  
  
    > [!IMPORTANT]  
    > Per un'installazione di MultiPoint Services, è necessario installare la versione a 64 bit di tutti i driver.  
  
> [!TIP]  
> Se si installa MultiPoint Services in un computer in cui è già installata una versione diversa di Windows, è necessario trovare la scheda video marca e modello in Device Manager prima di avviare l'installazione di Windows Server e verificare che sia possibile ottenere i driver disponibili per Windows Server 2016. Aprire Device Manager, aprire **Gestione computer** dalla schermata **Start** . Quindi, nell'albero della console fare clic su **Device Manager**.