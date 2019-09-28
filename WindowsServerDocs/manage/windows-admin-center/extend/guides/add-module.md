---
title: Aggiungere un modulo a un'estensione dello strumento
description: Sviluppare un'estensione dello strumento Windows Admin Center SDK (Project Honolulu)-aggiungere un modulo a un'estensione dello strumento
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 9d30980ca404187ff1481242c1c0ef0a3d571416
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357102"
---
# <a name="add-a-module-to-a-tool-extension"></a>Aggiungere un modulo a un'estensione dello strumento

>Si applica a: Windows Admin Center, Windows Admin Center Preview

In questo articolo verrà aggiunto un modulo vuoto a un'estensione dello strumento creata con l'interfaccia della riga di comando di Windows Admin Center.

## <a name="prepare-your-environment"></a>Preparazione dell'ambiente

Se non è già stato fatto, seguire le istruzioni riportate nell'estensione sviluppare uno [strumento](../develop-tool.md) o una [soluzione](../develop-solution.md)per preparare l'ambiente e creare una nuova estensione dello strumento vuota.

## <a name="use-the-angular-cli-to-create-a-module-and-component"></a>Usare l'interfaccia della riga di comando angolare per creare un modulo (e componente)

Se non conosci Angular, è consigliabile leggere la documentazione sul sito Web Angular.Io per ulteriori informazioni su Angular e NgModule. Per ulteriori informazioni su NgModule, vai qui: https://angular.io/guide/ngmodule

* Ulteriori informazioni sulla creazione di un nuovo modulo in CLI Angular: https://github.com/angular/angular-cli/wiki/generate-module
* Ulteriori informazioni sulla creazione di un nuovo componente in CLI Angular: https://github.com/angular/angular-cli/wiki/generate-component


Aprire un prompt dei comandi, passare alla directory \src\app nel progetto, quindi eseguire i comandi seguenti, sostituendo ```{!ModuleName}``` con il nome del modulo (spazi rimossi):

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

### <a name="add-content-to-new-component-typescript-file"></a>Aggiungi contenuto al nuovo file typescript del componente

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
### <a name="update-app-routingmodulets"></a>Aggiornare l'app-routing. Module. TS

Aprire il file ```app-routing.module.ts``` e modificare il percorso predefinito in modo che venga caricato il nuovo modulo appena creato.  Trovare la voce per ```path: ''``` e aggiornare ```loadChildren``` per caricare il modulo anziché il modulo predefinito:

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
Di seguito è riportato un esempio di un percorso predefinito aggiornato:
``` ts
    {
        path: '', 
        loadChildren: 'app/manage-foo-works-portal/manage-foo-works-portal.module#ManageFooWorksPortalModule'
    },
```


## <a name="build-and-side-load-your-extension"></a>Compilazione e caricamento laterale dell'estensione

A questo punto è stato aggiunto un modulo all'estensione.  Successivamente, è possibile [compilare e caricare](../develop-tool.md#build-and-side-load-your-extension) il proprio interno nell'interfaccia di amministrazione di Windows per visualizzare i risultati.
