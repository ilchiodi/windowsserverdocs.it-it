---
title: Gestire stazioni utente
description: Informazioni su come gestire stazioni utente in servizi MultiPoint
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b418578d-3a4c-49b0-90db-8389b320b2f6
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 8c5351f3e8ec9890ef72905b646c37e9b049745e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823722"
---
# <a name="manage-user-stations"></a>Gestire stazioni utente
Questa sezione illustra come gestire le *stazioni* che costituiscono il sistema MultiPoint Services. La gestione di un sistema MultiPoint Services include la gestione dei componenti hardware e software di gestione MultiPoint. In un sistema MultiPoint Services, un desktop è l'interfaccia utente del software visualizzata sul monitor per ogni stazione utente.  
  
## <a name="station-status"></a>Stato della stazione  
È possibile visualizzare i tipi di stato seguenti per ogni desktop nella scheda **Stations** (Stazioni). Lo stato include:  
  
-   Utenti connessi  
  
-   Sessioni utente sospese ma ancora attive nel computer  
  
-   Stazioni usate e utenti da cui sono usate  
  
Per altre informazioni sulla visualizzazione dello stato dei desktop, vedere l'argomento [Visualizzare lo stato delle connessioni utente](View-User-Connection-Status.md).  

>[!TIP] 
> È possibile assegnare nomi descrittivi a ogni stazione per identificare più facilmente le stazioni. Usare **Identify station** (Identifica stazione) per visualizzare il nome della stazione sullo schermo assegnato.
  
## <a name="different-ways-to-log-standard-users-off-of-the-multipoint-services-system"></a>Metodi per scollegare utenti standard dal sistema MultiPoint Services  
Un *utente con privilegi amministrativi* può scollegarsi da Windows in qualunque momento, senza conseguenze per gli utenti attivi collegati al sistema MultiPoint Services. Gli *utenti standard* possono *disconnettere* la sessione o anche *scollegarsi* dal sistema MultiPoint Services. Quando la protezione del disco è abilitata, è opportuno che a fine giornata l'utente salvi il proprio lavoro sul computer o su un dispositivo di archiviazione esterno, in modo tale che quando si arresta il sistema MultiPoint Services il lavoro venga salvato e possa essere recuperato in un secondo momento.  
  
Un utente con privilegi amministrativi può avere necessità di chiudere la *sessione* di un utente standard, anziché lasciare che sia l'utente a scollegarsi. È possibile chiudere la sessione di un utente standard in due modi:  
  
-   Chiudere la sessione e scollegare l'utente. Per altre informazioni sulla chiusura di una sessione utente, vedere l'argomento [Chiusura di una sessione utente](End-a-User-Session.md).  
  
-   Sospendere l'utente per chiudere temporaneamente la sessione utente, ma tenere la sessione attiva nella memoria del computer del sistema MultiPoint Services. L'utente sospeso può quindi riconnettersi alla sessione dalla stessa stazione o da un'altra stazione e continuare il proprio lavoro. Per altre informazioni sulla sospensione di una sessione utente, vedere l'argomento [Sospendere e lasciare attiva una sessione utente](Suspend-and-Leave-User-Session-Active.md).  
  
## <a name="set-a-station-to-automatically-log-on"></a>Impostare l'accesso automatico per una stazione  
Un utente con privilegi amministrativi può configurare l'accesso automatico di una o più stazioni all'avvio del computer che esegue MultiPoint Services. Per altre informazioni sull'accesso automatico, vedere l'argomento [Configurazione di una stazione per l'accesso automatico](Set-up-a-Station-for-Automatic-Logon.md).  
  
## <a name="split-a-station"></a>Dividere una stazione  
Qualsiasi monitor di stazione con risoluzione superiore a 1024x768 può essere diviso in due stazioni. Per altre informazioni sulla divisione di una stazione, vedere l'argomento [Dividere una stazione utente](Split-a-User-Station.md).  
  
## <a name="see-also"></a>Vedere anche  
[Visualizzare lo stato di connessione utente](View-User-Connection-Status.md)  
[Scollegare o disconnettere sessioni utente](Log-off-or-Disconnect-User-Sessions.md)  
[Sospendere e lasciare attiva la sessione utente](Suspend-and-Leave-User-Session-Active.md)  
[Configurare una stazione per l'accesso automatico](Set-up-a-Station-for-Automatic-Logon.md)  
[Chiudere una sessione utente](End-a-User-Session.md)  
[Dividere una stazione utente](Split-a-User-Station.md)