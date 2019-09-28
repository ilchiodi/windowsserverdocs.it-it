---
title: Creazione di un provider di connessione per un'estensione della soluzione
description: Sviluppare un'estensione di soluzione Windows Admin Center SDK (Project Honolulu)-creare un provider di connessione
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 9c04db3196d1e806e50af9164b3c8bcdfb19b079
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406884"
---
# <a name="create-a-connection-provider-for-a-solution-extension"></a>Creazione di un provider di connessione per un'estensione della soluzione

>Si applica a: Windows Admin Center, Windows Admin Center Preview

I provider di connessione svolgono un ruolo importante nel modo in cui l'interfaccia di amministrazione di Windows definisce e comunica con oggetti o destinazioni collegabili. In primo luogo, un provider di connessione esegue azioni durante la connessione, ad esempio per garantire che la destinazione sia online e disponibile e che l'utente che si connette disponga dell'autorizzazione per accedere alla destinazione.

Per impostazione predefinita, il centro di amministrazione di Windows viene fornito con i provider di connessione seguenti:

* Server
* Client Windows
* Cluster di failover
* Cluster HCI

Per creare un provider di connessione personalizzato, attenersi alla procedura seguente:

* Aggiungere i dettagli del provider di connessione a```manifest.json```
* Definire il provider di stato della connessione
* Implementare il provider di connessione nel livello applicazione

## <a name="add-connection-provider-details-to-manifestjson"></a>Aggiungere i dettagli del provider di connessione a manifest. JSON

Verranno ora illustrate le informazioni necessarie per definire un provider di connessione nel ```manifest.json``` file del progetto.

### <a name="create-entry-in-manifestjson"></a>Creare una voce in manifest. JSON

Il ```manifest.json``` file si trova nella cartella \src e contiene, tra le altre cose, le definizioni dei punti di ingresso nel progetto. I tipi di punti di ingresso includono strumenti, soluzioni e provider di connessioni. Verrà definito un provider di connessione.

Di seguito è riportato un esempio di una voce del provider di connessione in manifest. JSON:

``` json
    {
      "entryPointType": "connectionProvider",
      "name": "addServer",
      "path": "/add",
      "displayName": "resources:strings:addServer_displayName",
      "icon": "sme-icon:icon-win-server",
      "description": "resources:strings:description",
      "connectionType": "msft.sme.connection-type.server",
      "connectionTypeName": "resources:strings:addServer_connectionTypeName",
      "connectionTypeUrlName": "server",
      "connectionTypeDefaultSolution": "msft.sme.server-manager!servers",
      "connectionTypeDefaultTool": "msft.sme.server-manager!overview",
      "connectionStatusProvider": {
        "powerShell": {
          "script": "## Get-My-Status ##\nfunction Get-Status()\n{\n# A function like this would be where logic would exist to identify if a node is connectable.\n$status = @{label = $null; type = 0; details = $null; }\n$caption = \"MyConstCaption\"\n$productType = \"MyProductType\"\n# A result object needs to conform to the following object structure to be interpreted properly by the Windows Admin Center shell.\n$result = @{ status = $status; caption = $caption; productType = $productType; version = $version }\n# DO FANCY LOGIC #\n# Once the logic is complete, the following fields need to be populated:\n$status.label = \"Display Thing\"\n$status.type = 0 # This value needs to conform to the LiveConnectionStatusType enum. >= 3 represents a failure.\n$status.details = \"success stuff\"\nreturn $result}\nGet-Status"
        },
        "displayValueMap": {
          "wmfMissing-label": "resources:strings:addServer_status_wmfMissing_label",
          "wmfMissing-details": "resources:strings:addServer_status_wmfMissing_details",
          "unsupported-label": "resources:strings:addServer_status_unsupported_label",
          "unsupported-details": "resources:strings:addServer_status_unsupported_details"
        }
      }
    },
```

Un punto di ingresso di tipo "connnectionProvider" indica alla shell dell'interfaccia di amministrazione di Windows che l'elemento in fase di configurazione è un provider che verrà usato da una soluzione per convalidare uno stato di connessione. I punti di ingresso del provider di connessioni contengono alcune proprietà importanti, definite di seguito:

| Proprietà | Descrizione |
| -------- | ----------- |
| entryPointType | Si tratta di una proprietà obbligatoria. Sono disponibili tre valori validi: "strumento", "soluzione" e "la". | 
| name | Identifica il provider di connessioni nell'ambito di una soluzione. Questo valore deve essere univoco all'interno di un'istanza completa dell'interfaccia di amministrazione di Windows (non solo una soluzione). |
| path | Rappresenta il percorso URL per l'interfaccia utente "Aggiungi connessione", se verrà configurato dalla soluzione. Questo valore deve essere mappato a una route configurata nel file di routing app. TS. Quando il punto di ingresso della soluzione è configurato per l'uso delle connessioni rootNavigationBehavior, questa route caricherà il modulo usato dalla Shell per visualizzare l'interfaccia utente di aggiunta connessione. Ulteriori informazioni sono disponibili nella sezione relativa a rootNavigationBehavior. |
| displayName | Il valore immesso qui viene visualizzato sul lato destro della shell, sotto la barra di amministrazione di Windows nera quando un utente carica la pagina delle connessioni della soluzione. |
| icona | Rappresenta l'icona utilizzata nel menu a discesa soluzioni per rappresentare la soluzione. |
| description | Immettere una breve descrizione del punto di ingresso. |
| connectionType | Rappresenta il tipo di connessione che il provider caricherà. Il valore immesso qui verrà usato anche nel punto di ingresso della soluzione per specificare che la soluzione può caricare tali connessioni. Il valore immesso qui verrà usato anche nei punti di ingresso degli strumenti per indicare che lo strumento è compatibile con questo tipo. Questo valore immesso qui verrà usato anche nell'oggetto connessione inviato alla chiamata RPC sulla "finestra Aggiungi" nel passaggio di implementazione del livello dell'applicazione. |
| connectionTypeName | Utilizzato nella tabella Connections per rappresentare una connessione che utilizza il provider di connessione. Questo dovrebbe essere il nome plurale del tipo. |
| connectionTypeUrlName | Usato per la creazione dell'URL per rappresentare la soluzione caricata, dopo che l'interfaccia di amministrazione di Windows è connessa a un'istanza di. Questa voce viene utilizzata dopo le connessioni e prima della destinazione. In questo esempio, "connectionexample" è la posizione in cui questo valore viene visualizzato nell'URL:`http://localhost:6516/solutionexample/connections/connectionexample/con-fake1.corp.contoso.com` |
| connectionTypeDefaultSolution | Rappresenta il componente predefinito che deve essere caricato dal provider di connessione. Questo valore è una combinazione di: <br>[a] nome del pacchetto di estensione definito nella parte superiore del manifesto; <br>[b] punto esclamativo (!); <br>[c] nome del punto di ingresso della soluzione.    <br>Per un progetto denominato "MSFT. SME. sottosample-Extension" e un punto di ingresso della soluzione denominato "example", questo valore sarà "MSFT. SME. solutionExample-Extension! example". |
| connectionTypeDefaultTool | Rappresenta lo strumento predefinito da caricare in caso di connessione riuscita. Il valore di questa proprietà è costituito da due parti, simili a connectionTypeDefaultSolution. Questo valore è una combinazione di: <br>[a] nome del pacchetto di estensione definito nella parte superiore del manifesto; <br>[b] punto esclamativo (!); <br>[c] nome del punto di ingresso dello strumento che deve essere caricato inizialmente. <br>Per un progetto denominato "MSFT. SME. solutionExample-Extension" e un punto di ingresso della soluzione denominato "example", questo valore sarà "MSFT. SME. solutionExample-Extension! example". |
| connectionStatusProvider | Vedere la sezione "definire il provider di stato della connessione" |

## <a name="define-connection-status-provider"></a>Definire il provider di stato della connessione

Il provider di stato della connessione è il meccanismo mediante il quale una destinazione viene convalidata per essere online e disponibile, assicurando anche che l'utente che si connette disponga delle autorizzazioni per accedere alla destinazione. Attualmente esistono due tipi di provider di stato della connessione:  PowerShell e RelativeGatewayUrl.

*   <strong>Provider di stato della connessione PowerShell</strong> : determina se una destinazione è online e accessibile con uno script di PowerShell. Il risultato deve essere restituito in un oggetto con una sola proprietà "status", definita di seguito.
*   <strong>Provider stato connessione RelativeGatewayUrl</strong> : determina se una destinazione è online e accessibile con una chiamata REST. Il risultato deve essere restituito in un oggetto con una sola proprietà "status", definita di seguito.

### <a name="define-status"></a>Definire lo stato

Per restituire un oggetto con una singola proprietà ```status``` conforme al formato seguente sono necessari provider di stato di connessione:

``` json
{
    status: {
        label: string;
        type: int;
        details: string;
    }
}
```

Proprietà stato:

* <strong>Label</strong> : etichetta che descrive il tipo restituito dello stato. Si noti che i valori per Label possono essere mappati in fase di esecuzione. Vedere la voce seguente per il mapping dei valori in fase di esecuzione.

* <strong>Type</strong> : tipo restituito di stato. Il tipo ha i seguenti valori di enumerazione. Per qualsiasi valore 2 o superiore, la piattaforma non passerà all'oggetto connesso e verrà visualizzato un errore nell'interfaccia utente.

   Tipi

  | Value | Descrizione |
  | ----- | ----------- |
  | 0 | Online |
  | 1 | Avviso |
  | 2 | Non autorizzato |
  | 3 | Tipi di errore |
  | 4 | Irreversibile |
  | 5 | Sconosciuta |

* <strong>Dettagli</strong> : dettagli aggiuntivi che descrivono il tipo restituito dello stato.

### <a name="powershell-connection-status-provider-script"></a>Script del provider di stato della connessione PowerShell

Lo script di PowerShell del provider di stato della connessione determina se una destinazione è online e accessibile con uno script di PowerShell. Il risultato deve essere restituito in un oggetto con una sola proprietà "status". Di seguito è riportato uno script di esempio.

Esempio di script di PowerShell:

```PowerShell
## Get-My-Status ##

function Get-Status()
{
    # A function like this would be where logic would exist to identify if a node is connectable.
    $status = @{label = $null; type = 0; details = $null; }
    $caption = "MyConstCaption"
    $productType = "MyProductType"

    # A result object needs to conform to the following object structure to be interperated properly by the Windows Admin Center shell.
    $result = @{ status = $status; caption = $caption; productType = $productType; version = $version }

    # DO FANCY LOGIC #

    # Once the logic is complete, the following fields need to be populated:
    $status.label = "Display Thing"
    $status.type = 0 # This value needs to conform to the LiveConnectionStatusType enum. >= 3 represents a failure.
    $status.details = "success stuff"

    return $result
}

Get-Status
```

### <a name="define-relativegatewayurl-connection-status-provider-method"></a>Definire il metodo del provider di stato della connessione RelativeGatewayUrl

Il metodo del provider ```RelativeGatewayUrl``` di stato della connessione chiama un'API REST per determinare se una destinazione è online e accessibile. Il risultato deve essere restituito in un oggetto con una sola proprietà "status". Di seguito è riportata una voce del provider di connessione di esempio in manifest. JSON di un RelativeGatewayUrl.

``` json
    {
      "entryPointType": "connectionProvider",
      "name": "addServer",
      "path": "/add/server",
      "displayName": "resources:strings:addServer_displayName",
      "icon": "sme-icon:icon-win-server",
      "description": "resources:strings:description",
      "connectionType": "msft.sme.connection-type.server",
      "connectionTypeName": "resources:strings:addServer_connectionTypeName",
      "connectionTypeUrlName": "server",
      "connectionTypeDefaultSolution": "msft.sme.server-manager!servers",
      "connectionTypeDefaultTool": "msft.sme.server-manager!overview",
      "connectionStatusProvider": {
        "relativeGatewayUrl": "<URL here post /api>",
        "displayValueMap": {
          "wmfMissing-label": "resources:strings:addServer_status_wmfMissing_label",
          "wmfMissing-details": "resources:strings:addServer_status_wmfMissing_details",
          "unsupported-label": "resources:strings:addServer_status_unsupported_label",
          "unsupported-details": "resources:strings:addServer_status_unsupported_details"
        }
      }
    },
```

Note sull'uso di RelativeGatewayUrl:

* "relativeGatewayUrl" specifica dove ottenere lo stato di connessione da un URL del gateway. Questo URI è relativo rispetto a/API. Se $connectionName viene trovato nell'URL, verrà sostituito con il nome della connessione.
* Tutte le proprietà di relativeGatewayUrl devono essere eseguite sul gateway host, operazione che può essere eseguita creando un'estensione del gateway

### <a name="map-values-in-runtime"></a>Eseguire il mapping dei valori in fase di esecuzione

I valori di etichetta e dettagli nell'oggetto restituito di stato possono essere formattati in fase di ottimizzazione includendo chiavi e valori nella proprietà "defaultValueMap" del provider.

Se ad esempio si aggiunge il valore riportato di seguito, ogni volta che "defaultConnection_test" viene indicato come valore per etichetta o dettagli, l'interfaccia di amministrazione di Windows sostituirà automaticamente la chiave con il valore della stringa di risorsa configurata.

``` json
    "defaultConnection_test": "resources:strings:addServer_status_defaultConnection_label"
```

## <a name="implement-connection-provider-in-application-layer"></a>Implementare il provider di connessione nel livello applicazione

Ora verrà implementato il provider di connessione nel livello applicazione creando una classe TypeScript che implementa OnInit. La classe dispone delle funzioni seguenti:

| Funzione | Descrizione |
| -------- | ----------- |
| Costruttore (appContextService privato: AppContextService, Route privata: ActivatedRoute) |  |
| public ngOnInit () |  |
| onsubmit pubblico () | Contiene la logica per aggiornare la shell quando viene eseguito un tentativo di aggiunta della connessione |
| OnCancel pubblico () | Contiene la logica per aggiornare la shell quando viene annullato un tentativo di aggiunta della connessione |

### <a name="define-onsubmit"></a>Definisci OnSubmit

```onSubmit```Invia una chiamata RPC al contesto dell'app per notificare alla shell una "aggiunta di una connessione". La chiamata di base USA "updateData" come segue:

``` ts
this.appContextService.rpc.updateData(
    EnvironmentModule.nameOfShell,
    '##',
    <RpcUpdateData>{
        results: {
            connections: connections,
            credentials: this.useCredentials ? this.creds : null
        }
    }
);
```

Il risultato è una proprietà di connessione, ovvero una matrice di oggetti conformi alla struttura seguente:

``` ts

/**
 * The connection attributes class.
 */
export interface ConnectionAttribute {

    /**
     * The id string of this attribute
     */
    id: string;

    /**
     * The value of the attribute. used for attributes that can have variable values such as Operating System
     */
    value?: string | number;
}

/**
 * The connection class.
 */
export interface Connection {

    /**
     * The id of the connection, this is unique per connection
     */
    id: string;

    /**
     * The type of connection
     */
    type: string;

    /**
     * The name of the connection, this is unique per connection type
     */
    name: string;

    /**
     * The property bag of the connection
     */
    properties?: ConnectionProperties;

    /**
     * The ids of attributes identified for this connection
     */
    attributes?: ConnectionAttribute[];

    /**
     * The tags the user(s) have assigned to this connection
     */
    tags?: string[];
}

/**
 * Defines connection type strings known by core
 * Be careful that these strings match what is defined by the manifest of @msft-sme/server-manager
 */
export const connectionTypeConstants = {
    server: 'msft.sme.connection-type.server',
    cluster: 'msft.sme.connection-type.cluster',
    hyperConvergedCluster: 'msft.sme.connection-type.hyper-converged-cluster',
    windowsClient: 'msft.sme.connection-type.windows-client',
    clusterNodesProperty: 'nodes'
};
```

### <a name="define-oncancel"></a>Definisci OnCancel

```onCancel```Annulla un tentativo di "aggiunta connessione" passando una matrice di connessioni vuote:

``` ts
this.appContextService.rpc.updateData(EnvironmentModule.nameOfShell, '##', <RpcUpdateData>{ results: { connections: [] } });
```

## <a name="connection-provider-example"></a>Esempio di provider di connessione

Di seguito è riportata la classe TypeScript completa per l'implementazione di un provider di connessione. Si noti che la stringa "connectionType" corrisponde a "connectionType come definito nel provider di connessioni in manifest. JSON.

``` ts
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';
import { AppContextService } from '@msft-sme/shell/angular';
import { Connection, ConnectionUtility } from '@msft-sme/shell/core';
import { EnvironmentModule } from '@msft-sme/shell/dist/core/manifest/environment-modules';
import { RpcUpdateData } from '@msft-sme/shell/dist/core/rpc/rpc-base';
import { Strings } from '../../generated/strings';

@Component({
  selector: 'add-example',
  templateUrl: './add-example.component.html',
  styleUrls: ['./add-example.component.css']
})
export class AddExampleComponent implements OnInit {
  public newConnectionName: string;
  public strings = MsftSme.resourcesStrings<Strings>().SolutionExample;
  private connectionType = 'msft.sme.connection-type.example'; // This needs to match the connectionTypes value used in the manifest.json.
  
  constructor(private appContextService: AppContextService, private route: ActivatedRoute) {
    // TODO:
  }

  public ngOnInit() {
    // TODO
  }

  public onSubmit() {
    let connections: Connection[] = [];

    let connection = <Connection> {
      id: ConnectionUtility.createConnectionId(this.connectionType, this.newConnectionName),
      type: this.connectionType,
      name: this.newConnectionName
    };

    connections.push(connection);

    this.appContextService.rpc.updateData(
      EnvironmentModule.nameOfShell,
      '##',
      <RpcUpdateData> {
        results: {
          connections: connections,
          credentials: null
        }
      }
    );
  }

  public onCancel() {
    this.appContextService.rpc.updateData(
      EnvironmentModule.nameOfShell, '##', <RpcUpdateData>{ results: { connections: [] } });
  }
}

```
