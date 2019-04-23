---
title: Modificare il comportamento di spostamento principale
description: Sviluppare un'estensione di soluzioni Windows Admin Center SDK (progetto Honolulu) - modificare il comportamento di navigazione principale
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 08/07/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 4a5cba228aa3a0afed99c0d853c3720a5b46f650
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861732"
---
# <a name="modify-root-navigation-behavior-for-a-solution-extension"></a>Modificare il comportamento di navigazione principale per un'estensione della soluzione

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

In questa guida si apprenderà come modificare il comportamento di navigazione principale per la soluzione di avere un comportamento elenco connessione diversa, nonché come nascondere o visualizzare l'elenco degli strumenti.

## <a name="modifying-root-navigation-behavior"></a>Modifica del comportamento di navigazione principale

Aprire il file manifest in {radice dell'estensione} \src. e trovare la proprietà "rootNavigationBehavior". Questa proprietà ha due valori validi: "connessioni" o "path". Il comportamento di "connessioni" descritta in dettaglio in un secondo momento nella documentazione.

### <a name="setting-path-as-a-rootnavigationbehavior"></a>Impostazione del percorso come un rootNavigationBehavior

Impostare il valore della ```rootNavigationBehavior``` al ```path```e quindi eliminare il ```requirements``` proprietà e lasciare invariato il ```path``` proprietà come stringa vuota. È stata completata la configurazione minima necessaria per compilare un'estensione della soluzione. Salvare il file e compilazione gulp -> gulp fungono da è uno strumento e quindi lato carica l'estensione nell'estensione Windows Admin Center locale.

Una matrice di punti di ingresso del manifesto valido è simile alla seguente:
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

Gli strumenti incorporati con questo tipo di struttura verranno connessioni non necessari da caricare, ma non saranno disponibili funzionalità di connettività del nodo sia.

### <a name="setting-connections-as-a-rootnavigationbehavior"></a>Impostando le connessioni come una rootNavigationBehavior

Quando si imposta la ```rootNavigationBehavior``` proprietà ```connections```, si indica la Shell di Windows Admin Center che sarà presente un nodo connesso (sempre un server dello stesso tipo) che si deve connettere e verificare lo stato della connessione. Con questa impostazione, esistono 2 passaggi di verifica della connessione. 1) Windows Admin Center proverà a effettuare un tentativo di accedere al nodo con le credenziali (per stabilire la sessione di PowerShell remota) e 2) non verrà eseguito lo script di PowerShell fornita per identificare se il nodo è in uno stato collegabile.

Una definizione di soluzione valido con le connessioni avrà un aspetto simile al seguente:

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

Quando il rootNavigationBehavior è impostato su "connessioni" è necessario per compilare la definizione di connessioni nel manifesto. Ciò include la proprietà "header" (verrà utilizzata da visualizzare nell'intestazione di soluzione quando viene selezionato un utente dal menu di scelta), una matrice di connectionTypes (si specificherà quale connectionTypes vengono usati nella soluzione. Informazioni su esso nella documentazione di connectionProvider.).

## <a name="enabling-and-disabling-the-tools-menu"></a>Abilitazione e disabilitazione del menu Strumenti ##

Un'altra proprietà disponibile nella definizione della soluzione è la proprietà "tools". Ciò determinerà se viene visualizzato il menu di strumenti, nonché lo strumento che verrà caricato. Quando abilitato, Windows Admin Center verranno visualizzati nel menu di strumenti a sinistra. Con defaultTool, è necessario aggiungere un punto di ingresso dello strumento per il manifesto per caricare le risorse appropriate. Il valore di "defaultTool" deve essere la proprietà "name" dello strumento con cui è definito nel manifesto.
