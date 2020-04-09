---
ms.assetid: ffb9d63c-ba7c-4ad1-b814-6db67f98c943
title: Ruolo delle pipeline delle attestazioni
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ab5cfba579313cbe6aa56c84fb59a87e06101f15
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853844"
---
# <a name="the-role-of-the-claims-pipeline"></a>Ruolo delle pipeline delle attestazioni
La pipeline delle attestazioni in Active Directory Federation Services \(AD FS\) rappresenta il percorso che le attestazioni devono seguire nel Servizio federativo prima che possano essere rilasciate. Il servizio federativo gestisce la fine intera\-a\-Termina processo di propagazione attestazioni le varie fasi della pipeline di attestazioni, che include anche l'elaborazione delle regole attestazione dal motore regole attestazione.  
  
Per ulteriori informazioni sulle regole di attestazione, vedere [ruolo delle attestazioni regole](The-Role-of-Claim-Rules.md). Per altre informazioni su come il motore delle regole attestazioni elabora le regole, vedere [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  
La sezione seguente illustra più in dettaglio il processo gestito dal Servizio federativo.  
  
## <a name="claims-pipeline-process"></a>Processo della pipeline delle attestazioni  
Il processo di attestazioni è costituito da tre elevata\-livello fasi. Ogni fase del processo inizializza il motore delle regole attestazioni per l'elaborazione delle regole attestazioni specifiche di tale fase. Queste fasi includono \(nell'ordine in cui sono presenti\):  
  
1.  Accettazione delle attestazioni in ingresso: questa fase della pipeline delle attestazioni viene usata per estrarre le attestazioni in ingresso dal token ed eliminare quelle non previste o non attendibili. Dopo l'estrazione, vengono eseguite le regole di accettazione che costituiscono il set di regole di trasformazione accettazione per un trust del provider di attestazioni. Queste regole possono essere usate per passare o aggiungere nuove attestazioni che possono quindi essere usate nelle fasi successive delle pipeline delle attestazioni. L'output di questa fase viene usato come input per la seconda e la terza fase.  
  
2.  Autorizzazione del richiedente delle attestazioni: questa fase viene usata dal motore delle attestazioni per rilasciare attestazioni che consentono o negano l'accesso a seconda che al richiedente di un token sia consentito ottenere o meno un token per la relying party specificata. Tuttavia, prima ancora, devono essere eseguite le regole di autorizzazione che costituiscono il set di regole di autorizzazione rilascio o il set di regole di autorizzazione di delega per un trust della relying party.  
  
3.  Rilascio di attestazioni in uscita: questa fase viene usata per rilasciare le attestazioni in uscita e inviarle lungo la pipeline dove verranno inserite in un token di sicurezza. Tuttavia, prima ancora, vengono eseguite le regole di rilascio che costituiscono il set di regole di trasformazione rilascio per un trust della relying party e viene così determinato quali attestazioni verranno rilasciate come attestazioni in uscita.  
  
Tutte e tre le fasi eseguono l'elaborazione delle regole di attestazione, ma usano un set di regole diverso. Come descritto in precedenza, ogni fase è associato un set di regole in base l'autorità emittente di attestazioni in ingresso \(le regole di accettazione\) o il servizio di destinazione per il quale vengono rilasciate le claimincludes \(le regole di autorizzazione e di rilascio\).  
  
Le attestazioni sono token\-indipendente ma che vengono trasmessi in rete incapsulata nei token di sicurezza. Le regole attestazioni vengono applicate alle attestazioni indipendentemente dal formato del token di sicurezza in ingresso o in uscita.  
  
Le regole attestazioni contengono l'amministratore\-la logica definita in base alla quale il motore delle attestazioni accetterà le attestazioni in ingresso, autorizzerà le attestazioni in base all'identità del richiedente ed emetterà le attestazioni necessarie per un relying party. Infine, è il motore delle attestazioni a determinare quali attestazioni entreranno nel token di sicurezza che verrà rilasciato al termine del flusso dell'attestazione nella pipeline delle attestazioni.  
  
Come illustrato nella figura seguente, la pipeline delle attestazioni è responsabile per l'intera entità finale\-a\-Termina processo di propagazione di un'attestazione le varie fasi della pipeline per finire con un'attestazione emessa che verrà inviata tramite un trust della relying party. L'attestazione in uscita nell'illustrazione rappresenta l'attestazione rilasciata.  
  
![Ruoli di AD FS](media/adfs2_pipeline.gif)  
  
Anche se non è mostrato nell'illustrazione, è il motore delle attestazioni a eseguire la vera e propria elaborazione delle regole in ogni fase. Per altre informazioni, vedere [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md).  
  

