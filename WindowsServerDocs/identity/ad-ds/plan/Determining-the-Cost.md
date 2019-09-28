---
ms.assetid: e3ea1f67-60d4-4566-b24c-37faa95c3b2a
title: Definizione dei costi
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 0ce7acddbfa9f7536f3d5a190c6968ea0d8cf6b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408872"
---
# <a name="determining-the-cost"></a>Definizione dei costi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

I valori di costo vengono assegnati ai collegamenti di sito per favorire connessioni convenienti su connessioni costose. Alcune applicazioni e servizi, ad esempio DCLocator (Domain controller Locator) e file system distribuito spazi dei nomi (DFSN), usano anche le informazioni sui costi per individuare le risorse più vicine. Il costo del collegamento di sito può essere utilizzato per determinare quale controller di dominio viene contattato dai client in un sito se il controller di dominio per il dominio specificato non esiste in tale sito. Il client contatta il controller di dominio usando il collegamento di sito a cui è assegnato il costo più basso.  
  
È consigliabile definire il valore del costo a livello di sito. Il costo si basa in genere non solo sulla larghezza di banda totale del collegamento, ma anche sulla disponibilità, la latenza e il costo monetario del collegamento.  
  
Per determinare i costi da collocare nei collegamenti di sito, documentare la velocità di connessione per ogni collegamento di sito. Per informazioni sulla velocità di connessione identificata, vedere il foglio di elaborazione "posizioni geografiche e collegamenti di comunicazione" (DSSTOPO_1. doc) in [raccolta delle informazioni sulla rete](../../ad-ds/plan/Collecting-Network-Information.md) .  
  
Nella tabella seguente sono elencate le velocità per diversi tipi di reti.  
  
|Tipo di rete|Velocità|  
|----------------|---------|  
|Molto lento|56 kilobit al secondo (Kbps)|  
|Lento|64 Kbps|  
|Rete digitale di servizi integrati (ISDN)|64 Kbps o 128 kbps|  
|Inoltro frame|Frequenza variabile, in genere tra 56 Kbps e 1,5 megabit al secondo (Mbps)|  
|T1|1,5 Mbps|  
|T3|45 Mbps|  
|10BaseT|10 Mbps|  
|Modalità di trasferimento asincrono (ATM)|Frequenza variabile, in genere tra 155 Mbps e 622 Mbps|  
|100BaseT|100 Mbps|  
|Ethernet Gigabit|1 Gigabit al secondo (Gbps)|  
  
Usare la tabella seguente per calcolare il costo di ogni collegamento di sito in base alla velocità di collegamento della velocità di Wide Area Network (WAN). Per la velocità del collegamento WAN non elencata nella tabella, è possibile calcolare un fattore di costo relativo dividendo 1.024 in base al log della larghezza di banda disponibile, misurato in kbps.  
  
|Larghezza di banda disponibile (Kbps)|Costo|  
|--------------------------------|--------|  
|9,6|1\.042|  
|19,2|798|  
|38,4|644|  
|56|586|  
|64|567|  
|128|486|  
|256|425|  
|512|378|  
|1\.024|340|  
|2\.048|309|  
|4\.096|283|  
  
Questi costi non riflettono le differenze nell'affidabilità tra i collegamenti di rete. Impostare costi maggiori per tutti i collegamenti di rete soggetti a errori, in modo da non dover utilizzare tali collegamenti per la replica. Impostando i costi di collegamento di sito più elevati, è possibile controllare il failover della replica quando un collegamento di sito non riesce.  
  


