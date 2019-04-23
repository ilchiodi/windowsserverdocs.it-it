---
title: Ottimizzazione delle prestazioni del sottosistema di rete
description: Questo argomento fa parte della Guida all'ottimizzazione delle prestazioni del sottosistema di rete di Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 45217fce-bfb9-47e8-9814-88ffdb3c7b7d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c0706c6ddbb678eacd3e609cfad3ccdda943fbd3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857412"
---
# <a name="network-subsystem-performance-tuning"></a>Ottimizzazione delle prestazioni del sottosistema di rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per una panoramica del sottosistema di rete e per i collegamenti ad altri argomenti in questa Guida.

>[!NOTE]
>Oltre a questo argomento, le sezioni seguenti di questa guida forniscono indicazioni di ottimizzazione delle prestazioni per i dispositivi di rete e lo stack di rete.
> - [Scelta di una scheda di rete](net-sub-choose-nic.md)
> - [Configurare l'ordine delle interfacce di rete](net-sub-interface-metric.md)
> - [Schede di rete di ottimizzazione delle prestazioni](net-sub-performance-tuning-nics.md)
> - [Contatori delle prestazioni correlati alla rete](net-sub-performance-counters.md)
> - [Strumenti per le prestazioni per carichi di lavoro di rete](net-sub-performance-tools.md)

Il sottosistema di rete, in particolare per i carichi di lavoro a elevato utilizzo rete, l'ottimizzazione delle prestazioni possono implicare ogni livello dell'architettura di rete, noto anche come lo stack di rete. Questi livelli sono divisi nelle sezioni seguenti.

1. **Interfaccia di rete**. Questo è il livello più basso nello stack di rete e contiene i driver di rete che comunica direttamente con la scheda di rete.

2. **Network Driver Interface Specification (NDIS)**. NDIS espone interfacce per il driver sottostante e per i livelli di sopra di esso, ad esempio lo Stack del protocollo.
  
3. **Stack del protocollo**. Lo stack del protocollo implementa i protocolli quali TCP/IP e UDP/IP. Questi livelli di espongano l'interfaccia di livello di trasporto per i livelli sottostanti.
  
4. **Driver di sistema**. Si tratta in genere i client che usano un'estensione di trasporto dati (TDX) o un'interfaccia WSK (Winsock Kernel) per esporre interfacce per le applicazioni in modalità utente. L'interfaccia WSK è stato introdotto in Windows Server 2008 e Windows&reg; Vista che è esposto dal Afd. sys. L'interfaccia migliora le prestazioni eliminando il passaggio tra modalità utente e kernel.
  
5. **Le applicazioni in modalità utente**. Si tratta in genere soluzioni di Microsoft o le applicazioni personalizzate.

La tabella seguente fornisce un'illustrazione verticale dei livelli dello stack di rete, inclusi gli esempi di elementi che vengono eseguiti in ogni livello.  

![Livelli dello Stack di rete](../../media/Network-Subsystem/network-layers.jpg)

