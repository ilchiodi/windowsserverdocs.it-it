---
ms.assetid: 20d48afc-2623-43e9-8ed9-aeb9a0505630
title: Configurare le regole attestazioni
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e7eedd907c07c2aaef1670c5db3a6892ca3e650d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189634"
---
# <a name="configure-claim-rules"></a>Configurare le regole di attestazione

In delle attestazioni\-identità basata su modello, la funzione di Active Directory Federation Services (ADFS) come servizi di federazione è per emettere un token che contiene un set di attestazioni. Le regole delle attestazioni regolano le decisioni in relazione le attestazioni che ADFS rilascia. Le regole attestazioni e tutti i dati di configurazione di server vengono archiviati nel database di configurazione di AD FS.  
  
AD FS prende decisioni di rilascio che si basano sulle informazioni di identità che vengano fornite sotto forma di attestazioni e altre informazioni contestuali. A livello generale, AD FS funziona come un elaboratore regole adottando una set di attestazioni come input, esegue una serie di trasformazioni e quindi restituisce un diverso set di attestazioni come output. 

Gli argomenti seguenti semplificheranno la creazione di regole che elaborerà AD FS: 
  
-   [Creare una regola per Pass-Through o filtrare un'attestazione in ingresso](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [Creare una regola per concedere l'accesso a tutti gli utenti](../../ad-fs/operations/Create-a-Rule-to-Permit-All-Users.md)  
  
-   [Creare una regola per concedere o rifiutare l'accesso agli utenti in base a un'attestazione in ingresso](../../ad-fs/operations/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [Creare una regola per inviare attributi LDAP come attestazioni](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [Creare una regola per inviare l'appartenenza al gruppo come attestazione](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [Creare una regola per trasformare un'attestazione in ingresso](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [Creare una regola per inviare un'attestazione di metodo di autenticazione](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)  
  
-   [Creare una regola per inviare attestazioni mediante una regola personalizzata](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-rule.md)  

## <a name="see-also"></a>Vedere anche  
[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
