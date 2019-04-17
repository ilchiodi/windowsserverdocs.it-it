---
title: Controllare la visibilità dello strumento in una soluzione
description: Controllare la visibilità dello strumento in una soluzione di Windows Admin Center SDK (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f3f34b4c86854bfc55cf4b1b57a0fd3c2baf2ffc
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2018
ms.locfileid: "4080968"
---
# Controllare la visibilità dello strumento in una soluzione #

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Potrebbero esserci delle volte quando si desidera escludere (o nascondere) l'estensione o lo strumento nell'elenco strumenti disponibili. Ad esempio, se lo strumento è destinata solo Windows Server 2016 (versioni precedenti non), non puoi un utente che si connette a un server Windows Server 2012 R2 per visualizzare lo strumento di affatto. (Immagina l'esperienza utente - fai clic su di esso, attendere lo strumento per caricare, solo per ottenere un messaggio che le funzionalità non sono disponibili per la connessione.) È possibile definire quando mostrare (o nascondere) le funzionalità nel file manifest.json dello strumento.

## Opzioni per decidere quando visualizzare uno strumento ##

Esistono tre diverse opzioni che puoi usare per determinare se il tuo strumento deve essere visualizzato e disponibile per una connessione di cluster o server specifici.

* localhost
* inventario (una matrice di proprietà)
* script

### LocalHost ###

La proprietà localHost dell'oggetto condizioni contiene un valore booleano che può essere valutato per dedurre se il nodo connessione localHost (lo stesso computer che è installato Windows Admin Center) o meno. Passando un valore alla proprietà, indicare quando (la condizione) per visualizzare lo strumento. Ad esempio se vuoi solo lo strumento per visualizzare l'utente è in realtà la connessione a host locale, configurare, come segue:

``` json
"conditions": [
{
    "localhost": true
}]
```

In alternativa, se vuoi solo lo strumento da visualizzare quando la connessione nodo *non* localhost:

``` json
"conditions": [
{
    "localhost": false
}]
```

Ecco le impostazioni di configurazione aspetto solo uno strumento quando il nodo di connessione non è localhost:

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

### Proprietà di inventario ###

il SDK include un set di proprietà di inventario che puoi usare per creare le condizioni per determinare quando il tuo strumento deve essere disponibile o non pre-accurato. Esistono diverse nove proprietà nella matrice 'inventario':

| Nome della proprietà | Tipo di valore previsto |
| ------------- | ------------------- |
| computerManufacturer | string |
| operatingSystemSKU | number |
| operatingSystemVersion | version_string (ad esempio: "10.1. *") |
| productType | number |
| FQDNCluster | string |
| isHyperVRoleInstalled | boolean |
| isHyperVPowershellInstalled | boolean |
| isManagementToolsAvailable | boolean |
| isWmfInstalled | boolean |

Ogni oggetto nella matrice di inventario deve essere conforme alla struttura di json seguente:

``` json
"<property name>": {
    "type": "<expected type>",
    "operator": "<defined operator to use>",
    "value": "<expected value to evaluate using the operator>"
}
```

#### Valori dell'operatore di telefonia ####

| Operatore | Descrizione |
| -------- | ----------- |
| gt | maggiore |
| Ge | maggiore o uguale a |
| lt | minore |
| a basso consumo | minore o uguale a |
| EQ | uguale a |
| ne | non è uguale a |
|  sia  | verifica se un valore è true |
| non | verifica se un valore è impostato su false |
| contiene | elemento esiste in una stringa |
| notContains | elemento non esiste in una stringa |

#### Tipi di dati ####

Opzioni disponibili per la proprietà di 'tipo':

| Tipo | Descrizione |
| ---- | ----------- |
| version | un numero di versione (ad esempio: 10.1. *) |
| number | un valore numerico |
| string | un valore di stringa |
| boolean | true o false |

#### Tipi di valore ####

La proprietà "valore" accetta questi tipi:

* string
* number
* boolean

Un set di condizioni di inventario in formato corretto ha questo aspetto:

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

### Script ###

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
La proprietà di stato è il valore importante che consente di controllare la decisione per mostrare o nascondere l'estensione nell'elenco degli strumenti.  I valori consentiti sono:
| Valore | Descrizione |
| ---- | ----------- |
| Disponibile | L'estensione deve essere visualizzato nell'elenco strumenti. |
| NotSupported | L'estensione non deve essere visualizzato nell'elenco strumenti. |
| NotConfigured | Questo è un valore del segnaposto per il lavoro futuro che chiede all'utente per la configurazione aggiuntiva prima che lo strumento viene reso disponibile.  Attualmente questo valore il risultato sarà lo strumento viene visualizzato e funzionali equivale a "Disponibile". |

Ad esempio, se vogliamo uno strumento per caricare solo se il server remoto ha BitLocker installato, lo script di questo aspetto:

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

Una configurazione del punto di ingresso usando l'opzione di script ha questo aspetto:

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

## Supporto di più set di requisito ##

È possibile utilizzare più di una serie di requisiti per determinare quando visualizzare lo strumento di definendo più blocchi "requisiti".

Ad esempio, per visualizzare lo strumento di se "scenario a" o "scenario B" è impostato su true, Definisci due blocchi di requisiti. in presenza di una (vale a dire tutte le condizioni all'interno di un blocco di requisiti sono soddisfatti), lo strumento viene visualizzato.

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

## Supporto di intervalli di condizione ##

È anche possibile definire una gamma di condizioni definendo più blocchi "condizioni" con la stessa proprietà, ma con gli operatori diversi.

Quando la stessa proprietà è definita con gli operatori diversi, lo strumento viene visualizzato, purché il valore è compreso tra le due condizioni.

Ad esempio, questo strumento viene visualizzato, purché il sistema operativo è una versione tra 6.3.0 e 10.0.0:

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
