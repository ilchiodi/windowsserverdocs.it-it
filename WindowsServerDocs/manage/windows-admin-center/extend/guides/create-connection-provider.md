---
title: Creare un provider di connessione per un'estensione della soluzione
description: Sviluppare un'estensione di soluzioni Windows Admin Center SDK (progetto Honolulu) - creare un provider di connessione
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: b79e832ee45990d18baf4c211ab68b907134ceb7
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811842"
---
# <a name="create-a-connection-provider-for-a-solution-extension"></a>Creare un provider di connessione per un'estensione della soluzione

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

Provider di connessioni svolgono un ruolo importante nel modo in cui Windows Admin Center definisce e comunica con gli oggetti collegabili, o le destinazioni. In primo luogo, un Provider di connessione esegue azioni quando è stata effettuata una connessione, ad esempio assicurandosi che la destinazione sia online e disponibili e garantire al contempo che l'utente che esegue la connessione disponga dell'autorizzazione di accesso di destinazione.

Per impostazione predefinita, Windows Admin Center viene fornito con i provider di connessione seguenti:

* Server
* Client Windows
* Cluster di failover
* HCL Cluster

Per creare un Provider personalizzato di connessione, seguire questa procedura:

* Aggiungere i dettagli del Provider di connessione per ```manifest.json```
* Definire il Provider di stato di connessione
* Implementare il Provider di connessione nel livello applicazione

## <a name="add-connection-provider-details-to-manifestjson"></a>Aggiungere dettagli di connessione Provider manifest

A questo punto si esamineranno quello che devi sapere per definire un Provider di connessione del progetto ```manifest.json``` file.

### <a name="create-entry-in-manifestjson"></a>Creare una voce in manifest

Il ```manifest.json``` file si trova nella cartella \src. e include, tra le altre cose, le definizioni dei punti di ingresso nel progetto. Tipi di punti di ingresso includono strumenti, soluzioni e i provider di connessione. Si verrà definito un Provider di connessione.

Un esempio di una voce di Provider di connessione in manifest è di sotto:

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

Un punto di ingresso di tipo "connnectionProvider" indica che l'elemento in corso di configurazione è un provider che verrà usato da una soluzione per convalidare uno stato di connessione per la shell di Windows Admin Center. I punti di ingresso di connessione del Provider contiene numerose proprietà importanti, definito di seguito:

| Proprietà | Descrizione |
| -------- | ----------- |
| entryPointType | Si tratta di una proprietà obbligatoria. Esistono tre valori validi: "strumento", "soluzione" e "connectionProvider". | 
| name | Identifica il Provider di connessioni all'interno dell'ambito di una soluzione. Questo valore deve essere univoco all'interno di un'istanza completa di Windows Admin Center (non solo una soluzione). |
| path | Rappresenta il percorso dell'URL per il "aggiungere" interfaccia utente di connessione, se viene configurato dalla soluzione. Questo valore deve corrispondere a una route che è configurata nel file app routing.module.ts. Quando il punto di ingresso della soluzione è configurato per usare le connessioni rootNavigationBehavior, questa route caricherà il modulo che viene usato dalla Shell per visualizzare l'interfaccia utente di connessione aggiunta. Altre informazioni disponibili nella sezione rootNavigationBehavior. |
| displayName | Il valore immesso viene visualizzato sul lato destro della shell, sotto la barra nera Windows Admin Center quando un utente carica pagina connessioni della soluzione. |
| icona | Rappresenta l'icona nel menu a discesa soluzioni utilizzata per rappresentare la soluzione. |
| description | Immettere una breve descrizione del punto di ingresso. |
| connectionType | Rappresenta il tipo di connessione che il provider verrà caricato. Il valore immesso in questo caso verrà anche utilizzabile per specificare che la soluzione possa caricare tali connessioni nel punto di ingresso di soluzione. Il valore immesso in questo caso verrà anche utilizzabile per indicare che lo strumento è compatibile con questo tipo in punti di ingresso dello strumento. Questo valore immesso verrà anche essere utilizzato l'oggetto di connessione che viene inviato a RPC chiamare su "Aggiungi finestra" nel passaggio di implementazione di livello applicazione. |
| connectionTypeName | Utilizzato nella tabella connessioni per rappresentare una connessione che utilizza il Provider di connessione. È previsto il nome plurale del tipo. |
| connectionTypeUrlName | Usato nella creazione di URL per rappresentare la soluzione caricata, dopo che Windows Admin Center è connesso a un'istanza. Questa voce viene utilizzata dopo le connessioni e prima della destinazione. In questo esempio, "connectionexample" è in cui questo valore viene visualizzato nell'URL: `http://localhost:6516/solutionexample/connections/connectionexample/con-fake1.corp.contoso.com` |
| connectionTypeDefaultSolution | Rappresenta il componente predefinito che deve essere caricato dal Provider di connessione. Questo valore è una combinazione di: <br>[a] il nome del pacchetto di estensione definito nella parte superiore del manifesto; <br>[b] punto esclamativo (!); <br>[c] il nome del punto di ingresso di soluzione.    <br>Per un progetto con nome "msft.sme.mySample-extension" e un punto di ingresso di soluzione con nome "example", questo valore sarà "msft.sme.solutionExample-extension esempio". |
| connectionTypeDefaultTool | Rappresenta il valore predefinito dello strumento che deve essere caricato in una connessione ha esito positivo. Valore di questa proprietà è costituito da due parti, simile al connectionTypeDefaultSolution. Questo valore è una combinazione di: <br>[a] il nome del pacchetto di estensione definito nella parte superiore del manifesto; <br>[b] punto esclamativo (!); <br>[c] il nome di punto di ingresso dello strumento per lo strumento che deve essere caricato inizialmente. <br>Per un progetto con nome "msft.sme.solutionExample-extension" e un punto di ingresso di soluzione con nome "example", questo valore sarà "msft.sme.solutionExample-extension esempio". |
| connectionStatusProvider | Vedere la sezione "Definiscono il Provider di stato di connessione" |

## <a name="define-connection-status-provider"></a>Definire il Provider di stato di connessione

Il Provider di stato di connessione è il meccanismo mediante il quale una destinazione viene convalidata per essere online e disponibile, anche verificare che l'utente che esegue la connessione abbia le autorizzazioni di accesso di destinazione. Esistono attualmente due tipi di provider di stato di connessione:  PowerShell e RelativeGatewayUrl.

*   <strong>Il Provider di stato connessione PowerShell</strong> -determina se una destinazione sia online e accessibile con uno script di PowerShell. Il risultato deve essere restituito in un oggetto con una singola proprietà "status", definiti di seguito.
*   <strong>Il Provider di stato connessione RelativeGatewayUrl</strong> -determina se una destinazione sia online e accessibile con una chiamata rest. Il risultato deve essere restituito in un oggetto con una singola proprietà "status", definiti di seguito.

### <a name="define-status"></a>Definire lo stato

I provider di stato di connessione sono necessari per restituire un oggetto con una singola proprietà ```status``` conforme al formato seguente:

``` json
{
    status: {
        label: string;
        type: int;
        details: string;
    }
}
```

Proprietà di stato:

* <strong>Etichetta</strong> - un'etichetta che descrive il tipo restituito di stato. Si noti che i valori per etichetta possono essere mappati in fase di esecuzione. Per vedere la voce sotto i valori di mapping in fase di esecuzione.

* <strong>Tipo</strong> -lo stato di tipo restituito. Tipo presenta i seguenti valori di enumerazione. Per qualsiasi valore 2 o versione successiva, la piattaforma non passerà a oggetto connesso e verrà visualizzato un errore nell'interfaccia utente.

   Tipi:

  | Value | Descrizione |
  | ----- | ----------- |
  | 0 | Online |
  | 1 | Avviso |
  | 2 | Non autorizzato |
  | 3 | Errore |
  | 4 | Errore irreversibile |
  | 5 | Sconosciuta |

* <strong>Dettagli</strong> - dettagli aggiuntivi che descrivono il tipo restituito di stato.

### <a name="powershell-connection-status-provider-script"></a>Script di PowerShell Provider dello stato della connessione

Lo script di PowerShell di Provider dello stato di connessione determina se una destinazione sia online e accessibile con uno script di PowerShell. Il risultato deve essere restituito in un oggetto con una singola proprietà "status". Seguito è riportato un esempio di script.

Script di PowerShell di esempio:

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

### <a name="define-relativegatewayurl-connection-status-provider-method"></a>Definire il Provider di stato connessione RelativeGatewayUrl metodo

Il Provider di stato di connessione ```RelativeGatewayUrl``` metodo chiama un rest API per determinare se una destinazione sia online e accessibile. Il risultato deve essere restituito in un oggetto con una singola proprietà "status". Un esempio di voce di Provider di connessione nel manifest di un RelativeGatewayUrl è illustrato di seguito.

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

Note sull'utilizzo RelativeGatewayUrl:

* "relativeGatewayUrl" specifica dove ottenere lo stato della connessione da un URL del gateway. Questo URI è relativo rispetto/API. Se viene trovato $connectionName nell'URL, questo verrà sostituito con il nome della connessione.
* Tutte le proprietà relativeGatewayUrl devono essere eseguite sul gateway host, che possono essere eseguite tramite la creazione di un'estensione di gateway

### <a name="map-values-in-runtime"></a>Eseguire il mapping di valori in fase di esecuzione

I valori di etichetta e i dettagli di stato oggetto di ritorno può essere formattato l'ottimizzazione di tempo includendo le chiavi e valori nella proprietà "defaultValueMap" del provider.

Ad esempio, se si aggiunge il valore riportato di seguito, ogni volta che tale "defaultConnection_test" bussato come valore per l'etichetta o informazioni dettagliate, Windows Admin Center sostituirà automaticamente la chiave con il valore di stringa di risorse configurati.

``` json
    "defaultConnection_test": "resources:strings:addServer_status_defaultConnection_label"
```

## <a name="implement-connection-provider-in-application-layer"></a>Implementare il Provider di connessione nel livello applicazione

A questo punto verranno per implementare il Provider di connessioni nel livello dell'applicazione, creando una classe di TypeScript che implementa OnInit. La classe presenta le seguenti funzioni:

| Funzione | Descrizione |
| -------- | ----------- |
| constructor(Private appContextService: AppContextService, route privata: ActivatedRoute) |  |
| public ngOnInit() |  |
| public onSubmit() | Contiene la logica per aggiornare shell quando viene effettuato un tentativo di connessione di componenti |
| public onCancel() | Contiene la logica per aggiornare shell quando viene annullato un tentativo di connessione di componenti |

### <a name="define-onsubmit"></a>Definire onSubmit

```onSubmit``` problemi di una chiamata RPC richiamare il contesto di app per notificare la shell di un "Aggiungi connessione". La chiamata di base Usa "updateData" simile al seguente:

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

Il risultato è una proprietà di connessione, che è una matrice di oggetti conformi alla struttura seguente:

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

### <a name="define-oncancel"></a>Definire onCancel

```onCancel``` Annulla un tentativo di "Aggiungi connessione" passando una matrice vuota di connessioni:

``` ts
this.appContextService.rpc.updateData(EnvironmentModule.nameOfShell, '##', <RpcUpdateData>{ results: { connections: [] } });
```

## <a name="connection-provider-example"></a>Esempio di Provider di connessione

La classe di TypeScript completa per l'implementazione di un provider di connessione è di sotto. Si noti che la stringa "connectionType" corrisponde il "connectionType come definito nel provider di connessione in manifest.

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
