---
title: Eseguire la migrazione da Windows Admin Center SDK 0,1 a 1,0
description: Questa guida consente di eseguire la migrazione da Windows Admin Center SDK versione 0,1 a 1,0
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 02/26/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: c52870178e7caff0abc8ddcccc62966d637dd3c9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357064"
---
# <a name="migrate-from-windows-admin-center-sdk-01-to-10"></a>Eseguire la migrazione da Windows Admin Center SDK 0,1 a 1,0

>Si applica a: Anteprima di Windows Admin Center

Questa guida consente di eseguire la migrazione da Windows Admin Center SDK versione 0,1 a 1,0.  

## <a name="1-learn-about-new-controls-with-the-dev-guide-extension"></a>1. Informazioni sui nuovi controlli con l'estensione della Guida per sviluppatori

L'interfaccia di amministrazione di Windows versione 1902 e successive include l'estensione della **Guida per sviluppatori** , che è possibile usare per trovare esempi di controlli (inclusi i nuovi controlli disponibili) e scenari che consentono di creare un'estensione personalizzata.  La guida per sviluppatori sostituisce l'estensione **strumenti di sviluppo** da versioni precedenti dell'SDK.

### <a name="use-the-dev-guide-in-windows-admin-center"></a>Usare la guida per sviluppatori nell'interfaccia di amministrazione di Windows

La guida per sviluppatori è disponibile come soluzione nell'interfaccia di amministrazione di Windows versione 1902 e successive.  La guida per sviluppatori è preinstallata, ma deve essere abilitata tramite le impostazioni.

**Abilitare la guida per sviluppatori nell'interfaccia di amministrazione di Windows:**

* Aprire l'interfaccia di amministrazione di Windows (versione 1902 e successive)
* Fare clic sull'icona **delle impostazioni** nell'angolo in alto a destra della finestra
* Selezionare la scheda **Avanzate**
* In *chiavi esperimento*fare clic su **Aggiungi** .
* Immettere un nuovo valore ```msft.sme.shell.devguide``` nel campo vuoto creato dal passaggio precedente
* Fare clic su **Salva e ricarica**

**Aprire la guida per sviluppatori nell'interfaccia di amministrazione di Windows:**

* Aprire l'interfaccia di amministrazione di Windows (versione 1902 e successive)
* Fare clic sull'elenco a discesa in alto a sinistra per visualizzare tutti i tipi di soluzioni
* Selezionare la soluzione **Guida per sviluppatori** 
    * Se non viene visualizzata la soluzione elencata, assicurarsi di aver abilitato la guida per lo sviluppo (vedere la sezione precedente) e di aver ricaricato l'interfaccia di amministrazione di Windows.
* Sfogliare il contenuto della Guida per lo sviluppo selezionando una delle schede
    * **Destinazione** Contiene esempi di codice per gli scenari di *gestione* e di *notifica*
    * **Controlli** Contiene esempi di ogni controllo disponibile nell'SDK
    * **Tubi** Contiene esempi di funzioni del convertitore e del formattatore disponibili
    * **Stili** Contiene esempi di stili CSS disponibili nell'SDK
    * **MsftSme:** Contiene esempi e indicazioni per gli scenari avanzati 

### <a name="browse-the-source-code-of-dev-guide-on-github"></a>Esplora il codice sorgente della Guida per sviluppatori su GitHub

È possibile esplorare il [codice sorgente](https://github.com/Microsoft/windows-admin-center-sdk/) della Guida per sviluppatori su GitHub per trovare esempi di codice HTML, CSS e typescript di esempio.

## <a name="2-prepare-your-development-environment-for-the-latest-sdk"></a>2. Preparare l'ambiente di sviluppo per l'SDK più recente

Installare o aggiornare node. js versione [10.15.1 LTS o versioni successive](https://nodejs.org/en/).

Aggiornare l'interfaccia della riga di comando di Windows Admin Center alla versione più recente:

[//]: # "disinstallazione di NPM-g windows-admin-center-cli@next"

``` cmd
npm uninstall -g windows-admin-center-cli
npm install -g windows-admin-center-cli
```

Aggiornare le dipendenze globali a queste versioni:

``` cmd
npm install npm@6.4.1 -g
npm install @angular/cli@7.1.2 -g
npm install gulp -g
npm install typescript@3.1.6 -g
npm install tslint@5.11.0 -g
```

## <a name="3-create-a-new-project-with-the-latest-sdk"></a>3. Creare un nuovo progetto con l'SDK più recente

Usare l'interfaccia della riga di comando di Windows Admin Center per creare un nuovo progetto destinato alla versione ```next``` (SDK 1,0):

[//]: # "WAC creare--azienda ' Contoso Inc '--strumento ' Gestisci foo Works '-versione sperimentale"

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version next
```

Successivamente, passare alla cartella appena creata, quindi installare le dipendenze locali necessarie eseguendo ```npm install ```.

## <a name="4-modify-an-existing-project-to-use-the-latest-sdk"></a>4. Modificare un progetto esistente per l'uso dell'SDK più recente

IMPORTANTE: Prima di continuare, eseguire un backup del progetto.

Modificare la riga seguente in ```package.json``` per fare riferimento alla versione ```next``` (SDK 1,0):

[//]: # "' @microsoft/windows-admin-center-sdk':' sperimentale '"

``` json
"@microsoft/windows-admin-center-sdk": "next",
```

Eseguire quindi ```npm install``` per aggiornare i riferimenti nell'intero progetto.

## <a name="5-use-the-sdk-cli-to-fix-common-migration-issues"></a>5. Usare l'interfaccia della riga di comando SDK per risolvere i problemi comuni di migrazione

IMPORTANTE: Prima di continuare, eseguire un backup del progetto.

Dalla cartella radice del progetto, eseguire il comando CLI seguente nel progetto per correggere automaticamente i problemi di migrazione comuni:

``` cmd
wac updateSeven --update
```

Questo comando CLI risolve automaticamente i seguenti problemi:

* Rigenera ```package-lock.json```
* Aggiornare i file nell'ambiente di compilazione angolare:
    - ```.gitignore```
    - ```tslint.json```
    - ```tsconfig.json```
    - ```tsconfig.inline.json```
    - ```ng-package.json```
    - ```angular.json```
    - ```src\tsconfig.app.json```
    - ```src\tsconfig.lib.json```
    - ```src\tsconfig.spec.json```

## <a name="6-use-the-sdk-cli-to-understand-common-migration-issues"></a>6. Usare l'interfaccia della riga di comando SDK per conoscere i problemi comuni relativi alla migrazione

Dalla cartella radice del progetto, eseguire il comando seguente dell'interfaccia della riga di comando per controllare il progetto e individuare i problemi di migrazione comuni che devono essere risolti manualmente:

``` cmd
wac updateSeven --audit
```

Questa operazione troverà le istanze dei seguenti problemi nel progetto:

### <a name="replace-usage-of-the-following-css-classes-with-these-sme-classes"></a>Sostituire l'utilizzo delle seguenti classi CSS con le classi SME seguenti:

| Classe CSS precedente | Nuova classe CSS |
| -- | -- |
| . auto-dimensioni Flex |  . SME-position-Flex-auto |
| . Border-tutto |  . SME-Border-Insert-SM e. SME-border-color-base-90 |
| . bordo inferiore |  . SME-border-bottom-SM e. SME-border-bottom-color-base-90 |
| . Border-orizzontale |  . SME-Border-Horizontal-SM e. SME-Border-Horizontal-color-base-90 |
| . bordo sinistro |  . SME-border-left-SM e. SME-border-left-color-base-90 |
| . Border-a destra |  . SME-border-right-SM e. SME-border-right-color-base-90 |
| . border-top |  . SME-border-top-SM e. SME-border-top-color-base-90 |
| . Border-Vertical |  . SME-Border-Vertical-SM e. SME-Border-Vertical-color-base-90 |
| . Break-Word |  . SME-Arrange-WS-wrap |
| . BTN |  . SME-pulsante o pulsante |
| . BTN-primario |  . SME-Button. SME-Button-Primary o. Button. SME-Button-Primary |
| . color-scuro |  . SME-color-Alt |
| . colore-chiaro |  . SME-color-base |
| . color-grigio chiaro |  . SME-color-base-90 |
| . Fixed-Flex-Size |  . SME-position-Flex-None |
| . Flex-layout |  . SME-Arrange-stack-h o. SME-Arrange-stack-v |
| . Font-Bold |  . SME-font-emphasis1 |
| . Highlight |  . SME-background-colore giallo |
| . orizzontale |  . SME-Arrange-stack-h |
| . No-scroll |  . SME-position-Flex-auto |
| . NoWrap |  . SME-Arrange-stack-h o. SME-Arrange-stack-v |
| . relativo |  . SME-layout-relativo |
| . relativo-al centro |  . SME-layout-Absolute. SME-position-Center |
| . Reverse |  . SME-Arrange-stack-invertito |
| . Stretch-Absolute |  . SME-layout-Absolute. SME-position-Insert-None |
| . Stretch-risolto |  . SME-layout-corretto. SME-position-Insert-None |
| . Stretch-verticale |  . SME-position-stretch-v |
| . Stretch-Width |  . SME-position-stretch-h |
| . verticale |  . SME-Arrange-stack-v |
| . Vertical-solo scorrimento |  . SME-Arrange-overflow-Hide-x PMI-Arrange-overflow-auto-y |
| . wrap |  . SME-Arrange-wrapstack-h o. SME-Arrange-wrapstack-v |

### <a name="replace-usage-of-the-following-components-with-these-sme-components"></a>Sostituire l'utilizzo dei componenti seguenti con questi componenti PMI:

| Componente precedente | Nuovo componente |
| -- | -- |
| . avviso |  PMI-avviso |
| . Alert-pericolo |  PMI-avviso |
| . navigazione |  PMI-avviso |
| . casella di controllo |  SME-Form-Field [tipo = "checkbox"] |
| . ComboBox |  PMI-Form-Field [tipo = "Select"] |
| . dashboard |  PMI-layout-content-zone-imbottito per le PMI-Arrange-stack-h |
| . Details-pannello |  PMI-Property-Grid |
| . Details-Panel-container |  PMI-Property-Grid |
| . Details-Tab |  PMI-Property-Grid o SME-pivot |
| . dettagli-wrapper |  PMI-Property-Grid |
| . disabled |  PMI-disabilitato |
| . Form-pulsanti | campo PMI-form |
| . Form-controllo | campo PMI-form |
| . form-controlli | campo PMI-form |
| . Form-gruppo | campo PMI-form |
| . Form-Group-label | campo PMI-form |
| . form-input | campo PMI-form |
| . Form-stretch | campo PMI-form |
| . input-file | campo PMI-form |
| . NAV-tabulazioni |  PMI-pivot |
| . radio |  SME-Form-Field [tipo = "Radio"] |
| . Required-Clue | campo PMI-form |
| . SearchBox |  SME-Form-Field [tipo = "Cerca"] |
| . interruttore-switch |  SME-Form-Field [tipo = "interruttore-switch"] |
| . strumento-contenitore |  PMI-layout-content-zone o SME-layout-content-zone-imbottito |

### <a name="these-css-classes-are-deprecated-and-are-no-longer-supported"></a>Queste classi CSS sono deprecate e non sono più supportate:

| Classe precedente | Funzionalità deprecate |
| -- | -- |
| . accettabile | deprecato |
| . Color-errore | deprecato |
| . Color-info | deprecato |
| . Color-operazione riuscita | deprecato |
| . Color-avviso | deprecato |
| . Delete-pulsante | deprecato |
| . dettagli-contenuto | deprecato |
| . errore-copertura | deprecato |
| . error-messaggio | deprecato |
| . Guid-pulsante riquadro | deprecato |
| . Header-contenitore | deprecato |
| . icona-Win | deprecato |
| . Indent | deprecato |
| . non valido | deprecato |
| . elemento-elenco | deprecato |
| . modale scorrevole | deprecato |
| . più sezioni | deprecato |
| . No-Action-bar | deprecato |
| . overflow-margini | deprecato |
| . overflow-strumento | deprecato |
| . Progress-copertura | deprecato |
| . pannello a destra | deprecato |
| Rollup | deprecato |
| . rollup-stato | deprecato |
| . rollup-titolo | deprecato |
| Rollup-valore | deprecato |
| . SearchBox-azione-barra | deprecato |
| . Size-h-1 | deprecato |
| . Size-h-2 | deprecato |
| . Size-h-3 | deprecato |
| . Size-h-4 | deprecato |
| . Size-h-completo | deprecato |
| . Size-h-metà | deprecato |
| . Size-v-1 | deprecato |
| . Size-v-2 | deprecato |
| . Size-v-3 | deprecato |
| . Size-v-4 | deprecato |
| . status-icona | deprecato |
| . svg-16px | deprecato |
| . Table-rientro | deprecato |
| . Table-SM | deprecato |
| . Thin | deprecato |
| . sezione | deprecato |
| . Tile-corpo | deprecato |
| . Tile-contenuto | deprecato |
| . Tile-piè di pagina | deprecato |
| . Tile-intestazione | deprecato |
| . riquadro-Layout | deprecato |
| . Tile-tabella | deprecato |
| . barra degli strumenti | deprecato |
| . barra degli strumenti | deprecato |
| . strumento-intestazione | deprecato |
| . strumento-Header-box | deprecato |
| . strumento-riquadro | deprecato |
| . Usage-barra | deprecato |
| . Usage-barra-area | deprecato |
| . Usage-barra-sfondo | deprecato |
| . Usage-barra-titolo | deprecato |
| . Usage-barra-valore | deprecato |
| . Usage-grafico | deprecato |
| . Usage-messaggio | deprecato |
| . Usage-area messaggio | deprecato |
| . Usage-messaggio-titolo | deprecato |
| . avviso | deprecato |
| . spazio vuoto | deprecato |

## <a name="7-understand-and-resolve-issues-with-observable-objects"></a>7. Comprendere e risolvere i problemi relativi agli oggetti osservabili

### <a name="update--rxjs-function-use-for-observable-objects"></a>Aggiornare l'utilizzo della funzione ```rxjs``` per gli oggetti osservabili

Questi sono alcuni nomi di funzione comuni che sono stati modificati, altri possono essere presenti nel progetto.

* Aggiornare ```Observable.empty()``` al ```empty()```
* Aggiornare ```Observable.of()``` al ```of()```
* Aggiornare ```.switchMap()``` al ```.pipe(switchMap())```
* Aggiornare ```.map()``` al ```.pipe(map())```
* Aggiornare ```flatMap()``` al ```mergeMap()```


### <a name="resolve-runtime-issues-with-map-and-filter-functions-on-observable-objects"></a>Risolvere i problemi di runtime con funzioni ```.map()``` e ```.filter()``` sugli oggetti osservabili

Se il compilatore non è in grado di identificare correttamente il tipo di un oggetto ```observable```, è possibile che venga eseguito il mapping delle funzioni ```.map()``` e ```.filter()``` dall'oggetto ```array``` all'oggetto, causando errori in fase di esecuzione.  Assicurarsi che le funzioni restituiscano un oggetto ```observable``` specificando un tipo di dati esplicito per evitare questo problema.

```any``` e nessun tipo restituito può causare questo problema, cercare il codice con questi modelli:

``` ts
public getMyObservable(): any { //any return type can cause issues
 ...
}

public getMyObservable() { //no return type can cause issues
...
}
```

## <a name="8-resolve-other-common-issues"></a>8. Risolvere altri problemi comuni

Queste tecniche aiuteranno a risolvere altri problemi comuni:

* Eseguire ```ng lint --fix``` per correggere i problemi di lanugine comuni
* Eseguire ripetutamente ```gulp build``` per risolvere in modo incrementale i problemi che ```gulp build``` può risolvere automaticamente

## <a name="9-build-and-serve-your-project"></a>9. Compilare e gestire il progetto

Eseguire i comandi seguenti per compilare e gestire il progetto con la versione più recente (SDK 1,0):

``` cmd
gulp build
gulp serve --port 4201
```

## <a name="10-turn-on-dark-theme-in-windows-admin-center"></a>10. Attiva tema scuro nell'interfaccia di amministrazione di Windows

Per attivare un tema scuro nell'interfaccia di amministrazione di Windows versione 1902 e successive, seguire questa procedura:

* Aprire l'interfaccia di amministrazione di Windows (versione 1902 e successive)
* Fare clic sull'icona **delle impostazioni** nell'angolo in alto a destra della finestra
* Selezionare la scheda **Avanzate**
* In *chiavi esperimento*fare clic su **Aggiungi** .
* Immettere un nuovo valore ```msft.sme.shell.personalization``` nel campo vuoto creato dal passaggio precedente
* Fare clic su **Salva e ricarica**
* Le impostazioni avranno ora una nuova scheda, **personalizzazione**.  Seleziona questa scheda
* Modificare **i colori** in **modalità scura (anteprima)**
