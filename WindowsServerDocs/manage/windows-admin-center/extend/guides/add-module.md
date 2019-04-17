---
title: Aggiungere un modulo a un'estensione dello strumento
description: "Sviluppare un'estensione dello strumento Windows Admin Center SDK (Project Honolulu): aggiungere un modulo a un'estensione dello strumento"
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: e6978ce20a7c6da8addb217de8d30f733b40d261
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081208"
---
# Aggiungere un modulo a un'estensione dello strumento

>Si applica a: Windows Admin Center, Windows Admin Center Preview

In questo articolo, aggiungeremo un modulo vuoto per un'estensione dello strumento che abbiamo creato con l'interfaccia CLI di Windows Admin Center.

## Preparazione dell'ambiente

Se hai già fatto, segui le istruzioni nel sviluppano un'estensione [dello strumento](..\develop-tool.md) (o [soluzione](..\develop-solution.md)) per preparare l'ambiente e creare un'estensione dello strumento di nuovo e vuoto.

## Usa l'interfaccia CLI Angular per creare un modulo (e componente)

Se non conosci Angular, è consigliabile leggere la documentazione sul sito Web Angular.Io per ulteriori informazioni su Angular e NgModule. Per ulteriori informazioni su NgModule, vai qui: https://angular.io/guide/ngmodule

* Ulteriori informazioni sulla creazione di un nuovo modulo in CLI Angular: https://github.com/angular/angular-cli/wiki/generate-module
* Ulteriori informazioni sulla creazione di un nuovo componente in CLI Angular: https://github.com/angular/angular-cli/wiki/generate-component


Apri un prompt dei comandi, passare alla \src\app nel tuo progetto, quindi Esegui i comandi seguenti, sostituendo ```{!ModuleName}``` con il nome del modulo (spazi rimossi):

```
cd \src\app
ng generate module {!ModuleName}
ng generate component {!ModuleName}
```

| Valore | Spiegazione | Esempio |
| ----- | ----------- | ------- |
| ```{!ModuleName}``` | Nome del modulo (spazi rimossi) | ```ManageFooWorksPortal``` |

Esempio d'uso:
```
cd \src\app
ng generate module ManageFooWorksPortal
ng generate component ManageFooWorksPortal
```


## Aggiungere le informazioni di routing

Se non conosci Angular, è altamente consigliabile ottenere informazioni sulla navigazione e il routing di Angular. Le sezioni seguenti definiscono elementi di routing necessari che consentono a Windows Admin Center di accedere alla tua estensione e spostarsi tra le visualizzazioni nell'estensione in risposta all'attività dell'utente. Per ulteriori informazioni, vai qui: https://angular.io/guide/router

Usa lo stesso nome di modulo utilizzato nel passaggio precedente.

### Aggiungere contenuto al nuovo file di routing

* Accedi alla cartella del modulo creata da ``` ng generate ``` nel passaggio precedente.

* Crea un nuovo file ```{!module-name}.routing.ts```, in base a questa convenzione di denominazione:

    | Valore | Spiegazione | Nome del file di esempio |
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

    | Valore | Spiegazione | Esempio |
    | ----- | ----------- | ------- |
    | ```{!ModuleName}``` | Nome del modulo (spazi rimossi) | ```ManageFooWorksPortal``` |
    | ```{!module-name}``` | Nome del modulo (lettere minuscole, spazi sostituiti da trattini) | ```manage-foo-works-portal``` |

### Aggiungere contenuto al nuovo file di modulo

Apri il file ```{!module-name}.module.ts``` trovato in base a questa convenzione di denominazione:

| Valore | Spiegazione | Nome del file di esempio |
| ----- | ----------- | ------- |
| ```{!module-name}``` | Nome del modulo (lettere minuscole, spazi sostituiti da trattini) | ```manage-foo-works-portal.module.ts``` |

* Aggiungi contenuto al file:

    ``` ts
    import { Routing } from './{!module-name}.routing';
    ```

* Sostituisci i valori nel contenuto appena aggiunto con i valori desiderati:

    | Valore | Spiegazione | Esempio |
    | ----- | ----------- | ------- |
    | ```{!module-name}``` | Nome del modulo (lettere minuscole, spazi sostituiti da trattini) | ```manage-foo-works-portal``` |

* Modifica l'istruzione di importazione per importare il routing:

    | Valore originale | Nuovo valore |
    | -------------- | --------- |
    | ```imports: [ CommonModule ]``` | ```imports: [ CommonModule, Routing ]``` |

* Verifica che le istruzioni ```import``` siano in ordine alfabetico in base all'origine.

### Aggiungere contenuto al nuovo file di typescript componente

Apri il file ```{!module-name}.component.ts``` trovato in base a questa convenzione di denominazione:

| Valore | Spiegazione | Nome del file di esempio |
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
### Aggiornare app routing.module.ts

Apri il file ```app-routing.module.ts```e modificare il percorso predefinito, in modo che verrà caricato il nuovo modulo appena creato.  Trovare la voce per ```path: ''```e aggiorna ```loadChildren``` per caricare il modulo invece il modulo predefinito:

| Valore | Spiegazione | Esempio |
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


## Crea e Trasferisci localmente estensione

Ora hai aggiunto un modulo per l'estensione.  Successivamente, è possibile [creare e trasferire localmente carico](..\develop-tool.md#build-and-side-load-your-extension) l'estensione in Windows Admin Center per visualizzare i risultati.
