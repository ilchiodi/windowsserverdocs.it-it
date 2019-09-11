---
title: Raccogliere hardware e driver di dispositivo necessari per l'installazione
description: Informazioni sui driver che è necessario installare per servizi MultiPoint
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4cf5fdbe-b871-4360-b003-d65ac43b491e
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: f7fec373bc62c93fbf31bbb24bf1a11a42c0736d
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871435"
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
> Se si installa MultiPoint Services in un computer in cui è già installata una versione diversa di Windows, è necessario trovare la scheda video marca e modello in Device Manager prima di avviare l'installazione di Windows Server e verificare che sia possibile ottenere i driver disponibile per Windows Server 2016. Aprire Device Manager, aprire **Gestione computer** dalla schermata **Start** . Quindi, nell'albero della console fare clic su **Device Manager**.