---
title: Sviluppare un'estensione dello strumento
description: Sviluppare un'estensione per strumento Windows Admin Center SDK (progetto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 1a068c0d33887e8e9287ff15c1aa14f3dc84915a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445929"
---
# <a name="install-extension-payload-on-a-managed-node"></a>Installare il payload di estensione in un nodo gestito

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

## <a name="setup"></a>Configurazione
> [!NOTE]
> Per seguire questa Guida, è necessario compilerà 1.2.1904.02001 o versione successiva. Per controllare la build numero aprire Windows Admin Center e fare clic sul punto interrogativo in alto a destra.

Se hai già fatto, creare un [dello strumento di estensione](../develop-tool.md) per Windows Admin Center. Dopo aver completato questa marca nota dei valori usati durante la creazione di un'estensione:

| Value | Spiegazione | Esempio |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Il nome della società (spazi inclusi) | ```Contoso``` |
| ```{!Tool Name}``` | Il nome dello strumento (spazi inclusi) | ```InstallOnNode``` |

All'interno della cartella dell'estensione degli strumenti creato un ```Node``` cartella (```{!Tool Name}\Node```). Qualsiasi elemento posizionato in questa cartella verrà copiato nel nodo gestito quando si utilizza questa API. Aggiungere eventuali file necessari per il caso d'uso. 

Creare anche un ```{!Tool Name}\Node\installNode.ps1``` script. Questo script vengono eseguiti nel nodo gestito dopo che tutti i file vengono copiati dal ```{!Tool Name}\Node``` cartella al nodo gestito. Aggiungere alcuna logica aggiuntiva per il caso d'uso. Un esempio ```{!Tool Name}\Node\installNode.ps1``` file:

``` ps1
# Add logic for installing payload on managed node
echo 'Success'
```

> [!NOTE]
> ```{!Tool Name}\Node\installNode.ps1``` include un nome specifico che cerca le API. La modifica del nome di questo file verrà generato un errore.


## <a name="integration-with-ui"></a>Integrazione con l'interfaccia utente

Aggiornamento ```\src\app\default.component.ts``` al seguente:

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
Aggiornare i segnaposto per i valori utilizzati durante la creazione dell'estensione:
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

Aggiornare anche ```\src\app\default.component.html``` per:
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

## <a name="creating-and-installing-a-nuget-package"></a>Creazione e installazione di un pacchetto NuGet

L'ultimo passaggio è la creazione di un pacchetto NuGet con i file che sono stati aggiunti e quindi l'installazione di tale pacchetto in Windows Admin Center.

Seguire le [estensioni pubblicazione](../publish-extensions.md) Guida se non è stato creato un pacchetto di estensione prima. 
> [!IMPORTANT]
> Nel file con estensione nuspec per questa estensione, è importante che il ```<id>``` valore corrisponde al nome del progetto ```manifest.json``` e il ```<version>``` corrisponde a ciò che è stato aggiunto al ```\src\app\default.component.ts```. Aggiungere anche una voce sotto ```<files>```: 
> 
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

Dopo aver creato questo pacchetto, aggiungere un percorso a questo feed. In Windows Admin Center passare a Impostazioni > estensioni > feed e aggiungere il percorso in cui è presente tale pacchetto. Quando viene eseguita l'estensione viene installato, sarà possibile fare clic su di ```install``` pulsante e l'API verrà chiamato.  