---
title: Modificare il comportamento di spostamento principale
description: Sviluppare un'estensione di soluzione Windows Admin Center SDK (Project Honolulu)-modificare il comportamento di navigazione radice
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 08/07/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 78c94f3ea13f54ac31f9de9dd60873b93eba2c17
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385284"
---
# <a name="modify-root-navigation-behavior-for-a-solution-extension"></a>Modificare il comportamento di navigazione radice per un'estensione della soluzione

>Si applica a: Windows Admin Center, Windows Admin Center Preview

In questa guida verrà illustrato come modificare il comportamento di navigazione radice per la soluzione in modo da ottenere un comportamento diverso per l'elenco di connessioni, nonché come nascondere o visualizzare l'elenco degli strumenti.

## <a name="modifying-root-navigation-behavior"></a>Modifica del comportamento di esplorazione radice

Aprire il file manifest. JSON in {Extension root} \src e trovare la proprietà "rootNavigationBehavior". Questa proprietà ha due valori validi: "Connections" o "Path". Il comportamento delle "connessioni" è descritto più avanti nella documentazione.

### <a name="setting-path-as-a-rootnavigationbehavior"></a>Impostazione del percorso come rootNavigationBehavior

Impostare il valore di ```rootNavigationBehavior``` su ```path```, quindi eliminare la proprietà ```requirements``` e lasciare la proprietà ```path``` come stringa vuota. È stata completata la configurazione minima necessaria per compilare un'estensione della soluzione. Salvare il file e Gulp Build-> Gulp fungerà da strumento, quindi caricare l'estensione nell'estensione dell'interfaccia di amministrazione di Windows locale.

Una matrice di entryPoints del manifesto valida è simile alla seguente:
```
    "entryPoints": [
        {
          "entryPointType": "solution",
          "name": "main",
          "urlName": "testsln",
          "displayName": "resources:strings:displayName",
          "description": "resources:strings:description",
          "icon": "sme-icon:icon-win-powerShell",
          "path": "",
          "rootNavigationBehavior": "path"
        }
    ],
```

Gli strumenti compilati con questo tipo di struttura non richiederanno le connessioni per il caricamento, ma non avranno una funzionalità di connettività del nodo.

### <a name="setting-connections-as-a-rootnavigationbehavior"></a>Impostazione delle connessioni come rootNavigationBehavior

Quando si imposta la proprietà ```rootNavigationBehavior``` su ```connections```, si indica alla shell dell'interfaccia di amministrazione di Windows che sarà presente un nodo connesso (sempre un server di qualche tipo) a cui deve connettersi e verificare lo stato della connessione. Questa operazione prevede 2 passaggi per la verifica della connessione. 1) l'interfaccia di amministrazione di Windows tenterà di effettuare un tentativo di accesso al nodo con le credenziali (per stabilire la sessione remota di PowerShell) e 2) verrà eseguito lo script di PowerShell fornito per identificare se il nodo è in uno stato collegabile.

Una definizione di soluzione valida con connessioni sarà simile alla seguente:

``` json
        {
          "entryPointType": "solution",
          "name": "example",
          "urlName": "solutionexample",
          "displayName": "resources:strings:displayName",
          "description": "resources:strings:description",
          "icon": "sme-icon:icon-win-powerShell",
          "rootNavigationBehavior": "connections",
          "connections": {
            "header": "resources:strings:connectionsListHeader",
            "connectionTypes": [
                "msft.sme.connection-type.example"
                ]
            },
            "tools": {
                "enabled": false,
                "defaultTool": "solution"
            }
        },
```

Quando rootNavigationBehavior è impostato su "Connections", è necessario compilare la definizione di connessione nel manifesto. Ciò include la proprietà "header" (verrà usata per la visualizzazione nell'intestazione della soluzione quando un utente lo seleziona dal menu), una matrice connectionTypes (in questo modo verranno specificati i connectionTypes usati nella soluzione. Per ulteriori informazioni, fare riferimento alla documentazione di la.

## <a name="enabling-and-disabling-the-tools-menu"></a>Abilitazione e disabilitazione del menu strumenti ##

Un'altra proprietà disponibile nella definizione della soluzione è la proprietà "Tools". Questa operazione determina se viene visualizzato il menu Strumenti, nonché lo strumento che verrà caricato. Se abilitata, l'interfaccia di amministrazione di Windows eseguirà il rendering del menu strumenti a sinistra. Con defaultTool, è necessario aggiungere un punto di ingresso dello strumento al manifesto per caricare le risorse appropriate. Il valore di "defaultTool" deve essere la proprietà "Name" dello strumento come definito nel manifesto.
