---
title: Scollegare o disconnettere sessioni utente
description: Informazioni su come disconnettersi manualmente da un utente
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3e9bbcdc-e33b-481e-8b46-787a4f6d58bc
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 518e9dc9ba9603d988a7e21e08caa29db9f04bde
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854942"
---
# <a name="log-off-or-disconnect-user-sessions"></a>Scollegare o disconnettere sessioni utente
Gli utenti di servizi multiPoint possono accedere e disconnettersi delle sessioni di desktop come per qualsiasi sessione di Windows. Gli utenti possono inoltre disconnettere o sospendere la sessione in modo che la stazione MultiPoint Services non è in uso, ma la sessione rimane attiva nella memoria del computer del sistema MultiPoint servizi.  
  
Inoltre, gli utenti amministrativi possono terminare una sessione utente se l'utente ha allontanato dalla sessione di servizi MultiPoint o ha dimenticato di disconnettersi dal sistema.  
  
## <a name="logging-off-or-disconnecting-a-session"></a>Scollegamento o disconnessione di una sessione  
La tabella seguente descrive le differenti opzioni che l'amministratore o un utente può usare per scollegare, sospendere o chiudere una sessione.  
  
|||  
|-|-|  
|**Azione**|**Effetto**|  
|Fare clic su **avviare**, fare clic su impostazioni, fare clic sul nome utente (angolo superiore destro) e quindi fare clic su **disconnettersi**.|La sessione viene chiusa e alla stazione può accedere qualsiasi utente.|  
|Fare clic su **Start** (Avvia), su **Settings** (Impostazioni), su Power (Arresta) e quindi su **Disconnect** (Disconnetti).|La sessione viene disconnessa e mantenuta nella memoria del computer. Alla stazione può accedere lo stesso utente o un utente diverso.|  
|Fare clic su **avviare**, fare clic su impostazioni, fare clic sul nome utente (angolo superiore destro) e quindi fare clic su **blocco**|La stazione viene bloccata e la sessione viene mantenuta nella memoria del computer.|  
  
## <a name="suspending-or-ending-a-users-session"></a>Sospensione o chiusura di una sessione utente  
La tabella seguente descrive le diverse opzioni che un utente con privilegi amministrativi può usare per disconnettere o chiudere una sessione.  
  
|||  
|-|-|  
|**Azione**|**Effetto**|  
|**Sospensione:** Selezionare Gestione MultiPoint, usare il **stazioni** scheda per sospendere la sessione dell'utente. Per altre informazioni, vedere l'argomento [Sospendere e lasciare attiva una sessione utente](Suspend-and-Leave-User-Session-Active.md).|La sessione utente viene chiusa e viene mantenuta nella memoria del computer. Alla stazione può accedere lo stesso utente o un utente diverso. L'utente può accedere alla stessa stazione o a una stazione diversa e continuare a lavorare.|  
|**Fine:** Selezionare Gestione MultiPoint, usare il **stazioni** scheda per terminare la sessione dell'utente. È anche possibile chiudere tutte le sessioni utente nella scheda **Stations** (Stazioni). Per altre informazioni, vedere l'argomento [Chiusura di una sessione utente](End-a-User-Session.md).|La sessione utente viene chiusa e alla stazione può accedere qualsiasi utente. La sessione utente non visualizza più la scheda **Stations** (Stazioni) e non è nella memoria del computer.|  
  
## <a name="see-also"></a>Vedere anche  
[Sospendere e lasciare attiva la sessione utente](Suspend-and-Leave-User-Session-Active.md)  
[Chiudere una sessione utente](End-a-User-Session.md)  
[Gestire desktop utente](manage-user-desktops-using-multipoint-dashboard.md)  
[Scollegamento delle sessioni utente](Log-Off-User-Sessions.md)    