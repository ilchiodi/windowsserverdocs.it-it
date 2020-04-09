---
ms.assetid: 96a6749c-6c9f-4f2f-ad0a-51272d282ace
title: Definizione dell'intervallo
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f39ad2ce2ce84e36d2faff2a07b8310d3600b6c9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822574"
---
# <a name="determining-the-interval"></a>Definizione dell'intervallo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È necessario impostare la proprietà intervallo di replica del collegamento di sito per indicare la frequenza con cui si desidera che la replica venga eseguita durante i periodi in cui la pianificazione consente la replica. Se, ad esempio, la pianificazione consente la replica tra 02:00 ore e 04:00 ore e l'intervallo di replica è impostato su 30 minuti, la replica può essere eseguita fino a quattro volte durante l'orario pianificato. L'intervallo di replica predefinito è 180 minuti o 3 ore. L'intervallo minimo è 15 minuti.  
  
Considerare i criteri seguenti per determinare la frequenza con cui viene eseguita la replica all'interno della finestra di pianificazione:  
  
-   Un intervallo ridotto riduce la latenza, ma aumenta la quantità di traffico Wide Area Network (WAN).  
  
-   Per rendere aggiornate le partizioni di directory del dominio, è preferibile una bassa latenza.  
  
Con una strategia di replica di archiviazione e di avanzamento, è difficile determinare solo la durata dell'aggiornamento di una directory da replicare in ogni controller di dominio. Per fornire una stima conservativa della latenza massima, eseguire le attività seguenti:  
  
-   Creare una tabella di tutti i siti nella rete, come illustrato nell'esempio seguente:  
  
    |Siti|Seattle|Boston|Los Angeles|New York|Washington, D.C.|  
    |---------|-----------|----------|---------------|------------|--------------------|  
    |Seattle|0,25|||||  
    |Boston||0,25||||  
    |Los Angeles|||0,25|||  
    |New York||||0,25||  
    |Washington, D.C.|||||0,25|  
  
    Una latenza peggiore in un sito è stimata come 15 minuti.  
  
-   Dalla pianificazione della replica, determinare la latenza di replica massima possibile in qualsiasi collegamento di sito che connette due siti hub.  
  
    Se, ad esempio, la replica viene eseguita tra Seattle e New York ogni tre ore, il ritardo massimo per la replica tra questi siti è di tre ore. Se il ritardo di replica tra New York e Seattle è il ritardo pianificato più lungo tra tutti i siti hub, la latenza massima tra tutti gli hub è di tre ore.  
  
-   Per ogni sito hub, creare una tabella delle latenze massime tra il sito hub e i siti satellite.  
  
    Se, ad esempio, la replica viene eseguita tra New York e Washington, D.C. ogni quattro ore e questo è il ritardo di replica più lungo tra New York e i relativi siti satellite, la latenza massima tra New York e i relativi satelliti è di quattro ore.  
  
-   Combinare queste latenze massime per determinare la latenza massima per l'intera rete.  
  
    Ad esempio, se la latenza massima tra Seattle e il relativo sito satellite a Los Angeles è un giorno, la latenza di replica massima per questo set di collegamenti (Washington, D.C.-New York-Seattle-Los Angeles) è 31 ore, ovvero 4 (Washington, D.C.-New York) + 3 (New York-Seattle) + 24 (Seattle-Los Angeles), come illustrato nella tabella seguente.  
  
    |Siti|Seattle|Boston|Los Angeles|New York|Washington, D.C.|  
    |---------|-----------|----------|---------------|------------|--------------------|  
    |Seattle|0,25|4 + 3|24,00|3,00|4 + 3|  
    |Boston||0,25|4 + 3 + 24|4,00|4,00|  
    |Los Angeles|||0,25|24 + 3|24 + 3 + 4|  
    |New York||||0,25|4,00|  
    |Washington, D.C.|||||0,25|  
  


