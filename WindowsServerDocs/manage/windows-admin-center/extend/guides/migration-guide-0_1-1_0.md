---
title: Eseguire la migrazione da Windows Admin Center SDK 0,1 a 1,0
description: Questa guida consentirà di eseguire la migrazione da Windows Admin Center SDK versione 0,1 a 1,0
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 02/26/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ba0e8cda35c51763b5c10b89c76e2bf07e064dfd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886472"
---
# <a name="migrate-from-windows-admin-center-sdk-01-to-10"></a>Eseguire la migrazione da Windows Admin Center SDK 0,1 a 1,0

>Si applica a: Anteprima di Windows Admin Center

Questa guida consentirà di eseguire la migrazione da Windows Admin Center SDK versione 0,1 a 1,0.  

## <a name="1-learn-about-new-controls-with-the-dev-guide-extension"></a>1. Informazioni sui nuovi controlli con l'estensione della Guida di sviluppo

Windows Admin Center versione 1902 e versioni successive include il **Guida di sviluppo** estensione, che è possibile usare per trovare esempi di controlli (tra cui controlli appena disponibili) e gli scenari per la compilazione di un'estensione personalizzata.  Guida di sviluppo sostituisce il **strumenti di sviluppo** estensione da versioni precedenti del SDK.

### <a name="use-the-dev-guide-in-windows-admin-center"></a>Usare la Guida di sviluppo in Windows Admin Center

Guida di sviluppo è disponibile come una soluzione nella versione di Windows Admin Center 1902 e versioni successive.  Guida di sviluppo è pre-installato, ma deve essere abilitata tramite le impostazioni.

**Abilitare la Guida di sviluppo in Windows Admin Center:**

* Aprire Windows Admin Center (versione 1902 e versioni successive)
* Fare clic sui **impostazioni** sull'icona nell'angolo superiore destro della finestra
* Selezionare il **avanzate** scheda
* Sotto *chiavi esperimento*, fare clic su **Add**
* Immettere un nuovo valore ```msft.sme.shell.devguide``` nel campo vuoto, che è stato creato nel passaggio precedente
* Fare clic su **salvare e ricaricare**

**Aprire la Guida di sviluppo in Windows Admin Center:**

* Aprire Windows Admin Center (versione 1902 e versioni successive)
* Fare clic su elenco a discesa in alto a sinistra per visualizzare tutti i tipi di soluzione
* Selezionare il **Guida di sviluppo** soluzione 
    * Se non viene visualizzata la soluzione elencata, assicurarsi che sia stata abilitata la Guida di sviluppo (vedere la sezione precedente) e avere ricaricato Windows Admin Center.
* Esplorare il contenuto della Guida di sviluppo selezionando una delle schede
    * **Destinazione:** Contiene esempi di codice per *gestire come* e *notifica* scenari
    * **Controlli:** Contiene esempi di ogni controllo disponibile nel SDK
    * **Pipe:** Sono inclusi esempi di funzioni del convertitore e formattatore disponibili
    * **Stili:** Sono inclusi esempi di stili CSS disponibili nel SDK
    * **MsftSme:** Contiene esempi e indicazioni per scenari avanzati 

### <a name="browse-the-source-code-of-dev-guide-on-github"></a>Esaminare il codice sorgente della Guida di sviluppo su GitHub

È possibile esplorare le [codice sorgente](https://github.com/Microsoft/windows-admin-center-sdk/) della Guida di sviluppo in GitHub per trovare gli esempi di codice HTML, CSS e TypeScript di esempio.

## <a name="2-prepare-your-development-environment-for-the-latest-sdk"></a>2. Preparare l'ambiente di sviluppo per il SDK più recente

Installare o aggiornare la versione di Node. js [10.15.1 LTS o in un secondo momento](https://nodejs.org/en/).

Aggiornare l'interfaccia di Windows Admin Center alla versione più recente:

[//]: # "npm disinstallare -g windows-admin-center-cli@next"

``` cmd
npm uninstall -g windows-admin-center-cli
npm install -g windows-admin-center-cli
```

Aggiornare le dipendenze globale queste versioni:

``` cmd
npm install npm@6.4.1 -g
npm install @angular/cli@7.1.2 -g
npm install gulp -g
npm install typescript@3.1.6 -g
npm install tslint@5.11.0 -g
```

## <a name="3-create-a-new-project-with-the-latest-sdk"></a>3. Creare un nuovo progetto con il SDK più recente

Usare l'interfaccia CLI di Windows Admin Center per creare un nuovo progetto destinato al ```next``` versione SDK 1.0:

[//]: # "wac create--aziendale 'Contoso Inc": strumento 'Gestisci Foo Works', versione sperimentale"

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version next
```

Successivamente, passare alla cartella appena creata, quindi installare le dipendenze locali necessari eseguendo ```npm install ```.

## <a name="4-modify-an-existing-project-to-use-the-latest-sdk"></a>4. Modificare un progetto esistente per usare il SDK più recente

IMPORTANTE: Eseguire il backup del progetto prima di continuare.

Modificare la riga seguente nel ```package.json``` di destinazione di ```next``` versione SDK 1.0:

[//]: # "'@microsoft/windows-admin-center-sdk': 'sperimentale'"

``` json
"@microsoft/windows-admin-center-sdk": "next",
```

Eseguire quindi ```npm install``` per aggiornare i riferimenti in tutto il progetto.

## <a name="5-use-the-sdk-cli-to-fix-common-migration-issues"></a>5. Usare il SDK di CLI per risolvere problemi comuni di migrazione

IMPORTANTE: Eseguire il backup del progetto prima di continuare.

Dalla cartella radice del progetto, eseguire il seguente comando CLI sul progetto per risolvere automaticamente problemi comuni di migrazione:

``` cmd
wac updateSeven --update
```

Questo comando risolve automaticamente i problemi seguenti:

* Rigenera chiave ```package-lock.json```
* File di aggiornamento nell'ambiente di compilazione angular:
    - ```.gitignore```
    - ```tslint.json```
    - ```tsconfig.json```
    - ```tsconfig.inline.json```
    - ```ng-package.json```
    - ```angular.json```
    - ```src\tsconfig.app.json```
    - ```src\tsconfig.lib.json```
    - ```src\tsconfig.spec.json```

## <a name="6-use-the-sdk-cli-to-understand-common-migration-issues"></a>6. Usare il SDK di CLI per comprendere i problemi di migrazione comuni

Dalla cartella radice del progetto, eseguire il comando seguente per il progetto di controllo e individuare i problemi di migrazione comuni che devono essere risolti manualmente:

``` cmd
wac updateSeven --audit
```

Si troveranno le istanze dei seguenti problemi nel progetto:

### <a name="replace-usage-of-the-following-css-classes-with-these-sme-classes"></a>Sostituire l'utilizzo delle classi CSS seguenti con queste classi di esperti di dominio:

| Vecchia classe CSS | Nuova classe CSS |
| -- | -- |
| .Auto-flex-dimensioni |  .SME-posizione-flex-automatico |
| .Border-all |  .sme-border-color-base-90 e .sme-border-inset-Service Manager |
| .border-bottom |  sm .sme-bordo inferiore e .sme-border-bottom-color-base-90 |
| .border-horizontal |  sm .sme-bordo orizzontale e .sme-border-horizontal-color-base-90 |
| .border-left |  .SME-border-left-Service Manager e .sme-border-left-color-base-90 |
| .Border a destra |  .SME-border-right-Service Manager e .sme-border-right-color-base-90 |
| .border-top |  .SME-border-top-Service Manager e .sme-border-top-color-base-90 |
| .border-vertical |  .SME-border-vertical-Service Manager e .sme-border-vertical-color-base-90 |
| break-parola |  .sme-arrange-ws-wrap |
| .btn |  pulsante di .sme pulsante OR |
| .btn primario |  .button.sme OR .sme-button.sme-pulsante-primary-pulsante-primario |
| .Color scuro |  .SME-color-alt |
| .Color-light |  .SME-color-base |
| .color-light-gray |  .SME-color-base-90 |
| .fixed-flex-size |  .SME-posizione-flex-none |
| .flex-layout |  .SME-disposizione-stack-h o .sme-disposizione-stack-v |
| .font-bold |  .sme-font-emphasis1 |
| .Highlight |  .SME-background-color-giallo |
| .Horizontal |  .sme-arrange-stack-h |
| .no-scroll |  .SME-posizione-flex-automatico |
| .NoWrap |  .SME-disposizione-stack-h o .sme-disposizione-stack-v |
| .relative |  .sme-layout-relative |
| .relative-center |  .SME-layout-assoluto .sme-posizione-centro |
| reverse |  .SME-disposizione-stack-invertito |
| .stretch-absolute |  .SME-layout-assoluto .sme-posizione-inset-none |
| .stretch-fixed |  .SME layout fisso .sme-posizione-inset-none |
| .stretch-vertical |  .sme-position-stretch-v |
| .stretch-width |  .sme-position-stretch-h |
| .Vertical |  .SME-disposizione-stack-v |
| .Vertical-scroll-only |  .SME-disposizione-overflow-hide-x considerarvi-Disponi-overflow-auto-y |
| .Wrap |  .SME-disposizione-wrapstack-h o .sme-disposizione-wrapstack-v |

### <a name="replace-usage-of-the-following-components-with-these-sme-components"></a>Sostituire l'utilizzo dei componenti seguenti con questi componenti di esperti di dominio:

| Componente precedente | Nuovo componente |
| -- | -- |
| .Alert |  PMI-avviso |
| .alert-danger |  PMI-avviso |
| .breadCrumb |  PMI-avviso |
| .checkbox |  sme-form-field[type="checkbox"] |
| .ComboBox |  sme-form-field[type="select"] |
| .dashboard |  sme-layout-content-zone-padded sme-arrange-stack-h |
| Pannello di dettagli come sopra |  griglia delle proprietà alle PMI |
| .details-panel-container |  griglia delle proprietà alle PMI |
| .details-tab |  griglia delle proprietà alle PMI OR considerarvi-pivot |
| wrapper di dettagli come sopra |  griglia delle proprietà alle PMI |
| .disabled |  PMI-disabilitato |
| pulsanti di .form | esperti di dominio del campo del form |
| .form-control | esperti di dominio del campo del form |
| .form-controls | esperti di dominio del campo del form |
| .form-group | esperti di dominio del campo del form |
| .form-group-label | esperti di dominio del campo del form |
| .form-input | esperti di dominio del campo del form |
| .form-stretch | esperti di dominio del campo del form |
| .input-file | esperti di dominio del campo del form |
| .NAV-schede |  considerarvi pivot |
| .radio |  sme-form-field[type="radio"] |
| . Required indizio | esperti di dominio del campo del form |
| .SearchBox |  sme-form-field[type="search"] |
| .toggle-switch |  sme-form-field[type="toggle-switch"] |
| .tool-container |  PMI-layout-contenuto-zone o sme-layout-contenuto-zona-riempito |

### <a name="these-css-classes-are-deprecated-and-are-no-longer-supported"></a>Queste classi CSS sono deprecate e non sono più supportate:

| Vecchia classe | Deprecata |
| -- | -- |
| .Acceptable | (deprecato) |
| .Color-errore | (deprecato) |
| .color-info | (deprecato) |
| caso di esito positivo .color | (deprecato) |
| .Color-avviso | (deprecato) |
| pulsante di .delete | (deprecato) |
| .details-content | (deprecato) |
| copertura del .error | (deprecato) |
| .Error-messaggio | (deprecato) |
| .guided-pane-button | (deprecato) |
| .header-container | (deprecato) |
| sull'icona-Windows | (deprecato) |
| .Indent | (deprecato) |
| .invalid | (deprecato) |
| .item-list | (deprecato) |
| .modal-scrollable | (deprecato) |
| . multi-sezione | (deprecato) |
| .no-action-bar | (deprecato) |
| .overflow margini | (deprecato) |
| .overflow-tool | (deprecato) |
| .progress-cover | (deprecato) |
| .right-panel | (deprecato) |
| .rollup | (deprecato) |
| .rollup-status | (deprecato) |
| .rollup-title | (deprecato) |
| .rollup-valore | (deprecato) |
| .searchbox-action-bar | (deprecato) |
| .size-h-1 | (deprecato) |
| .size-h-2 | (deprecato) |
| .size-h-3 | (deprecato) |
| .size-h-4 | (deprecato) |
| Size-h-full | (deprecato) |
| .size-h-half | (deprecato) |
| .size-v-1 | (deprecato) |
| .size-v-2 | (deprecato) |
| .size-v-3 | (deprecato) |
| .size-v-4 | (deprecato) |
| .status-icon | (deprecato) |
| .svg-16px | (deprecato) |
| .table-indent | (deprecato) |
| .table-sm | (deprecato) |
| .thin | (deprecato) |
| .tile | (deprecato) |
| .Tile-corpo | (deprecato) |
| .tile-content | (deprecato) |
| .tile-footer | (deprecato) |
| .tile-header | (deprecato) |
| .tile-layout | (deprecato) |
| .tile-table | (deprecato) |
| .ToolBar | (deprecato) |
| .tool-bar | (deprecato) |
| .tool-header | (deprecato) |
| .tool-header-box | (deprecato) |
| riquadro Tool | (deprecato) |
| con estensione Usage-barra | (deprecato) |
| .usage-bar-area | (deprecato) |
| sfondo della barra di con estensione Usage | (deprecato) |
| .usage-bar-title | (deprecato) |
| con estensione Usage-barra-value | (deprecato) |
| .usage-chart | (deprecato) |
| gestione dei messaggi con estensione Usage | (deprecato) |
| .usage-message-area | (deprecato) |
| .usage-message-title | (deprecato) |
| .Warning | (deprecato) |
| .white-space | (deprecato) |

## <a name="7-understand-and-resolve-issues-with-observable-objects"></a>7. Comprendere e risolvere i problemi con gli oggetti osservabili

### <a name="update--rxjs-function-use-for-observable-objects"></a>Aggiornamento ```rxjs``` funzione Usa gli oggetti osservabili

Questi sono alcuni nomi di funzione comuni che sono stati modificati, potrebbero esserci altri utenti nel progetto.

* Aggiornamento ```Observable.empty()``` a ```empty()```
* Aggiornamento ```Observable.of()``` a ```of()```
* Aggiornamento ```.switchMap()``` a ```.pipe(switchMap())```
* Aggiornamento ```.map()``` a ```.pipe(map())```
* Aggiornamento ```flatMap()``` a ```mergeMap()```


### <a name="resolve-runtime-issues-with-map-and-filter-functions-on-observable-objects"></a>Risolvere i problemi di runtime relativi ```.map()``` e ```.filter()``` funzioni sugli oggetti osservabili

Se il compilatore non è possibile identificare in modo corretto un' ```observable``` tipo di oggetto, ```.map()``` e ```.filter()``` le funzioni di ```array``` oggetto potrebbe essere invece mappato all'oggetto, causando errori in fase di esecuzione.  Assicurarsi che le funzioni restituiscono un ```observable``` oggetto che specifica un tipo di dati esplicito per evitare questo problema.

```any``` e senza tipo restituito può causare questo problema, cercare il codice con questi modelli:

``` ts
public getMyObservable(): any { //any return type can cause issues
 ...
}

public getMyObservable() { //no return type can cause issues
...
}
```

## <a name="8-resolve-other-common-issues"></a>8. Risolvere altri problemi comuni

Queste tecniche consentono di risolvere altri problemi comuni:

* Eseguire ```ng lint --fix``` per risolvere problemi comuni di lint
* Eseguire ```gulp build``` ripetutamente in modo incrementale correggere i problemi che ```gulp build``` può risolvere automaticamente

## <a name="9-build-and-serve-your-project"></a>9. Crea e fornisce il progetto

Eseguire i comandi seguenti per compilare e distribuire il progetto con la versione più recente (1.0 SDK):

``` cmd
gulp build
gulp serve --port 4201
```

## <a name="10-turn-on-dark-theme-in-windows-admin-center"></a>10. Attivare il tema scuro in Windows Admin Center

Per attivare il tema scuro in Windows Admin Center versione 1902 e versioni successive, seguire questa procedura:

* Aprire Windows Admin Center (versione 1902 e versioni successive)
* Fare clic sui **impostazioni** sull'icona nell'angolo superiore destro della finestra
* Selezionare il **avanzate** scheda
* Sotto *chiavi esperimento*, fare clic su **Add**
* Immettere un nuovo valore ```msft.sme.shell.personalization``` nel campo vuoto, che è stato creato nel passaggio precedente
* Fare clic su **salvare e ricaricare**
* Le impostazioni avranno ora una nuova scheda del **personalizzazione**.  In questa scheda
* Change **colori** a **modalità scura (anteprima)**
