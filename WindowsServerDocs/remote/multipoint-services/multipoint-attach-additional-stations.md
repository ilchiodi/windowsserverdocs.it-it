---
title: Alleghi altre stazioni al server MultiPoint
description: Aggiungere altre stazioni alla distribuzione di MultiPoint Services
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 45340f02b120b1431b1f58a58ed03ea40e17a14c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394754"
---
# <a name="attach-additional-stations-to-multipoint-services"></a>Connetti stazioni aggiuntive a servizi MultiPoint
Nell'ambiente MultiPoint Services gli utenti usano le stazioni per connettersi a MultiPoint Services ed eseguire le operazioni. Le stazioni sono gli endpoint utente per la connessione al computer che esegue multipoint Services.  
  
MultiPoint Services supporta tre tipi di stazione:  
  
-   Direct-video-stazioni connesse  
  
-   USB zero stazioni connesse al client (e USB su Ethernet zero-stazioni connesse client)  
  
-   Stazioni connesse RDP-over-LAN  
  
Le classificazioni si basano sull'hardware di una stazione e sul tipo di connessione utilizzato. È possibile combinare e abbinare i tipi di connessione per le stazioni. L'unico requisito è che la stazione primaria, installata in precedenza, deve essere una stazione connessa con video diretto. Per ulteriori informazioni sulle configurazioni delle stazioni, vedere [multipoint](MultiPoint-services-Stations.md)Station.  
  
Per istruzioni su come configurare ogni tipo di stazione, vedere gli argomenti seguenti:  
  
-   [Configurare una stazione con connessione direct-video](Set-up-a-direct-video-connected-station-in-MultiPoint-services.md)  
  
-   [Configurare una stazione con connessione USB zero-client](Set-up-a-USB-zero-client-connected-station-in-MultiPoint-services.md)  
  
-   [Configurare una stazione con connessione RDP su LAN](Set-up-an-RDP-over-LAN-connected-station-in-MultiPoint-services.md)  
  
Per un confronto dettagliato dei tipi di stazione, vedere [confronto tra](multipoint-services-stations.md#BKMK_StationTypeComparison)i tipi di stazione.  
  
> [!NOTE]  
> -   Le procedure per il fissaggio delle stazioni non descrivono come configurare gli hub intermedi o gli hub downstream. Per informazioni sulla posizione in cui installare questi hub, vedere [multipoint stations](MultiPoint-services-Stations.md).  
> -   In alcuni casi, potrebbe essere necessario creare desktop virtuali della stazione, che vengono eseguiti in macchine virtuali. Ad esempio, si usano applicazioni che non possono essere installate in Windows Server o applicazioni che non eseguono più istanze nello stesso computer host. Per altre informazioni, vedere [creare desktop virtuali Windows 10 Enterprise per le stazioni](Create-Windows-10-Enterprise-virtual-desktops-for-stations.md).  
  
> [!TIP]  
> È utile creare le stazioni nell'ordine dei percorsi fisici in modo che vengano identificate in sequenza in MultiPoint Server. Se in seguito si vuole modificare il nome di una stazione, è possibile farlo in Gestione MultiPoint. Per ulteriori informazioni, vedere l'argomento relativo al mapping di tutte le stazioni in MultiPoint Server Guida e supporto tecnico.