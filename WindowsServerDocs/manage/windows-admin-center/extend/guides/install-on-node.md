---
title: Sviluppare un'estensione dello strumento
description: Sviluppare un'estensione dello strumento Windows Admin Center SDK (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 2269a2ac2cabda6f8fdd829994f36e89d581bd23
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297013"
---
# Installare il payload di estensione in un nodo gestito

>Si applica a: Windows Admin Center, Windows Admin Center Preview

## Installazione
> [!NOTE]
> Per seguire questa Guida, sarà necessario creare 1.2.1904.02001 o versione successiva. Per controllare la build numero Apri Windows Admin Center e fai clic sul punto interrogativo in alto a destra.

Se hai già fatto, creare un' [estensione dello strumento](../develop-tool.md) di Windows Admin Center. Dopo aver completato questa marca nota dei valori usati durante la creazione di un'estensione:
| Valore | Spiegazione | Esempio |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Il nome della società (con spazi) | ```Contoso``` |
| ```{!Tool Name}``` | Il nome dello strumento (con spazi) | ```InstallOnNode``` |

All'interno della cartella dell'estensione dello strumento crea un ```Node``` cartella (```{!Tool Name}\Node```). Qualsiasi elemento posizionato in questa cartella verrà copiato il nodo gestito quando si utilizza questa API. Aggiungere i file necessari per le tue esigenze. 

Creare anche un ```{!Tool Name}\Node\installNode.ps1``` script. Questo script sarà eseguito sul nodo gestito dopo tutti i file copiati dal ```{!Tool Name}\Node``` cartella al nodo gestito. Aggiungere logica aggiuntiva per le tue esigenze. Un esempio ```{!Tool Name}\Node\installNode.ps1``` file:

``` ps1
# Add logic for installing payload on managed node
echo 'Success'
```

> [!NOTE]
> ```{!Tool Name}\Node\installNode.ps1``` ha un nome specifico che avrà un aspetto per l'API. Modifica del nome di questo file causerà un errore.


## Integrazione con l'interfaccia utente

Aggiornamento ```\src\app\default.component.ts``` per il comando seguente:

``` ts
import { Component } from '@angular/core';
import { AppContextService } from '@microsoft/windows-admin-center-sdk/angular';
import { Observable } from 'rxjs';

@Component({
  selector: 'default-component',
  templateUrl: './default.component.html',
  styleUrls: ['./default.component.css']
})

export class DefaultComponent {
  constructor(private appContextService: AppContextService) { }

  public response: any;
  public loading = false;

  public installOnNode() {
    this.loading = true;
    this.post('{!Company Name}.{!Tool Name}', '1.0.0',
      this.appContextService.activeConnection.nodeName).subscribe(
        (response: any) => {
          console.log(response);
          this.response = response;
          this.loading = false;
        },
        (error) => {
          console.log(error);
          this.response = error;
          this.loading = false;
        }
      );
  }

  public post(id: string, version: string, targetNode: string): Observable<any> {
    return this.appContextService.node.post(targetNode,
      `features/extensions/${id}/versions/${version}/install`);
  }

}
```
Aggiornare segnaposto con i valori che sono stati usati durante la creazione dell'estensione:
``` ts
this.post('contoso.install-on-node', '1.0.0',
      this.appContextService.activeConnection.nodeName).subscribe(
        (response: any) => {
          console.log(response);
          this.response = response;
          this.loading = false;
        },
        (error) => {
          console.log(error);
          this.response = error;
          this.loading = false;
        }
      );
```

Aggiorna anche ```\src\app\default.component.html``` per:
``` html
<button (click)="installOnNode()">Click to install</button>
<sme-loading-wheel *ngIf="loading" size="large"></sme-loading-wheel>
<p *ngIf="response">{{response}}</p>
```
E infine ```\src\app\default.module.ts```:
``` ts
import { CommonModule } from '@angular/common';
import { NgModule } from '@angular/core';

import { LoadingWheelModule } from '@microsoft/windows-admin-center-sdk/angular';
import { DefaultComponent } from './default.component';
import { Routing } from './default.routing';

@NgModule({
  imports: [
    CommonModule,
    LoadingWheelModule,
    Routing
  ],
  declarations: [DefaultComponent]
})
export class DefaultModule { }

```

## Creazione e l'installazione di un pacchetto NuGet

L'ultimo passaggio è la creazione di un pacchetto NuGet con i file che abbiamo aggiunto e quindi l'installazione di tale pacchetto in Windows Admin Center.

Se non hai creato un pacchetto di estensione prima, segui la Guida alla [Pubblicazione delle estensioni](../publish-extensions.md) . 
> [!IMPORTANT]
> Nel file .nuspec per questa estensione, è importante che il ```<id>``` valore corrispondente al nome del progetto ```manifest.json``` e ```<version>``` corrisponde a ciò che è stato aggiunto al ```\src\app\default.component.ts```. Anche aggiungere una voce nella sezione ```<files>```: 

> ```<file src="Node\**\*.*" target="Node" />```.

``` xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2011/08/nuspec.xsd">
  <metadata>
    <id>contoso.install-on-node</id>
    <version>1.0.0</version>
    <authors>Contoso</authors>
    <owners>Contoso</owners>
    <requireLicenseAcceptance>false</requireLicenseAcceptance>
    <projectUrl>https://msft-sme.myget.org/feed/windows-admin-center-feed/package/nuget/contoso.sme.install-on-node-extension</projectUrl>
    <licenseUrl>http://YourLicenseLink</licenseUrl>
    <iconUrl>http://YourLogoLink</iconUrl>
    <description>Install on node extension by Contoso</description>
    <copyright>(c) Contoso. All rights reserved.</copyright> 
  </metadata>
    <files>
    <file src="bundle\**\*.*" target="ux" />
    <file src="package\**\*.*" target="gateway" />
    <file src="Node\**\*.*" target="Node" />
  </files>
</package>
```

Dopo aver creato il pacchetto, aggiungere un percorso a tale feed. In Windows Admin Center Vai a Impostazioni gt _ estensioni gt _ feed e aggiungere il percorso in cui è presente che il pacchetto. Quando viene eseguita l'estensione in fase di installazione, dovresti essere in grado di fare clic sul ```install``` pulsante e l'API verrà chiamato.  