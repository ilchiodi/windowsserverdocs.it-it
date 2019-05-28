---
title: Utilizzo di PowerShell nell'estensione
description: Uso di PowerShell nella propria estensione Windows Admin Center SDK (progetto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 05/09/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 7375732fd464519cd1533043d271065e488fd46a
ms.sourcegitcommit: 7cb939320fa2613b7582163a19727d7b77debe4b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/14/2019
ms.locfileid: "65621360"
---
# <a name="using-powershell-in-your-extension"></a>Utilizzo di PowerShell nell'estensione #

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

Apriamo più approfondita delle estensioni SDK di Windows Admin Center, esaminiamo l'aggiunta di comandi di PowerShell per l'estensione.

## <a name="powershell-in-typescript"></a>PowerShell in TypeScript ##

Il processo di compilazione gulp includa un passaggio di generazione che sarà eseguite eventuali ```{!ScriptName}.ps1``` che viene inserito nella ```\src\resources\scripts``` cartella e compilarle nel ```powershell-scripts``` classe sotto il ```\src\generated``` cartella.

>[!NOTE] 
> Non aggiornare manualmente il ```powershell-scripts.ts``` né il ```strings.ts``` file. Eventuali modifiche apportate verranno sovrascritte durante la generazione successiva.

## <a name="running-a-powershell-script"></a>Esecuzione di uno Script di PowerShell ##
Tutti gli script che si desidera eseguire in un nodo possono essere inseriti in ```\src\resources\scripts\{!ScriptName}.ps1```. 
>[!IMPORTANT] 
> Eventuali modifiche apportate un ```{!ScriptName}.ps1``` file non si rifletteranno nel progetto finché ```gulp generate``` è stato eseguito.

L'API funziona creando innanzitutto una sessione di PowerShell nei nodi sono destinate a, la creazione di script di PowerShell con tutti i parametri che devono essere passati e quindi eseguire lo script per le sessioni che sono state create.

Ad esempio, è necessario questo script ```\src\resources\scripts\Get-NodeName.ps1```:
``` ps1
Param
 (
    [String] $stringFormat
 )
 $nodeName = [string]::Format($stringFormat,$env:COMPUTERNAME)
 Write-Output $nodeName
```

Verrà creata una sessione di PowerShell per il nodo di destinazione:
``` ts
const session = this.appContextService.powerShell.createSession('{!TargetNode}'); 
```
Quindi si creerà lo script di PowerShell con un parametro di input:
```ts
const script = PowerShell.createScript(PowerShellScripts.Get_NodeName, {stringFormat: 'The name of the node is {0}!'});
```
Infine, è necessario eseguire lo script nella sessione che è stata creata:
``` ts
  public ngOnInit(): void {
    this.session = this.appContextService.powerShell.createAutomaticSession('{!TargetNode}');
  }

  public getNodeName(): Observable<any> {
    const script = PowerShell.createScript(PowerShellScripts.Get_NodeName.script, { stringFormat: 'The name of the node is {0}!'});
    return this.appContextService.powerShell.run(this.session, script)
    .pipe(
        map(
        response => {
            if (response && response.results) {
                return response.results;
            }
            return 'no response';
        }
      ) 
    );
  }

  public ngOnDestroy(): void {
    this.session.dispose()
  }

```
A questo punto è necessario effettuare la sottoscrizione per la funzione osservabile che appena creato. Inserire questa in cui è necessario chiamare la funzione per eseguire lo script di PowerShell:
```ts
this.getNodeName().subscribe(
     response => {
    console.log(response)
     }
);
```
Fornendo il nome del nodo per il metodo createSession, una nuova sessione di PowerShell viene creata, utilizzata e quindi viene eliminata immediatamente dopo il completamento della chiamata di PowerShell. 

### <a name="key-options"></a>Opzioni della chiave ###
Alcune opzioni sono disponibili quando si chiama l'API di PowerShell. Ogni volta che viene creata una sessione può essere creato con o senza una chiave. 

**Chiave:** In questo modo viene creata una sessione con chiave che può essere cercata e riutilizzata, persino tra componenti (ossia Component1 può creare una sessione con la chiave "SME-ROCKS" e Component2 può usare la stessa sessione). Se viene fornita una chiave, la sessione in cui viene creata deve essere eliminata da chiamata Dispose () come è stato fatto nell'esempio precedente. Una sessione non deve essere conservata senza essere eliminati per più di 5 minuti. 
```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNode}', '{!Key}');
```

**Senza chiave:** Verrà creata automaticamente una chiave per la sessione. In questa sessione con venga eliminato automaticamente dopo 3 minuti. L'uso senza chiave consente l'estensione per l'uso di qualsiasi spazio di esecuzione che è già disponibile al momento della creazione di una sessione di riciclo. Se non lo spazio di esecuzione è disponibile quanto verrà creato uno nuovo. Questa funzionalità è utile per le chiamate non standard, ma l'uso ripetuto può influire sulle prestazioni. Una sessione richiede circa 1 secondo per creare, quindi, in modo continuo il riciclo sessioni può provocare rallentamenti.

```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNodeName}');
```
oppure 
``` ts 
const session = this.appContextService.powerShell.createAutomaticSession('{!TargetNodeName}');
```
Nella maggior parte dei casi, creare una sessione con chiave nel ```ngOnInit()``` metodo e quindi eliminarla in ```ngOnDestroy()```. Seguire questo modello quando sono presenti più script di PowerShell in un componente, ma la sessione sottostante che non viene condivisa tra i componenti.
Per ottenere risultati ottimali, assicurarsi che la creazione della sessione viene gestito all'interno di componenti invece services: questo contribuisce a garantire tale durata e la pulitura può essere gestita correttamente.

Per ottenere risultati ottimali, assicurarsi che la creazione della sessione viene gestito all'interno di componenti invece services: questo contribuisce a garantire tale durata e la pulitura può essere gestita correttamente.

### <a name="powershell-stream"></a>PowerShell Stream ###
Se si dispone di una lunga esecuzione dello script e i dati. viene restituite la progressivamente, che un flusso di PowerShell consente di elaborare i dati senza dover attendere il fine dello script. Verrà chiamata l'osservabile Next (), non appena vengono ricevuti i dati.
```ts
this.appContextService.powerShellStream.run(session, script);
```

### <a name="long-running-scripts"></a>Script con esecuzione prolungata ###
Se si dispone di uno script di esecuzione prolungata che si desidera eseguire in background, è possibile inviare un elemento di lavoro. Lo stato dello script verrà rilevato tramite il Gateway e gli aggiornamenti per lo stato possono essere inviati a una notifica. 
```ts
const workItem: WorkItemSubmitRequest = {
    typeId: 'Long Running Script',
    objectName: 'My long running service',
    powerShellScript: script,
    
    //in progress notifications
    inProgressTitle: 'Executing long running request',
    startedMessage: 'The long running request has been started',
    progressMessage: 'Working on long running script – {{ percent }} %',

    //success notification
    successTitle: 'Successfully executed a long running script!',
    successMessage: '{{objectName}} was successful',
    successLinkText: 'Bing',
    successLink: 'http://www.bing.com',
    successLinkType: NotificationLinkType.Absolute,

    //error notification
    errorTitle: 'Failed to execute long running script',
    errorMessage: 'Error: {{ message }}'

    nodeRequestOptions: {
       logAudit: true,
       logTelemetry: true
    }
};

return this.appContextService.workItem.submit('{!TargetNode}', workItem);
```

>[!NOTE] 
> Per lo stato di avanzamento da visualizzare, Write-Progress deve essere incluso nello script che è stato scritto. Ad esempio: 
> ``` ps1
>  Write-Progress -Activity ‘The script is almost done!’ -percentComplete 95
>```

#### <a name="workitem-options"></a>Opzioni di elemento di lavoro ####

| function | Spiegazione |
| ----- | ----------- |
| submit() | Invia l'elemento di lavoro 
| submitAndWait() | Inviare l'elemento di lavoro e attendere il completamento della propria esecuzione.
| wait() | Attendere il lavoro esistente elemento per il completamento
| query() | Eseguire una query per un elemento di lavoro esistente tramite id
| find() | Trovare ed elemento di lavoro da TargetNodeName, ModuleName o typeId esistente.

### <a name="powershell-batch-apis"></a>API di Batch di PowerShell ###
Se è necessario eseguire lo stesso script in più nodi, può quindi essere utilizzata una sessione di PowerShell di batch. Ad esempio: 
```ts
const batchSession = this.appContextService.powerShell.createBatchSession(
    ['{!TargetNode1}', '{!TargetNode2}', sessionKey);
  this.appContextService.powerShell.runBatchSingleCommand(batchSession, command).subscribe((responses: PowerShellBatchResponseItem[]) => {
    for (const response of responses) { 
      if (response.error || response.errors) {
        //handle error
      } else {
        const results = response.properties && response.properties.results;
        //response.nodeName
        //results[0]
      }
    }
     },
     Error => { /* handle error */ });  

```


#### <a name="powershellbatch-options"></a>Opzioni PowerShellBatch ####
| Opzione | Spiegazione |
| ----- | ----------- |
| runSingleCommand | Eseguire un singolo comando su tutti i nodi nella matrice 
| run | Esecuzione del comando corrispondente nel nodo associato
| annulla | Annullare il comando su tutti i nodi nella matrice
