---
ms.assetid: 9cafa3e1-8118-4a75-a7c2-1dbe40b1a444
title: Configurare le regole delle attestazioni
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 7051040feb023ec9ab647d6e368136fb04b7db34
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817144"
---
# <a name="configure-claim-rules"></a>Configurare le regole di attestazione

In un modello di identità basato su attestazioni\-, la funzione di Active Directory Federation Services \(AD FS\) come Federation Services consiste nell'emettere un token che contiene un set di attestazioni. Le regole attestazioni regolano le decisioni in relazione alle attestazioni che AD FS problemi. Le regole attestazioni e tutti i dati di configurazione del server vengono archiviati nel database di configurazione AD FS.  
  
AD FS prende decisioni di rilascio basate sulle informazioni di identità fornite sotto forma di attestazioni e altre informazioni contestuali. A livello generale, AD FS funge da processore di regole prendendo un set di attestazioni come input, esegue una serie di trasformazioni e quindi restituisce un set di attestazioni diverso come output. 

Gli argomenti seguenti aiuteranno a creare le regole che AD FS elaborerà: 
  
-   [Creare una regola per il pass-through o filtrare un'attestazione in ingresso](Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [Creare una regola per concedere l'accesso a tutti gli utenti](Create-a-Rule-to-Permit-All-Users.md)  
  
-   [Creare una regola per concedere o rifiutare l'accesso agli utenti in base a un'attestazione in ingresso](Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [Creare una regola per inviare attributi LDAP come attestazioni](Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [Creare una regola per inviare l'appartenenza al gruppo come attestazione](Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [Creare una regola per trasformare un'attestazione in ingresso](Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [Creare una regola per inviare un'attestazione di metodo di autenticazione](Create-a-Rule-to-Send-an-Authentication-Method-Claim.md) 
-   [Creare una regola per inviare un'attestazione compatibile con AD FS 1. x](Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md) 
  
-   [Creare una regola per inviare attestazioni mediante una regola personalizzata](Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule.md)  

## <a name="see-also"></a>Vedi anche  
[Operazioni di AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
