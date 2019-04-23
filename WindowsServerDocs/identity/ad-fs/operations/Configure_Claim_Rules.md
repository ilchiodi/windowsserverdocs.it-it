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
ms.openlocfilehash: 259e2b266b64a3b34c237cfe209a3558124c8ef2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871632"
---
# <a name="configure-claim-rules"></a>Configurare le regole di attestazione

>Si applica a: Windows Server 2016, Windows Server 2012 R2

In delle attestazioni\-identità basata su modello, la funzione di Active Directory Federation Services (ADFS) come servizi di federazione è per emettere un token che contiene un set di attestazioni. Le regole delle attestazioni regolano le decisioni in relazione le attestazioni che ADFS rilascia. Le regole attestazioni e tutti i dati di configurazione di server vengono archiviati nel database di configurazione di AD FS.  
  
AD FS prende decisioni di rilascio che si basano sulle informazioni di identità che vengano fornite sotto forma di attestazioni e altre informazioni contestuali. A livello generale, AD FS funziona come un elaboratore regole adottando una set di attestazioni come input, esegue una serie di trasformazioni e quindi restituisce un diverso set di attestazioni come output. 

Gli argomenti seguenti semplificheranno la creazione di regole che elaborerà AD FS: 
  
-   [Creare una regola per Pass-Through o filtrare un'attestazione in ingresso](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [Creare una regola per consentire tutti gli utenti](../../ad-fs/operations/Create-a-Rule-to-Permit-All-Users.md)  
  
-   [Creare una regola per consentire o negare agli utenti con un'attestazione in ingresso](../../ad-fs/operations/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [Creare una regola per inviare attributi LDAP come attestazioni](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [Creare una regola per l'appartenenza al gruppo di trasmissione come attestazione](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [Creare una regola per trasformare un'attestazione in ingresso](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [Creare una regola per inviare un'attestazione di metodo di autenticazione](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)  
  
-   [Creare una regola per inviare attestazioni mediante una regola personalizzata](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-rule.md)  

## <a name="see-also"></a>Vedere anche  
[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
