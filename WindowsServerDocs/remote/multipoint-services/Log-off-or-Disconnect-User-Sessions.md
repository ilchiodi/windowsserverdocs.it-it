---
title: Scollegare o disconnettere sessioni utente
description: Informazioni su come disconnettere manualmente un utente
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c636af35a78eab76d69c68b6f506b64dcb555f81
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395271"
---
# <a name="log-off-or-disconnect-user-sessions"></a>Scollegare o disconnettere sessioni utente
Gli utenti di servizi multiPoint possono accedere e disconnettersi delle sessioni di desktop come per qualsiasi sessione di Windows. Gli utenti possono anche disconnettere o sospendere la sessione in modo che la stazione MultiPoint Services non venga usata, ma la sessione rimane attiva nella memoria del computer del sistema MultiPoint Services.  
  
Inoltre, gli utenti amministratori possono terminare la sessione di un utente se l'utente si è distolto dalla sessione MultiPoint Services o ha dimenticato di disconnettersi dal sistema.  
  
## <a name="logging-off-or-disconnecting-a-session"></a>Scollegamento o disconnessione di una sessione  
La tabella seguente descrive le differenti opzioni che l'amministratore o un utente può usare per scollegare, sospendere o chiudere una sessione.  
  
|||  
|-|-|  
|**Azione**|**Effetto**|  
|Fare clic su **avviare**, fare clic su impostazioni, fare clic sul nome utente (angolo superiore destro) e quindi fare clic su **disconnettersi**.|La sessione viene chiusa e alla stazione può accedere qualsiasi utente.|  
|Fare clic su **Start** (Avvia), su **Settings** (Impostazioni), su Power (Arresta) e quindi su **Disconnect** (Disconnetti).|La sessione viene disconnessa e mantenuta nella memoria del computer. Alla stazione può accedere lo stesso utente o un utente diverso.|  
|Fare clic su **avviare**, fare clic su impostazioni, fare clic sul nome utente (angolo superiore destro) e quindi fare clic su **blocco**|La stazione viene bloccata e la sessione viene mantenuta nella memoria del computer.|  
  
## <a name="suspending-or-ending-a-users-session"></a>Sospensione o chiusura di una sessione utente  
Nella tabella seguente vengono descritte le diverse opzioni che un utente amministratore può utilizzare per disconnettere o terminare la sessione di un utente.  
  
|||  
|-|-|  
|**Azione**|**Effetto**|  
|**Sospendere** In Gestione MultiPoint usare la scheda **stations (stazioni** ) per sospendere la sessione dell'utente. Per altre informazioni, vedere l'argomento [Sospendere e lasciare attiva una sessione utente](Suspend-and-Leave-User-Session-Active.md).|La sessione dell'utente termina e viene mantenuta nella memoria del computer. Alla stazione può accedere lo stesso utente o un utente diverso. L'utente può accedere alla stessa stazione o a una stazione diversa e continuare a lavorare.|  
|**Fine** In Gestione MultiPoint usare la scheda **stations (stazioni** ) per terminare la sessione dell'utente. È anche possibile chiudere tutte le sessioni utente nella scheda **Stations** (Stazioni). Per altre informazioni, vedere l'argomento [Chiusura di una sessione utente](End-a-User-Session.md).|La sessione dell'utente termina e la stazione diventa disponibile per l'accesso da parte di qualsiasi utente. La sessione dell'utente non viene più visualizzata nella scheda **stazioni** e non si trova nella memoria del computer.|  
  
## <a name="see-also"></a>Vedere anche  
[Sospendere e lasciare attiva la sessione utente](Suspend-and-Leave-User-Session-Active.md)  
[Terminare una sessione utente](End-a-User-Session.md)  
[Gestire i desktop degli utenti](manage-user-desktops-using-multipoint-dashboard.md)  
[Scollegare le sessioni utente](Log-Off-User-Sessions.md)    