---
title: Strumenti di prestazioni per i carichi di lavoro di rete
description: Questo argomento fa parte della Guida di ottimizzazione delle prestazioni del sottosistema di rete per Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c7789781-87e8-464e-981b-af887d01badd
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 81c31871b3dfa4644690fe074ae15eaaa680d7f2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="performance-tools-for-network-workloads"></a>Strumenti di prestazioni per i carichi di lavoro di rete

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni sugli strumenti di prestazioni.

In questo argomento contiene le sezioni relative al Client per il traffico tra Server strumento, dimensioni della finestra TCP/IP e Microsoft Server Performance Advisor.

##  <a name="bkmk_tuning"></a>Client allo strumento di traffico dei Server

Il Client allo strumento \(ctsTraffic\) traffico dei Server offre la possibilità di creare e verificare il traffico di rete.

Per ulteriori informazioni e per scaricare lo strumento, vedere [ctsTraffic (Client-Server traffico)](http://ctstraffic.codeplex.com/).
  
##  <a name="bkmk_size"></a>Dimensioni della finestra TCP/IP

Per schede di 1 GB, le impostazioni visualizzate nella tabella precedente devono fornire una velocità effettiva perché NTttcp imposta le dimensioni della finestra TCP predefinito di 64 KB tramite un processore logico specifico opzione \(SO_RCVBUF\) per la connessione. Questo offre buone prestazioni in una rete a bassa latenza.  

Al contrario, per le reti a latenza elevata o schede da 10 GB, il valore di dimensioni finestra TCP predefinito per NTttcp produce minore di ottenere prestazioni ottimali. In entrambi i casi, è necessario regolare le dimensioni della finestra TCP per consentire il prodotto di ritardo maggiore larghezza di banda.  

In modo statico, è possibile impostare le dimensioni della finestra TCP su un valore elevato con il **-rb** opzione. Questa opzione Disabilita la regolazione automatica finestra TCP ed è consigliabile eseguire questa operazione solo se l'utente conosce la modifica nel comportamento di TCP/IP risultante. Per impostazione predefinita, le dimensioni della finestra TCP viene impostata un valore sufficiente e regola solo con un notevole carico o sui collegamenti a latenza elevata.  

##  <a name="bkmk_advisor"></a>Microsoft Server Performance Advisor

Microsoft Server Performance Advisor \(SPA\) consente agli amministratori IT di raccogliere le metriche per identificare, confrontare e diagnosticare i potenziali problemi di prestazioni in un Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o distribuzione di Windows Server 2008. 

SPA genera grafici e report di diagnostica completi e fornisce indicazioni che consentono rapidamente di analizzano i problemi e sviluppano azioni correttive.  
  
 Per ulteriori informazioni e per scaricare lo strumento advisor, vedere [Microsoft Server Performance Advisor](https://msdn.microsoft.com/library/windows/hardware/dn481522.aspx) in Windows Hardware Dev Center.

Per i collegamenti a tutti gli argomenti in questa Guida, vedere [ottimizzazione delle prestazioni del sottosistema di rete](net-sub-performance-top.md).