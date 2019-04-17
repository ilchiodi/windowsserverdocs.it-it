---
ms.assetid: ffb9d63c-ba7c-4ad1-b814-6db67f98c943
title: Il ruolo delle Pipeline delle attestazioni
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5076a686b5d0b9a539f6cad8594aaf84dccc3edb
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-the-claims-pipeline"></a>Il ruolo delle Pipeline delle attestazioni
La pipeline delle attestazioni in Active Directory Federation Services \(AD FS\) rappresenta il percorso che le attestazioni devono seguire attraverso il servizio federativo prima che possano essere rilasciati. Il servizio federativo gestisce l'intero processo end-to-end di propagazione attestazioni le varie fasi della pipeline delle attestazioni, che include anche l'elaborazione delle regole attestazione dal motore delle regole attestazioni.  
  
Per ulteriori informazioni sulle regole attestazione, vedere [ruolo delle attestazioni regole](The-Role-of-Claim-Rules.md). Per ulteriori informazioni su come il motore delle regole attestazione elabora le regole, vedere [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  
La sezione seguente illustra il processo che controlla il servizio federativo in modo più dettagliato.  
  
## <a name="claims-pipeline-process"></a>Processo della pipeline delle attestazioni  
Il processo della pipeline delle attestazioni è costituito da tre fasi di alto livello. Ogni fase del processo inizializza il motore delle regole attestazione per elaborare le regole di attestazione che sono specifiche di tale fase. Queste fasi includono \ (nell'ordine che essi occur\):  
  
1.  L'accettazione di attestazioni in ingresso: questa fase della pipeline delle attestazioni viene usata per estrarre le attestazioni in ingresso dal token ed eliminare quelle non previste o non attendibili. Dopo l'estrazione, le regole di accettazione che costituiscono la regola di trasformazione accettazione impostato per un vengono eseguite le attendibilità del provider di attestazioni. Queste regole possono essere utilizzate per passare o aggiungere nuove attestazioni che possono quindi essere usate nelle fasi successive delle pipeline delle attestazioni. L'output di questa fase viene usato come input per la gestione temporanea secondo e terzo.  
  
2.  Autorizzazione del richiedente delle attestazioni: questa fase viene usata dal motore delle attestazioni per rilasciare consentire o negare le attestazioni in base indica se il token richiedente è consentito per ottenere un token per la relying party specificata o non. Tuttavia, prima di impostare le regole di autorizzazione che costituiscono una regola di autorizzazione rilascio o imposta la regola di autorizzazione di delega per, ciò può verificarsi un trust della relying party sono stato eseguito.  
  
3.  Rilasciare le attestazioni in uscita: questa fase viene usata per rilasciare attestazioni in uscita e inviarle lungo la pipeline dove verranno inserite in un token di sicurezza. Tuttavia, prima che ciò può verificarsi ancora, le regole di rilascio che costituiscono la regola di trasformazione rilascio impostare per un trust della relying party, che determina quali attestazioni verranno rilasciate come attestazioni in uscita.  
  
Tutte le tre fasi eseguono l'elaborazione delle regole attestazioni ma utilizzano un diverso set di regole. Come descritto in precedenza, ogni fase è associato un set di regole in base all'emittente delle attestazioni in ingresso \(the acceptance rules\) o il servizio di destinazione per il quale vengono rilasciate le claimincludes \ (rules\ autorizzazione e rilascio).  
  
Le attestazioni sono indipendenti dal token\ ma vengono trasmesse in rete incapsulate in token di sicurezza. Le regole attestazioni vengono applicate alle attestazioni indipendentemente dal formato del token di sicurezza in ingresso o in uscita.  
  
Le regole attestazioni contengono la logica definita administrator\ mediante il quale il motore delle attestazioni accetterà le attestazioni in ingresso, autorizzare le attestazioni in base all'identità del richiedente e rilasciano attestazioni che sono necessari per una relying party. Infine, è il motore delle attestazioni che determina quali attestazioni entreranno nel token di sicurezza che verranno rilasciate dopo l'attestazione è stata transitata attraverso la pipeline delle attestazioni.  
  
Come illustrato nella figura seguente, la pipeline delle attestazioni è responsabile per l'intero processo end-to-end di flusso di un'attestazione nelle diverse fasi della pipeline per finire con un'attestazione emessa che verrà inviata tramite un trust della relying party. L'attestazione in uscita nell'illustrazione rappresenta l'attestazione rilasciata.  
  
![Ruoli di AD FS](media/adfs2_pipeline.gif)  
  
Anche se non è mostrato nell'illustrazione, è il motore delle attestazioni esegue effettivamente l'elaborazione delle regole in ogni fase. Per ulteriori informazioni, vedere [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  

