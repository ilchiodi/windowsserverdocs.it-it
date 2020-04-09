---
title: Configurare una workstation connessa direct-video in servizi MultiPoint
description: Informazioni su come creare una stazione connessa con video diretto in MultiPoint Services
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 82ba3517-9743-4cde-8eea-63a17edb016f
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 4bab07a21c6e2de529797e240325b3ff27446fe0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859354"
---
# <a name="set-up-a-direct-video-connected-station-in-multipoint-services"></a>Configurare una workstation connessa direct-video in servizi MultiPoint
In una stazione connesso video diretta, il monitoraggio è connesso direttamente a una porta video sul computer del Server MultiPoint. Tastiera e mouse vengono quindi connessi a un hub USB e sono associati il monitoraggio.  
  
Nella figura seguente mostra un ambiente MultiPoint Server che dispone di un singolo computer MultiPoint Server e quattro le stazioni direct-video-connesso. Per ulteriori informazioni, vedere [MultiPoint Server stazioni](MultiPoint-services-Stations.md).  
  
**Sistema MultiPoint Services con quattro connessioni video dirette**  
  
![Immagine del layout del sistema basato su USB servizi MultiPoint](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
> [!NOTE]  
> Per configurare una stazione di direct-video-connessi, è necessario utilizzare una tastiera latino (ad esempio una tastiera nella lingua inglese o lo spagnolo).  
  
## <a name="to-set-up-a-direct-video-connected-station"></a>Per configurare una workstation connessa a video diretta  
  
1.  Collegare il cavo di monitoraggio per la porta di visualizzazione video sul computer, come illustrato di seguito.  
  
    ![Immagine della connessione video al sistema basato su hub USB](./media/WMSVideoConnection.gif) 
  
2.  Collegare il cavo di alimentazione del monitor video a una presa elettrica.  
  
3.  Collegare un hub USB a una porta USB aperta nel computer, come illustrato di seguito.  
  
    ![Immagine della connessione hub USB di MultiPoint servizi](./media/WMSUSBHubConnection.gif)  
  
4.  Connettersi all'hub USB stazione tastiera e mouse.  
  
    ![Immagine delle connessioni del dispositivo di input dell'hub USB](./media/WMSUSBDeviceConnection.gif)  
  
5.  Connettere le periferiche aggiuntive, ad esempio le cuffie, hub USB.  
  
6.  Se si utilizza un hub esternamente spenta, collegare il cavo di alimentazione dell'hub a una presa.  
  
    > [!IMPORTANT]  
    > È consigliabile l'utilizzo di un hub spenta. Comportamento di funzionamento del sistema può dipendere da condizioni insufficiente corrente.  
    >   
    > Gli utenti non devono collegare tastiere e mouse direttamente alle porte USB del computer. In questo modo è probabile che causano l'associazione non corretta di più tastiere e mouse per la stazione stesso o per nessuna stazione affatto.  
  
7.  Seguire le istruzioni visualizzate sul monitor per creare la stazione.  
  
Se si aggiunta più diretta stazione video connessi all'ambiente di servizi MultiPoint, stazione principale potrebbe cambiare. È possibile facilmente scoprire che spingono stazione connesso video è la stazione principale.  
  
## <a name="to-find-out-which-direct-video-connected-station-is-the-primary-station"></a>Per scoprire quale indirizzare stazione connesso video è la stazione principale  
  
1.  Attivare tutti i monitoraggi connessi direttamente alle schede video del computer (schede video).  
  
2.  Avviare o riavviare il computer che esegue servizi MultiPoint e vedere quale monitoraggio consente di visualizzare le schermate di avvio. Che è la stazione principale.  
  
    > [!NOTE]  
    > In alcuni casi, le informazioni di avvio del BIOS vengono visualizzate contemporaneamente su più monitor. In tal caso, uno dei monitoraggi può essere considerato la "stazione primaria" allo scopo di accedere al BIOS.