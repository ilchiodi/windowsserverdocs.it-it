---
ms.assetid: e3ea1f67-60d4-4566-b24c-37faa95c3b2a
title: Definizione dei costi
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b32d1930fcef944fbd719338797f0f29b70fa29d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="determining-the-cost"></a>Definizione dei costi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Assegnare valori di costo per i collegamenti di sito di preferire connessioni poco costose connessioni. Determinate applicazioni e servizi, ad esempio il localizzatore di Controller di dominio (DC Locator) e Distributed File System spazi dei nomi (DFSN), utilizzano anche le informazioni sui costi per individuare le risorse più vicina. Costo del collegamento di sito consente di determinare quale controller di dominio contattato dal client in un sito se il controller di dominio per il dominio specificato non esiste in tale sito. Il client contatta il controller di dominio utilizzando il collegamento di sito con il costo più basso assegnato a esso.  
  
È consigliabile che il valore di costo viene definita in base a livello di sito. Costo è basato in genere non solo la larghezza di banda totale del collegamento ma anche la disponibilità, latenza e sui costi di collegamento.  
  
Per determinare i costi di collegamenti di sito, documentare la velocità di connessione per ciascun collegamento di sito. Fare riferimento al "Geografica percorsi e collegamenti di comunicazione" (DSSTOPO_1.doc) foglio di lavoro [la raccolta di informazioni sulla rete](../../ad-ds/plan/Collecting-Network-Information.md) per informazioni sulla velocità di connessione che sono stati individuati.  
  
Nella tabella seguente elenca le velocità per diversi tipi di reti.  
  
|Tipo di rete|Velocità|  
|----------------|---------|  
|Molto lento|56 kilobit al secondo (Kbps)|  
|Lento|64 kbps|  
|Rete digitale servizi integrati (ISDN)|64 kbps o 128 Kbps|  
|Inoltro di frame|Velocità variabile, generalmente tra 56 Kbps e 1,5 megabit al secondo (Mbps)|  
|T1|1,5 a Mbps|  
|T3|45 Mbps|  
|10BaseT|10 Mbps|  
|Modalità di trasferimento asincrono (ATM)|Velocità variabile, generalmente tra 155 Mbps e 622 Mbps|  
|100BaseT|100 Mbps|  
|Gigabit Ethernet|Da 1 gigabit al secondo (Gbps)|  
  
Utilizzare la tabella seguente per calcolare il costo di ciascun collegamento di sito in base alla velocità del collegamento di wide area network (WAN) velocità. Per la velocità del collegamento WAN che non è elencato nella tabella, è possibile calcolare un fattore di costo relativo dividendo 1.024 per il registro della larghezza di banda disponibile, misurato in Kbps.  
  
|Larghezza di banda disponibile (Kbps)|Costo|  
|--------------------------------|--------|  
|9.6|1,042|  
|19.2|798|  
|38.4|644|  
|56|586|  
|64|567|  
|128|486|  
|256|425|  
|512|378|  
|1,024|340|  
|2,048|309|  
|4,096|283|  
  
Questi costi non riflettono differenze nell'affidabilità dei collegamenti di rete. Impostare i costi più elevati su collegamenti di rete soggetti a errori in modo da non dover si basano su tali collegamenti per la replica. I costi di collegamento del sito superiore impostazione, è possibile controllare il failover della replica quando un collegamento di sito non riesce.  
  


