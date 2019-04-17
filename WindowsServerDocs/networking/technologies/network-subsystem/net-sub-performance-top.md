---
title: Prestazioni del sottosistema di rete ottimizzazione
description: Questo argomento fa parte della Guida di ottimizzazione delle prestazioni del sottosistema di rete per Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 45217fce-bfb9-47e8-9814-88ffdb3c7b7d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c7ef50335a6dcc7dc5187cc30ff1b2dc2c5cdfed
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="network-subsystem-performance-tuning"></a>Prestazioni del sottosistema di rete ottimizzazione

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per una panoramica del sottosistema di rete e per i collegamenti ad altri argomenti in questa Guida.

>[!NOTE]
>Oltre a questo argomento, le sezioni seguenti di questa guida forniscono indicazioni di ottimizzazione delle prestazioni per i dispositivi di rete e lo stack di rete.
> - [Scelta di una scheda di rete](net-sub-choose-nic.md)
> - [Configurare l'ordine delle interfacce di rete](net-sub-interface-metric.md)
> - [Schede di rete di ottimizzazione delle prestazioni](net-sub-performance-tuning-nics.md)
> - [Contatori delle prestazioni di rete](net-sub-performance-counters.md)
> - [Strumenti di prestazioni per i carichi di lavoro di rete](net-sub-performance-tools.md)

Il sottosistema di rete, in particolare per i carichi di lavoro con utilizzo intensivo rete, l'ottimizzazione delle prestazioni possono comportare ogni livello dell'architettura di rete, noto anche come stack di rete. Questi livelli sono suddivise su larga scala nelle sezioni seguenti.

1. **Interfaccia di rete**. Questo è il livello più basso nello stack di rete e contiene il driver di rete che comunica direttamente con la scheda di rete.

2. **Driver Interface Specification (NDIS) di rete**. NDIS espone le interfacce per il driver di sotto e per i livelli superiori, ad esempio lo Stack di protocolli.
  
3. **Stack del protocollo**. Lo stack di protocolli implementa i protocolli, ad esempio TCP/IP e UDP/IP. Questi livelli espongano l'interfaccia layer di trasporto per i livelli superiori.
  
4. **Driver di sistema**. In genere, questi sono i client che utilizzano un'estensione di dati di trasporto (TDX) o l'interfaccia WSK (Winsock Kernel) per esporre le interfacce di applicazioni in modalità utente. L'interfaccia WSK è stata introdotta in Windows Server 2008 e Windows&reg; Vista ed è esposto dal AFD.sys. L'interfaccia migliora le prestazioni eliminando il passaggio dalla modalità kernel e utente.
  
5. **Le applicazioni in modalità utente**. Si tratta in genere soluzioni di Microsoft o le applicazioni personalizzate.

Nella tabella seguente fornisce una verticale dei livelli dello stack di rete, inclusi gli esempi di elementi che vengono eseguiti in ogni livello.  

![Livelli dello Stack di rete](../../media/Network-Subsystem/network-layers.jpg)

