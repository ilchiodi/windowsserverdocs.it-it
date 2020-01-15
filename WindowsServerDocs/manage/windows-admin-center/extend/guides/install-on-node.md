---
title: Sviluppare un'estensione dello strumento
description: Sviluppare un'estensione dello strumento Windows Admin Center SDK (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 3a93a1105862ffbf4fcbd1d23b15d9bcaa6010dc
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950508"
---
# <a name="install-extension-payload-on-a-managed-node"></a>Installare il payload dell'estensione in un nodo gestito

>Si applica a: Windows Admin Center, Windows Admin Center Preview

## <a name="setup"></a>Installazione
> [!NOTE]
> Per seguire questa guida, è necessario compilare 1.2.1904.02001 o versione successiva. Per controllare il numero di build, aprire l'interfaccia di amministrazione di Windows e fare clic sul punto interrogativo in alto a destra.

Se non è già stato fatto, creare un' [estensione dello strumento](../develop-tool.md) per l'interfaccia di amministrazione di Windows. Una volta completata questa operazione, prendere nota dei valori utilizzati durante la creazione di un'estensione:

| Value | Spiegazione | Esempio |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nome della società (con spazi) | ```Contoso``` |
| ```{!Tool Name}``` | Nome dello strumento (con spazi) | ```InstallOnNode``` |

All'interno della cartella dell'estensione dello strumento creare una cartella di ```Node``` (```{!Tool Name}\Node```). Qualsiasi elemento inserito in questa cartella verrà copiato nel nodo gestito quando si usa questa API. Aggiungere tutti i file necessari per il caso d'uso. 

Creare anche uno script ```{!Tool Name}\Node\installNode.ps1```. Questo script verrà eseguito nel nodo gestito dopo che tutti i file vengono copiati dalla cartella ```{!Tool Name}\Node``` al nodo gestito. Aggiungere qualsiasi logica aggiuntiva per il caso d'uso. Un esempio di file di ```{!Tool Name}\Node\installNode.ps1```:

``` ps1
# Add logic for installing payload on managed node
echo 'Success'
```

> [!NOTE]
> ```{!Tool Name}\Node\installNode.ps1``` ha un nome specifico che sarà ricercato dall'API. Se si modifica il nome di questo file, verrà generato un errore.


## <a name="integration-with-ui"></a>Integrazione con l'interfaccia utente

Aggiornare ```\src\app\default.component.ts``` al seguente:

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
Aggiornare i segnaposto ai valori usati durante la creazione dell'estensione:
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

Aggiornare anche ```\src\app\default.component.html``` a:
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

L'ultimo passaggio consiste nel creare un pacchetto NuGet con i file aggiunti e quindi nell'installare il pacchetto nell'interfaccia di amministrazione di Windows.

Se non è stato creato un pacchetto di estensione prima, attenersi alla Guida alle [estensioni di pubblicazione](../publish-extensions.md) . 
> [!IMPORTANT]
> Nel file. nuspec per questa estensione è importante che il valore ```<id>``` corrisponda al nome nel ```manifest.json``` del progetto e che il ```<version>``` corrisponda a ciò che è stato aggiunto a ```\src\app\default.component.ts```. Aggiungere inoltre una voce in ```<files>```: 
> 
> ```<file src="Node\**\*.*" target="Node" />```.

``` xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="https://schemas.microsoft.com/packaging/2011/08/nuspec.xsd">
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

Una volta creato il pacchetto, aggiungere un percorso al feed. Nel centro di amministrazione di Windows passare a Impostazioni > estensioni > feed e aggiungere il percorso in cui è presente il pacchetto. Al termine dell'installazione dell'estensione, sarà possibile fare clic sul pulsante ```install``` e verrà chiamata l'API.  