---
ms.assetid: e3ea1f67-60d4-4566-b24c-37faa95c3b2a
title: Definizione dei costi
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0b34f1672311768d644c467fda10dc2fc643282d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847082"
---
# <a name="determining-the-cost"></a>Definizione dei costi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È assegnare valori di costo per i collegamenti di sito di preferire connessioni conveniente connessioni costosa. Alcune applicazioni e servizi, ad esempio il localizzatore di Controller di dominio (DC Locator) e Distributed File System gli spazi dei nomi (DFSN), usano anche le informazioni sui costi per individuare le risorse più vicina. Costo del collegamento di sito è utilizzabile per determinare quale controller di dominio è stato contattato dal client in un sito se il controller di dominio per il dominio specificato non esiste in quel sito. Il client contatta il controller di dominio usando il collegamento di sito che è inferiore a quello assegnato a esso.  
  
È consigliabile che il valore del costo deve essere definito in base a livello di sito. Costo si basa in genere non solo sulla larghezza di banda totale del collegamento, ma anche la disponibilità, latenza e un costo monetario del collegamento.  
  
Per determinare i costi dei collegamenti di sito, documentare la velocità di connessione per ogni collegamento di sito. Fare riferimento al foglio di lavoro "Geografica posizioni e collegamenti di comunicazione" (DSSTOPO_1.doc) nella [la raccolta di informazioni di rete](../../ad-ds/plan/Collecting-Network-Information.md) per informazioni sulla velocità di connessione identificato.  
  
Nella tabella seguente elenca la velocità per i diversi tipi di reti.  
  
|Tipo di rete|Velocità|  
|----------------|---------|  
|Molto lento|56 kilobit per secondo (Kbps)|  
|Lento|64 Kbps|  
|Integrated Services Digital Network ISDN)|Kbps 64 o 128 Kbps|  
|Inoltro di frame|Velocità variabili, in genere compreso tra 56 Kbps e 1.5 megabit al secondo (Mbps)|  
|T1|1.5 Mbps|  
|T3|45 Mbps|  
|10BaseT|10 Mbps|  
|Modalità di trasferimento asincrono (ATM)|Velocità variabile, generalmente tra 155 e 622 Mbps|  
|100BaseT|100 Mbps|  
|Gigabit Ethernet|da 1 gigabit al secondo (Gbps)|  
  
Usare la tabella seguente per calcolare il costo di ogni collegamento di sito in base alla velocità del collegamento di wide area network (WAN) velocità. Per la velocità di collegamento WAN che non è elencata nella tabella, è possibile calcolare un fattore relativo costo dividendo 1.024 per il log della larghezza di banda disponibile, misurata in Kbps.  
  
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
  
Questi costi non riflettono le differenze di affidabilità tra i collegamenti di rete. Impostare costi più elevati in tutti i collegamenti tendenti all'errore di rete in modo da non dover fare affidamento su tali collegamenti per la replica. I costi di collegamento sito superiore impostazione, è possibile controllare il failover della replica quando un collegamento di sito non riesce.  
  


