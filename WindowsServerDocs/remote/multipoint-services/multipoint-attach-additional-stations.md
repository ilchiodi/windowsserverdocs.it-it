---
title: Collegare stazioni aggiuntive al server MultiPoint
description: Aggiungere ulteriori stazioni alla distribuzione di MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d78ebf4e-0968-4014-9a42-9f75cc50cb52
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: 57fc8ed6774c3266298ecd98e8f609ec01f63ef6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889262"
---
# <a name="attach-additional-stations-to-multipoint-services"></a>Collegare stazioni aggiuntive ai servizi MultiPoint
Nell'ambiente MultiPoint Services, gli utenti usano le stazioni per connettersi ai servizi MultiPoint e svolgere il proprio lavoro. Le stazioni sono gli endpoint di utente per la connessione al computer che esegue Multipoint Services.  
  
Servizi multiPoint supporta tre tipi di stazione:  
  
-   Stazioni Direct-video-connessi  
  
-   USB zero client connesso stazioni (e USB su stazioni client connesso Ethernet zero)  
  
-   Stazioni con connessione RDP-over-LAN  
  
Le classificazioni si basano su hardware della stazione e il tipo di connessione che usa. È possibile combinare e associare i tipi di connessione per le stazioni. L'unico requisito è che la stazione principale (che è stato installato in precedenza) deve essere una stazione di direct-video-connessi. Per altre informazioni sulle impostazioni di stazione, vedere [stazioni MultiPoint](MultiPoint-services-Stations.md).  
  
Per le istruzioni su come configurare ogni tipo di stazione, vedere gli argomenti seguenti:  
  
-   [Configurare una stazione di direct-video-connessi](Set-up-a-direct-video-connected-station-in-MultiPoint-services.md)  
  
-   [Configurare una porta USB zero stazione client connesso](Set-up-a-USB-zero-client-connected-station-in-MultiPoint-services.md)  
  
-   [Configurare una stazione di RDP-over-LAN connessa](Set-up-an-RDP-over-LAN-connected-station-in-MultiPoint-services.md)  
  
Per un confronto dettagliato dei tipi di stazione, vedere [confronto di tipo stazione](multipoint-services-stations.md#BKMK_StationTypeComparison).  
  
> [!NOTE]  
> -   Le procedure per collegare stazioni non descrivono come configurare hub intermedio o hub downstream. Per informazioni su come installare questi hub, vedere [stazioni MultiPoint](MultiPoint-services-Stations.md).  
> -   In alcuni casi, potrebbe essere necessario creare stazione di desktop virtuali, che vengono eseguite in macchine virtuali. Ad esempio, si utilizzano applicazioni che non possono essere installate in Windows Server o applicazioni che non vengono eseguite più istanze nello stesso computer host. Per altre informazioni, vedere [desktop virtuale creare Windows 10 Enterprise per le stazioni](Create-Windows-10-Enterprise-virtual-desktops-for-stations.md).  
  
> [!TIP]  
> È utile creare le stazioni nell'ordine dei relativi percorsi fisici in modo che vengono identificati in modo sequenziale in MultiPoint Server. Se in un secondo momento si desidera modificare il nome di una stazione, è possibile farlo selezionare Gestione MultiPoint. Per altre informazioni, vedere la modifica del mapping tutte le stazioni MultiPoint Server Guida e supporto tecnico.