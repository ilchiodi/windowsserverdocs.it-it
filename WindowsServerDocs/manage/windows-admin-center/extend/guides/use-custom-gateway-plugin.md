---
title: Utilizzare un plug-in del gateway personalizzato nell'estensione dello strumento
description: Sviluppare un'estensione dello strumento Windows Admin Center SDK (Project Honolulu) - utilizzare un plug-in gateway personalizzato nell'estensione dello strumento
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 4652616478b7b05bde97db48bf84648984b5a325
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296763"
---
# Utilizzare un plug-in del gateway personalizzato nell'estensione dello strumento

>Si applica a: Windows Admin Center, Windows Admin Center Preview

In questo articolo, usiamo un plug-in di gateway personalizzato in un'estensione dello strumento di nuovo e vuoto che abbiamo creato con l'interfaccia CLI di Windows Admin Center.

## Preparazione dell'ambiente ##

Se hai già fatto, segui le istruzioni nel [sviluppare un'estensione dello strumento](..\develop-tool.md) per preparare l'ambiente e creare un'estensione dello strumento di nuovo e vuoto.

## Aggiungere un modulo al progetto ##

Se hai già fatto, aggiungere un nuovo [modulo vuota](add-module.md) al tuo progetto, che verrà utilizzato nel passaggio successivo.  

## Aggiungere l'integrazione di plug-in del gateway personalizzato ##

Ora useremo un plug-in gateway personalizzato nel modulo di nuovo e vuoto che abbiamo appena creato.

### Creare plugin.service.ts

Modificare la directory del nuovo modulo strumento creato in precedenza (```\src\app\{!Module-Name}```) e creare un nuovo file ```plugin.service.ts```.

Aggiungi il codice seguente al file appena creato:
``` ts
import { Injectable } from '@angular/core';
import { AppContextService, HttpService } from '@microsoft/windows-admin-center-sdk/angular';
import { Cim, Http, PowerShell, PowerShellSession } from '@microsoft/windows-admin-center-sdk/core';
import { AjaxResponse, Observable } from 'rxjs';

@Injectable()
export class PluginService {
    constructor(private appContextService: AppContextService, private http: Http) {
    }
    
    public getGatewayRestResponse(): Observable<any> {
        let callUrl = this.appContextService.activeConnection.nodeName;

        return this.appContextService.node.get(callUrl, 'features/Sample%20Uno').map(
            (response: any) => {
                return response;
            }
        )
    }
}
```

Modificare i riferimenti a ```Sample Uno``` e ```Sample%20Uno``` per il nome della funzionalità come appropriato.

[!WARNING]
> È consigliabile che l'integrati ```this.appContextService.node``` viene usato per chiamare un'API definito nel plug-in gateway personalizzato. Ciò garantisce che se le credenziali sono necessari all'interno di plug-in gateway che essi verranno gestite correttamente.

### Modificare module.ts

Apri il ```module.ts``` file del nuovo modulo creato in precedenza (ad esempio ```{!Module-Name}.module.ts```):

Aggiungi le istruzioni di importazione seguenti:

``` ts
import { HttpService } from '@microsoft/windows-admin-center-sdk/angular';
import { Http } from '@microsoft/windows-admin-center-sdk/core';
import { PluginService } from './plugin.service';
```

Aggiungere i provider seguenti (dopo dichiarazioni):

``` ts
  ,
  providers: [
    HttpService,
    PluginService,
    Http
  ]
```

### Modificare component.ts

Apri il ```component.ts``` file del nuovo modulo creato in precedenza (ad esempio ```{!Module-Name}.component.ts```):

Aggiungi le istruzioni di importazione seguenti:

``` ts
import { ActivatedRouteSnapshot } from '@angular/router';
import { AppContextService } from '@microsoft/windows-admin-center-sdk/angular';
import { Subscription } from 'rxjs';
import { Strings } from '../../generated/strings';
import { PluginService } from './plugin.service';
```

Aggiungi le variabili seguenti:

``` ts
  private serviceSubscription: Subscription;
  private responseResult: string;
```

Modificare il costruttore e modificare/aggiungere le funzioni seguenti:

``` ts
  constructor(private appContextService: AppContextService, private plugin: PluginService) {
    //
  }

  public ngOnInit() {
    this.responseResult = 'click go to do something';
  }

  public onClick() {
    this.serviceSubscription = this.plugin.getGatewayRestResponse().subscribe(
      (response: any) => {
        this.responseResult = 'response: ' + response.message;
      },
      (error) => {
        console.log(error);
      }
    );
  }
```

### Modificare component.html ###

Apri il ```component.html``` file del nuovo modulo creato in precedenza (ad esempio ```{!Module-Name}.component.html```):

Aggiungi il contenuto seguente al file html:
``` html
<button (click)="onClick()" >go</button>
{{ responseResult }}
```

## Crea e Trasferisci localmente estensione

Ora sei pronto per [creare e trasferire localmente carico](..\develop-tool.md#build-and-side-load-your-extension) l'estensione in Windows Admin Center.
