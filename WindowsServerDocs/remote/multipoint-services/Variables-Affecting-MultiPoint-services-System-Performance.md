---
title: Variabili che incidono sulle prestazioni del sistema di Servizi MultiPoint
description: Informazioni sulle prestazioni di servizi MultiPoint
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0f3e8875-1b5e-4789-b16c-d06d6e31f38e
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 7a06fcc763283114dc12ad106aa7ec146502dbd9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867582"
---
# <a name="variables-affecting-multipoint-services-system-performance"></a>Variabili che incidono sulle prestazioni del sistema di Servizi MultiPoint
Esistono numerose variabili che possono influenzare le prestazioni complessive del sistema MultiPoint Services. È possibile considerare questi quando si progetta il sistema.  
  
## <a name="usage"></a>Uso  
  
-   **Le applicazioni** il tipo e il numero di applicazioni in esecuzione nello stesso momento, soprattutto graphic\-pesanti o memoria applicazioni con utilizzo intensivo influirà sulle prestazioni complessive del sistema. Per altre informazioni, vedere [applicazioni e Internet contenuto](hardware-and-performance-recommendations.md#applications-and-internet-content).  
  
-   **Utilizzo di Internet** considerare se gli utenti visualizzeranno dei contenuti multimediali o pagine web che utilizzano i video di movimento. Questo tipo di contenuto può sovraccaricare il sistema se troppi utenti visualizzano contemporaneamente.  
  
    > [!NOTE]  
    > La funzionalità di proiezione in MultiPoint Services, che consente ai docenti di proiettare le schermate nei monitoraggi per studenti, non è progettata per proiettare i video in movimento. La funzionalità di proiezione è destinata a scopo dimostrativo, ad esempio che illustra una procedura.  
  
-   **I dispositivi ad alta velocità** se troppi utenti simultaneamente Usa un dispositivo ad alta velocità, ad esempio una webcam o un lettore DVD, questo influisce sulle prestazioni complessive del sistema.  
  
## <a name="configuration"></a>Configurazione  
  
-   **CPU, GPU e memoria RAM** visualizzare [Ottimizza prestazioni del sistema MultiPoint Services](hardware-and-performance-recommendations.md#optimize-multipoint-services-system-performance) in questa guida per le indicazioni della CPU, GPU e memoria RAM.  
-   **Larghezza di banda** per RDP-over-LAN stazioni connesse, la larghezza di banda di rete e la funzionalità del client (ad esempio, un thin client, desktop PC o laptop) è importante, in particolare se video è in esecuzione nella sessione dell'utente. Se si usano client USB-over-Ethernet zero, la larghezza di banda di rete deve anche essere presi in considerazione. Video dati per tutti i dispositivi vengono inviati tramite la stessa connessione Ethernet, pertanto è consigliabile provare a configurare una rete Gigabit Ethernet distinta quando si usano questi dispositivi.  
-   **RemoteFX** sulle stazioni RDP-over-LAN connessa, è possibile utilizzare RemoteFX per migliorare notevolmente la distribuzione di contenuti multimediali ad alta definizione.  
-   **Risoluzione dello schermo** nel caso di utilizzo intenso di video a schermo intero, è possibile provare a ridurre la risoluzione dei monitor per ottimizzare le prestazioni.  
-   **Numero di client USB zero** il numero totale di client USB zero in un hub singolo radice nel server direttamente influirà sulle prestazioni di video. Per altre informazioni, vedere [Layout per la stazioni USB Zero Client connesso](MultiPoint-services-Site-Planning.md#layout-for-usb-zero-client-connected-stations). Il numero di stazioni client USB-over-Ethernet zero supportate potrà essere leggermente inferiore rispetto al numero di client USB zero.  
-   **Larghezza di banda USB** prendere in considerazione la larghezza di banda USB durante la progettazione del sistema.  Ciò è particolarmente importante per i client USB zero, che inviano dati video tramite la connessione USB. Per ottimizzare la larghezza di banda, ridurre al minimo il numero di dispositivi connessi a una singola porta USB nel server. Questo vale per le stazioni margherita e hub intermedio. Per altre informazioni, vedere [hub di stazione](MultiPoint-services-Site-Planning.md#station-hubs) e [hub a livello intermedio](MultiPoint-services-Site-Planning.md#intermediate-hubs).  
  
-   **Tipo USB** tramite USB 3.0 invece di USB 2.0 aumenta la larghezza di banda disponibile tra il server e l'hub intermedio se ci si connette all'hub di più di tre client USB zero o se si usa dispositivi USB larghezza di banda elevata.  
  
-   **Le stazioni** il numero totale di stazioni influisce sulle prestazioni. Se hai esigenze video, l'elaborazione o grafiche complesse, è possibile limitare il numero complessivo di stazioni. Per altre informazioni, vedere [le prestazioni del sistema di servizi MultiPoint ottimizzare](hardware-and-performance-recommendations.md#optimize-multipoint-services-system-performance).