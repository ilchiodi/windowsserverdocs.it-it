---
title: Utilizzare un plug-in del gateway personalizzato nell'estensione dello strumento
description: "Sviluppare un'estensione dello strumento Windows Admin Center SDK (Project Honolulu): usare un plug-in del gateway personalizzato nell'estensione dello strumento"
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 5bcaaa452a2b42a54cbc3b1d8f9a296504054e34
ms.sourcegitcommit: 20d07170c7f3094c2fb4455f54b13ec4b102f2d7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/13/2020
ms.locfileid: "81269228"
---
# <a name="use-a-custom-gateway-plugin-in-your-tool-extension"></a>Utilizzare un plug-in del gateway personalizzato nell'estensione dello strumento

>Si applica a: Windows Admin Center, Windows Admin Center Preview

In questo articolo verrà usato un plug-in del gateway personalizzato in una nuova estensione dello strumento vuota creata con l'interfaccia della riga di comando di Windows Admin Center.

## <a name="prepare-your-environment"></a>Preparazione dell'ambiente ##

Se non è già stato fatto, seguire le istruzioni riportate in [sviluppare un'estensione dello strumento](../develop-tool.md) per preparare l'ambiente e creare una nuova estensione dello strumento vuota.

## <a name="add-a-module-to-your-project"></a>Aggiungere un modulo al progetto ##

Se non è già stato fatto, aggiungere un nuovo [modulo vuoto](add-module.md) al progetto, che verrà usato nel passaggio successivo.  

## <a name="add-integration-to-custom-gateway-plugin"></a>Aggiungere l'integrazione al plug-in gateway personalizzato ##

A questo punto verrà usato un plug-in del gateway personalizzato nel nuovo modulo vuoto appena creato.

### <a name="create-pluginservicets"></a>Creare plugin. Service. TS

Passare alla directory del nuovo modulo di strumenti creato in precedenza (```\src\app\{!Module-Name}```) e creare un nuovo file ```plugin.service.ts```.

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

Modificare i riferimenti a ```Sample Uno``` e ```Sample%20Uno``` al nome della funzionalità nel modo appropriato.

> [!WARNING]
> È consigliabile usare il ```this.appContextService.node``` incorporato per chiamare qualsiasi API definita nel plug-in del gateway personalizzato. In questo modo, se le credenziali sono necessarie all'interno del plug-in del gateway, verranno gestite correttamente.

### <a name="modify-modulets"></a>Modificare Module. TS

Aprire il file di ```module.ts``` del nuovo modulo creato in precedenza (ad esempio ```{!Module-Name}.module.ts```):

Aggiungere le istruzioni Import seguenti:

``` ts
import { HttpService } from '@microsoft/windows-admin-center-sdk/angular';
import { Http } from '@microsoft/windows-admin-center-sdk/core';
import { PluginService } from './plugin.service';
```

Aggiungere i seguenti provider (dopo le dichiarazioni):

``` ts
  ,
  providers: [
    HttpService,
    PluginService,
    Http
  ]
```

### <a name="modify-componentts"></a>Modificare Component. TS

Aprire il file di ```component.ts``` del nuovo modulo creato in precedenza (ad esempio ```{!Module-Name}.component.ts```):

Aggiungere le istruzioni Import seguenti:

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

### <a name="modify-componenthtml"></a>Modificare Component. html ###

Aprire il file di ```component.html``` del nuovo modulo creato in precedenza (ad esempio ```{!Module-Name}.component.html```):

Aggiungere il contenuto seguente al file HTML:
``` html
<button (click)="onClick()" >go</button>
{{ responseResult }}
```

## <a name="build-and-side-load-your-extension"></a>Compilazione e caricamento laterale dell'estensione

A questo punto è possibile [compilare e caricare](../develop-tool.md#build-and-side-load-your-extension) il proprio interno nell'interfaccia di amministrazione di Windows.
