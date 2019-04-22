---
title: Sospendere e lasciare attiva una sessione utente
description: Informazioni su come sospendere un utente da una sessione di MultiPoint senza eseguire la disconnessione
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5263bce3-fe92-4398-8393-2e3a4e05d530
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: cc4310e6f7609464cf037b750bec6e5e805e0b26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815222"
---
# <a name="suspend-and-leave-user-session-active"></a>Sospendere e lasciare attiva una sessione utente
È possibile scollegare o sospendere gli utenti dal sistema servizi MultiPoint quando non si desidera terminare le sessioni degli utenti. Una sessione può essere disconnessa anche dall'utente stesso. Quando una sessione utente è sospesa, tale sessione rimane attiva nella memoria del computer del sistema MultiPoint Services finché il computer non viene arrestato o riavviato. In quel momento, tutte le sessioni sospese vengono chiuse e il lavoro non salvato viene perso.  
  
1.  Aprire Gestione MultiPoint in modalità stazione e quindi fare clic sui **stazioni** scheda.  
  
2.  Nella colonna **Computer** fare clic sul computer di cui si vuole sospendere la sessione.  
  
3.  In **Stations Tasks** (Attività stazioni) fare clic su **Suspend all stations** (Sospendi tutte le stazioni).  
  
Dopo che una sessione utente è stata sospesa, l'utente può accedere alla stessa stazione o a una stazione diversa e continuare a lavorare nella sessione originaria.  
  
## <a name="see-also"></a>Vedere anche  
[Gestire desktop utente](manage-user-desktops-using-multipoint-dashboard.md)  
[Scollegare o disconnettere sessioni utente](Log-off-or-Disconnect-User-Sessions.md)