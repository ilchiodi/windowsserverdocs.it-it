---
title: Configurare il computer fisico e la stazione principale
description: Informazioni su come configurare il sistema prima, la stazione principale, in servizi MultiPoint
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4e83b126-ce9a-4cd7-a0bd-6627c9e0f81b
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 6569c4963d31b72216943bf29b71411e702caf84
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812012"
---
# <a name="set-up-the-physical-computer-and-primary-station"></a>Configurare il computer fisico e la stazione principale
Prima di installare servizi MultiPoint, è necessario configurare la stazione principale per il sistema MultiPoint Services. Se si usa una rete locale (LAN) collegare il computer alla rete LAN.  
  
Oggetto *stazione* è un endpoint mediante il quale è accedere ai servizi MultiPoint. Il *stazione principale* è la stazione prima di avviare all'avvio di servizi MultiPoint. Gli amministratori possono utilizzarlo per i menu di avvio di accesso e le impostazioni. La stazione principale fornisce l'accesso alla configurazione di sistema e risolvere i problemi relativi che è disponibile solo durante l'avvio e prima di servizi MultiPoint sistema è in esecuzione. Dopo l'avvio, è possibile usare la stazione principale come si farebbe con qualsiasi altra stazione.  
  
La stazione principale deve essere una stazione di direct-video-connessi. La procedura seguente viene descritto come connettere l'hardware necessari al computer MultiPoint Services.  
  
Per altre informazioni sulle stazioni, vedere [stazioni MultiPoint](multipoint-services-stations.md). Per assistenza per effettuare le selezioni di hardware, vedere [selezionando l'Hardware per il sistema MultiPoint Services](Selecting-Hardware-for-Your-MultiPoint-services-System.md). Per informazioni sulla connessione altre stazioni tipi ai servizi MultiPoint, vedere [collegare stazioni aggiuntive al computer MultiPoint Services](Attach-additional-stations-to-your-MultiPoint-services-computer.md).  
  
> [!NOTE]  
> Per creare una stazione connesso video, è necessario utilizzare una tastiera latino (ad esempio una tastiera inglese o spagnolo).  
  
## <a name="to-set-up-your-primary-station"></a>Per configurare la stazione principale  
  
1.  Assicurarsi che il computer che esegue servizi MultiPoint è spento e scollegato.  
  
2.  Collegare il cavo di alimentazione del monitoraggio a una presa elettrica e collegare il cavo del monitor alla porta di visualizzazione video sul computer, come illustrato di seguito.  
  
    ![Immagine della connessione video al sistema basato su hub USB](./media/WMSVideoConnection.gif)  
  
3.  Se la stazione utilizzerà una tastiera USB e un mouse, completare i passaggi seguenti:  
  
    1.  Collegare un hub USB esterni a una porta USB aperta nel computer, come illustrato di seguito.  
  
        ![Immagine della connessione hub USB di MultiPoint servizi](./media/WMSUSBHubConnection.gif)  
  
    2.  Connettere la tastiera e mouse USB all'hub USB.  
  
        ![Immagine delle connessioni del dispositivo di input dell'hub USB](./media/WMSUSBDeviceConnection.gif)  
  
        > [!NOTE]  
        > Se nel computer di servizi MultiPoint è PS/2 porte, è possibile, se necessario, utilizzare una tastiera PS/2 e un mouse collegato direttamente al computer. Tuttavia, il programma di installazione presenta limitazioni significative. Gli utenti non possono usare dispositivi audio, CAM, web e sulle stazioni PS/2 unità flash.  
  
    3.  Se si utilizza un hub esternamente spenta, collegare il cavo di alimentazione dell'hub a una presa.  
  
        > [!IMPORTANT]  
        > È consigliabile l'utilizzo di un hub spenta. Comportamento di funzionamento del sistema può dipendere da condizioni insufficiente corrente.  
        >   
        > Gli utenti non devono collegare tastiere e mouse direttamente alle porte USB del computer. In questo modo è probabile che causano l'associazione non corretta di più tastiere e mouse per la stazione stesso o per nessuna stazione affatto.  
  
        > [!NOTE]  
        > Il dispositivo audio host sulla scheda madre del sistema è disponibile solo mentre servizi MultiPoint è in modalità console. Per garantire continuità audio per una stazione che usa un hub USB esterno, è necessario usare un dispositivo audio USB collegato all'hub.  
  
## <a name="to-connect-the-computer-to-the-lan"></a>Per connettere il computer alla rete locale  
  
-   Se si dispone di una rete LAN, connettere il computer alla rete con un cavo di rete.