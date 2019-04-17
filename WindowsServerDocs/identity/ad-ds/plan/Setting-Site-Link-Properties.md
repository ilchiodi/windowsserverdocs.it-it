---
ms.assetid: de054ac2-a386-43ec-a537-c0de21549741
title: "Impostazione delle proprietà di collegamento di sito"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 495ed006ecac5458877191a14060c5fd4b746d96
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="setting-site-link-properties"></a>Impostazione delle proprietà di collegamento di sito

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La replica tra siti viene eseguita in base alle proprietà degli oggetti di connessione. Quando il controllo di coerenza informazioni (KCC) crea gli oggetti connessione, deriva la pianificazione della replica dalle proprietà di oggetti collegamento di sito. Ogni oggetto collegamento di sito rappresenta la connessione di wide area network (WAN) tra due o più siti.  
  
Impostazione delle proprietà dell'oggetto collegamento di sito include i passaggi seguenti:  
  
-   Definizione dei costi associata a tale percorso di replica. KCC Usa costo per determinare la route per la replica tra due siti che replicano la stessa partizione di directory meno costosa.  
  
-   Definizione della pianificazione che definisce gli orari in cui la replica tra siti possono verificarsi.  
  
-   Definizione dell'intervallo di replica che definisce la frequenza con cui la replica verrà eseguita durante i periodi di quando è consentita la replica, come definito nella pianificazione.  
  
## <a name="in-this-guide"></a>In questa Guida  
  
-   [Definizione dei costi](../../ad-ds/plan/Determining-the-Cost.md)  
  
-   [Definizione della pianificazione](../../ad-ds/plan/Determining-the-Schedule.md)  
  
-   [Definizione dell'intervallo](../../ad-ds/plan/Determining-the-Interval.md)  
  


