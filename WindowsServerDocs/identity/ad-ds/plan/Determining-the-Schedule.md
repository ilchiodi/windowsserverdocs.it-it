---
ms.assetid: 28ecaf5c-9131-406c-b211-a230162e462e
title: Definizione della pianificazione
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: dee63ce0fb687b2b722ce64614c54388fc544433
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839002"
---
# <a name="determining-the-schedule"></a>Definizione della pianificazione

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile controllare la disponibilità di collegamento di sito impostando una pianificazione per i collegamenti di sito. Quando la replica tra due siti attraversa più collegamenti di sito, punto di intersezione tra le pianificazioni di replica in tutti i collegamenti rilevanti determina la pianificazione della connessione tra i due siti.  
  
Per pianificare l'impostazione di pianificazione del collegamento di sito, creare due pianificazioni sovrapposte tra i collegamenti di sito che contengono i controller di dominio che vengono replicati direttamente tra loro. Usare la pianificazione predefinita (100% disponibili) su tali collegamenti a meno che non si vuole bloccare il traffico di replica durante le ore di picco. Per il blocco della replica, viene data priorità al resto del traffico, ma è anche aumentare la latenza di replica.  
  
I controller di dominio archiviano l'ora in Coordinated Universal Time (UTC). Impostazioni di tempo nelle pianificazioni oggetto collegamento di sito sono conformi all'ora locale del sito e computer in cui la pianificazione è impostata. Quando un controller di dominio contatta un computer che si trova in un altro sito e il fuso orario, la pianificazione nel controller di dominio viene visualizzata l'impostazione di ora secondo l'ora locale per il sito del computer.  
  


