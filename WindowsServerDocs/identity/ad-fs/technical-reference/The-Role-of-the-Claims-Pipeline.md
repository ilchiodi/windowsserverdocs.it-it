---
ms.assetid: ffb9d63c-ba7c-4ad1-b814-6db67f98c943
title: Ruolo delle pipeline delle attestazioni
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5076a686b5d0b9a539f6cad8594aaf84dccc3edb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887062"
---
>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-the-claims-pipeline"></a>Ruolo delle pipeline delle attestazioni
La pipeline delle attestazioni in Active Directory Federation Services \(ADFS\) rappresenta il percorso che le attestazioni devono seguire attraverso il servizio federativo prima che possano essere rilasciati. Il servizio federativo gestisce la fine intera\-a\-Termina processo di propagazione attestazioni le varie fasi della pipeline di attestazioni, che include anche l'elaborazione delle regole attestazione dal motore regole attestazione.  
  
Per ulteriori informazioni sulle regole di attestazione, vedere [ruolo delle attestazioni regole](The-Role-of-Claim-Rules.md). Per altre informazioni su come il motore delle regole attestazioni elabora le regole, vedere [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  
La sezione seguente illustra più in dettaglio il processo gestito dal Servizio federativo.  
  
## <a name="claims-pipeline-process"></a>Processo della pipeline delle attestazioni  
Il processo di attestazioni è costituito da tre elevata\-livello fasi. Ogni fase del processo inizializza il motore delle regole attestazioni per l'elaborazione delle regole attestazioni specifiche di tale fase. Queste fasi includono \(nell'ordine in cui sono presenti\):  
  
1.  Accettazione delle attestazioni in ingresso: questa fase della pipeline delle attestazioni viene usata per estrarre le attestazioni in ingresso dal token ed eliminare quelle non previste o non attendibili. Dopo l'estrazione, vengono eseguite le regole di accettazione che costituiscono il set di regole di trasformazione accettazione per un trust del provider di attestazioni. Queste regole possono essere usate per passare o aggiungere nuove attestazioni che possono quindi essere usate nelle fasi successive delle pipeline delle attestazioni. L'output di questa fase viene usato come input per la seconda e la terza fase.  
  
2.  Autorizzazione del richiedente delle attestazioni: questa fase viene usata dal motore delle attestazioni per rilasciare attestazioni che consentono o negano l'accesso a seconda che al richiedente di un token sia consentito ottenere o meno un token per la relying party specificata. Tuttavia, prima ancora, devono essere eseguite le regole di autorizzazione che costituiscono il set di regole di autorizzazione rilascio o il set di regole di autorizzazione di delega per un trust della relying party.  
  
3.  Rilascio di attestazioni in uscita: questa fase viene usata per rilasciare le attestazioni in uscita e inviarle lungo la pipeline dove verranno inserite in un token di sicurezza. Tuttavia, prima ancora, vengono eseguite le regole di rilascio che costituiscono il set di regole di trasformazione rilascio per un trust della relying party e viene così determinato quali attestazioni verranno rilasciate come attestazioni in uscita.  
  
Tutte e tre le fasi eseguono l'elaborazione delle regole di attestazione, ma usano un set di regole diverso. Come descritto in precedenza, ogni fase è associato un set di regole in base l'autorità emittente di attestazioni in ingresso \(le regole di accettazione\) o il servizio di destinazione per il quale vengono rilasciate le claimincludes \(le regole di autorizzazione e di rilascio\).  
  
Le attestazioni sono token\-indipendente ma che vengono trasmessi in rete incapsulata nei token di sicurezza. Le regole attestazioni vengono applicate alle attestazioni indipendentemente dal formato del token di sicurezza in ingresso o in uscita.  
  
Attestazioni regole contengono l'amministratore\-logica definita mediante il quale il motore delle attestazioni verrà accettati le attestazioni in ingresso, autorizzare le attestazioni in base all'identità del richiedente e rilasciano attestazioni che sono necessari per una relying party. Infine, è il motore delle attestazioni a determinare quali attestazioni entreranno nel token di sicurezza che verrà rilasciato al termine del flusso dell'attestazione nella pipeline delle attestazioni.  
  
Come illustrato nella figura seguente, la pipeline delle attestazioni è responsabile per l'intera entità finale\-a\-Termina processo di propagazione di un'attestazione le varie fasi della pipeline per finire con un'attestazione emessa che verrà inviata tramite un trust della relying party. L'attestazione in uscita nell'illustrazione rappresenta l'attestazione rilasciata.  
  
![Ruoli di AD FS](media/adfs2_pipeline.gif)  
  
Anche se non è mostrato nell'illustrazione, è il motore delle attestazioni a eseguire la vera e propria elaborazione delle regole in ogni fase. Per altre informazioni, vedere [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  

