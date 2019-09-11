---
title: Utilizzo di PowerShell nell'estensione
description: Uso di PowerShell nella propria estensione SDK di Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 05/09/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: c30f8a9b856db8250a16210931e6f8dd73c07aa7
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869611"
---
# <a name="using-powershell-in-your-extension"></a>Utilizzo di PowerShell nell'estensione #

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Per approfondire la conoscenza di Windows Admin Center Extensions SDK, è ora possibile aggiungere comandi di PowerShell all'estensione.

## <a name="powershell-in-typescript"></a>PowerShell in TypeScript ##

Il processo di compilazione Gulp include un passaggio di ```{!ScriptName}.ps1``` generazione che consentirà ```\src\resources\scripts``` di inserire tutti gli elementi presenti nella cartella ```powershell-scripts``` e di compilarli ```\src\generated``` nella classe nella cartella.

>[!NOTE] 
> Non aggiornare ```powershell-scripts.ts``` manualmente né i ```strings.ts``` file. Eventuali modifiche apportate verranno sovrascritte alla successiva generazione.

## <a name="running-a-powershell-script"></a>Esecuzione di uno script di PowerShell ##
Tutti gli script che si desidera eseguire in un nodo possono essere inseriti in ```\src\resources\scripts\{!ScriptName}.ps1```. 
>[!IMPORTANT] 
> Eventuali modifiche apportate ```{!ScriptName}.ps1``` in un file non verranno riflesse nel ```gulp generate``` progetto finché non è stato eseguito.

L'API funziona creando prima una sessione di PowerShell nei nodi di destinazione, creando lo script di PowerShell con tutti i parametri che devono essere passati e quindi eseguendo lo script nelle sessioni create.

Ad esempio, lo script ```\src\resources\scripts\Get-NodeName.ps1```è:
``` ps1
Param
 (
    [String] $stringFormat
 )
 $nodeName = [string]::Format($stringFormat,$env:COMPUTERNAME)
 Write-Output $nodeName
```

Si creerà una sessione di PowerShell per il nodo di destinazione:
``` ts
const session = this.appContextService.powerShell.createSession('{!TargetNode}'); 
```
Si creerà quindi lo script di PowerShell con un parametro di input:
```ts
const script = PowerShell.createScript(PowerShellScripts.Get_NodeName, {stringFormat: 'The name of the node is {0}!'});
```
Infine, è necessario eseguire lo script nella sessione creata:
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
A questo punto è necessario sottoscrivere la funzione osservabile appena creata. Posizionare questa posizione in cui è necessario chiamare la funzione per eseguire lo script di PowerShell:
```ts
this.getNodeName().subscribe(
     response => {
    console.log(response)
     }
);
```
Fornendo il nome del nodo al metodo createSession, viene creata una nuova sessione di PowerShell, usata e quindi eliminata immediatamente dopo il completamento della chiamata di PowerShell. 

### <a name="key-options"></a>Opzioni chiave ###
Sono disponibili alcune opzioni quando si chiama l'API di PowerShell. Ogni volta che viene creata una sessione, è possibile crearla con o senza una chiave. 

**Chiave** In questo modo viene creata una sessione con chiave che può essere cercata e riutilizzata anche tra i componenti, ovvero Component1 può creare una sessione con la chiave "PMI-ROCKs" e Component2 può utilizzare la stessa sessione. Se viene fornita una chiave, la sessione creata deve essere eliminata chiamando Dispose () come è stato fatto nell'esempio precedente. Una sessione non deve essere mantenuta senza essere eliminata per più di 5 minuti. 
```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNode}', '{!Key}');
```

**Keyless** Verrà creata automaticamente una chiave per la sessione. Questa sessione con viene eliminata automaticamente dopo 3 minuti. L'uso della chiave digitale consente all'estensione di riciclare l'uso di qualsiasi spazio già disponibile al momento della creazione di una sessione. Se non è disponibile alcun spazio, verrà creato un nuovo. Questa funzionalità è utile per le chiamate monouso, ma l'uso ripetuto può influire sulle prestazioni. Una sessione richiede circa 1 secondo per la creazione, quindi le sessioni di riciclo continuo possono causare rallentamenti.

```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNodeName}');
```
oppure 
``` ts 
const session = this.appContextService.powerShell.createAutomaticSession('{!TargetNodeName}');
```
Nella maggior parte dei casi, creare una sessione con ```ngOnInit()``` chiave nel metodo e quindi eliminarla in ```ngOnDestroy()```. Seguire questo modello quando sono presenti più script PowerShell in un componente, ma la sessione sottostante non è condivisa tra i componenti.
Per ottenere risultati ottimali, assicurarsi che la creazione della sessione sia gestita all'interno di componenti anziché servizi, in modo da garantire che la durata e la pulizia possano essere gestite correttamente.

Per ottenere risultati ottimali, assicurarsi che la creazione della sessione sia gestita all'interno di componenti anziché servizi, in modo da garantire che la durata e la pulizia possano essere gestite correttamente.

### <a name="powershell-stream"></a>Flusso di PowerShell ###
Se si dispone di uno script con esecuzione prolungata e i dati vengono generati progressivamente, un flusso di PowerShell consentirà di elaborare i dati senza dover attendere il completamento dello script. L'osservabile Next () verrà chiamata non appena vengono ricevuti i dati.
```ts
this.appContextService.powerShellStream.run(session, script);
```

### <a name="long-running-scripts"></a>Script con esecuzione prolungata ###
Se si dispone di uno script con esecuzione prolungata che si desidera eseguire in background, è possibile inviare un elemento di lavoro. Lo stato dello script verrà rilevato dal gateway e gli aggiornamenti allo stato possono essere inviati a una notifica. 
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
> Per visualizzare lo stato di avanzamento, Write-Progress deve essere incluso nello script che è stato scritto. Esempio:
> ``` ps1
>  Write-Progress -Activity ‘The script is almost done!' -percentComplete 95
>```

#### <a name="workitem-options"></a>Opzioni WorkItem ####

| function | Spiegazione |
| ----- | ----------- |
| Invia () | Invia l'elemento di lavoro 
| submitAndWait() | Inviare l'elemento di lavoro e attendere il completamento dell'esecuzione
| Wait () | Attendi il completamento dell'elemento di lavoro esistente
| query () | Eseguire una query per un elemento di lavoro esistente in base all'ID
| Find () | Trovare ed elemento di lavoro esistente da TargetNodeName, moduleName o typeId.

### <a name="powershell-batch-apis"></a>API batch di PowerShell ###
Se è necessario eseguire lo stesso script in più nodi, è possibile usare una sessione di PowerShell per batch. Esempio:
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


#### <a name="powershellbatch-options"></a>Opzioni di PowerShellBatch ####
| Opzione | Spiegazione |
| ----- | ----------- |
| runSingleCommand | Eseguire un singolo comando su tutti i nodi della matrice 
| Correre | Eseguire il comando corrispondente nel nodo associato
| annulla | Annulla il comando in tutti i nodi della matrice
