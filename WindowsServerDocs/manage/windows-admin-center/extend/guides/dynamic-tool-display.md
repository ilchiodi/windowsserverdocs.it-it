---
title: Controllare la visibilità dello strumento in una soluzione
description: Controllare la visibilità dello strumento in una soluzione Windows Admin Center SDK (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 440ba3d11da671beedc2c2fb90caa3e176f83877
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385317"
---
# <a name="control-your-tools-visibility-in-a-solution"></a>Controllare la visibilità dello strumento in una soluzione #

>Si applica a: Windows Admin Center, Windows Admin Center Preview

In alcuni casi è possibile che si desideri escludere o nascondere l'estensione o lo strumento dall'elenco degli strumenti disponibili. Se, ad esempio, lo strumento è destinato solo a Windows Server 2016 (versioni precedenti), potrebbe non essere necessario un utente che si connette a un server Windows Server 2012 R2 per visualizzare lo strumento. (Immaginate l'esperienza utente, facendo clic su di essa, attendo che lo strumento venga caricato, solo per ricevere un messaggio per cui le funzionalità non sono disponibili per la connessione.) È possibile definire quando visualizzare o nascondere la funzionalità nel file manifest. JSON dello strumento.

## <a name="options-for-deciding-when-to-show-a-tool"></a>Opzioni per decidere quando visualizzare uno strumento ##

Esistono tre diverse opzioni che è possibile usare per determinare se lo strumento deve essere visualizzato e disponibile per una connessione cluster o un server specifico.

* localhost
* inventario (una matrice di proprietà)
* script

### <a name="localhost"></a>LocalHost ###

La proprietà localHost dell'oggetto conditions contiene un valore booleano che può essere valutato per dedurre se il nodo di connessione è localHost (lo stesso computer in cui è installato l'interfaccia di amministrazione di Windows) o meno. Passando un valore alla proprietà, si indica quando (condizione) visualizzare lo strumento. Se, ad esempio, si desidera che lo strumento visualizzi solo se l'utente è effettivamente connesso all'host locale, configurarlo come segue:

``` json
"conditions": [
{
    "localhost": true
}]
```

In alternativa, se si desidera che lo strumento venga visualizzato solo quando il nodo di connessione *non è* localhost:

``` json
"conditions": [
{
    "localhost": false
}]
```

Di seguito sono riportate le impostazioni di configurazione da visualizzare solo se il nodo che si connette non è localhost:

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

### <a name="inventory-properties"></a>Proprietà inventario ###

L'SDK include un set precurato di proprietà di inventario che è possibile usare per creare condizioni per determinare quando lo strumento deve essere disponibile o meno. Nella matrice ' Inventory ' sono presenti nove proprietà diverse:

| Nome proprietà | Tipo di valore previsto |
| ------------- | ------------------- |
| computerManufacturer | string |
| operatingSystemSKU | number |
| operatingSystemVersion | version_string (ad esempio: "10,1. *") |
| productType | number |
| FQDNCluster | string |
| isHyperVRoleInstalled | boolean |
| isHyperVPowershellInstalled | boolean |
| isManagementToolsAvailable | boolean |
| isWmfInstalled | boolean |

Ogni oggetto nella matrice di inventario deve essere conforme alla struttura JSON seguente:

``` json
"<property name>": {
    "type": "<expected type>",
    "operator": "<defined operator to use>",
    "value": "<expected value to evaluate using the operator>"
}
```

#### <a name="operator-values"></a>Valori dell'operatore ####

| Operator | Descrizione |
| -------- | ----------- |
| gt | Maggiore di |
| GE | Maggiore o uguale a |
| Lt | Minore di |
| le | Minore o uguale a |
| EQ | Uguale a |
| ne | Diverso da |
| è impostata su | Verifica della presenza di un valore true |
| not | Verifica della presenza di un valore false |
| Contiene | elemento esistente in una stringa |
| notContains | l'elemento non esiste in una stringa |

#### <a name="data-types"></a>Tipi di dati ####

Opzioni disponibili per la proprietà' type ':

| Type | Descrizione |
| ---- | ----------- |
| version | un numero di versione (ad esempio: 10,1. *) |
| number | Un valore numerico |
| string | valore stringa |
| boolean | true o false |

#### <a name="value-types"></a>Tipi di valore ####

La proprietà' value ' accetta questi tipi:

* string
* number
* boolean

Un set di condizioni di inventario correttamente formattato ha un aspetto simile al seguente:

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

Infine, è possibile eseguire uno script di PowerShell personalizzato per identificare la disponibilità e lo stato del nodo. Tutti gli script devono restituire un oggetto con la struttura seguente:

``` ps
@{
    State = 'Available' | 'NotSupported' | 'NotConfigured';
    Message = '<Message to explain the reason of state such as not supported and not configured.>';
    Properties =
        @{ Name = 'Prop1'; Value = 'prop1 data'; Type = 'string' },
        @{Name='Prop2'; Value = 12345678; Type='number'; };
}
```
La proprietà state è il valore importante che controllerà la decisione di mostrare o nascondere l'estensione nell'elenco degli strumenti.  I valori consentiti sono:

| Value | Descrizione |
| ---- | ----------- |
| Disponibile | L'estensione deve essere visualizzata nell'elenco di strumenti. |
| NotSupported | L'estensione non deve essere visualizzata nell'elenco di strumenti. |
| NotConfigured | Si tratta di un valore segnaposto per le attività future che richiederanno all'utente una configurazione aggiuntiva prima che lo strumento venga reso disponibile.  Attualmente questo valore comporterà la visualizzazione dello strumento e l'equivalente funzionale a "available". |

Se, ad esempio, si desidera caricare uno strumento solo se nel server remoto è installato BitLocker, lo script avrà un aspetto simile al seguente:

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

Una configurazione del punto di ingresso con l'opzione script è simile alla seguente:

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

È possibile utilizzare più di un set di requisiti per determinare quando visualizzare lo strumento definendo più blocchi "requisiti".

Ad esempio, per visualizzare lo strumento se "scenario A" o "scenario B" è true, definire due blocchi requisiti; Se è true, ovvero se vengono soddisfatte tutte le condizioni in un blocco requirements, viene visualizzato lo strumento.

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

## <a name="supporting-condition-ranges"></a>Intervalli di condizioni di supporto ##

È anche possibile definire un intervallo di condizioni definendo più blocchi "Conditions" con la stessa proprietà, ma con operatori diversi.

Quando la stessa proprietà viene definita con operatori diversi, lo strumento viene visualizzato purché il valore sia compreso tra le due condizioni.

Questo strumento, ad esempio, viene visualizzato purché il sistema operativo sia una versione tra 6.3.0 e 10.0.0:

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
