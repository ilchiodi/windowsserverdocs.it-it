---
ms.assetid: 9cafa3e1-8118-4a75-a7c2-1dbe40b1a444
title: Configurare le regole attestazioni
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f9e0509e2f870fd0edc7f0c6a241d789945e7ccb
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="configure-claim-rules"></a>Configurare le regole di attestazione

>Si applica a: Windows Server 2016, Windows Server 2012 R2

In un modello di identità basate su claims\, la funzione di Active Directory Federation Services \(AD FS\) come servizi federativi consiste nel rilasciare un token che contiene un set di attestazioni. Attestazioni regole determinano le decisioni relativamente che ADFS rilascia attestazioni. Regole attestazione e tutti i dati di configurazione server vengono archiviati nel database di configurazione di ADFS.  
  
AD FS decisioni di rilascio che si basano sulle informazioni di identità che viene fornite in forma di attestazioni e altre informazioni contestuali. A un livello elevato, AD FS funziona come un processore regole effettuando una set di attestazioni come input, esegue un numero di trasformazioni e quindi restituisce un set diverso di attestazioni di output. 

Gli argomenti seguenti semplificheranno la creazione di regole che verranno elaborate AD FS: 
  
-   [Creare una regola per Pass-Through o filtrare un'attestazione in ingresso](Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [Creare una regola per consentire tutti gli utenti](Create-a-Rule-to-Permit-All-Users.md)  
  
-   [Creare una regola per consentire o negare agli utenti in base a un'attestazione in ingresso](Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [Creare una regola per inviare attributi LDAP come attestazioni](Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [Creare una regola per l'appartenenza al gruppo di trasmissione come attestazione](Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [Creare una regola per trasformare un'attestazione in ingresso](Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [Creare una regola per inviare un'attestazione di metodo di autenticazione](Create-a-Rule-to-Send-an-Authentication-Method-Claim.md) 
-   [Creare una regola per inviare un'attestazione compatibile di AD FS 1. x](Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md) 
  
-   [Creare una regola per inviare attestazioni mediante una regola personalizzata](Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule.md)  

## <a name="see-also"></a>Vedere anche  
[Operazioni di ADFS](../../ad-fs/AD-FS-2016-Operations.md) 
