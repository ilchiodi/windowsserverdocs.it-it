---
title: Contatori delle prestazioni di rete
description: Questo argomento fa parte della Guida di ottimizzazione delle prestazioni del sottosistema di rete per Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 7ebaa271-2557-4c24-a679-c3d863e6bf9e
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 33551dfd4f76bc13ba69863b782ddae279e0ad16
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="network-related-performance-counters"></a>Contatori delle prestazioni di rete

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

In questo argomento elenca i contatori che sono rilevanti per la gestione delle prestazioni di rete e contiene le sezioni seguenti.  
  
-   [Utilizzo delle risorse](#bkmk_ru)  
  
-   [Potenziali problemi di rete](#bkmk_np)  
  
-   [Ottenere prestazioni Side Coalescing (RSC)](#bkmk_rsc)  
  
##  <a name="bkmk_ru"></a>Utilizzo delle risorse  

I seguenti contatori delle prestazioni sono rilevanti per l'utilizzo delle risorse di rete.  
  
-   IPv4, IPv6  
  
    -   Datagrammi ricevuti al secondo  
  
    -   Datagrammi inviati al secondo  
  
-   TCPv4, TCPv6  
  
    -   Segmenti ricevuti al secondo  
  
    -   Segmenti inviati al secondo  
  
    -   Segmenti ritrasmesso al secondo  
  
-   Rete Interface(*), Adapter(\*) di rete  
  
    -   Byte ricevuti al secondo  
  
    -   Byte inviati al secondo  
  
    -   Pacchetti ricevuti/sec  
  
    -   I pacchetti inviati al secondo  
  
    -   Lunghezza coda di output  
  
     Questo contatore è l'intervallo del \(in packets\) coda pacchetto output. Se questo è supera a 2, si verificano ritardi. È necessario individuare il collo di bottiglia ed eliminarla se possibile. Poiché NDIS code le richieste, questa lunghezza deve sempre essere 0.  
  
-   Informazioni sul processore  
  
    -   % Tempo processore  
  
    -   Interrupt/sec  
  
    -   Le DPC accodati/sec  
  
     Questo contatore è una frequenza media, in cui le DPC sono stati aggiunti alla coda DPC del processore logico. Ogni processore logico è una coda DPC. Questo contatore misura la frequenza con cui le DPC vengono aggiunti alla coda, non il numero di DPC nella coda. Visualizza la differenza tra i valori che sono stati rilevati negli ultimi due esempi, divisi per la durata dell'intervallo di campionamento.  
  
##  <a name="bkmk_np"></a>Potenziali problemi di rete  

I seguenti contatori delle prestazioni sono rilevanti per eventuali problemi di rete.  
  
-   Rete Interface(*), Adapter(\*) di rete  
  
    -   Pacchetti ricevuti scartati  
  
    -   Pacchetti ricevuti errori  
  
    -   Pacchetti in uscita eliminati  
  
    -   Errori di pacchetti in uscita  
  
-   WFPv4, WFPv6  
  
    -   I pacchetti ignorati al secondo

-   UDPv4, UDPv6

    -   Errori ricevuti datagrammi  
  
-   TCPv4, TCPv6  
  
    -   Errori di connessione  
  
    -   Reimpostazione di connessioni  
  
-   Criteri di QoS di rete  
  
    -   Pacchetti scartati  
  
    -   I pacchetti ignorati al secondo  
  
-   Per ogni attività scheda interfaccia di rete del processore  
  
    -   Risorse insufficienti ricevuti indicazioni/sec  
  
    -   Risorse insufficienti ricevuti pacchetti al secondo  
  
-   Microsoft Winsock base Service Provider  
  
    -   Datagrammi ignorati  
  
    -   Datagrammi ignorati al secondo  
  
    -   Connessioni rifiutate  
  
    -   Connessioni rifiutate al secondo  
  
##  <a name="bkmk_rsc"></a>Ottenere prestazioni Side Coalescing (RSC)  

I seguenti contatori delle prestazioni sono rilevanti per le prestazioni di RSC.  
  
-   Rete Adapter(*)  
  
    -   Connessioni TCP attive RSC  
  
    -   Dimensioni del pacchetto Media RSC TCP  
  
    -   RSC TCP fuse pacchetti al secondo  
  
    -   RSC TCP eccezioni al secondo

Per i collegamenti a tutti gli argomenti in questa Guida, vedere [ottimizzazione delle prestazioni del sottosistema di rete](net-sub-performance-top.md).