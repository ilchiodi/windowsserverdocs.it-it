---
title: Creare un provider di connessione per un'estensione della soluzione
description: "Sviluppare un'estensione della soluzione Windows Admin Center SDK (Project Honolulu): creare un provider di connessione"
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 883fba96fcb71cb1c6e8162c1564d66924c4e24d
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081138"
---
# Creare un provider di connessione per un'estensione della soluzione

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Provider di connessione svolgere un ruolo importante nel modo in cui Windows Admin Center definisce e comunica con oggetti collegabili o destinazioni. Soprattutto, un Provider di connessione esegue azioni mentre è stata effettuata una connessione, ad esempio verificando che la destinazione online e disponibile e anche garantire che l'utente connessione abbia l'autorizzazione per accedere alla destinazione.

Per impostazione predefinita, Windows Admin Center viene fornito con i provider di connessione seguenti:

* Server
* Client di Windows
* Cluster di failover
* Cluster HCI

Per creare un Provider personalizzato di connessione, segui questi passaggi:

* Aggiungere i dettagli di Provider di connessione a ```manifest.json```
* Definire il Provider di stato della connessione
* Implementare i Provider di connessione nel livello di applicazione

## Aggiungere i dettagli di connessione Provider manifest.json

Ora verranno illustrati ciò che devi conoscere per definire un Provider di connessione del progetto ```manifest.json``` file.

### Creare movimento in manifest.json

Il ```manifest.json``` file si trova nella cartella \src e contiene, tra le altre cose, le definizioni dei punti di ingresso nel progetto. Tipi di punti di ingresso includono gli strumenti, le soluzioni e provider di connessione. Ti verrà definire un Provider di connessione.

Di seguito viene riportato un esempio di una voce di Provider di connessione in manifest.json:

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

Un punto di ingresso di tipo "connnectionProvider" indica alla shell di Windows Admin Center che l'elemento da configurare è un provider che verrà utilizzato da una soluzione per convalidare uno stato di connessione. Punti di ingresso di connessione Provider contiene un numero di proprietà importanti, definito di seguito:

| Proprietà | Descrizione |
| -------- | ----------- |
| entryPointType | Questa è una proprietà richiesta. Esistono tre valori validi: "strumento", "soluzione" e "connectionProvider". | 
| name | Identifica il Provider di connessione nell'ambito di una soluzione. Questo valore deve essere univoco all'interno di un'istanza di Windows Admin Center completa (non solo una soluzione). |
| path | Rappresenta il percorso dell'URL per la "Aggiungi connessione" dell'interfaccia utente, se verrà configurato per la soluzione. Questo valore deve eseguire il mapping di un itinerario configurato nel file di app-routing.module.ts. Quando il punto di ingresso soluzione è configurato per utilizzare il rootNavigationBehavior le connessioni, la route caricherà il modulo che viene usato dalla Shell per visualizzare l'interfaccia utente di aggiungere connessione. Altre informazioni disponibili nella sezione sulle rootNavigationBehavior. |
| displayName | Il valore immesso viene visualizzato sul lato destro della shell, sotto la barra di Windows Admin Center nera quando un utente carica pagina connessioni della soluzione. |
| icon | Rappresenta l'icona nel menu a discesa soluzioni utilizzata per rappresentare la soluzione. |
| description | Immetti una breve descrizione del punto di ingresso. |
| connectionType | Rappresenta il tipo di connessione che verrà caricato il provider. Verrà inoltre utilizzato il valore immesso nel punto di ingresso soluzione per specificare che la soluzione può caricare le connessioni. Il valore immesso verrà anche essere usato in punti di ingresso lo strumento per indicare che lo strumento sia compatibile con questo tipo. Questo valore immesso verrà utilizzato anche nell'oggetto di connessione che viene inviata a RPC chiamare sulla "Aggiungi finestra", il passaggio di implementazione di livello di applicazione. |
| connectionTypeName | Utilizzato nella tabella delle connessioni per rappresentare una connessione che utilizza il Provider di connessione. È previsto che il nome plurale del tipo. |
| connectionTypeUrlName | Usato nella creazione l'URL per rappresentare la soluzione caricata, dopo che Windows Admin Center è connesso a un'istanza. Questa voce viene usata dopo le connessioni e prima della destinazione. In questo esempio, "connectionexample" è in cui questo valore viene visualizzato nell'URL:http://localhost:6516/solutionexample/connections/connectionexample/con-fake1.corp.contoso.com |
| connectionTypeDefaultSolution | Rappresenta il componente predefinito che deve essere caricato dal Provider di connessione. Questo valore è una combinazione di: [un] il nome del pacchetto dell'estensione definito nella parte superiore del manifesto; [b] punto esclamativo (!); [c] il nome del punto di ingresso di soluzione.    Per un progetto con nome "msft.sme.mySample-estensione" e un punto di ingresso soluzione con nome "example", questo valore sarà "msft.sme.solutionExample estensione! esempio". |
| connectionTypeDefaultTool | Rappresenta il valore predefinito dello strumento che deve essere caricato su una connessione ha esito positiva. Il valore della proprietà è costituito da due parti, simile al connectionTypeDefaultSolution. Questo valore è una combinazione di: [un] il nome del pacchetto dell'estensione definito nella parte superiore del manifesto; [b] punto esclamativo (!); [c] il nome di punto di ingresso lo strumento per lo strumento che deve essere caricato inizialmente. Per un progetto con nome "msft.sme.solutionExample-estensione" e un punto di ingresso soluzione con nome "example", questo valore sarà "msft.sme.solutionExample estensione! esempio". |
| connectionStatusProvider | Vedi la sezione "Definire Provider lo stato di connessione" |

## Definire il Provider di stato della connessione

Provider di stato della connessione è il meccanismo con cui una destinazione viene convalidata per essere online e disponibile, anche garantire che l'utente connessione abbia l'autorizzazione per accedere alla destinazione. Attualmente esistono due tipi di provider di stato della connessione: PowerShell e RelativeGatewayUrl.

*   Provider di stato della connessione di PowerShell
    *   Determina se la destinazione è online e accessibili con uno script di PowerShell. Il risultato deve essere restituito un oggetto con una singola proprietà "status", definita di seguito.
*   Provider di stato della connessione RelativeGatewayUrl
    *   Determina se una destinazione online e accessibili con una chiamata per il resto. Il risultato deve essere restituito un oggetto con una singola proprietà "status", definita di seguito.

### Definire lo stato

I provider di stato della connessione sono necessari per restituire un oggetto con una singola proprietà ```status``` che è conforme nel formato seguente:

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

* Label
    * Un'etichetta che descrive il tipo restituito lo stato. Nota: i valori di etichetta possono essere mappati in fase di esecuzione. Vedere di seguito per i valori di mapping in fase di esecuzione.

* Tipo
    * Il tipo restituito di stato. Tipo di ha i seguenti valori di enumerazione. Per qualsiasi valore 2 o versione successiva, la piattaforma non passeranno a oggetto connesso e verrà visualizzato un errore nell'interfaccia utente.

Tipi di:

| Valore | Descrizione |
| ----- | ----------- |
| 0 | Online |
| 1 | Warning |
| 2 | Autorizzazione negata |
| 3 | Errore |
| 4 | Irreversibile |
| 5 | Sconosciuto |

* Dettagli
    * Dettagli aggiuntivi che descrive lo stato del tipo restituiscono.

### Script di PowerShell Provider lo stato della connessione

Lo script di PowerShell del Provider di stato della connessione determina se una destinazione online e accessibili con uno script di PowerShell. Il risultato deve essere restituito un oggetto con una singola proprietà "status". Di seguito è riportato un esempio di script.

Script di PowerShell di esempio:

``` ts
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

### Definire il metodo di Provider di stato della connessione RelativeGatewayUrl

Il Provider di stato della connessione ```RelativeGatewayUrl``` metodo chiama un'API per determinare se una destinazione è online e accessibili per il resto. Il risultato deve essere restituito un oggetto con una singola proprietà "status". Di seguito è riportato un esempio voce del Provider di connessione nel manifest.json di un RelativeGatewayUrl.

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

* "relativeGatewayUrl" Specifica la posizione ottenere lo stato di connessione da un URL di gateway. Questo URI è relativo da /api. Se viene trovato $connectionName nell'URL, verrà sostituito con il nome della connessione.
* Tutte le proprietà relativeGatewayUrl devono essere eseguite in di gateway host, che possono essere eseguiti tramite la creazione di un'estensione di gateway

### Mappare i valori di runtime

I valori di etichetta e i dettagli di stato restituito oggetto può essere formattato ottimizzare tempo includendo chiavi e i valori nella proprietà "defaultValueMap" del provider.

Ad esempio, se si aggiunge il valore di seguito, ogni volta che è stato rilevato come valore per l'etichetta o dettagli, Windows Admin Center verrà automaticamente "defaultConnection_test" sostituire la chiave con il valore di stringa della risorsa configurato.

``` json
    "defaultConnection_test": "resources:strings:addServer_status_defaultConnection_label"
```

## Implementare i Provider di connessione nel livello di applicazione

Ora aggiungeremo per implementare il Provider di connessione nel livello di applicazione, creando una classe TypeScript che implementa OnInit. La classe ha le funzioni seguenti:

| Funzione | Descrizione |
| -------- | ----------- |
| costruttore (appContextService privato: AppContextService, route privato: ActivatedRoute) |  |
| ngOnInit() pubblica |  |
| onSubmit() pubblica | Contiene la logica per aggiornare shell quando viene eseguito un tentativo di connessione Aggiungi |
| onCancel() pubblica | Contiene la logica per aggiornare shell quando viene annullato un tentativo di connessione Aggiungi |

### Definire onSubmit

```onSubmit``` problemi di un RPC richiamerà al contesto dell'app per notificare all'utente la shell di un "Aggiungi connessione". La chiamata di base Usa "updateData" come segue:

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

### Definire onCancel

```onCancel``` Annulla un tentativo di "Aggiungi connessione" passando una matrice di connessioni vuota:

``` ts
this.appContextService.rpc.updateData(EnvironmentModule.nameOfShell, '##', <RpcUpdateData>{ results: { connections: [] } });
```

## Esempio di Provider di connessione

La classe TypeScript completa per l'implementazione di un provider di connessione è seguito. Nota che la stringa "connectionType" corrisponde al "connectionType come definito nel provider di connessione a manifest.json.

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
