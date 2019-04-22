---
title: Requisiti hardware e consigli sulle prestazioni
description: Fornisce i requisiti hardware e prestazioni e consigli per i servizi MultiPoint
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99a5c9c2-270f-4753-a28c-434882c03125
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: b47f4c224d4a4f6f4aa104b6d5ee5d93590ac0c4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823392"
---
# <a name="hardware-requirements-and-performance-recommendations"></a>Requisiti hardware e consigli sulle prestazioni
Questo argomento descrive l'hardware necessario per eseguire un sistema MultiPoint Services e supportare scenari di applicazione utente. Lo scenario utente influisce direttamente sulla CPU, RAM e requisiti di larghezza di banda di rete.  

## <a name="optimize-multipoint-services-system-performance"></a>Ottimizzare le prestazioni del sistema MultiPoint Services  
Le prestazioni del sistema MultiPoint Services ne risentirà direttamente dalla capacità di CPU, GPU e la quantità di RAM disponibile nel computer che esegue MultiPoint Services.  
  
### <a name="applications-and-internet-content"></a>Le applicazioni e contenuto Internet  
Poiché servizi MultiPoint è una soluzione di computing risorsa condivisa, il tipo e il numero di applicazioni che eseguono le stazioni possono compromettere le prestazioni del sistema MultiPoint Services. È importante considerare i tipi di programmi che vengono utilizzati regolarmente durante la pianificazione del sistema. Ad esempio, un'applicazione a elevato utilizzo di grafica richiede un computer più potente rispetto a un'applicazione, ad esempio un elaboratore di testo. L'overload del computer con le applicazioni a elevato utilizzo di grafica si comportano problemi di ritardo per l'intero sistema.  
  
Il tipo di contenuto a cui accessibile applicazioni influisce anche sulle prestazioni del sistema. Se più stazioni Usa web browser per accedere al contenuto multimedia, ad esempio video in movimento, meno le stazioni possono essere connesse prima di produrre effetti negativi sulle prestazioni del sistema. Viceversa, se le stazioni più Usa web browser per accedere al contenuto web statico, altre stazioni possono essere connesse senza un impatto significativo sulle prestazioni.  
  
### <a name="hardware-recommendations"></a>Suggerimenti hardware  
Per ottenere prestazioni ottimali con il sistema MultiPoint Services in vari carichi elevati, utilizzare le linee guida nella tabella seguente durante la pianificazione e la verifica del sistema. Questi sono i requisiti di base forMultiPoint servizi. Il dimensionamento delle configurazione effettiva varia a seconda della configurazione del sistema, il carico di lavoro che è in esecuzione e la funzionalità di hardware. È sempre opportuno convalidare testando le applicazioni e hardware.  
  
> [!NOTE]  
> 2. C = 2 core, 4, C = 4 core, 6, C = 6 core, server di destinazione master = il multithreading. Velocità del processore deve essere almeno 2.0 gigahertz (GHz).  
  
### <a name="minimum-recommended-hardware-for-running-default-multipoint-server-stations"></a>Consigliati minimo hardware per l'esecuzione di stazioni MultiPoint Server predefinito  
  
|Application Scenario|Stazioni fino a 5|6 a 8 stazioni|Stazioni di 9 a 12|13-16 stazioni|17 a 20 stazioni|21 a 24 stazioni|  
|------------------------|----------------------|-------------------|------------------|-------------------|-------------------|-----------------|  
|**Produttività**<br /><br />Office, applicazioni line-of-business, esplorazione web|CPU: 2C<br /><br />RAM: 2 GB|CPU: 2C<br /><br />RAM: 4 GB|CPU: 4C<br /><br />RAM: 6 GB|CPU: 4C<br /><br />RAM: 8 GB|CPU: C + MT 4 o 6C<br /><br />RAM: 10 GB| CPU: 6C+MT<br /><br />RAM: 12 GB|
|**Mixed**<br /><br />Office, esplorazione del web, applicazioni line-of-business e l'utilizzo occasionale video da alcuni utenti|CPU: 2C<br /><br />RAM: 2 GB|CPU: 2C<br /><br />RAM: 4 GB|CPU: 4C<br /><br />RAM: 6 GB|CPU: C + MT 4 o 6C<br /><br />RAM: 8 GB|CPU: 6C+MT<br /><br />RAM: 10 GB| CPU: 6C+MT<br /><br />RAM: 12 GB| 
|**Video con utilizzo intensivo**<br /><br />Office, esplorazione del web, applicazioni line-of-business e uso frequente di video da tutti gli utenti **Nota:** Video di test è stato eseguito usando lo standard video H.264 360p risoluzione nativa.|CPU: 4C+MT<br /><br />RAM: 2 GB|CPU: 6C+MT<br /><br />RAM: 4 GB|CPU: 8C+MT<br /><br />RAM: 6 GB|CPU: 12C+MT<br /><br />RAM: 8 GB|CPU: 16C+MT<br /><br />RAM: 10 GB<br /><br />-Thin Client: RemoteFX<br />-Video USB non consigliata| CPU: 20C+MT<br /><br />RAM: 12 GB<br /><br />-Thin Client: RemoteFX<br />-Video USB non consigliata|   
  
## <a name="minimum-recommended-hardware-for-running-full-windows-10-virtual-desktops"></a>Consigliati minimo hardware per l'esecuzione completo desktop virtuali di Windows 10  
Esecuzione di un'istanza virtuale completo del sistema operativo per ogni stazione è che più grande quantità di risorse di calcolo rispetto all'esecuzione, le sessioni di desktop MultiPoint predefinito, in modo che i requisiti di hardware host per ogni stazione sono superiori:  
  
1.  CPU: 1 core o thread per ogni stazione  
  
2.  Unità di stato solido (SSD)  
  
    1.  Capacità > = 20GB per stazione + 40GB per sistema operativo dell'host di WMS  
  
    2.  IOPS di lettura/scrittura casuali > = 3K per stazione  
  
3.  RAM > = 2GB per stazione + 2GB per sistema operativo dell'host di WMS  
  
Impostazione del BIOS CPU configurato per abilitare la virtualizzazione – secondo SLAT Level Address Translation)  
  
Per altre informazioni sulla scelta dell'hardware di MultiPoint Services migliore per le proprie esigenze, contattare il fornitore dell'hardware.  