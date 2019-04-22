---
ms.assetid: 96a6749c-6c9f-4f2f-ad0a-51272d282ace
title: Definizione dell'intervallo
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 57b282102f10a595ce3842ac3a4eb1d289b86d22
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822162"
---
# <a name="determining-the-interval"></a>Definizione dell'intervallo

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È necessario impostare la proprietà di interval replica collegamento del sito per indicare con quale frequenza si desidera che la replica si verificano durante le ore consentite dalla pianificazione della replica. Ad esempio, se la pianificazione consente la replica tra 02.00 04.00 e l'intervallo di replica è impostata per 30 minuti, la replica può verificarsi fino a quattro volte durante l'orario pianificato. L'intervallo di replica predefinito è 180 minuti o 3 ore. L'intervallo minimo è 15 minuti.  
  
Considerare i criteri seguenti per determinare con quale frequenza viene eseguita la replica all'interno della finestra di pianificazione:  
  
-   Un piccolo intervallo riduce la latenza, ma aumenta la quantità di traffico di rete WAN WAN.  
  
-   Per mantenere aggiornate le partizioni di directory dominio, è preferibile a bassa latenza.  
  
Con una strategia di replica di archiviazione e inoltro, è difficile determinare quanto tempo un aggiornamento della directory potrebbe essere necessario devono essere replicate in ogni controller di dominio. Per fornire una stima conservativa di latenza massima, eseguire queste attività:  
  
-   Creare una tabella di tutti i siti della rete, come illustrato nell'esempio seguente:  
  
    |Siti|Seattle|Boston|Los Angeles|New York|Washington, D.C.|  
    |---------|-----------|----------|---------------|------------|--------------------|  
    |Seattle|0.25|||||  
    |Boston||0.25||||  
    |Los Angeles|||0.25|||  
    |New York||||0.25||  
    |Washington, D.C.|||||0.25|  
  
    Una caso peggiore latenza all'interno di un sito viene stimata in 15 minuti.  
  
-   La pianificazione della replica, determinare la latenza di replica massima che è possibile eseguire su un collegamento di sito che si connette due siti hub.  
  
    Ad esempio, in caso di replica tra Seattle e New York ogni tre ore, il ritardo massimo per la replica tra questi siti è tre ore. Se il ritardo di replica tra Seattle e New York è pianificato il ritardo più lungo tra tutti i siti hub, la latenza massima tra tutti gli hub è 3 ore.  
  
-   Per ogni sito hub, creare una tabella della latenza massima tra il sito hub e uno qualsiasi dei relativi siti satellite.  
  
    Ad esempio, se viene eseguita la replica tra New York e Washington D.C., ogni quattro ore e questo è il ritardo di replica più lungo tra New York e uno qualsiasi dei relativi siti satellite, la latenza massima tra New York e i relativi satelliti corrisponde a quattro ore.  
  
-   Combinare le latenze massime per determinare la latenza massima per l'intera rete.  
  
    Ad esempio, se la latenza massima tra Seattle e il relativo sito satellite di Los Angeles è un giorno, la latenza di replica massima per questo set di collegamenti (Washington D.C.-New York-Seattle-Los Angeles) è 31 ore, vale a dire, 4 (Washington D.C.-New York) + 3 (nuovo York-Seattle) + 24 (Seattle-Los Angeles), come illustrato nella tabella seguente.  
  
    |Siti|Seattle|Boston|Los Angeles|New York|Washington, D.C.|  
    |---------|-----------|----------|---------------|------------|--------------------|  
    |Seattle|0.25|4+3|24.00|3.00|4+3|  
    |Boston||0.25|4+3+24|4,00|4,00|  
    |Los Angeles|||0.25|24 + 3|24+3+4|  
    |New York||||0.25|4,00|  
    |Washington, D.C.|||||0.25|  
  


