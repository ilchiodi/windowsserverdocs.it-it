---
title: Modificare il comportamento di spostamento principale
description: Sviluppare un'estensione della soluzione Windows Admin Center SDK (Project Honolulu) - modifica il comportamento dello spostamento radice
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 08/07/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 4a5cba228aa3a0afed99c0d853c3720a5b46f650
ms.sourcegitcommit: 546229d6b5fa7e16f725c6c35f4dcc272711b811
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/18/2018
ms.locfileid: "4905041"
---
# Modificare il comportamento di spostamento principale per un'estensione della soluzione

>Si applica a: Windows Admin Center, Windows Admin Center Preview

In questa guida ti viene descritto come modificare il comportamento di spostamento principale per la soluzione di avere un comportamento elenco connessione diversi, nonché come visualizzare o nascondere l'elenco degli strumenti.

## Modifica il comportamento dello spostamento radice

Apri il file manifest.json in \src {radice estensione} e trova la proprietà "rootNavigationBehavior". Questa proprietà sono disponibili due valori validi: "connessioni" o "path". Il comportamento "connessioni" nel dettaglio più avanti in questa documentazione.

### Impostazione del percorso come un rootNavigationBehavior

Imposta il valore di ```rootNavigationBehavior``` a ```path```e quindi Elimina il ```requirements``` proprietà e lasciare il ```path``` proprietà come una stringa vuota. Avere completato la configurazione minima necessaria per creare un'estensione della soluzione. Salva il file e build gulp-& gt; gulp fungono è uno strumento e quindi sul lato carica l'estensione nell'estensione di Windows Admin Center locale.

Una matrice valido entryPoints manifesto ha questo aspetto:
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

Strumenti creati con questo tipo di struttura verranno connessioni non necessarie per caricare, ma non avrà una funzionalità di connettività nodo.

### Impostazione delle connessioni come un rootNavigationBehavior

Quando imposti il ```rootNavigationBehavior``` proprietà ```connections```, si indica Shell di Windows Admin Center che ci sia un nodo connesso (sempre un server di qualche tipo) che devono connettersi, e verificare lo stato della connessione. Questo aspetto, sono 2 i passaggi per la verifica di connessione. 1) Windows Admin Center tenterà di tentare di accedere al nodo con le credenziali (per stabilire la sessione remota di PowerShell) e 2) che consentirà di eseguire lo script di PowerShell fornite per stabilire se il nodo è in uno stato collegabile.

Una definizione di soluzione valida con connessioni avrà un aspetto simile:

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

Quando il rootNavigationBehavior è impostata su "connessioni" sono necessari per creare la definizione di connessioni nel manifesto. Ciò include la proprietà "intestazione" (verrà utilizzato da visualizzare nella tua intestazione soluzione quando un utente la seleziona dal menu), una matrice connectionTypes (ciò verrà specificare quali connectionTypes vengono utilizzati nella soluzione. Informazioni su questo nella documentazione di connectionProvider.).

## Abilitazione e disabilitazione dal menu Strumenti ##

Un'altra proprietà disponibile nella definizione della soluzione è la proprietà "strumenti". Questo determinerà se viene visualizzato il menu Strumenti, nonché lo strumento che verrà caricato. Quando è abilitata, Windows Admin Center verrà eseguito il rendering dal menu Strumenti sinistra. Con defaultTool, è necessario che aggiungere un punto di ingresso strumento al manifesto per caricare le risorse appropriate. Il valore di "defaultTool" deve essere la proprietà "name" dello strumento come definito nel manifesto.
