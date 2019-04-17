---
ms.assetid: 20d48afc-2623-43e9-8ed9-aeb9a0505630
title: Configurare le regole attestazioni
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 259e2b266b64a3b34c237cfe209a3558124c8ef2
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="configure-claim-rules"></a>Configurare le regole di attestazione

>Si applica a: Windows Server 2016, Windows Server 2012 R2

In un modello di identità basate su claims\, la funzione di Active Directory Federation Services (ADFS) come servizi federativi consiste nel rilasciare un token che contiene un set di attestazioni. Attestazioni regole determinano le decisioni relativamente che ADFS rilascia attestazioni. Regole attestazione e tutti i dati di configurazione server vengono archiviati nel database di configurazione di ADFS.  
  
AD FS decisioni di rilascio che si basano sulle informazioni di identità che viene fornite in forma di attestazioni e altre informazioni contestuali. A un livello elevato, AD FS funziona come un processore regole effettuando una set di attestazioni come input, esegue un numero di trasformazioni e quindi restituisce un set diverso di attestazioni di output. 

Gli argomenti seguenti semplificheranno la creazione di regole che verranno elaborate AD FS: 
  
-   [Creare una regola per Pass-Through o filtrare un'attestazione in ingresso](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [Creare una regola per consentire tutti gli utenti](../../ad-fs/operations/Create-a-Rule-to-Permit-All-Users.md)  
  
-   [Creare una regola per consentire o negare agli utenti in base a un'attestazione in ingresso](../../ad-fs/operations/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [Creare una regola per inviare attributi LDAP come attestazioni](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [Creare una regola per l'appartenenza al gruppo di trasmissione come attestazione](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [Creare una regola per trasformare un'attestazione in ingresso](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [Creare una regola per inviare un'attestazione di metodo di autenticazione](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)  
  
-   [Creare una regola per inviare attestazioni mediante una regola personalizzata](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-rule.md)  

## <a name="see-also"></a>Vedere anche  
[Operazioni di ADFS](../../ad-fs/AD-FS-2016-Operations.md) 
