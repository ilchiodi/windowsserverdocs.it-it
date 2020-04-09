---
title: Strumenti per le prestazioni per i carichi di lavoro di rete
description: Questo argomento fa parte della Guida all'ottimizzazione delle prestazioni del sottosistema di rete per Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: c7789781-87e8-464e-981b-af887d01badd
manager: dcscontentpm
ms.author: v-tea
author: Teresa-Motiv
ms.date: 07/16/2018
ms.openlocfilehash: 2ae321e25ca22e79958207f7e67c6517eb32bfcb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862204"
---
# <a name="performance-tools-for-network-workloads"></a>Strumenti per le prestazioni per i carichi di lavoro di rete

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per ottenere informazioni sugli strumenti per le prestazioni.

In questo argomento sono incluse le sezioni relative allo strumento per il traffico da client a server, alle dimensioni della finestra TCP/IP e a Microsoft Server Performance Advisor.

##  <a name="client-to-server-traffic-tool"></a><a name="bkmk_tuning"></a>Strumento di traffico da client a server

Il traffico da client a server \(ctsTraffic\) Tool offre la possibilità di creare e verificare il traffico di rete.

Per ulteriori informazioni e per scaricare lo strumento, vedere [ctsTraffic (traffico da client a server)](https://github.com/Microsoft/ctsTraffic).
  
##  <a name="tcpip-window-size"></a><a name="bkmk_size"></a>Dimensioni della finestra TCP/IP

Per gli adapter da 1 GB, le impostazioni visualizzate nella tabella precedente forniscono una corretta velocità effettiva, perché NTttcp imposta le dimensioni predefinite della finestra TCP su 64 K tramite un'opzione specifica del processore logico \(SO_RCVBUF\) per la connessione. Ciò garantisce prestazioni ottimali in una rete a bassa latenza.  

Al contrario, per le reti a latenza elevata o per gli adapter da 10 GB, il valore predefinito delle dimensioni della finestra TCP per NTttcp produce prestazioni inferiori rispetto a quelle ottimali. In entrambi i casi, è necessario modificare le dimensioni della finestra TCP per consentire un prodotto con un ritardo maggiore della larghezza di banda.  

È possibile impostare in modo statico la dimensione della finestra TCP su un valore elevato utilizzando l'opzione **-RB** . Questa opzione Disabilita l'ottimizzazione automatica della finestra TCP ed è consigliabile usarla solo se l'utente è pienamente consapevole della modifica risultante nel comportamento TCP/IP. Per impostazione predefinita, le dimensioni della finestra TCP sono impostate su un valore sufficiente e si adattano solo al carico elevato o ai collegamenti a latenza elevata.  

##  <a name="microsoft-server-performance-advisor"></a><a name="bkmk_advisor"></a>Microsoft Server Performance Advisor

Microsoft Server Performance Advisor \(SPA\) consente agli amministratori IT di raccogliere le metriche per identificare, confrontare e diagnosticare i potenziali problemi di prestazioni in una distribuzione di Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008. 

SPA genera report e grafici di diagnostica completi e fornisce indicazioni utili per analizzare rapidamente i problemi e sviluppare azioni correttive.  
  
 Per ulteriori informazioni e per scaricare l'Advisor, vedere [Microsoft Server Performance Advisor](https://msdn.microsoft.com/library/windows/hardware/dn481522.aspx) in Windows hardware Dev Center.

Per i collegamenti a tutti gli argomenti di questa guida, vedere [ottimizzazione delle prestazioni del sottosistema di rete](net-sub-performance-top.md).