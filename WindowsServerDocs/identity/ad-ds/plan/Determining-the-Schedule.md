---
ms.assetid: 28ecaf5c-9131-406c-b211-a230162e462e
title: Definizione della pianificazione
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0ec953b34475c50e62553a9ba95e4d45d9904bf3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="determining-the-schedule"></a>Definizione della pianificazione

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile controllare la disponibilità di collegamento di sito tramite l'impostazione di una pianificazione per i collegamenti di sito. Quando la replica tra due siti attraversa più collegamenti di sito, l'intersezione tra le pianificazioni di replica in tutti i collegamenti rilevanti determina la pianificazione di connessione tra i due siti.  
  
Per pianificare l'impostazione di pianificazione del collegamento di sito, creare due pianificazioni sovrapposte tra i collegamenti di sito che contengono i controller di dominio che eseguono la replica direttamente tra loro. Utilizzare la pianificazione predefinita (100% disponibile) su tali collegamenti a meno che non si desidera bloccare il traffico di replica durante le ore di punta. Per il blocco della replica, si dà la priorità a altro traffico, ma si aumenta anche la latenza di replica.  
  
I controller di dominio memorizzano l'ora in formato Coordinated Universal Time (UTC). Le impostazioni di tempo in pianificazioni oggetto collegamento di sito conforme all'ora locale del sito e computer in cui la pianificazione è impostata. Quando un controller di dominio contatta un computer che si trova in un sito diverso e il fuso orario, la pianificazione nel controller di dominio Visualizza l'impostazione di tempo in base all'ora locale per il sito del computer.  
  


