---
title: Modifiche al tipo di connessione cluster nell'interfaccia di amministrazione di Windows v1909
description: Modifiche al tipo di connessione cluster nell'interfaccia di amministrazione di Windows v1909
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 10/01/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: a07b30517f0d45b7e6f4f41f0ef9a6549e6e2117
ms.sourcegitcommit: de71970be7d81b95610a0977c12d456c3917c331
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/04/2019
ms.locfileid: "71952772"
---
# <a name="cluster-connection-type-changes-in-windows-admin-center-v1909"></a>Modifiche al tipo di connessione cluster nell'interfaccia di amministrazione di Windows v1909

>Si applica a: Windows Admin Center, Windows Admin Center Preview

> [!IMPORTANT]
> Questo documento descrive le modifiche richieste dagli sviluppatori di estensioni di Windows Admin Center che sviluppano gli strumenti di amministrazione di Windows per il cluster di failover e le soluzioni di cluster iperconvergenti (HCI). Si tratta di una modifica obbligatoria necessaria per la compatibilità dell'estensione con la versione di anteprima di Windows Admin Center v1909 e le versioni future di GA.

Nell'interfaccia di amministrazione di Windows v1909 sono stati unificati i due diversi tipi di connessione cluster (cluster di failover e connessioni cluster iperconvergenti) in un unico tipo di connessione cluster. Gli utenti non dovranno più identificare in anticipo la configurazione di un cluster per decidere a quale tipo di connessione aggiungere il cluster, né aggiungere il cluster due volte come tipi di connessione diversi per ottenere l'accesso ai diversi set di strumenti. È ora possibile aggiungere i cluster come "cluster di Windows Server" e vengono caricati gli strumenti appropriati, principalmente a seconda che la Spazi di archiviazione diretta sia abilitata o meno.

Poiché questa operazione ha richiesto una modifica nella definizione del tipo di connessione e in che modo gli strumenti correlati ai cluster decidono quando devono essere caricati, le estensioni che forniscono gli strumenti per i cluster, ovvero i cluster HCI o non HCI, o entrambi, richiederanno le modifiche nell'implementazione come descritto sotto.

## <a name="manifestjson---solutionsids-and-connectiontypes"></a>manifest. JSON-solutionsIds e connectionTypes

In precedenza, per visualizzare lo strumento per un cluster di failover o un tipo di connessione cluster HCI, era possibile utilizzare una delle seguenti definizioni nel file ```manifest.json```.

Per i cluster di failover:
``` json
    {
        "entryPointType": "tool",
        "name": "MyToolName",
        "urlName": "MyToolUrl",
        "displayName": "MyToolDisplayName",
        "description": "MyToolDescription",
        "icon": "MyToolIcon",
        "path": "MyToolPath",
        "iframeId": "MyToolIframeId",
        "requirements": [
        {
            "solutionIds": [
                "msft.sme.failover-cluster!failover-cluster"
            ],
            "connectionTypes": [
                "msft.sme.connection-type.cluster"
            ]
        }
        ]
    }
```

Per i cluster HCI, la sezione evidenziata sopra sarebbe stata sostituita con quanto segue:
``` json
    "requirements": [
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!SDDC"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.hyper-converged-cluster"
        ]
    }
    ]
```

Nell'interfaccia di amministrazione di Windows 1909 e versioni successive, le due solutionIds e connectionTypes sono state sostituite con le seguenti:
``` json
    "requirements": [
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!cluster"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.cluster"
        ]
    }
    ]
```

Questo è l'unico tipo di solutionIds e connectionTypes correlato al cluster supportato da ora in poi. Se lo strumento è definito solo con questo tipo di solutionIds e connectionTypes, verrà caricato per qualsiasi connessione del cluster di failover, indipendentemente dal fatto che sia o meno un cluster HCI. Se si vuole limitare lo strumento in modo che sia disponibile solo per i cluster HCI o non HCI, sarà necessario usare anche le nuove proprietà di inventario descritte nella sezione seguente.

## <a name="manifestjson--inventory-properties"></a>manifest. JSON-proprietà inventario
Quando ci si connette a un server o a un cluster, l'interfaccia di amministrazione di Windows esegue una query su un set di proprietà di inventario che è possibile usare per creare le condizioni per determinare quando lo strumento deve essere disponibile o meno (vedere la sezione relativa alle proprietà di inventario nel [controllo del proprio strumento ](dynamic-tool-display.md)documento di visibilità per ulteriori informazioni). Nell'interfaccia di amministrazione di Windows v1909 sono state aggiunte a questo elenco due nuove proprietà che possono essere usate per determinare se un cluster è iperconvergente o meno. 

### <a name="iss2denabled"></a>isS2dEnabled
Tecnicamente, un cluster iperconvergente viene definito come cluster di failover con Spazi di archiviazione diretta (S2D) abilitato. Se si vuole che lo strumento sia disponibile solo per i cluster iperconvergenti, ad esempio quando S2D è abilitato, aggiungere la condizione di inventario seguente:
``` json
    "requirements": [
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!cluster"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.cluster"
        ],
        "conditions": [
        {
            "inventory": {
                "isS2dEnabled": {
                    "type": "boolean",
                    "operator": "is"
                }
            }
        }
        ]
    }
    ]
```
> [!TIP] 
> Se si vuole che lo strumento sia disponibile solo se S2D non è abilitato, impostare il valore "operator" su "not".

### <a name="isbritannicaenabled"></a>isBritannicaEnabled
Inoltre, se si dipende dalla risorsa cluster di gestione SDDC e si usa il modello a oggetti SDDC, è possibile verificare la condizione seguente:
``` json
    "conditions": [
        {
            "inventory": {
                "isS2dEnabled": {
                    "type": "boolean",
                    "operator": "is"
                },
                "isBritannicaEnabled": {
                    "type": "boolean",
                    "operator": "is"
                }
            }
        }
    ]
```

## <a name="backward-compatibility-to-support-previous-versions-of-windows-admin-center"></a>Compatibilità con le versioni precedenti per supportare le versioni precedenti dell'interfaccia di amministrazione di Windows
Per assicurarsi che l'estensione continui a funzionare con le versioni precedenti dell'interfaccia di amministrazione di Windows, ad esempio la versione GA di v1904, è possibile usare la definizione solutionIds e connectionTypes precedente insieme alla nuova definizione. Vedere l'esempio seguente per visualizzare lo strumento solo per i cluster HCI in tutte le versioni dell'interfaccia di amministrazione di Windows.
``` json
    "requirements": [
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!SDDC"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.hyper-converged-cluster"
        ]
    },
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!SDDC",
            "msft.sme.software-defined-data-center!cluster"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.hyper-converged-cluster",
            "msft.sme.connection-type.cluster"
        ],
        "conditions": [
            {
                "inventory": {
                    "isS2dEnabled": {
                        "type": "boolean",
                        "operator": "is"
                    }
                }
            }
        ]
    }
    ]
```

## <a name="known-issue-appcontextserviceactiveconnectionishyperconvergedclusterisfailovercluster-is-not-set-properly-in-windows-admin-center-v1909"></a>Problema noto: AppContextService. activeConnection. isHyperConvergedCluster/isFailoverCluster non è impostato correttamente nell'interfaccia di amministrazione di Windows v1909
Una regressione dalle modifiche recenti è che le proprietà ```AppContextService.activeConnection.isHyperConvergedCluster/isFailoverCluster``` non sono impostate correttamente nell'interfaccia di amministrazione di Windows v1909 e saranno sempre false. Questo problema verrà risolto nella versione successiva, V1910, ma sarà anche deprecato e non sarà più disponibile nella versione GA seguente nel 2020. In futuro, è possibile sostituire questa operazione con il codice seguente e utilizzare ```this.connectHCI```.
```
    import { ClusterInventoryCache } from '@msft-sme/core';

    constructor(
        ...
        private appContextService: AppContextService,
        ...
    ) {
        ...
        this.clusterInventoryCache = new ClusterInventoryCache(
            this.appContextService,
            <SharedCacheOptions>{ expiration: Constants.sharedCacheExpiration }
        );

         this.isBritannicEnabled().subscribe(result => this.connectHCI = result);
    }

    private isBritannicEnabled(): Observable<boolean> {
        return this.clusterInventoryCache.query(this.getClusterInventoryQueryParameters())
        .pipe(
            map(inventory => {
                return inventory.instance.isBritannicaEnabled;
        }));
    }

    private getClusterInventoryQueryParameters(): ClusterInventoryParams {
        const nodeName = this.appContextService.activeConnection.validNodeName;
        const endpoint = this.appContextService.authorizationManager.getJeaEndpoint(nodeName);
        const options = { powerShellEndpoint: endpoint };
        return {
            name: nodeName,
            requestOptions: options
        };
    }
```