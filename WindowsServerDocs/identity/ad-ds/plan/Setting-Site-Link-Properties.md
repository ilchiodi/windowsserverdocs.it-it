---
ms.assetid: de054ac2-a386-43ec-a537-c0de21549741
title: Impostazione delle proprietà di collegamento di sito
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f448a29384b9ae9409981a67c515dab3640899dd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821864"
---
# <a name="setting-site-link-properties"></a>Impostazione delle proprietà di collegamento di sito

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La replica tra siti viene eseguita in base alle proprietà degli oggetti connessione. Quando il controllo di coerenza informazioni (KCC) crea oggetti connessione, deriva la pianificazione della replica dalle proprietà degli oggetti collegamento di sito. Ogni oggetto collegamento di sito rappresenta la connessione Wide Area Network (WAN) tra due o più siti.  
  
L'impostazione delle proprietà dell'oggetto collegamento di sito include i passaggi seguenti:  
  
-   Determinazione del costo associato al percorso di replica. Il KCC utilizza il costo per determinare la route meno costosa per la replica tra due siti che replicano la stessa partizione di directory.  
  
-   Determinazione della pianificazione che definisce le ore durante le quali può essere eseguita la replica tra siti.  
  
-   Determinare l'intervallo di replica che definisce la frequenza con cui la replica deve essere eseguita durante i periodi in cui è consentita la replica, come definito nella pianificazione.  
  
## <a name="in-this-guide"></a>In questa guida  
  
-   [Definizione dei costi](../../ad-ds/plan/Determining-the-Cost.md)  
  
-   [Definizione della pianificazione](../../ad-ds/plan/Determining-the-Schedule.md)  
  
-   [Definizione dell'intervallo](../../ad-ds/plan/Determining-the-Interval.md)  
  


