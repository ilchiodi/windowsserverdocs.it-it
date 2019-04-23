---
title: Configurare una porta USB zero stazione client connesso in servizi MultiPoint
description: Informazioni su come creare una porta USB zero stazione client in servizi MultiPoint
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d2908865-6be3-474d-88f1-995f40bb61d0
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 1a64373f4ed5e0d1ac96a0257ac5697ff94ffcbe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878932"
---
# <a name="set-up-a-usb-zero-client-connected-station-in-multipoint-services"></a>Configurare una porta USB zero stazione client connesso in servizi MultiPoint
Quando si utilizza il client USB zero per creare le stazioni MultiPoint servizi, il monitoraggio per ogni stazione è connesso alla porta video sul client USB zero, come illustrato nella figura seguente. Per ulteriori informazioni su questo e altri tipi di espansione, vedere [MultiPoint stazioni](MultiPoint-services-Stations.md).
  
**Sistema multiPoint Services con una stazione di direct-video-connessi e due stazioni USB zero client connesso**  
  
![Stazioni con connessione USB zero-client](./media/WMS11_diagram7.gif)  
  
> [!IMPORTANT]  
> Prima di impostare USB zero stazioni client connesso, assicurarsi di installare i driver più recenti per le schede video e il client USB zero. Driver obsoleto possono impedire la configurazione di servizi MultiPoint venga completato correttamente. Per istruzioni, vedere [Update e installare i driver di dispositivo, se necessario](Update-and-install-device-drivers-if-needed.md).  
  
> [!IMPORTANT]  
> Se si utilizza un client USB-over-Ethernet zero, seguire le istruzioni del fornitore, invece di questa procedura, utilizzare la connessione Ethernet per configurare il dispositivo di rete.  
  
### <a name="to-set-up-a-usb-zero-client-connected-station"></a>Per configurare una porta USB zero stazione client connesso  
  
1.  Connettere il cavo del monitor video la porta di visualizzazione video VGA o DVI su USB zero client, come illustrato nella figura seguente.  
  
    ![Immagine della connessione video al sistema basato su hub USB](./media/WMSVideoConnection.gif)  
  
2.  Connettere il client USB zero a una porta USB aperta nel computer.  
  
    ![Immagine della connessione hub USB di MultiPoint servizi](./media/WMSUSBHubConnection.gif)  
  
3.  Collegare una tastiera e mouse al client USB zero.  
  
    ![Immagine delle connessioni del dispositivo di input dell'hub USB](./media/WMSUSBDeviceConnection.gif)  
  
4.  Se si utilizza un tecnologia esternamente USB zero-client, collegare il cavo di alimentazione del client USB zero a una presa di alimentazione.  
  
5.  Collegare il cavo di alimentazione del monitor video a una presa elettrica.  
  
6.  Se viene chiesto di associare la stazione di dispositivi, seguire le istruzioni per completare la configurazione del monitoraggio. (In genere, USB zero stazioni client connesso sono associate stazioni automaticamente quando vengono aggiunti al server.)