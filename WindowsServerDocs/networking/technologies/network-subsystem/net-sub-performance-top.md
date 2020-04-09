---
title: Ottimizzazione delle prestazioni del sottosistema di rete
description: Questo argomento fa parte della Guida all'ottimizzazione delle prestazioni del sottosistema di rete per Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 45217fce-bfb9-47e8-9814-88ffdb3c7b7d
manager: dcscontentpm
ms.author: v-tea
author: Teresa-Motiv
ms.openlocfilehash: 66f4401235e8eccfe0ab6e10f86cddb15386621c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854754"
---
# <a name="network-subsystem-performance-tuning"></a>Ottimizzazione delle prestazioni del sottosistema di rete

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per una panoramica del sottosistema di rete e per i collegamenti ad altri argomenti in questa guida.

>[!NOTE]
>Oltre a questo argomento, le sezioni seguenti di questa guida forniscono indicazioni per l'ottimizzazione delle prestazioni per i dispositivi di rete e lo stack di rete.
> - [Scelta di una scheda di rete](net-sub-choose-nic.md)
> - [Configurare l'ordine delle interfacce di rete](net-sub-interface-metric.md)
> - [Ottimizzazione delle prestazioni delle schede di rete](net-sub-performance-tuning-nics.md)
> - [Contatori delle prestazioni relativi alla rete](net-sub-performance-counters.md)
> - [Strumenti per le prestazioni per i carichi di lavoro di rete](net-sub-performance-tools.md)

L'ottimizzazione delle prestazioni del sottosistema di rete, in particolare per i carichi di lavoro con utilizzo intensivo della rete, può coinvolgere ogni livello dell'architettura di rete, detto anche stack di rete. Questi livelli sono ampiamente divisi nelle sezioni seguenti.

1. **Interfaccia di rete**. Si tratta del livello più basso nello stack di rete e contiene il driver di rete che comunica direttamente con la scheda di rete.

2. **Specifiche di Network Driver Interface (NDIS)** . NDIS espone le interfacce per il driver sottostante e per i livelli superiori, ad esempio lo stack di protocolli.
  
3. **Stack di protocolli**. Lo stack di protocolli implementa protocolli quali TCP/IP e UDP/IP. Questi livelli espongono l'interfaccia del livello di trasporto per i livelli al di sopra di essi.
  
4. **Driver di sistema**. Si tratta in genere di client che usano un'interfaccia TDX (Transport data Extension) o un kernel Winsock (WSK) per esporre le interfacce alle applicazioni in modalità utente. L'interfaccia WSK è stata introdotta in Windows Server 2008 e Windows&reg; vista ed è esposta da AFD. sys. L'interfaccia consente di migliorare le prestazioni eliminando il cambio tra la modalità utente e la modalità kernel.
  
5. **Applicazioni in modalità utente**. Si tratta in genere di soluzioni Microsoft o di applicazioni personalizzate.

La tabella seguente fornisce un'illustrazione verticale dei livelli dello stack di rete, inclusi esempi di elementi che vengono eseguiti in ogni livello.  

![Livelli dello stack di rete](../../media/Network-Subsystem/network-layers.jpg)

