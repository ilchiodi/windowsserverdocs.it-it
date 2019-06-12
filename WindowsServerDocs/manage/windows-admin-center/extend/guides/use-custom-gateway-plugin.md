---
title: Utilizzare un plug-in del gateway personalizzato nell'estensione dello strumento
description: Sviluppare un'estensione per strumento Windows Admin Center SDK (progetto Honolulu) - usare un plug-in gateway personalizzato nella propria estensione degli strumenti
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 348ebf5b99de7f582a3edf57b0a190f87f1c4a5b
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452606"
---
# <a name="use-a-custom-gateway-plugin-in-your-tool-extension"></a>Utilizzare un plug-in del gateway personalizzato nell'estensione dello strumento

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

In questo articolo si userà un plug-in di gateway personalizzato in un'estensione per strumento nuovo e vuoto che sono stati creati con la CLI di Windows Admin Center.

## <a name="prepare-your-environment"></a>Preparazione dell'ambiente ##

Se hai già fatto, seguire le istruzioni disponibili nel [sviluppare un'estensione per strumento](../develop-tool.md) per preparare l'ambiente e creare un nuovo, vuoto estensione degli strumenti.

## <a name="add-a-module-to-your-project"></a>Aggiungere un modulo al progetto ##

Se hai già fatto, aggiungere un nuovo [modulo vuoto](add-module.md) al progetto, che verrà usato nel passaggio successivo.  

## <a name="add-integration-to-custom-gateway-plugin"></a>Aggiungere l'integrazione al plug-in gateway personalizzato ##

A questo punto si userà un plug-in gateway personalizzato nel modulo nuovo e vuoto appena creato.

### <a name="create-pluginservicets"></a>Creare plugin.service.ts

Passare alla directory del nuovo modulo strumento creato in precedenza (```\src\app\{!Module-Name}```) e creare un nuovo file ```plugin.service.ts```.

Aggiungere il codice seguente al file appena creato:
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

Modificare i riferimenti a ```Sample Uno``` e ```Sample%20Uno``` al nome di funzionalità come appropriato.

[!WARNING]
> È consigliabile che incorporato ```this.appContextService.node``` viene usata per chiamare qualsiasi API che è definito nel plug-in gateway personalizzato. Questo garantisce che se le credenziali sono necessarie all'interno del plug-in gateway che verranno gestiti correttamente.

### <a name="modify-modulets"></a>Modificare Module

Aprire il ```module.ts``` file del nuovo modulo creato in precedenza (ad esempio ```{!Module-Name}.module.ts```):

Aggiungere le istruzioni import seguenti:

``` ts
import { HttpService } from '@microsoft/windows-admin-center-sdk/angular';
import { Http } from '@microsoft/windows-admin-center-sdk/core';
import { PluginService } from './plugin.service';
```

Aggiungere i provider seguenti (dopo le dichiarazioni):

``` ts
  ,
  providers: [
    HttpService,
    PluginService,
    Http
  ]
```

### <a name="modify-componentts"></a>Modificare component.ts

Aprire il ```component.ts``` file del nuovo modulo creato in precedenza (ad esempio ```{!Module-Name}.component.ts```):

Aggiungere le istruzioni import seguenti:

``` ts
import { ActivatedRouteSnapshot } from '@angular/router';
import { AppContextService } from '@microsoft/windows-admin-center-sdk/angular';
import { Subscription } from 'rxjs';
import { Strings } from '../../generated/strings';
import { PluginService } from './plugin.service';
```

Aggiungere le variabili seguenti:

``` ts
  private serviceSubscription: Subscription;
  private responseResult: string;
```

Modificare il costruttore e modificare o aggiungere le funzioni seguenti:

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

### <a name="modify-componenthtml"></a>Modificare component.html ###

Aprire il ```component.html``` file del nuovo modulo creato in precedenza (ad esempio ```{!Module-Name}.component.html```):

Il file html, aggiungere il contenuto seguente:
``` html
<button (click)="onClick()" >go</button>
{{ responseResult }}
```

## <a name="build-and-side-load-your-extension"></a>Compilazione e sul lato caricare l'estensione

A questo punto si è pronti per [compilazione e sul lato carico](../develop-tool.md#build-and-side-load-your-extension) dell'estensione in Windows Admin Center.
