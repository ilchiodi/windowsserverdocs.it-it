---
title: Requisiti hardware e consigli sulle prestazioni
description: Fornisce i requisiti di hardware e prestazioni e consigli per MultiPoint Services
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 99a5c9c2-270f-4753-a28c-434882c03125
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: dcb139cddf6a7838511365c6a85dc12bd06a81eb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820344"
---
# <a name="hardware-requirements-and-performance-recommendations"></a>Requisiti hardware e consigli sulle prestazioni
Questo argomento descrive l'hardware necessario per eseguire un sistema MultiPoint Services e supportare scenari di applicazioni utente. Lo scenario utente influiscono direttamente sulla CPU, sulla RAM e sui requisiti della larghezza di banda di rete.  

## <a name="optimize-multipoint-services-system-performance"></a>Ottimizzare le prestazioni del sistema MultiPoint Services  
Le prestazioni del sistema MultiPoint Services saranno influenzate direttamente dalla capacità della CPU, dalla GPU e dalla quantità di RAM disponibile nel computer che esegue MultiPoint Services.  
  
### <a name="applications-and-internet-content"></a>Applicazioni e contenuto Internet  
Poiché MultiPoint Services è una soluzione per l'elaborazione di risorse condivise, il tipo e il numero di applicazioni in esecuzione nelle stazioni possono avere un effetto sulle prestazioni del sistema MultiPoint Services. È importante prendere in considerazione i tipi di programmi usati regolarmente quando si pianifica il sistema. Un'applicazione a elevato utilizzo di grafica, ad esempio, richiede un computer più potente rispetto a un'applicazione, ad esempio un elaboratore di testo. L'overload del computer con applicazioni a elevato utilizzo di grafica provocherà probabilmente problemi di ritardo nell'intero sistema.  
  
Il tipo di contenuto a cui si accede dalle applicazioni influiscono anche sulle prestazioni del sistema. Se più stazioni usano Web browser per accedere al contenuto multimediale, ad esempio un video con movimento completo, è possibile connettere un minor numero di stazioni prima di influire negativamente sulle prestazioni del sistema. Viceversa, se le più stazioni utilizzano Web browser per accedere al contenuto Web statico, è possibile connettere più stazioni senza un effetto significativo sulle prestazioni.  
  
### <a name="hardware-recommendations"></a>Suggerimenti hardware  
Per ottenere prestazioni ottimali con il sistema MultiPoint Services in vari carichi, usare le linee guida riportate nella tabella seguente quando si pianifica e si testa il sistema. Questi sono i requisiti di base di forMultiPoint Services. Il dimensionamento effettivo della configurazione dipende dalla configurazione del sistema, dal carico di lavoro in esecuzione e dalla funzionalità hardware. È sempre necessario eseguire la convalida testando le applicazioni e l'hardware.  
  
> [!NOTE]  
> 2C = 2 Core, 4C = 4 core, 6C = 6 core, MT = multithreading. La velocità del processore deve essere almeno di 2,0 gigahertz (GHz).  
  
### <a name="minimum-recommended-hardware-for-running-default-multipoint-server-stations"></a>Hardware minimo consigliato per l'esecuzione di stazioni MultiPoint Server predefinite  
  
|Scenario dell'applicazione|Fino a 5 stazioni|6-8 stazioni|9-12 stazioni|13-16 stazioni|17-20 stazioni|21-24 stazioni|  
|------------------------|----------------------|-------------------|------------------|-------------------|-------------------|-----------------|  
|**Produttività**<p>Office, esplorazione Web, applicazioni line-of-business|CPU: 2C<p>RAM: 2 GB|CPU: 2C<p>RAM: 4 GB|CPU: 4C<p>RAM: 6 GB|CPU: 4C<p>RAM: 8 GB|CPU: 4C + MT o 6C<p>RAM: 10 GB| CPU: 6C + MT<p>RAM: 12 GB|
|**Misto**<p>Office, esplorazione Web, applicazioni line-of-business e uso occasionale di video da parte di alcuni utenti|CPU: 2C<p>RAM: 2 GB|CPU: 2C<p>RAM: 4 GB|CPU: 4C<p>RAM: 6 GB|CPU: 4C + MT o 6C<p>RAM: 8 GB|CPU: 6C + MT<p>RAM: 10 GB| CPU: 6C + MT<p>RAM: 12 GB| 
|**Video intensivo**<p>Office, esplorazione Web, applicazioni line-of-business e utilizzo frequente di video da parte di tutti gli utenti **Nota:** i test video sono stati eseguiti con il video 360p H. 264 alla risoluzione nativa.|CPU: 4C + MT<p>RAM: 2 GB|CPU: 6C + MT<p>RAM: 4 GB|CPU: 8C + MT<p>RAM: 6 GB|CPU: 12C + MT<p>RAM: 8 GB|CPU: 16C + MT<p>RAM: 10 GB<p>-Thin client: RemoteFX<br />-Video USB non consigliato| CPU: 20 M + MT<p>RAM: 12 GB<p>-Thin client: RemoteFX<br />-Video USB non consigliato|   
  
## <a name="minimum-recommended-hardware-for-running-full-windows-10-virtual-desktops"></a>Hardware minimo consigliato per l'esecuzione di desktop completi Windows 10 virtuali  
L'esecuzione di un'istanza del sistema operativo virtuale completa per ogni stazione è un numero di risorse di calcolo molto elevato rispetto all'esecuzione delle sessioni di MultiPoint desktop predefinite, quindi i requisiti hardware per ogni stazione sono superiori:  
  
1.  CPU: 1 core o thread per stazione  
  
2.  Unità di stato solido (SSD)  
  
    1.  Capacity > = 20 GB per stazione + 40 GB per il sistema operativo host WMS  
  
    2.  IOPS di lettura/scrittura casuale > = 3K per stazione  
  
3.  RAM > = 2GB per stazione + 2 GB per il sistema operativo host WMS  
  
L'impostazione della CPU BIOS è stata configurata per abilitare la virtualizzazione: secondo livello Address Translation (stecca)  
  
Per ulteriori informazioni sulla scelta del migliore hardware MultiPoint Services per le proprie esigenze, rivolgersi al fornitore dell'hardware.  