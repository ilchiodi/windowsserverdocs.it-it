---
ms.assetid: de054ac2-a386-43ec-a537-c0de21549741
title: Impostazione delle proprietà di collegamento di sito
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4fa9a1fa8d2a463fe5f361a5a27ee2b9e3edc0f6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870362"
---
# <a name="setting-site-link-properties"></a>Impostazione delle proprietà di collegamento di sito

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In base alle proprietà degli oggetti di connessione viene eseguita la replica tra siti. Quando il controllo di coerenza informazioni (KCC) crea gli oggetti di connessione, la pianificazione della replica deriva dalle proprietà degli oggetti collegamento sito. Ogni oggetto collegamento di sito rappresenta la connessione di rete WAN WAN tra due o più siti.  
  
L'impostazione di proprietà dell'oggetto collegamento sito include i passaggi seguenti:  
  
-   Determinare il costo associato a tale percorso di replica. KCC Usa i costi per determinare la route meno costosa per la replica tra due siti che eseguono la replica stessa partizione di directory.  
  
-   Definizione della pianificazione che definisce i tempi in cui la replica tra siti possono verificarsi.  
  
-   Determinare l'intervallo di replica che definisce con quale frequenza debba eseguire la replica durante i tempi di quando è consentita la replica, come definito nella pianificazione.  
  
## <a name="in-this-guide"></a>Contenuto della guida  
  
-   [Definizione dei costi](../../ad-ds/plan/Determining-the-Cost.md)  
  
-   [Definizione della pianificazione](../../ad-ds/plan/Determining-the-Schedule.md)  
  
-   [Definizione dell'intervallo](../../ad-ds/plan/Determining-the-Interval.md)  
  


