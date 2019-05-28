---
ms.assetid: 7d230527-f4fe-4572-8838-0b354ee0b06b
title: Aggiungere una descrizione di attestazione
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 454d261aa520778a6129ac9809f53894937b036a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190136"
---
# <a name="add-a-claim-description"></a>Aggiungere una descrizione di attestazione


In un'organizzazione partner account, gli amministratori creano le attestazioni per rappresentare l'appartenenza dell'utente in un gruppo o ruolo oppure per rappresentare alcuni dati relativi a un utente, ad esempio, numero di identificazione dipendente dell'utente.

In un'organizzazione partner risorse, gli amministratori di creare attestazioni corrispondente per rappresentare i gruppi e utenti che possono essere riconosciuti come gli utenti di risorse. Poiché in uscita attestazioni nella mappa dell'organizzazione partner account per le attestazioni in ingresso nell'organizzazione partner risorse, il partner risorse è in grado di accettare le credenziali fornite al partner account. 

È possibile utilizzare la procedura seguente per aggiungere un'attestazione.

Per completare questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente nel computer locale.  Informazioni dettagliate sull'utilizzo degli account appropriati e appartenenze [dominio gruppi predefiniti locali e](https://go.microsoft.com/fwlink/?LinkId=83477).

## <a name="to-add-a-claim-description"></a>Per aggiungere una descrizione di attestazione

1. In Server Manager, fare clic su **strumenti**, quindi selezionare **Gestione ADFS**. 

2.  Espandere **servizio** clic destro sul **Aggiungi descrizione attestazione**.
![aggiungere una descrizione di attestazione](media\Add-a-Claim-Description\claimdesc1.png)

3.  Aggiungi una descrizione di attestazione della finestra di dialogo **nome**, digitare un nome univoco che identifica il gruppo o ruolo per l'attestazione.

4.  Aggiungere un **breve nome**.

5.  In **identificatore attestazioni**, digitare un URI che viene associato al gruppo o ruolo dell'attestazione che verrà utilizzato.

6.  In **Descrizione**, digitare un testo che descrive lo scopo di questa attestazione.

7.  A seconda delle esigenze dell'organizzazione, selezionare una delle seguenti caselle di controllo, come appropriato, per pubblicare l'attestazione nei metadati di federazione:


    - Per pubblicare questa attestazione per rendere i partner tenere presente che questo server può accettare la richiesta, fare clic su **pubblica l'attestazione nei metadati federativi come tipo di attestazione in grado di accettare questo servizio federativo**.
    - Per pubblicare questa attestazione per rendere i partner tenere presente che questo server può generare la richiesta, fare clic su **pubblica l'attestazione nei metadati federativi come tipo di attestazione che questo servizio federativo può inviare**.

8.  Fare clic su **OK**.

![aggiungere una descrizione di attestazione](media\Add-a-Claim-Description\claimdesc2.png)

  
## <a name="see-also"></a>Vedere anche  
[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
