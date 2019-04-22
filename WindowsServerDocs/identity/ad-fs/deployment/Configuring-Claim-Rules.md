---
ms.assetid: 46dce9d4-7293-4b1c-9710-78b04f2e347a
title: Configurazione delle regole attestazioni
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6494f584edd5f84a5987707953f79edbce15cc02
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814662"
---
# <a name="configuring-claim-rules"></a>Configurazione delle regole attestazioni

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In delle attestazioni\-identità basata su modello, la funzione di Active Directory Federation Services \(ADFS\) come federation services consiste nel rilasciare un token che contiene un set di attestazioni. Le regole delle attestazioni regolano le decisioni in materia di attestazioni che ADFS rilascia. Le regole attestazioni e tutti i dati di configurazione di server vengono archiviati nel database di configurazione di AD FS.  
  
AD FS prende decisioni di rilascio che si basano sulle informazioni di identità che vengano fornite sotto forma di attestazioni e altre informazioni contestuali. A livello generale, AD FS funziona come un elaboratore regole adottando una set di attestazioni come input, esegue una serie di trasformazioni e quindi restituisce un diverso set di attestazioni come output.  
  
-   [Creare una regola per Pass-Through o filtrare un'attestazione in ingresso](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [Creare una regola per consentire tutti gli utenti](../../ad-fs/operations/Create-a-Rule-to-Permit-All-Users.md)  

-   [Creare una regola per inviare un'attestazione compatibile di AD FS 1.x](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)
  
-   [Creare una regola per consentire o negare agli utenti con un'attestazione in ingresso](../../ad-fs/operations/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [Creare una regola per inviare attributi LDAP come attestazioni](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [Creare una regola per l'appartenenza al gruppo di trasmissione come attestazione](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [Creare una regola per trasformare un'attestazione in ingresso](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [Creare una regola per inviare un'attestazione di metodo di autenticazione](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)  
  
-   [Creare una regola per inviare attestazioni mediante una regola personalizzata](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule.md)  

## <a name="additional-references"></a>Altri riferimenti  

[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md)
