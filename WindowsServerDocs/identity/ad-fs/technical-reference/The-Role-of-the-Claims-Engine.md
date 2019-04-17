---
ms.assetid: 8b15d44e-e4e6-4510-aa91-cc7ec7161b0a
title: Il ruolo del motore di attestazioni
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 930b6f8034f17d8902104419042f944b82e90b4f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-the-claims-engine"></a>Il ruolo del motore di attestazioni
Al livello più alto, il motore delle attestazioni in Active Directory Federation Services \(AD FS\) è un motore basato su rule\ è dedicato alla gestione e l'elaborazione delle richieste di attestazione per il servizio federativo. Il motore delle attestazioni è l'unica entità del servizio federativo che è responsabile dell'esecuzione di ogni set di regole in tutte le relazioni di trust federative configurate e consegna il risultato di output alla pipeline delle attestazioni.  
  
Mentre la pipeline delle attestazioni è soprattutto un concetto logico del processo end-to-end per il flusso di attestazioni, regole attestazione sono un elemento amministrativo effettivo che è possibile utilizzare per personalizzare il flusso delle attestazioni durante il processo di esecuzione delle regole attestazione. Per ulteriori informazioni sul processo della pipeline, vedere [ruolo delle Pipeline delle attestazioni](The-Role-of-the-Claims-Pipeline.md).  
  
Come illustrato nella figura seguente, l'atto di accettazione \(acceptance rules\) attestazioni in ingresso, autorizzazione delle attestazioni richiedenti \(authorization rules\) e rilasciare le attestazioni in uscita \(issuance rules\) tramite attestazione regole in tutte le relazioni di trust federative nell'organizzazione viene eseguita dal motore delle attestazioni.  
  
![Ruoli di AD FS](media/adfs2_enginepipeline.gif)  
  
## <a name="claim-rules-execution-process"></a>Processo di esecuzione delle regole attestazioni  
Quando si configura un trust del provider di attestazioni o relying party trust nell'organizzazione con regole attestazione, il set\(s\) di regola attestazione per tale act trust come un custode dei attestazioni in ingresso richiamando il motore delle attestazioni per applicare la logica necessaria nelle regole attestazione per determinare se emettere le attestazioni e le attestazioni da rilasciare.  
  
Nella sezione seguente illustra ogni passaggio eseguito dal motore durante il flusso di attestazioni attraverso il processo di esecuzione delle regole attestazione. Ogni passaggio illustrato di seguito si verifica per ogni fase descritta nel processo della pipeline delle attestazioni. Tali passaggi includono:  
  
-   Passaggio 1: inizializzazione  
  
-   Passaggio 2: esecuzione  
  
-   Passaggio 3: risultato dell'esecuzione  
  
Per ulteriori informazioni sul processo della pipeline, vedere [ruolo delle Pipeline delle attestazioni](The-Role-of-the-Claims-Pipeline.md).  
  
### <a name="step-1--initialization"></a>Passaggio 1: inizializzazione  
Nel primo passaggio del processo di esecuzione di regole attestazione, il motore delle attestazioni accetta le attestazioni in ingresso aggiungendole innanzitutto per il *set di attestazioni di input*. Un set di attestazioni di input è simile a una cache in memoria che viene utilizzata per archiviare temporaneamente i dati solo come un processo obbligatorio richiede di essere reso disponibile per il recupero dei dati. Il set di attestazioni di input vengono eliminati al termine dell'esecuzione della regola.  
  
#### <a name="adding-a-claim-to-the-input-claim-set-for-a-rule-set"></a>Aggiunta di un'attestazione al set per un set di regole di attestazioni di input  
Il set di attestazioni di input viene creato dal motore delle attestazioni quando deve archiviare temporaneamente i dati delle attestazioni nella memoria mentre elabora la logica associata a un set di regole attestazione. Il motore delle attestazioni copia tutte le attestazioni in ingresso per il set di attestazioni di input in cui possono essere recuperate dalla prima regola nel set di regole.  
  
Nell'illustrazione seguente, ad esempio, il motore delle attestazioni legge le attestazioni A e B in ingresso di attestazioni e li copia al set di attestazioni di input. Dopo che sono nel set di attestazioni di input, il motore delle attestazioni recupera ed elabora attestazioni A e B come input per la logica nella prima regola nel set di regole attestazione.  
  
![Ruoli di AD FS](media/adfs2_context1.gif)  
  
Tutte le regole in un set di regole attestazioni condividono lo stesso set di attestazioni di input. Ogni regola di tale set può aggiungere al set di attestazioni di input condiviso, influendo così su tutte le regole successive nel set.  
  
### <a name="step-2--execution"></a>Passaggio 2: esecuzione  
In questo passaggio del processo di regole attestazione regole attestazione vengono elaborate quando il motore delle attestazioni in ordine cronologico tutte le regole all'interno di un set di regole particolare uno alla volta. Ogni regola all'interno di un set di regole solo viene eseguita una volta e viene eseguito nell'ordine in cui vengono visualizzati dall'alto verso il basso visualizzato nella finestra di dialogo Modifica regole attestazione nella gestione di AD FS snap-in. La regola attestazione impostato nella parte superiore della regola viene elaborata per prima e quindi le regole successive vengono elaborate fino a quando non sono state eseguite tutte le regole.  
  
Come definito nel linguaggio di regola attestazione, una regola attestazioni è costituito da due parti, la condizione e rilascio istruzione. Set di attestazioni processi primo l'attestazioni motore set per determinare se la condizione specificata all'interno della regola vale per le attestazioni contenute nell'input di attestazioni parte condizione utilizzando i dati di input \ (le attestazioni che corrispondono alla condizione della regola vengono definite come una corrispondenza claims\). Se vengono trovate attestazioni corrispondenti, il motore delle attestazioni esegue l'istruzione di rilascio della regola per ogni set di attestazioni corrispondenti. L'istruzione di rilascio della regola può eseguire una delle seguenti operazioni con le attestazioni corrispondenti:  
  
1.  Copiare un'attestazione corrispondente nel set di attestazioni di output  
  
2.  Trasformare i campi delle attestazioni e creare una nuova attestazione in uno solo il set di attestazioni di input o set di attestazioni sia di valutazione e output.  
  
3.  Utilizzare il claim\(s\) corrispondenti come chiave per cercare altre informazioni da un archivio di attributi per creare nuove claim\(s\) in uno solo il set di attestazioni di input o set di attestazioni sia l'input e output.  
  
#### <a name="adding-a-claim-to-the-output-claim-set-for-a-rule-set"></a>Aggiunta di un'attestazione al set di un set di regole di attestazioni di output  
Il *set di attestazioni di output* è una posizione nella memoria inizialmente vuota ed è importante perché il motore delle attestazioni restituirà solo le attestazioni presenti nel set dopo aver completato il processo di esecuzione di attestazioni di output. Ciò significa che le attestazioni presenti solo nelle attestazioni di input impostano e non nel set di attestazioni di output verranno ignorate al momento di calcolare il set finale di attestazioni in uscita.  
  
#### <a name="adding-a-claim-to-both-claim-sets-for-a-rule-set"></a>Aggiunta di un'attestazione a entrambi i set di attestazioni per un set di regole  
Durante l'elaborazione di una regola attestazioni vengono aggiunte di input set di attestazioni o set di attestazioni di input e output di set di attestazioni in base all'istruzione usata nell'istruzione di rilascio della regola. Il linguaggio di regola attestazione fa riferimento a queste istruzioni sono chiamate *aggiungere* o *problema*.  
  
Se il *aggiungere* istruzione viene utilizzata, le attestazioni vengono aggiunte al proprio set di attestazioni di input e le attestazioni esisteranno solo ai fini dell'esecuzione e smetteranno di esistere al termine dell'esecuzione. Se il *problema* istruzione viene utilizzata, le attestazioni vengono aggiunte sia al set di attestazioni di input e set di attestazioni di output e le attestazioni verranno restituite nel set al termine dell'esecuzione di attestazioni di output. Per ulteriori informazioni su queste istruzioni, vedere [il ruolo del linguaggio di regola attestazione di](The-Role-of-the-Claim-Rule-Language.md).  
  
Se la parte della condizione di una regola all'interno di un set di regole non corrisponde a qualsiasi attestazioni nel set di attestazioni di input, la parte di istruzione di rilascio della regola viene ignorata e quindi nessuna attestazione viene aggiunta a uno dei due set di attestazioni di output o al set di attestazioni di input. La figura seguente e i passaggi corrispondenti mostrano che cosa accade quando il motore delle attestazioni esegue una regola di trasformazione:  
  
![Ruoli di AD FS](media/adfs2_context2.gif)  
  
1.  Le attestazioni in ingresso vengono aggiunti al set dal motore delle attestazioni di attestazioni di input.  
  
2.  Quando viene eseguita la prima regola, le attestazioni A e B, che sono in quel momento che le attestazioni di input sole set di attestazioni, ed elabora la parte condizionale della logica della regola 1.  
  
3.  Poiché l'attestazione è presente nel set di attestazioni di input, la condizione della regola è considerata true \ (corrispondente all'attestazione A\) e viene aggiunta una nuova attestazione C set di attestazioni sia l'input e set di attestazioni di output.  
  
4.  Regola 2 ora è possibile utilizzare le attestazioni A, B e C \ (set di attestazioni di tutte le attestazioni di input) come input per la logica di elaborazione.  
  
Per ulteriori informazioni sulla trasformazione delle attestazioni, vedere [When to Use a Transform Claim Rule](When-to-Use-a-Transform-Claim-Rule.md).  
  
### <a name="step-3--execution-result"></a>Passaggio 3: risultato dell'esecuzione  
La fase finale dell'esecuzione del set di regola attestazione inizia dopo che tutte le regole sono state eseguite in un determinato set di regole e set di attestazioni finale è presente nel set di attestazioni di output. A questo punto, il motore delle attestazioni restituisce il contesto del set come l'output dell'esecuzione del set di regole di attestazioni di output. Da questo punto in poi è la pipeline delle attestazioni a prendere il controllo e Sposta questo output finale nella fase successiva del processo.  
  
## <a name="sending-the-execution-output-to-the-claims-pipeline"></a>Invia l'output dell'esecuzione alla pipeline delle attestazioni  
Quando il motore delle attestazioni processi una set di regole, che dispone di set di regole disponibili posizioni dedicate nella memoria per l'input e i set di attestazioni di output. Ciò significa che l'input e output attestazione set utilizzato da un set di regole sono separate dall'input e quelli usati in un altro set di regole attestazioni di output.  
  
Dopo l'intero processo di esecuzione per un determinato set di regole \ (passaggi 1, 2 e 3 \), le attestazioni in uscita appena rilasciate \ (contenuto del set di attestazioni di output) verrà usato come input per il successiva set di regole in pipeline delle attestazioni. In questo modo le attestazioni possono passare dall'output di un set di regole per l'input per un altro set di regole, come illustrato nella figura seguente.  
  
![Ruoli di AD FS](media/adfs2_enginecontexts.gif)  
  
> [!NOTE]  
> Anche se il set di regole di rilascio è anche una fase critica nella pipeline, figura sopra riportata non viene visualizzato il solo scopo di semplificarla. Per un'illustrazione del set di regole di rilascio e come si inserisca nella pipeline delle attestazioni, vedere [ruolo delle Pipeline delle attestazioni](The-Role-of-the-Claims-Pipeline.md).  
  
In questo caso, l'output delle regole di accettazione viene usato dalla pipeline di flusso del set di attestazioni generati da regole di accettazione verso la seconda fase nella pipeline, ovvero l'elaborazione delle regole di autorizzazione finale. A questo punto, l'intera richiesta il processo di esecuzione delle regole \ (passaggi 1, 2 e 3 above\) verrà eseguito nuovamente per il set di regole di autorizzazione. Questo ciclo continua fino a quando il rilascio set di regole \ (la fase finale nel pipeline\) è stata completata.  
  
Una volta le attestazioni in uscita finalizzate, dopo essere state restituite dal motore per il set di regole di rilascio, verranno inserite in un token SAML e il servizio federativo invierà il token al client.  
  
## <a name="processing-authorization-rules"></a>L'elaborazione delle regole di autorizzazione  
Se il set di regole attestazione che viene eseguito durante il passaggio 2 del processo di esecuzione di regole attestazioni è costituito da regole di autorizzazione \ (che hanno un input diverso e set di rules\ accettazione o il rilascio di attestazioni di output), quindi verranno eseguito regole di autorizzazione per determinare se un token richiedente è autorizzato a ottenere un token di sicurezza per una determinata relying party dal servizio federativo basata sulle attestazioni del richiedente.  
  
L'obiettivo delle regole di autorizzazione è rilasciare consentire o negare l'attestazione basata sul fatto che l'utente sia consentito per ottenere un token per la relying party specificata o non. Come illustrato nella figura seguente, l'output dell'esecuzione dell'autorizzazione viene usato dalla pipeline per determinare se il set di regole di rilascio viene eseguito oppure No, basato su presenza o assenza di and\ il permesso di / o negare l'attestazione, ma l'output dell'esecuzione autorizzazione stessa non viene usato come input per il set di regole attestazione.  
  
![Ruoli di AD FS](media/adfs2_authorization.gif)  
  
Per ulteriori informazioni sull'autorizzazione delle attestazioni, vedere [When to Use an Authorization Claim Rule](When-to-Use-an-Authorization-Claim-Rule.md).  
  

