---
ms.assetid: 20d48afc-2623-43e9-8ed9-aeb9a0505630
title: Configurare le regole delle attestazioni
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b46e228f202eeae7f8cbcf4c1a6851686f905e48
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407635"
---
# <a name="configure-claim-rules"></a>Configurare le regole di attestazione

In un modello di identità Claims @ no__t-0based la funzione di Active Directory Federation Services (AD FS) come Federation Services consiste nell'emettere un token che contiene un set di attestazioni. Le regole attestazioni regolano le decisioni in relazione alle attestazioni che AD FS problemi. Le regole attestazioni e tutti i dati di configurazione del server vengono archiviati nel database di configurazione AD FS.  
  
AD FS prende decisioni di rilascio basate sulle informazioni di identità fornite sotto forma di attestazioni e altre informazioni contestuali. A livello generale, AD FS funge da processore di regole prendendo un set di attestazioni come input, esegue una serie di trasformazioni e quindi restituisce un set di attestazioni diverso come output. 

Gli argomenti seguenti aiuteranno a creare le regole che AD FS elaborerà: 
  
-   [Creare una regola per il pass-through o filtrare un'attestazione in ingresso](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [Creare una regola per concedere l'accesso a tutti gli utenti](../../ad-fs/operations/Create-a-Rule-to-Permit-All-Users.md)  
  
-   [Creare una regola per concedere o rifiutare l'accesso agli utenti in base a un'attestazione in ingresso](../../ad-fs/operations/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [Creare una regola per inviare attributi LDAP come attestazioni](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [Creare una regola per inviare l'appartenenza al gruppo come attestazione](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [Creare una regola per trasformare un'attestazione in ingresso](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [Creare una regola per inviare un'attestazione di metodo di autenticazione](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)  
  
-   [Creare una regola per inviare attestazioni mediante una regola personalizzata](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-rule.md)  

## <a name="see-also"></a>Vedere anche  
[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
