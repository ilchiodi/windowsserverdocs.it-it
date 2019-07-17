---
title: Contatori delle prestazioni di rete
description: Questo argomento fa parte della Guida all'ottimizzazione delle prestazioni del sottosistema di rete di Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 7ebaa271-2557-4c24-a679-c3d863e6bf9e
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bcb0c1c5a08a306fbd9b419d0c458c3bc54e1786
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446198"
---
# <a name="network-related-performance-counters"></a>Contatori delle prestazioni di rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento sono elencati i contatori sono rilevanti per la gestione delle prestazioni di rete e include le sezioni seguenti.  
  
-   [Utilizzo delle risorse](#bkmk_ru)  
  
-   [Potenziali problemi di rete](#bkmk_np)  
  
-   [Ricezione prestazioni Side Coalescing (RSC)](#bkmk_rsc)  
  
##  <a name="bkmk_ru"></a> Utilizzo delle risorse  

Contatori delle prestazioni seguenti sono rilevanti per l'utilizzo delle risorse di rete.  
  
- IPv4, IPv6  
  
  -   Datagrammi ricevuti/sec  
  
  -   Datagrammi inviati/sec  
  
- TCPv4, TCPv6  
  
  -   Segmenti ricevuti/sec  
  
  -   Segmenti inviati/sec  
  
  -   Segmenti ritrasmesso/sec  
  
- Interface(*) di rete, scheda di rete (\*)  
  
  - Byte ricevuti/sec  
  
  - Byte inviati/sec  
  
  - I pacchetti ricevuti/sec  
  
  - I pacchetti inviati/sec  
  
  - Lunghezza coda di output  
  
    Questo contatore è la lunghezza della coda di pacchetti di output \(nei pacchetti\). Se si tratta più di 2, si verifichino ritardi. È necessario individuare il collo di bottiglia ed eliminarlo se possibile. Poiché NDIS mette in coda le richieste, la lunghezza deve essere sempre 0.  
  
- Informazioni sul processore  
  
  - % Tempo processore  
  
  - Interrupt/sec  
  
  - Le DPC accodati/sec  
  
    Questo contatore è un tasso medio in corrispondenza del quale le DPC sono stati aggiunti alla coda DPC del processore logico. Ogni processore logico ha la propria coda DPC. Questo contatore misura la frequenza con cui le DPC vengono aggiunte alla coda, non il numero di DPC nella coda. Visualizza la differenza tra i valori osservati negli ultimi due esempi divisi per la durata dell'intervallo di campionamento.  
  
##  <a name="bkmk_np"></a> Potenziali problemi di rete  

Contatori delle prestazioni seguenti sono rilevanti per potenziali problemi di rete.  
  
-   Interface(*) di rete, scheda di rete (\*)  
  
    -   Pacchetti ricevuti scartati  
  
    -   Errori pacchetti ricevuti  
  
    -   Pacchetti scartati in uscita  
  
    -   Errori pacchetti in uscita  
  
-   WFPv4, WFPv6  
  
    -   Pacchetti scartati/sec

-   UDPv4, UDPv6

    -   Datagrammi ricevuti errori  
  
-   TCPv4, TCPv6  
  
    -   Errori di connessione  
  
    -   Connessioni ripristinate  
  
-   Criteri di QoS di rete  
  
    -   Pacchetti eliminati  
  
    -   I pacchetti eliminati/sec  
  
-   Per ogni attività di scheda di interfaccia di rete del processore  
  
    -   Risorse insufficienti ricevere indicazioni/sec  
  
    -   Risorse insufficienti ricevuti pacchetti/sec  
  
-   Microsoft Winsock base Service Provider  
  
    -   Datagrammi ignorati  
  
    -   Datagrammi ignorati al secondo  
  
    -   Connessioni rifiutate  
  
    -   Connessioni rifiutate al secondo  
  
##  <a name="bkmk_rsc"></a> Ricezione prestazioni Side Coalescing (RSC)  

Contatori delle prestazioni seguenti sono rilevanti per le prestazioni di RSC.  
  
-   Scheda di rete(*)  
  
    -   Connessioni TCP attive RSC  
  
    -   Dimensione media dei pacchetti RSC TCP  
  
    -   RSC TCP fuse pacchetti/sec  
  
    -   RSC TCP. eccezioni/sec

Per collegamenti a tutti gli argomenti in questa Guida, vedere [ottimizzazione delle prestazioni di rete sottosistema](net-sub-performance-top.md).