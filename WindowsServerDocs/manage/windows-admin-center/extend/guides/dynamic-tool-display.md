---
title: Controllare la visibilità dello strumento in una soluzione
description: Controllare la visibilità dello strumento in una soluzione Windows Admin Center SDK (progetto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f3f34b4c86854bfc55cf4b1b57a0fd3c2baf2ffc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839252"
---
# <a name="control-your-tools-visibility-in-a-solution"></a>Controllare la visibilità dello strumento in una soluzione #

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

Potrebbero esserci situazioni quando si vuole escludere (o nascondere) dell'estensione o lo strumento nell'elenco di strumenti disponibili. Ad esempio, se lo strumento è destinato solo Windows Server 2016 (versioni precedenti non), è possibile evitare un utente che si connette a un server Windows Server 2012 R2 per vedere tutto lo strumento. (Immaginare l'esperienza utente - fare clic su di esso, attendere che lo strumento per caricare, solo per ricevere un messaggio che le relative funzionalità non sono disponibili per la connessione.) È possibile definire quando visualizzare (o nascondere) le funzionalità nel file manifest dello strumento.

## <a name="options-for-deciding-when-to-show-a-tool"></a>Opzioni per decidere quando visualizzare uno strumento ##

Esistono tre diverse opzioni che è possibile usare per determinare se lo strumento deve essere visualizzata e disponibili per un server specifico o una connessione al cluster.

* localhost
* inventario (una matrice di proprietà)
* script

### <a name="localhost"></a>LocalHost ###

La proprietà localHost dell'oggetto condizioni contiene un valore booleano che può essere valutato per dedurre se il nodo che esegue la connessione è localHost (stesso computer in cui è installato Windows Admin Center) o meno. Passando un valore alla proprietà, si indicano quando (la condizione) per visualizzare lo strumento. Ad esempio se si vuole solo lo strumento da visualizzare se l'utente è in realtà la connessione all'host locale, configurarlo come segue:

``` json
"conditions": [
{
    "localhost": true
}]
```

In alternativa, se si vuole solo lo strumento da visualizzare quando il nodo che si connette *non è* localhost:

``` json
"conditions": [
{
    "localhost": false
}]
```

Ecco le impostazioni di configurazione come appaiono per visualizzare solo uno strumento quando il nodo che esegue la connessione non localhost:

``` json
"entryPoints": [
{
    "entryPointType": "tool",
    "name": "main",
    "urlName": "processes",
    "displayName": "resources:strings:displayName",
    "description": "resources:strings:description",
    "icon": "sme-icon:icon-win-serverProcesses",
    "path": "",
    "requirements": [
    {
        "solutionIds": [
        "msft.sme.server-manager!windowsClients"
        ],
        "connectionTypes": [
        "msft.sme.connection-type.windows-client"
        ],
        "conditions": [
        {
            "localhost": true
        }
        ]
    }
    ]
}
```

### <a name="inventory-properties"></a>Proprietà dell'inventario ###

il SDK include un set di proprietà dell'inventario è possibile usare per compilare le condizioni per determinare quando lo strumento deve essere disponibile o non pre-curato. Sono disponibili nove proprietà diverse nella matrice 'inventario':

| Nome proprietà | Tipo di valore previsto |
| ------------- | ------------------- |
| computerManufacturer | string |
| operatingSystemSKU | number |
| operatingSystemVersion | version_string (eg: "10.1.*") |
| productType | number |
| clusterFqdn | string |
| isHyperVRoleInstalled | booleano |
| isHyperVPowershellInstalled | booleano |
| isManagementToolsAvailable | booleano |
| isWmfInstalled | booleano |

Ogni oggetto nella matrice di inventario deve essere conforme per la struttura json seguente:

``` json
"<property name>": {
    "type": "<expected type>",
    "operator": "<defined operator to use>",
    "value": "<expected value to evaluate using the operator>"
}
```

#### <a name="operator-values"></a>Valori di operatori ####

| Operatore | Descrizione |
| -------- | ----------- |
| gt | Maggiore di |
| Ge | Maggiore o uguale a |
| lt | Minore di |
| le | Minore o uguale a |
| eq | Uguale a |
| ne | Non è uguale a |
| è impostata su | verifica se un valore è true |
| not | verifica se un valore è false |
| Contiene | elemento esistente in una stringa |
| notContains | elemento non esiste in una stringa |

#### <a name="data-types"></a>Tipi di dati ####

Opzioni disponibili per la proprietà 'type':

| Tipo | Descrizione |
| ---- | ----------- |
| version | un numero di versione (ad esempio: 10.1.*) |
| number | un valore numerico |
| string | valore stringa |
| booleano | true o false |

#### <a name="value-types"></a>Tipi di valore ####

La proprietà 'value' accetta questi tipi:

* string
* number
* booleano

Un set di condizioni di inventario in formato corretto è simile alla seguente:

``` json
"entryPoints": [
{
    "entryPointType": "tool",
    "name": "main",
    "urlName": "processes",
    "displayName": "resources:strings:displayName",
    "description": "resources:strings:description",
    "icon": "sme-icon:icon-win-serverProcesses",
    "path": "",
    "requirements": [
    {
        "solutionIds": [
        "msft.sme.server-manager!servers"
        ],
        "connectionTypes": [
        "msft.sme.connection-type.server"
        ],
        "conditions": [
        {
            "inventory": {
            "operatingSystemVersion": {
                "type": "version",
                "operator": "gt",
                "value": "6.3"
            },
            "operatingSystemSKU": {
                "type": "number",
                "operator": "eq",
                "value": "8"
            }
            }
        }
        ]
    }
    ]
}
```

### <a name="script"></a>Script ###

Infine, è possibile eseguire uno script di PowerShell personalizzato per identificare la disponibilità e stato del nodo. Tutti gli script devono restituire un oggetto con la struttura seguente:

``` ps
@{
    State = 'Available' | 'NotSupported' | 'NotConfigured';
    Message = '<Message to explain the reason of state such as not supported and not configured.>';
    Properties =
        @{ Name = 'Prop1'; Value = 'prop1 data'; Type = 'string' },
        @{Name='Prop2'; Value = 12345678; Type='number'; };
}
```
La proprietà stato è il valore importante che consentono di controllare la decisione per mostrare o nascondere l'estensione nell'elenco degli strumenti.  I valori consentiti sono:
| Value | Descrizione |
| ---- | ----------- |
| Disponibile | L'estensione deve essere visualizzato nell'elenco degli strumenti. |
| NotSupported | L'estensione non deve essere visualizzato nell'elenco degli strumenti. |
| NotConfigured | Si tratta di un valore di segnaposto per il lavoro futuro che richiederà all'utente per un'ulteriore configurazione prima che lo strumento è reso disponibile.  Attualmente questo valore comporterà lo strumento di visualizzazione e l'equivalente funzionale in 'Disponibile'. |

Ad esempio, se si desidera che uno strumento per caricare solo se il server remoto è BitLocker installata, lo script di aspetto simile al seguente:

``` ps
$response = @{
    State = 'NotSupported';
    Message = 'Not executed';
    Properties = @{ Name = 'Prop1'; Value = 'prop1 data'; Type = 'string' },
        @{Name='Prop2'; Value = 12345678; Type='number'; };
}

if (Get-Module -ListAvailable -Name servermanager) {
    Import-module servermanager; 
    $isInstalled = (Get-WindowsFeature -name bitlocker).Installed;
    $isGood = $isInstalled;
}

if($isGood) {
    $response.State = 'Available';
    $response.Message = 'Everything should work.';
}

$response
```

Una configurazione del punto di ingresso usando l'opzione di script è simile alla seguente:

``` json
"entryPoints": [
{
    "entryPointType": "tool",
    "name": "main",
    "urlName": "processes",
    "displayName": "resources:strings:displayName",
    "description": "resources:strings:description",
    "icon": "sme-icon:icon-win-serverProcesses",
    "path": "",
    "requirements": [
    {
        "solutionIds": [
        "msft.sme.server-manager!windowsClients"
        ],
        "connectionTypes": [
        "msft.sme.connection-type.windows-client"
        ],
        "conditions": [
        {
            "localhost": true,
            "inventory": {
            "operatingSystemVersion": {
                "type": "version",
                "operator": "eq",
                "value": "10.0.*"
            },
            "operatingSystemSKU": {
                "type": "number",
                "operator": "eq",
                "value": "4"
            }
            },
            "script": "$response = @{ State = 'NotSupported'; Message = 'Not executed'; Properties = @{ Name = 'Prop1'; Value = 'prop1 data'; Type = 'string' }, @{Name='Prop2'; Value = 12345678; Type='number'; }; }; if (Get-Module -ListAvailable -Name servermanager) { Import-module servermanager; $isInstalled = (Get-WindowsFeature -name bitlocker).Installed; $isGood = $isInstalled; }; if($isGood) { $response.State = 'Available'; $response.Message = 'Everything should work.'; }; $response"
        }
        ]
    }
    ]
}
```

## <a name="supporting-multiple-requirement-sets"></a>Supporto di più set di requisiti ##

È possibile usare più di un set di requisiti per determinare quando visualizzare lo strumento con la definizione di più blocchi "requisiti".

Ad esempio, per visualizzare lo strumento se "scenario A" o "scenario B" è true, definire due blocchi di requisiti. Se viene soddisfatta una (vale a dire tutte le condizioni all'interno di un blocco di requisiti vengono soddisfatti), viene visualizzato lo strumento.

``` json
"entryPoints": [
{
    "requirements": [
    {
        "solutionIds": [
            …"scenario A"…
        ],
        "connectionTypes": [
            …"scenario A"…
        ],
        "conditions": [
            …"scenario A"…
        ]
    },
    {
        "solutionIds": [
            …"scenario B"…
        ],
        "connectionTypes": [
            …"scenario B"…
        ],
        "conditions": [
            …"scenario B"…
        ]
    }
    ]
}

```

## <a name="supporting-condition-ranges"></a>Supporto di intervalli di condizione ##

È anche possibile definire una gamma di condizioni mediante la definizione di più blocchi "condizioni" con la stessa proprietà, ma con diversi operatori.

Quando la stessa proprietà è definita con diversi operatori, viene visualizzato lo strumento, purché il valore è compreso tra le due condizioni.

Ad esempio, questo strumento viene visualizzato fino a quando il sistema operativo è una versione 6.3.0 quella 10.0.0:

``` json
"entryPoints": [
{
    "entryPointType": "tool",
    "name": "main",
    "urlName": "processes",
    "displayName": "resources:strings:displayName",
    "description": "resources:strings:description",
    "icon": "sme-icon:icon-win-serverProcesses",
    "path": "",
    "requirements": [
    {
        "solutionIds": [
             "msft.sme.server-manager!servers"
        ],
        "connectionTypes": [
             "msft.sme.connection-type.server"
        ],
        "conditions": [
        {
            "inventory": {
                "operatingSystemVersion": {
                    "type": "version",
                    "operator": "gt",
                    "value": "6.3.0"
                },
            }
        },
        {
            "inventory": {
                "operatingSystemVersion": {
                    "type": "version",
                    "operator": "lt",
                    "value": "10.0.0"
                }
            }
        }
        ]
    }
    ]
}

```
