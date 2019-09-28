---
title: Configurare il computer fisico e la stazione principale
description: Informazioni su come configurare il primo sistema, la stazione primaria, in MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4e83b126-ce9a-4cd7-a0bd-6627c9e0f81b
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 1a5865b6bd15b6cd07cde393012afd495e3378be
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395293"
---
# <a name="set-up-the-physical-computer-and-primary-station"></a>Configurare il computer fisico e la stazione principale
Prima di installare MultiPoint Services, è necessario configurare la stazione primaria per il sistema MultiPoint Services. Se si utilizzerà una rete locale (LAN), connettere il computer alla LAN.  
  
Una *stazione* è un endpoint con cui si accede a MultiPoint Services. La *stazione primaria* è la prima stazione da avviare quando si avvia multipoint Services. Gli amministratori possono usarlo per accedere ai menu e alle impostazioni di avvio. La stazione primaria fornisce l'accesso alla configurazione del sistema e alle informazioni per la risoluzione dei problemi disponibili solo durante l'avvio e prima che il sistema MultiPoint Services sia in esecuzione. Al termine dell'avvio, è possibile usare la stazione primaria come qualsiasi altra stazione.  
  
La stazione primaria deve essere una stazione connessa a video diretto. Nella procedura seguente viene descritto come connettere l'hardware necessario al computer Servizi MultiPoint.  
  
Per altre informazioni sulle stazioni, vedere [multipoint stations](multipoint-services-stations.md). Per informazioni sull'esecuzione di selezioni hardware, vedere [selezione dell'hardware per il sistema MultiPoint Services](Selecting-Hardware-for-Your-MultiPoint-services-System.md). Per informazioni sulla connessione di altri tipi di stazioni a MultiPoint Services, vedere [collegare stazioni aggiuntive al computer Servizi multipoint](Attach-additional-stations-to-your-MultiPoint-services-computer.md).  
  
> [!NOTE]  
> Per creare una stazione connessa al video, è necessario usare una tastiera latina, ad esempio una tastiera in lingua inglese o spagnola.  
  
## <a name="to-set-up-your-primary-station"></a>Per configurare la stazione primaria  
  
1.  Verificare che il computer in cui è in esecuzione MultiPoint Services sia disattivato e scollegato.  
  
2.  Connettere il cavo di alimentazione del monitor a una presa di alimentazione e connettere il cavo di monitoraggio alla porta video visualizzata sul computer, come illustrato di seguito.  
  
    ![Immagine della connessione video al sistema basato su hub USB](./media/WMSVideoConnection.gif)  
  
3.  Se la stazione userà una tastiera USB e un mouse, completare i passaggi seguenti:  
  
    1.  Connettere un hub USB esterno a una porta USB aperta nel computer, come illustrato di seguito.  
  
        ![Immagine della connessione hub USB di MultiPoint servizi](./media/WMSUSBHubConnection.gif)  
  
    2.  Connettere la tastiera e il mouse USB all'hub USB.  
  
        ![Immagine delle connessioni del dispositivo di input dell'hub USB](./media/WMSUSBDeviceConnection.gif)  
  
        > [!NOTE]  
        > Se il computer Servizi MultiPoint dispone di porte PS/2, è possibile, se necessario, utilizzare una tastiera e un mouse PS/2 collegati direttamente al computer. Tuttavia, questa configurazione presenta limitazioni significative. Gli utenti non possono usare dispositivi audio, Web Cams e unità flash nelle stazioni PS/2.  
  
    3.  Se si utilizza un hub esternamente spenta, collegare il cavo di alimentazione dell'hub a una presa.  
  
        > [!IMPORTANT]  
        > È consigliabile l'utilizzo di un hub spenta. Comportamento di funzionamento del sistema può dipendere da condizioni insufficiente corrente.  
        >   
        > Gli utenti non devono collegare tastiere e mouse direttamente alle porte USB del computer. In questo modo è probabile che causano l'associazione non corretta di più tastiere e mouse per la stazione stesso o per nessuna stazione affatto.  
  
        > [!NOTE]  
        > Il dispositivo audio host nella scheda madre del sistema è disponibile solo se MultiPoint Services è in modalità console. Per garantire l'audio senza interruzioni per una stazione che usa un hub USB esterno, è necessario usare un dispositivo audio USB collegato all'hub.  
  
## <a name="to-connect-the-computer-to-the-lan"></a>Per connettere il computer alla LAN  
  
-   Se si dispone di una LAN, connettere il computer alla rete con un cavo di rete.