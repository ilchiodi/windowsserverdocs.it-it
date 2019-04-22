---
title: Strumenti per le prestazioni per i carichi di lavoro di rete
description: Questo argomento fa parte della Guida all'ottimizzazione delle prestazioni del sottosistema di rete di Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c7789781-87e8-464e-981b-af887d01badd
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 07/16/2018
ms.openlocfilehash: e71c5f34041145907c30b279dc91a94c03c2abed
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824932"
---
# <a name="performance-tools-for-network-workloads"></a>Strumenti per le prestazioni per i carichi di lavoro di rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Per altre informazioni sugli strumenti di prestazioni, è possibile utilizzare questo argomento.

In questo argomento contiene le sezioni relative al Client per il traffico dei Server strumento dimensioni della finestra TCP/IP e Microsoft Server Performance Advisor.

##  <a name="bkmk_tuning"></a> Client allo strumento di traffico del Server

Il Client per il traffico del Server \(ctsTraffic\) strumento offre la possibilità di creare e verificare il traffico di rete.

Per altre informazioni e per scaricare lo strumento, vedere [ctsTraffic (traffico Client-Server)](https://github.com/Microsoft/ctsTraffic).
  
##  <a name="bkmk_size"></a> Dimensioni finestra TCP/IP

Per le schede di 1 GB, le impostazioni riportate nella tabella precedente devono fornire una buona velocità effettiva perché NTttcp imposta le dimensioni della finestra TCP predefinito su 64K tramite un'opzione di processore logico specifico \(SO_RCVBUF\) per la connessione. Ciò offre buone prestazioni in una rete a bassa latenza.  

Al contrario, per le reti a latenza elevata o per le schede di 10 GB, il valore di dimensioni finestra TCP predefinito per NTttcp produce minore di ottenere prestazioni ottimali. In entrambi i casi, è necessario regolare le dimensioni della finestra TCP per consentire il prodotto di ritardo maggiore della larghezza di banda.  

In modo statico, è possibile impostare le dimensioni della finestra TCP su un valore di grandi dimensioni usando il **-rb** opzione. Questa opzione Disabilita l'ottimizzazione automatica della finestra TCP ed è consigliabile usarla solo se l'utente a fondo la modifica risultante nel comportamento di TCP/IP. Per impostazione predefinita, le dimensioni della finestra TCP sono impostata su un valore sufficiente e viene modificata solo in sovraccarico o sui collegamenti ad alta latenza.  

##  <a name="bkmk_advisor"></a> Microsoft Server Performance Advisor

Microsoft Server Performance Advisor \(SPA\) consente agli amministratori IT raccolgono le metriche per identificare, confrontare e diagnosticare i potenziali problemi di prestazioni in un Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o distribuzione di Windows Server 2008. 

SPA genera grafici e report di diagnostica completa, e fornisce indicazioni che consentono rapidamente analizzano i problemi e lo sviluppo di azioni correttive.  
  
 Per altre informazioni e per scaricare l'advisor, vedere [Microsoft Server Performance Advisor](https://msdn.microsoft.com/library/windows/hardware/dn481522.aspx) in Hardware Windows Dev Center.

Per collegamenti a tutti gli argomenti in questa Guida, vedere [ottimizzazione delle prestazioni di rete sottosistema](net-sub-performance-top.md).