---
title: Variabili che incidono sulle prestazioni del sistema di Servizi MultiPoint
description: Informazioni sulle prestazioni per servizi MultiPoint
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 0f3e8875-1b5e-4789-b16c-d06d6e31f38e
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 44f268c958ed32e527b66cebe1a10d33652eb9b0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858914"
---
# <a name="variables-affecting-multipoint-services-system-performance"></a>Variabili che incidono sulle prestazioni del sistema di Servizi MultiPoint
Sono disponibili molte variabili che possono influire sulle prestazioni complessive del sistema MultiPoint Services. Quando si progetta il sistema, è consigliabile prendere in considerazione questi casi.  
  
## <a name="usage"></a>Utilizzo  
  
-   **Applicazioni** Il tipo e il numero di applicazioni in esecuzione nello stesso momento, in particolare grafica\-applicazioni pesanti o con utilizzo intensivo di memoria avranno effetto sulle prestazioni complessive del sistema. Per ulteriori informazioni, vedere [applicazioni e contenuti Internet](hardware-and-performance-recommendations.md#applications-and-internet-content).  
  
-   **Uso di Internet** Tenere presente che se gli utenti visualizzeranno contenuto multimediale o pagine Web che usano video con movimento completo. Questo tipo di contenuto può sovraccaricare il sistema se viene visualizzato un numero eccessivo di utenti contemporaneamente.  
  
    > [!NOTE]  
    > La funzionalità di proiezione in MultiPoint Services, che consente agli insegnanti di proiettare le schermate sui propri monitor degli studenti, non è progettata per proiettare video in movimento completo. La funzionalità di proiezione è progettata a scopo dimostrativo, ad esempio la visualizzazione di una procedura.  
  
-   **Dispositivi ad alta velocità** Se un numero eccessivo di utenti usa contemporaneamente un dispositivo ad alta velocità, ad esempio una fotocamera Web o un lettore DVD, questo influiscono sulle prestazioni complessive del sistema.  
  
## <a name="configuration"></a>Configurazione  
  
-   **CPU, GPU e RAM** Vedere [ottimizzare le prestazioni del sistema MultiPoint Services](hardware-and-performance-recommendations.md#optimize-multipoint-services-system-performance) in questa guida per le raccomandazioni relative a CPU, GPU e RAM.  
-   **Larghezza di banda di rete** Per le stazioni connesse RDP-over-LAN, la larghezza di banda di rete e le funzionalità del client, ad esempio un thin client, un PC desktop o un portatile, sono importanti, soprattutto se il video è in esecuzione nella sessione dell'utente. Se si usano client USB-over-Ethernet zero, è opportuno considerare anche la larghezza di banda di rete. I dati video per tutti i dispositivi vengono inviati tramite la stessa connessione Ethernet, quindi è consigliabile configurare una rete Ethernet Gigabit separata quando si usano questi dispositivi.  
-   **RemoteFX** Per le stazioni connesse RDP-over-LAN, è possibile usare RemoteFX per migliorare significativamente la distribuzione di contenuti multimediali ad alta definizione.  
-   **Risoluzione dello schermo** Se si dispone di un utilizzo elevato di video a schermo intero, è consigliabile ridurre la risoluzione del monitor per ottimizzare le prestazioni.  
-   **Numero di client USB zero** Il numero totale di client USB zero in un singolo hub radice sul server influirà direttamente sulle prestazioni del video. Per altre informazioni, vedere [layout per le stazioni USB zero client connesse](MultiPoint-services-Site-Planning.md#layout-for-usb-zero-client-connected-stations). Il numero di stazioni USB-over-Ethernet zero client supportate potrebbe essere leggermente inferiore al numero di client USB zero.  
-   **Larghezza di banda USB** Prendere in considerazione la larghezza di banda USB durante la progettazione del sistema.  Questa operazione è particolarmente importante per i client USB zero, che inviano dati video tramite la connessione USB. Per ottimizzare la larghezza di banda, ridurre al minimo il numero di dispositivi connessi a una singola porta USB sul server. Questo vale per le stazioni concatenate a Margherita e per gli hub intermedi. Per altre informazioni, vedere [Hub di stazione](MultiPoint-services-Site-Planning.md#station-hubs) e [hub intermedi](MultiPoint-services-Site-Planning.md#intermediate-hubs).  
  
-   **Tipo USB** L'uso di USB 3,0 anziché USB 2,0 aumenta la larghezza di banda disponibile tra il server e l'hub intermedio se si connettono più di tre client USB all'hub o se si usano dispositivi USB a larghezza di banda elevata.  
  
-   **Stazioni** di Il numero totale di stazioni influiscono sulle prestazioni. Se si dispone di un elevato livello di grafica, elaborazione o esigenze video, è possibile che si desideri limitare il numero complessivo di stazioni. Per altre informazioni, vedere [ottimizzare le prestazioni del sistema MultiPoint Services](hardware-and-performance-recommendations.md#optimize-multipoint-services-system-performance).