---
ms.assetid: 96a6749c-6c9f-4f2f-ad0a-51272d282ace
title: Definizione dell'intervallo
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d5eb824b3b15c8b0734b2df23b79649778b28e9d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="determining-the-interval"></a>Definizione dell'intervallo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È necessario impostare la proprietà di intervallo di replica collegamento di sito per indicare la frequenza con cui si desidera che la replica in orari quando la pianificazione consente la replica. Ad esempio, se la pianificazione consente la replica tra 00:02 ore e ore 04:00 e l'intervallo di replica è impostato per 30 minuti, la replica fino a quattro volte durante l'orario pianificato. L'intervallo di replica predefinito è 180 minuti o 3 ore. L'intervallo minimo è 15 minuti.  
  
Prendere in considerazione i criteri seguenti per determinare la frequenza con cui viene eseguita la replica all'interno della finestra di pianificazione:  
  
-   Un intervallo di piccole dimensioni riduce latenza ma aumenta la quantità di traffico di wide area network (WAN).  
  
-   Per mantenere aggiornate le partizioni di directory dominio, è preferibile a bassa latenza.  
  
Con una strategia di replica di archiviazione e inoltro, è difficile determinare quanto tempo potrebbe richiedere un aggiornamento della directory devono essere replicate in ogni controller di dominio. Per fornire una stima della latenza massima, eseguire queste attività:  
  
-   Creare una tabella di tutti i siti della rete, come illustrato nell'esempio seguente:  
  
    |Siti|Seattle|Boston|Los Angeles|New York|Firenze|  
    |---------|-----------|----------|---------------|------------|--------------------|  
    |Seattle|0.25|||||  
    |Boston||0.25||||  
    |Los Angeles|||0.25|||  
    |New York||||0.25||  
    |Firenze|||||0.25|  
  
    È previsto di una latenza peggiore all'interno di un sito in 15 minuti.  
  
-   Dalla pianificazione della replica, determinare la latenza di replica massima che è possibile su qualsiasi collegamento di sito che connette i due siti hub.  
  
    Ad esempio, se viene eseguita la replica tra Seattle e New York ogni tre ore, il ritardo massimo per la replica tra questi siti è tre ore. Se il ritardo della replica tra New York e Seattle è lungo ritardo pianificato tra tutti i siti hub, la latenza massima tra tutti gli hub è tre ore.  
  
-   Per ogni sito hub, creare una tabella delle latenze massime tra il sito hub e uno dei suoi siti satellite.  
  
    Ad esempio, se viene eseguita la replica tra New York e Washington, D.C., ogni quattro ore e questo è il più lungo ritardo di replica tra New York e uno dei suoi siti satellite, la latenza massima tra New York e i relativi satelliti è quattro ore.  
  
-   Combinare le latenze massime per determinare la latenza massima per l'intera rete.  
  
    Ad esempio, se la latenza massima tra Seattle e il relativo sito satellite Los Angeles è un giorno, la latenza di replica massima per il set di collegamenti (Washington, D.C.-New York-Seattle-Los Angeles) è 31 ore, 4, ovvero (Washington, D.C.-New York) + 3 (New York-Seattle) + 24 (Seattle-Los Angeles), come illustrato nella tabella seguente.  
  
    |Siti|Seattle|Boston|Los Angeles|New York|Firenze|  
    |---------|-----------|----------|---------------|------------|--------------------|  
    |Seattle|0.25|4+3|24.00|3.00|4+3|  
    |Boston||0.25|4+3+24|4.00|4.00|  
    |Los Angeles|||0.25|24 + 3|24+3+4|  
    |New York||||0.25|4.00|  
    |Firenze|||||0.25|  
  


