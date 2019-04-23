---
title: Aggiungere un modulo a un'estensione dello strumento
description: Sviluppare un'estensione per strumento Windows Admin Center SDK (progetto Honolulu) - aggiungere un modulo a un'estensione degli strumenti
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: e6978ce20a7c6da8addb217de8d30f733b40d261
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834402"
---
# <a name="add-a-module-to-a-tool-extension"></a>Aggiungere un modulo a un'estensione dello strumento

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

In questo articolo, si aggiungerà un modulo vuoto per un'estensione degli strumenti che è stata creata con la CLI di Windows Admin Center.

## <a name="prepare-your-environment"></a>Preparazione dell'ambiente

Se hai già fatto, seguire le istruzioni di sviluppare un [lo strumento](..\develop-tool.md) (o [soluzione](..\develop-solution.md)) estensione a preparare l'ambiente e creare un'estensione per strumento nuovo e vuoto.

## <a name="use-the-angular-cli-to-create-a-module-and-component"></a>Usare il comando di Angular per creare un modulo (e component)

Se non conosci Angular, è consigliabile leggere la documentazione sul sito Web Angular.Io per ulteriori informazioni su Angular e NgModule. Per ulteriori informazioni su NgModule, vai qui: https://angular.io/guide/ngmodule

* Ulteriori informazioni sulla creazione di un nuovo modulo in CLI Angular: https://github.com/angular/angular-cli/wiki/generate-module
* Ulteriori informazioni sulla creazione di un nuovo componente in CLI Angular: https://github.com/angular/angular-cli/wiki/generate-component


Aprire un prompt dei comandi, passare alla \src\app nel progetto, quindi eseguire i comandi seguenti, sostituendo ```{!ModuleName}``` con il nome del modulo (spazi rimossi):

```
cd \src\app
ng generate module {!ModuleName}
ng generate component {!ModuleName}
```

| Value | Spiegazione | Esempio |
| ----- | ----------- | ------- |
| ```{!ModuleName}``` | Nome del modulo (spazi rimossi) | ```ManageFooWorksPortal``` |

Esempio d'uso:
```
cd \src\app
ng generate module ManageFooWorksPortal
ng generate component ManageFooWorksPortal
```


## <a name="add-routing-information"></a>Aggiungere le informazioni di routing

Se non conosci Angular, è altamente consigliabile ottenere informazioni sulla navigazione e il routing di Angular. Le sezioni seguenti definiscono elementi di routing necessari che consentono a Windows Admin Center di accedere alla tua estensione e spostarsi tra le visualizzazioni nell'estensione in risposta all'attività dell'utente. Per ulteriori informazioni, vai qui: https://angular.io/guide/router

Usare lo stesso nome di modulo usato nel passaggio precedente.

### <a name="add-content-to-new-routing-file"></a>Aggiungere contenuto al nuovo file di routing

* Accedi alla cartella del modulo creata da ``` ng generate ``` nel passaggio precedente.

* Crea un nuovo file ```{!module-name}.routing.ts```, in base a questa convenzione di denominazione:

    | Value | Spiegazione | Nome del file di esempio |
    | ----- | ----------- | ------- |
    | ```{!module-name}``` | Nome del modulo (lettere minuscole, spazi sostituiti da trattini) | ```manage-foo-works-portal.routing.ts``` |

* Aggiungi questo contenuto al file appena creato:

    ``` ts
    import { NgModule } from '@angular/core';
    import { RouterModule, Routes } from '@angular/router';
    import { {!ModuleName}Component } from './{!module-name}.component';

    const routes: Routes = [
        {
            path: '',
            component: {!ModuleName}Component,
            // if the component has child components that need to be routed to, include them in the children array.
            children: [
                {
                    path: '', 
                    redirectTo: 'base',
                    pathMatch: 'full'
                }
            ]
    }];

    @NgModule({
        imports: [
            RouterModule.forChild(routes)
        ],
        exports: [
            RouterModule
        ]
    })
    export class Routing { }
    ```

* Sostituisci i valori nel file appena creato con i valori desiderati:

    | Value | Spiegazione | Esempio |
    | ----- | ----------- | ------- |
    | ```{!ModuleName}``` | Nome del modulo (spazi rimossi) | ```ManageFooWorksPortal``` |
    | ```{!module-name}``` | Nome del modulo (lettere minuscole, spazi sostituiti da trattini) | ```manage-foo-works-portal``` |

### <a name="add-content-to-new-module-file"></a>Aggiungere contenuto al nuovo file di modulo

Apri il file ```{!module-name}.module.ts``` trovato in base a questa convenzione di denominazione:

| Value | Spiegazione | Nome del file di esempio |
| ----- | ----------- | ------- |
| ```{!module-name}``` | Nome del modulo (lettere minuscole, spazi sostituiti da trattini) | ```manage-foo-works-portal.module.ts``` |

* Aggiungi contenuto al file:

    ``` ts
    import { Routing } from './{!module-name}.routing';
    ```

* Sostituisci i valori nel contenuto appena aggiunto con i valori desiderati:

    | Value | Spiegazione | Esempio |
    | ----- | ----------- | ------- |
    | ```{!module-name}``` | Nome del modulo (lettere minuscole, spazi sostituiti da trattini) | ```manage-foo-works-portal``` |

* Modifica l'istruzione di importazione per importare il routing:

    | Valore originale | Nuovo valore |
    | -------------- | --------- |
    | ```imports: [ CommonModule ]``` | ```imports: [ CommonModule, Routing ]``` |

* Verifica che le istruzioni ```import``` siano in ordine alfabetico in base all'origine.

### <a name="add-content-to-new-component-typescript-file"></a>Aggiungere contenuto al nuovo file del componente typescript

Apri il file ```{!module-name}.component.ts``` trovato in base a questa convenzione di denominazione:

| Value | Spiegazione | Nome del file di esempio |
| ----- | ----------- | ------- |
| ```{!module-name}``` | Nome del modulo (lettere minuscole, spazi sostituiti da trattini) | ```manage-foo-works-portal.component.ts``` |
    
Modifica il contenuto del file in base a quanto segue:

``` ts
constructor() {
    // TODO
}

public ngOnInit() {
    // TODO
}
```
### <a name="update-app-routingmodulets"></a>Aggiornare app routing.module.ts

Apri file ```app-routing.module.ts```e modificare il percorso predefinito in modo che caricherà il nuovo modulo appena creato.  Trovare la voce relativa ```path: ''```e aggiornare ```loadChildren``` per caricare il modulo invece il modulo predefinito:

| Value | Spiegazione | Esempio |
| ----- | ----------- | ------- |
| ```{!ModuleName}``` | Nome del modulo (spazi rimossi) | ```ManageFooWorksPortal``` |
| ```{!module-name}``` | Nome del modulo (lettere minuscole, spazi sostituiti da trattini) | ```manage-foo-works-portal``` |

``` ts
    {
        path: '', 
        loadChildren: 'app/{!module-name}/{!module-name}.module#{!ModuleName}Module'
    },
```
Ecco un esempio di un percorso predefinito aggiornato:
``` ts
    {
        path: '', 
        loadChildren: 'app/manage-foo-works-portal/manage-foo-works-portal.module#ManageFooWorksPortalModule'
    },
```


## <a name="build-and-side-load-your-extension"></a>Compilazione e sul lato caricare l'estensione

Viene aggiunto un modulo all'estensione.  Successivamente, è possibile [compilazione e sul lato carico](..\develop-tool.md#build-and-side-load-your-extension) dell'estensione in Windows Admin Center per visualizzare i risultati.
