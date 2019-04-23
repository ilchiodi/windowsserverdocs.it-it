---
title: Raccogliere hardware e driver di dispositivo necessari per l'installazione
description: Informazioni sui driver, è necessario installare per i servizi MultiPoint
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
ms.openlocfilehash: a9d902e2599cdcd69e156d1fabec87a067b1d8ea
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833422"
---
# <a name="collect-hardware-and-device-drivers-needed-for-the-installation"></a>Raccogliere hardware e driver di dispositivo necessari per l'installazione
Prima di iniziare la distribuzione del sistema MultiPoint Services, è necessario:  
  
-   **Componenti hardware per il server** -installare eventuali altre schede video e altri componenti di sistema in questo momento.  
  
-   **Componenti hardware per le stazioni** : per informazioni sulla pianificazione stazioni per l'ambiente, vedere [selezionando l'Hardware per il sistema MultiPoint Services](Selecting-Hardware-for-Your-MultiPoint-services-System.md).
-   **I driver più recenti per le schede video** -se il produttore OEM o il dispositivo non ha fornito queste, è necessario eseguirne il download dal sito Web del produttore del dispositivo.  
  
-   **I driver più recenti del client USB zero** -se si usa stazioni client USB zero, è necessario installare i driver più recenti del client USB zero.  
  
    > [!IMPORTANT]  
    > Per un'installazione di MultiPoint Services, è necessario installare la versione a 64 bit di qualsiasi driver.  
  
> [!TIP]  
> Se si installa Servizi MultiPoint in un computer con una versione diversa di Windows già installata, è necessario scoprire scheda video marca e modello in Gestione dispositivi prima di avviare l'installazione di Windows Server e verificare che è possibile ottenere i driver che sono è disponibile per Windows Server 2016. Aprire Gestione dispositivi, aprire **Gestione Computer** dalle **avviare** dello schermo. Quindi, nell'albero della console, fare clic su **Gestione dispositivi**.