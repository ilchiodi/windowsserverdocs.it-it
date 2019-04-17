---
title: Utilizzo di PowerShell nell'estensione
description: Utilizzo di PowerShell nell'estensione Windows Admin Center SDK (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: b1be4fe7639d913243cc28371dff9e98e0f5827e
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296723"
---
# Utilizzo di PowerShell nell'estensione #

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Soffermeremo più accurato in Windows Admin Center estensioni SDK - Esaminiamo sull'aggiunta di comandi di PowerShell per l'estensione.

## PowerShell in TypeScript ##

Il processo di compilazione gulp ha un passaggio genera che porterà qualsiasi ```{!ScriptName}.ps1``` che viene posizionato nel ```\src\resources\scripts``` cartella e compilarli nel ```powershell-scripts``` classe sotto il ```\src\generated``` cartella.

>[!NOTE] 
> Non aggiornare manualmente il ```powershell-scripts.ts``` né il ```strings.ts``` file. Qualsiasi modifica apportata verrà sovrascritta in genera successivo.

## Esecuzione di uno Script Powerhell ##
Tutti gli script che si desiderano eseguire su un nodo possono essere inseriti nel ```\src\resources\scripts\{!ScriptName}.ps1```. 
>[!IMPORTANT] 
> Eventuali modifiche apportate un ```{!ScriptName}.ps1``` file non vengono riflesse nel progetto a meno che non genera un 

L'API funziona creando innanzitutto una sessione di PowerShell nei nodi sono destinati, creazione lo script di PowerShell con i parametri che devono essere passato e quindi eseguire lo script in sessioni che sono state create.

Ad esempio, abbiamo questo script ```\src\resources\scripts\Get-NodeName.ps1```:
``` ps1
Param
 (
    [String] $stringFormat
 )
 $nodeName = [string]::Format($stringFormat,$env:COMPUTERNAME)
 Write-Output $nodeName
```

Creeremo una sessione di PowerShell per il nodo di destinazione:
``` ts
const session = this.appContextService.powerShell.createSession('{!TargetNode}'); 
```
Quindi creiamo lo script di PowerShell con un parametro di input:
```ts
const script = PowerShell.createScript(PowerShellScripts.Get_NodeName, {stringFormat: 'The name of the node is {0}!'});
```
Infine, è necessario eseguire lo script nella sessione che abbiamo creato:
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
Ora ti sarà necessario sottoscrivere la funzione osservabile che abbiamo appena creato. Inserisci questo in cui è necessario chiamare la funzione per eseguire lo script di PowerShell:
```ts
this.getNodeName().subscribe(
     response => {
    console.log(response)
     }
);
```
Fornendo il nome del nodo al metodo createSession, una nuova sessione di PowerShell è creata, usata e quindi viene eliminata immediatamente dopo il completamento della chiamata di PowerShell. 

### Opzioni chiave ###
Alcune opzioni sono disponibili quando si chiama l'API di PowerShell. Ogni volta che viene creata una sessione può essere creata con o senza una chiave. 

**Chiave:** Ciò crea una sessione con chiave che possa essere cercata e riutilizzata, anche su componenti (ovvero Component1 possono creare una sessione con chiave "PMI ROCCE" e Component2 usare tale sessione stesso). Se viene fornita una chiave, la sessione creato deve essere eliminata dal chiamante Dispose () come avveniva nell'esempio precedente. Una sessione non deve essere mantenuta senza essere eliminato per più di 5 minuti. 
```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNode}', '{!Key}');
```

**Senza chiave:** Verrà creata automaticamente una chiave per la sessione. In questa sessione con essere eliminati automaticamente dopo 3 minuti. Senza chiave consente l'estensione di riciclare l'uso di qualsiasi Runspace che è già disponibile al momento della creazione di una sessione. Se nessun Runspace è disponibile che verrà creato uno nuovo. Questa funzionalità è ottimale per le chiamate e occasionali, ma Usa ripetuto può influire sulle prestazioni. Una sessione richiede circa 1 secondo per creare, pertanto continuamente riciclo sessioni possa causare rallentamenti.

```ts
  const session = this.appContextService.powerShell.createSession('{!TargetNodeName}');
```
oppure 
``` ts 
const session = this.appContextService.powerShell.createAutomaticSession('{!TargetNodeName}');
```
Nella maggior parte dei casi, creare una sessione con chiave nel ```ngOnInit()``` metodo e quindi dispose di farlo in ```ngOnDestroy()```. Seguire questo modello quando sono presenti più script di PowerShell in un componente, ma la sessione sottostante che non viene condiviso tra i componenti.
Per ottenere risultati ottimali, assicurati di creazione della sessione è gestita all'interno di componenti anziché servizi - questo contribuisce a garantire tale durata e pulizia possa essere gestita correttamente.

Per ottenere risultati ottimali, assicurati di creazione della sessione è gestita all'interno di componenti anziché servizi - questo contribuisce a garantire tale durata e pulizia possa essere gestita correttamente.

### Flusso di PowerShell ###
Se hai un script e i dati di lunga output progressivamente, che consente di elaborare i dati senza dover attendere lo script completare un flusso di PowerShell. Viene chiamato il Random osservabile non appena vengono ricevuti i dati.
```ts
this.appContextService.powerShellStream.run(session, script);
```

### Tempo script in esecuzione ###
Se disponi di uno script a esecuzione prolungata che vorresti per l'esecuzione in background, è possibile inviare un elemento di lavoro. Lo stato dello script verrà rilevato dal Gateway e gli aggiornamenti per lo stato possono essere inviati a una notifica. 
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
> Per lo stato da visualizzare, lo stato di scrittura deve essere inclusi nello script che hai scritto. Ad esempio:
> ``` ps1
>  Write-Progress -Activity ‘The script is almost done!’ -percentComplete 95
>```

#### Opzioni di elementi di lavoro ####

| function | Spiegazione |
| ----- | ----------- |
| Submit) | Invia l'elemento di lavoro 
| submitAndWait() | Inviare l'elemento di lavoro e attendere il completamento dell'esecuzione
| Wait | Attendere il lavoro esistente elemento per completare
| query() | Eseguire una query per un elemento di lavoro esistente per id
| Find() | Trova ed elemento di lavoro per la TargetNodeName, ModuleName o typeId esistente.

### PowerShell Batch API # # #
Se hai bisogno di eseguire lo script stesso su più nodi, una sessione di PowerShell batch può essere usata. Ad esempio:
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


#### Opzioni PowerShellBatch ####
| opzione | Spiegazione |
| ----- | ----------- |
| runSingleCommand | Eseguire un singolo comando contro tutti i nodi nella matrice 
| Correre | Eseguire il comando corrispondente nel nodo associato
| Annulla | Annullare il comando su tutti i nodi nella matrice