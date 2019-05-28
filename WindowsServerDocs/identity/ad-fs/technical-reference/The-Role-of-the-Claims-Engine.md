---
ms.assetid: 8b15d44e-e4e6-4510-aa91-cc7ec7161b0a
title: Ruolo del motore delle attestazioni
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b69277cdedd697605f57aa4cf7214f5b65bb2e81
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188470"
---
# <a name="the-role-of-the-claims-engine"></a>Ruolo del motore delle attestazioni
Al livello più alto, il motore delle attestazioni in Active Directory Federation Services \(ADFS\) è una regola\-basato su motore dedicato al servizio e l'elaborazione delle richieste di attestazione per il servizio federativo. Il motore delle attestazioni è l'unica entità del Servizio federativo responsabile dell'esecuzione di ogni set di regole in tutte le relazioni di trust federative configurate e della trasmissione del risultato di output alla pipeline delle attestazioni.  
  
Mentre la pipeline delle attestazioni è più un concetto logico della fine\-a\-Termina processo per il flusso di attestazioni, le regole sono un elemento amministrativo effettivo che è possibile utilizzare per personalizzare il flusso di attestazioni durante il processo di esecuzione delle regole attestazione di attestazione. Per ulteriori informazioni sul processo di pipeline, vedere [ruolo delle Pipeline delle attestazioni](The-Role-of-the-Claims-Pipeline.md).  
  
Come illustrato nella figura seguente, l'atto di accettazione di attestazioni in ingresso \(regole accettazione\), l'autorizzazione delle attestazioni richiedenti \(regole di autorizzazione\) e rilasciare le attestazioni in uscita \(regole rilascio\) tramite attestazione regole in tutte le relazioni di trust federativa nell'organizzazione viene eseguita dal motore delle attestazioni.  
  
![Ruoli di AD FS](media/adfs2_enginepipeline.gif)  
  
## <a name="claim-rules-execution-process"></a>Processo di esecuzione delle regole attestazioni  
Quando si configura un trust del provider di attestazioni o trust della relying party dell'organizzazione con regole attestazione, imposta la regola attestazione\(s\) per il trust agire come un custode dei attestazioni in ingresso richiamando il motore delle attestazioni per applicare la logica necessaria nelle regole attestazione per determinare se emettere le attestazioni e che le attestazioni per l'emissione.  
  
La sezione seguente illustra ogni passaggio eseguito dal motore durante il flusso delle attestazioni nel processo di esecuzione delle regole attestazioni. Ogni passaggio illustrato di seguito viene eseguito per ogni fase descritta nel processo della pipeline delle attestazioni. Questi passaggi includono:  
  
-   Passaggio 1: Inizializzazione  
  
-   Passaggio 2: Esecuzione  
  
-   Passaggio 3: Risultato dell'esecuzione  
  
Per ulteriori informazioni sul processo di pipeline, vedere [ruolo delle Pipeline delle attestazioni](The-Role-of-the-Claims-Pipeline.md).  
  
### <a name="step-1--initialization"></a>Passaggio 1: Inizializzazione  
Nel primo passaggio del processo di esecuzione delle regole attestazione il motore delle attestazioni accetta le attestazioni in ingresso aggiungendole innanzitutto al *set di attestazioni di input*. Un set di attestazioni di input è simile a una cache di memoria usata per archiviare temporaneamente i dati solo finché, nell'ambito di un processo obbligatorio, è necessario che i dati siano disponibili per il recupero. I dati del set di attestazioni di input vengono rimossi quando termina l'esecuzione delle regole.  
  
#### <a name="adding-a-claim-to-the-input-claim-set-for-a-rule-set"></a>Aggiunta di un'attestazione al set di attestazioni di input per un set di regole  
Il set di attestazioni di input viene creato dal motore delle attestazioni quando deve archiviare temporaneamente i dati delle attestazioni nella memoria mentre elabora la logica associata a un set di regole attestazioni. Il motore delle attestazioni copia tutte le attestazioni in ingresso nel set di attestazioni di input dove possono essere recuperate dalla prima regola del set di regole.  
  
Nell'illustrazione seguente, ad esempio, il motore delle attestazioni legge le attestazioni A e B dalle attestazioni in ingresso e le copia nel set di attestazioni di input. Una volta nel set di attestazioni di input, il motore delle attestazioni recupera ed elabora le attestazioni A e B come input per la logica nella prima regola del set di regole attestazioni.  
  
![Ruoli di AD FS](media/adfs2_context1.gif)  
  
Tutte le regole di un set di regole attestazioni condividono lo stesso set di attestazioni di input. Ogni regola di tale set può essere aggiunta al set di attestazioni di input condiviso, influendo così su tutte le regole successive nel set.  
  
### <a name="step-2--execution"></a>Passaggio 2: Esecuzione  
In questo passaggio del processo delle regole attestazioni le regole attestazioni vengono elaborate quando il motore delle attestazioni esegue una alla volta in ordine cronologico tutte le regole di un determinato set di regole. Ogni regola all'interno di un solo set di regole viene eseguita una volta e viene eseguita nell'ordine in cui vengono visualizzati dall'alto verso il basso visualizzato nella finestra di dialogo Modifica regole attestazione nello snap di gestione di ADFS\-in. La regola attestazioni all'inizio del set di regole viene elaborata per prima e quindi vengono elaborate le regole successive finché vengono eseguite tutte le regole.  
  
Come definito nel linguaggio delle regole attestazioni, una regola attestazioni è costituita da due parti, la condizione e l'istruzione di rilascio. Set di attestazioni processi prima di attestazioni motore set per determinare se la condizione specificata all'interno della regola vale per le attestazioni contenute nell'input di attestazioni parte condizione utilizzando i dati di input \(le attestazioni che corrispondono alla condizione della regola vengono definite come una corrispondenza attestazioni\). Se vengono trovate attestazioni corrispondenti, il motore delle attestazioni esegue l'istruzione di rilascio della regola per ogni set delle attestazioni corrispondenti. L'istruzione di rilascio della regola può eseguire una delle attività seguenti con le regole corrispondenti:  
  
1.  Copiare un'attestazione corrispondente nel set di attestazione di output  
  
2.  Trasformare i campi delle attestazioni e creare una nuova attestazione solo nel set di attestazioni di input oppure nei set di attestazioni sia di valutazione che di output.  
  
3.  Usare l'attestazione corrispondente\(s\) come chiave per cercare ulteriori informazioni da un archivio di attributi per creare una nuova attestazione\(s\) in set di attestazioni solo l'input o sia di input e output in set di attestazioni.  
  
#### <a name="adding-a-claim-to-the-output-claim-set-for-a-rule-set"></a>Aggiunta di un'attestazione al set di attestazioni di output per un set di regole  
Il *set di attestazioni di output* è una posizione nella memoria inizialmente vuota ed è importante perché il motore delle attestazioni restituirà solo le attestazioni presenti nel set di attestazioni di output al termine del processo di esecuzione. Per questo motivo le attestazioni presenti solo nel set di attestazioni di input e non nel set di attestazioni di output verranno ignorate al momento di calcolare il set finale di attestazioni in uscita.  
  
#### <a name="adding-a-claim-to-both-claim-sets-for-a-rule-set"></a>Aggiunta di un'attestazione a entrambi i set di attestazioni per un set di regole  
Quando una regola viene elaborata, vengono aggiunte attestazioni nel set di attestazioni di input oppure sia nel set di attestazioni di input che nel set di attestazioni di output in base all'istruzione usata nell'istruzione di rilascio della regola. Nel linguaggio delle regole attestazione queste istruzioni sono chiamate *add* o *issue*.  
  
Se viene usata l'istruzione *add* , le attestazioni vengono aggiunte solo al set di attestazioni di input e le attestazioni esisteranno solo ai fini dell'esecuzione e smetteranno di esistere al termine dell'esecuzione. Se viene usata l'istruzione *issue*, le attestazioni vengono aggiunte sia al set di attestazioni di input che al set di attestazioni di output e le attestazioni verranno restituite nel set di attestazioni di output al termine dell'esecuzione. Per ulteriori informazioni su queste istruzioni, vedere [il ruolo del linguaggio di regola attestazione](The-Role-of-the-Claim-Rule-Language.md).  
  
Se la parte della condizione di una regola in un set di regole non corrisponde ad alcuna attestazione del set di attestazioni di input, la parte dell'istruzione di rilascio della regola viene ignorata e quindi nessuna attestazione viene aggiunta al set di attestazioni di output né al set di attestazioni di input. La figura seguente e i passaggi corrispondenti mostrano che cosa accade quando il motore delle attestazioni esegue una regola di trasformazione:  
  
![Ruoli di AD FS](media/adfs2_context2.gif)  
  
1.  Le attestazioni in ingresso vengono aggiunte al set di attestazioni di input dal motore delle attestazioni.  
  
2.  La prima regola, quando viene eseguita, vede le attestazioni A e B, che in quel momento sono le uniche presenti nel set di attestazioni di input, ed elabora la parte condizionale della logica della regola 1.  
  
3.  Poiché l'attestazione di oggetto è presente nel set di attestazioni di input, la condizione della regola è considerata true \(corrispondenza l'attestazione A\) e viene aggiunta una nuova attestazione C set di attestazioni sia l'input e output di set di attestazioni.  
  
4.  Regola 2 ora è possibile utilizzare le attestazioni A, B e C \(set di attestazioni di tutte le attestazioni di input\) come input per la logica di elaborazione.  
  
Per altre informazioni sulla trasformazione delle attestazioni, vedere [When to Use a Transform Claim Rule](When-to-Use-a-Transform-Claim-Rule.md).  
  
### <a name="step-3--execution-result"></a>Passaggio 3: Risultato dell'esecuzione  
La fase finale dell'esecuzione del set di regole attestazioni inizia dopo che tutte le regole sono state eseguite in un determinato set di regole e il set di attestazioni finale è presente nel set di attestazioni di output. A questo punto, il motore delle attestazioni restituisce il contesto del set di attestazioni di output come output dell'esecuzione del set di regole. Da questo punto in poi è la pipeline delle attestazioni a prendere il controllo e a spostare questo output finale nella fase successiva del processo.  
  
## <a name="sending-the-execution-output-to-the-claims-pipeline"></a>Invio dell'output dell'esecuzione alla pipeline delle attestazioni  
Quando il motore delle attestazioni elabora un set di regole, per questo set sono disponibili posizioni dedicate nella memoria per i set di attestazioni di input e di output. Ciò significa che i set di attestazioni di input e di output usati da un set di regole sono distinti da quelli usati in un altro set di regole.  
  
Dopo l'intero processo di esecuzione per un set di regole consentono di \(i passaggi 1, 2 e 3\), appena rilasciate le attestazioni in uscita \(contenuto dell'output di set di attestazioni\) verrà utilizzato come input per il successiva set di regole in pipeline delle attestazioni. In questo modo le attestazioni possono passare dall'output di un set di regole all'input di un altro set di regole, come mostrato nella figura seguente.  
  
![Ruoli di AD FS](media/adfs2_enginecontexts.gif)  
  
> [!NOTE]  
> Anche il set di regole di rilascio è una fase cruciale della pipeline, tuttavia non è stato inserito nella figura precedente al solo scopo di semplificarla. Per un'illustrazione del set di regole di rilascio e di come si inserisca nella pipeline delle attestazioni, vedere [The Role of the Claims Pipeline](The-Role-of-the-Claims-Pipeline.md).  
  
In questo caso, l'output delle regole di accettazione viene usato dalla pipeline per il flusso del set di attestazioni finale generato dalle regole di accettazione verso la seconda fase nella pipeline, ovvero l'elaborazione delle regole di autorizzazione. A questo punto, l'intera richiesta il processo di esecuzione di regole \(i passaggi 1, 2 e 3 precedente\) nuovamente eseguito per il set di regole di autorizzazione. Questo ciclo continua fino a quando il rilascio dei set di regole \(la fase finale nella pipeline\) è stata completata.  
  
Le attestazioni in uscita finalizzate, dopo essere state restituite dal motore per il set di regole di rilascio, verranno inserite in un token SAML e il Servizio federativo invierà il token di nuovo al client.  
  
## <a name="processing-authorization-rules"></a>Elaborazione delle regole di autorizzazione  
Se il set di regole attestazione che viene eseguito durante il passaggio 2, il processo di esecuzione delle regole attestazione è costituito da regole di autorizzazione \(che hanno un input diverso e set di regole di accettazione o il rilascio di attestazioni di output\), quindi verranno eseguite le regole di autorizzazione per determinare se un token richiedente è autorizzato a ottenere un token di sicurezza per una determinata relying party dal servizio federativo basata sulle attestazioni del richiedente.  
  
L'obiettivo delle regole di autorizzazione è rilasciare un'attestazione per il consenso o il rifiuto a seconda che all'utente sia consentito o meno ottenere un token per la relying party specifica. Come illustrato nella figura seguente, l'output dell'esecuzione di autorizzazione viene utilizzato per determinare se il set di regole di rilascio viene eseguito o meno dalla pipeline, basato su presenza o meno dell'autorizzazione e\/o negare l'attestazione, ma l'output dell'esecuzione autorizzazione stessa non viene usato come input per il set di regole attestazione.  
  
![Ruoli di AD FS](media/adfs2_authorization.gif)  
  
Per altre informazioni sull'autorizzazione delle attestazioni, vedere [When to Use an Authorization Claim Rule](When-to-Use-an-Authorization-Claim-Rule.md).  
  

