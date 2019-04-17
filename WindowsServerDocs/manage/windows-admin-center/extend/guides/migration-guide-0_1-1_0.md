---
title: Eseguire la migrazione da Windows Admin Center SDK 0,1 a 1.0
description: Questa guida consentirà di eseguire la migrazione da Windows Admin Center SDK versione 0,1 a 1.0
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 02/26/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ba0e8cda35c51763b5c10b89c76e2bf07e064dfd
ms.sourcegitcommit: 10d7606a5c2a1ddb234af597f199b6779b0058d4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/27/2019
ms.locfileid: "9116587"
---
# Eseguire la migrazione da Windows Admin Center SDK 0,1 a 1.0

>Si applica a: Windows Admin Center Preview

Questa guida consentirà di eseguire la migrazione da Windows Admin Center SDK versione 0,1 a 1.0.  

## 1. Scopri nuovi controlli con l'estensione Dev Guida

1902 e versioni successive di Windows Admin Center include l'estensione **Dev Guida** che puoi usare per trovare esempi di scenari per aiutarti a creare un'estensione personalizzata e controlli (compresi i controlli appena disponibili).  Guida DEV sostituisce l'estensione di **Strumenti di sviluppo** da versioni precedenti del SDK.

### Usare la Guida Dev in Windows Admin Center

DEV Guida è disponibile come soluzione nella versione di Windows Admin Center 1902 e versioni successive.  Guida DEV è pre-installata, ma deve essere abilitato tramite le impostazioni.

**Abilitare la Guida Dev in Windows Admin Center:**

* Apri Windows Admin Center (versione 1902 e versioni successive)
* Fai clic sull'icona **Impostazioni** nell'angolo superiore destro della finestra
* Seleziona la scheda **Avanzate**
* *Le chiavi esperimento*, fare clic su **Aggiungi**
* Immetti un nuovo valore ```msft.sme.shell.devguide``` nel campo vuoto che è stato creato nel passaggio precedente
* Fai clic su **Salva e ricaricare**

**Aprire Dev Guida in Windows Admin Center:**

* Apri Windows Admin Center (versione 1902 e versioni successive)
* Fai clic sul menu a discesa in alto a sinistra per visualizzare tutti i tipi di soluzione
* Selezionare la soluzione **Dev Guida** 
    * Se non vedi la soluzione elencata, assicurati di aver abilitato la Guida dev (vedere la sezione precedente) e hanno ricaricato Windows Admin Center.
* Passa il contenuto della Guida Dev selezionando una delle schede
    * **Di destinazione:** Contiene esempi di codice per gli scenari di *Account di gestione* e delle *notifiche*
    * **Controlli:** Contiene alcuni esempi di ogni controllo disponibile nel SDK
    * **Pipe:** Contiene alcuni esempi di funzioni convertitore e formattatore disponibili
    * **Stili:** Contiene alcuni esempi di stili CSS disponibili nel SDK
    * **MsftSme:** Contiene esempi e linee guida per scenari avanzati 

### Sfoglia il codice sorgente di Guida Dev su GitHub

È possibile esplorare il [codice sorgente](https://github.com/Microsoft/windows-admin-center-sdk/) di Guida Dev su GitHub per trovare esempi di codice HTML, CSS e TypeScript di esempio.

## 2. preparare l'ambiente di sviluppo per il SDK più recente

Installare o aggiornare la versione di Node. js [10.15.1 risultati o versione successiva](https://nodejs.org/en/).

Aggiornare l'interfaccia CLI di Windows Admin Center per la versione più recente:

[//]: # "npm disinstallare windows-admin-center-cli@next -g"

``` cmd
npm uninstall -g windows-admin-center-cli
npm install -g windows-admin-center-cli
```

Aggiorna le dipendenze globale a queste versioni:

``` cmd
npm install npm@6.4.1 -g
npm install @angular/cli@7.1.2 -g
npm install gulp -g
npm install typescript@3.1.6 -g
npm install tslint@5.11.0 -g
```

## 3. creare un nuovo progetto con il SDK più recente

Usa l'interfaccia CLI di Windows Admin Center per creare un nuovo progetto come destinazione il ```next``` versione (1.0 SDK):

[//]: # "creare wac - società 'Contoso Inc' - strumento 'Gestire Foo funziona' - versione sperimentali"

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version next
```

Successivamente, Imposta directory nella cartella appena creata, quindi installare necessarie dipendenze locali eseguendo ```npm install ```.

## 4. modificare un progetto esistente per usare il SDK più recente

Importante: Eseguire un backup del progetto prima di continuare.

Modificare la riga seguente in ```package.json``` alla destinazione il ```next``` versione (1.0 SDK):

[//]: # "'@microsoft/windows-admin-center-sdk': 'sperimentali'"

``` json
"@microsoft/windows-admin-center-sdk": "next",
```

Quindi Esegui ```npm install``` per aggiornare i riferimenti in tutto il progetto.

## 5. usare CLI SDK per risolvere i problemi di migrazione comuni

Importante: Eseguire un backup del progetto prima di continuare.

Dalla cartella radice del progetto, eseguire il comando seguente CLI nel progetto per risolvere i problemi di migrazione comuni automaticamente:

``` cmd
wac updateSeven --update
```

Questo comando CLI risolve automaticamente i problemi seguenti:

* Rigenerare ```package-lock.json```
* Aggiornare i file nell'ambiente di compilazione angular:
    - ```.gitignore```
    - ```tslint.json```
    - ```tsconfig.json```
    - ```tsconfig.inline.json```
    - ```ng-package.json```
    - ```angular.json```
    - ```src\tsconfig.app.json```
    - ```src\tsconfig.lib.json```
    - ```src\tsconfig.spec.json```

## 6. usare CLI SDK per individuare i problemi di migrazione comuni

Dalla cartella radice del progetto, eseguito il comando seguente comando per controllare il progetto e individuare i problemi di migrazione comuni che devono essere risolti manualmente:

``` cmd
wac updateSeven --audit
```

Questo troveranno le istanze dei seguenti problemi nel tuo progetto:

### Sostituire l'utilizzo delle classi CSS seguente con queste classi sme:

| Classe CSS precedente | Nuova classe CSS |
| -- | -- |
| .Auto-flessibilità-dimensioni |  posizione .sme-flessibilità-automatico |
| .Border-all |  .SME-border-incassato-sm e .sme-border-colore-base-90 |
| .Border il basso |  sm .sme-bordo inferiore e .sme-border-bottom-color-base-90 |
| .Border-orizzontale |  border .sme-sm orizzontale e .sme-border-horizontal-color-base-90 |
| .Border a sinistra |  sm .sme-bordo sinistro e .sme-border-left-color-base-90 |
| .Border a destra |  sm .sme-bordo destro e .sme-border-right-color-base-90 |
| .Border-top |  sm .sme-bordo superiore e .sme-border-top-color-base-90 |
| .Border-verticale |  border .sme-sm verticale e .sme-border-vertical-color-base-90 |
| .Break DWORD |  .SME-disporre-ws-wrap |
| .btn |  .SME-pulsante pulsante OR |
| .btn principale |  .SME button.sme-pulsante principale OR.button.sme-pulsante-principale |
| .Color-scuro |  .SME-colore-alt |
| illuminazione .color |  base di colore .sme |
| .Color-grigio chiaro |  .SME-colore-base-90 |
| .fixed-flessibilità-dimensioni |  .SME-posizione-flessibilità-nessuno |
| layout di .flex |  .SME-disporre-stack-h o .sme-disporre-stack-v |
| .Font-bold |  .SME-tipo di carattere-emphasis1 |
| .Highlight |  .SME-in background-colore-giallo |
| .Horizontal |  .SME-disporre-stack-h |
| scorrimento .no |  posizione .sme-flessibilità-automatico |
| .NoWrap |  .SME-disporre-stack-h o .sme-disporre-stack-v |
| .relative |  relativo al layout .sme |
| .relative-center |  .SME-layout-assoluto .sme-posizione-center |
| reverse |  .SME-disporre-stack-invertito |
| .Stretch assoluti |  .SME-layout-assoluto .sme-posizione-incassato-nessuno |
| fisso .stretch |  .SME layout fisso .sme-posizione-incassato-nessuno |
| .Stretch-verticale |  .SME-posizione-stretch-v |
| larghezza .stretch |  .SME-posizione-stretch-h |
| .Vertical |  .SME-disporre-stack-v |
| solo scorrimento .vertical |  .SME-disporre-overflow-Nascondi-x sme-disporre-overflow-auto-y |
| .Wrap |  .SME-disporre-wrapstack-h o .sme-disporre-wrapstack-v |

### Con questi componenti PMI, Sostituisci utilizzo dei componenti seguenti:

| Componente precedente | Nuovo componente |
| -- | -- |
| .Alert |  PMI-alert |
| .Alert-pericolo |  PMI-alert |
| .breadCrumb |  PMI-alert |
| .CheckBox |  PMI form-field [tipo = "checkbox"] |
| .ComboBox |  PMI form-field [tipo = "seleziona"] |
| .dashboard |  riempiti SME-layout-contenuto-area sme-disporre-stack-h |
| Pannello di dettagli come sopra |  PMI-proprietà-griglia |
| dettagli come sopra-pannello-container |  PMI-proprietà-griglia |
| scheda di dettagli come sopra |  PMI-proprietà-griglia OR sme-pivot |
| wrapper di dettagli come sopra |  PMI-proprietà-griglia |
| .Disabled |  PMI disattivata |
| .Form-pulsanti | PMI-form-campo |
| controllo .form | PMI-form-campo |
| .Form-controlli | PMI-form-campo |
| gruppo .form | PMI-form-campo |
| Etichetta-gruppo-.form | PMI-form-campo |
| input .form | PMI-form-campo |
| .Form-stretch | PMI-form-campo |
| -il file .input | PMI-form-campo |
| .NAV-schede |  PMI-pivot |
| .radio |  PMI form-field [tipo = "radio"] |
| .Required-indizio | PMI-form-campo |
| .SearchBox |  PMI form-field [tipo = "Cerca"] |
| .Toggle-switch |  PMI form-field [tipo = "interruttore Attiva/disattiva"] |
| contenitore Tool |  area del contenuto di layout SME o riempiti sme-layout-contenuto-area |

### Queste classi CSS sono deprecate e non sono più supportate:

| Classe precedente | Deprecato |
| -- | -- |
| .Acceptable | (deprecato) |
| errore .color | (deprecato) |
| .Color-info | (deprecato) |
| .Color-operazione riuscita | (deprecato) |
| .Color-avviso | (deprecato) |
| .Delete-pulsante | (deprecato) |
| contenuto di dettagli come sopra | (deprecato) |
| .Error-cover con tasti | (deprecato) |
| messaggio di .error | (deprecato) |
| .Guided--pulsante del riquadro | (deprecato) |
| .Header-contenitore | (deprecato) |
| win-sull'icona | (deprecato) |
| .Indent | (deprecato) |
| .Invalid | (deprecato) |
| elenco di .item | (deprecato) |
| .Modal scorrevole | (deprecato) |
| . multi-sezione | (deprecato) |
| barra di azione .no | (deprecato) |
| margini di .overflow | (deprecato) |
| .overflow-strumento | (deprecato) |
| .Progress-cover con tasti | (deprecato) |
| Pannello di .right | (deprecato) |
| .rollup | (deprecato) |
| .rollup-stato | (deprecato) |
| .rollup-titolo | (deprecato) |
| .rollup-valore | (deprecato) |
| barra di azione .searchbox | (deprecato) |
| Size-h-1 | (deprecato) |
| Size-h-2 | (deprecato) |
| Size-h-3 | (deprecato) |
| Size-h-4 | (deprecato) |
| Size-h-completo | (deprecato) |
| Size-h-metà | (deprecato) |
| Size-v-1 | (deprecato) |
| Size-v-2 | (deprecato) |
| Size-v-3 | (deprecato) |
| Size-v-4 | (deprecato) |
| icona di Status | (deprecato) |
| SVG-16 | (deprecato) |
| rientro .table | (deprecato) |
| .Table sm | (deprecato) |
| .thin | (deprecato) |
| .Tile | (deprecato) |
| .Tile-body | (deprecato) |
| .Tile-content | (deprecato) |
| .Tile-piè di pagina | (deprecato) |
| .Tile-intestazione | (deprecato) |
| layout di .tile | (deprecato) |
| la tabella .tile | (deprecato) |
| .ToolBar | (deprecato) |
| barra di Tool | (deprecato) |
| Tool-intestazione | (deprecato) |
| casella di intestazione Tool | (deprecato) |
| Tool-riquadro | (deprecato) |
| barra di .usage | (deprecato) |
| area di barra .usage | (deprecato) |
| barra-.usage in background | (deprecato) |
| .Usage-barra-titolo | (deprecato) |
| .Usage-barra-value | (deprecato) |
| .Usage-grafico | (deprecato) |
| messaggio di .usage | (deprecato) |
| area di messaggio .usage | (deprecato) |
| titolo del messaggio .usage | (deprecato) |
| .Warning | (deprecato) |
| spazio .white | (deprecato) |

## 7. comprendere e risolvere i problemi con oggetti osservabili

### Aggiornamento ```rxjs``` funzione utilizza per oggetti osservabili

Questi sono alcuni nomi di funzione comune che sono state modificate, potrebbero esserci ad altri utenti nel tuo progetto.

* Aggiornamento ```Observable.empty()``` per ```empty()```
* Aggiornamento ```Observable.of()``` per ```of()```
* Aggiornamento ```.switchMap()``` per ```.pipe(switchMap())```
* Aggiornamento ```.map()``` per ```.pipe(map())```
* Aggiornamento ```flatMap()``` per ```mergeMap()```


### Risolvere i problemi di runtime con ```.map()``` e ```.filter()``` funzioni sugli oggetti osservabili

Se il compilatore non è possibile identificare correttamente un ```observable``` tipo di oggetto, ```.map()``` e ```.filter()``` funzioni dal ```array``` oggetto può essere mappato invece per l'oggetto, determinando errori in fase di esecuzione.  Assicurati che le funzioni restituiscono un ```observable``` oggetto specificando un tipo di dati esplicito per evitare questo problema.

```any``` e possono causare questo problema, cercare codice con questi modelli di alcun tipo restituito:

``` ts
public getMyObservable(): any { //any return type can cause issues
 ...
}

public getMyObservable() { //no return type can cause issues
...
}
```

## 8. risolvere altri problemi comuni

Queste tecniche consentono di risolvere altri problemi comuni:

* Eseguire ```ng lint --fix``` risolvere i problemi comuni fili
* Eseguire ```gulp build``` ripetutamente in modo incrementale risolvere i problemi che ```gulp build``` può risolvere automaticamente

## 9. crea e il progetto

Eseguire i comandi seguenti per creare e gestire il progetto con la versione più recente (1.0 SDK):

``` cmd
gulp build
gulp serve --port 4201
```

## 10. attivare il tema scuro in Windows Admin Center

Per attivare il tema scuro in Windows Admin Center versione 1902 e versioni successive, segui questi passaggi:

* Apri Windows Admin Center (versione 1902 e versioni successive)
* Fai clic sull'icona **Impostazioni** nell'angolo superiore destro della finestra
* Seleziona la scheda **Avanzate**
* *Le chiavi esperimento*, fare clic su **Aggiungi**
* Immetti un nuovo valore ```msft.sme.shell.personalization``` nel campo vuoto che è stato creato nel passaggio precedente
* Fai clic su **Salva e ricaricare**
* Impostazioni avrà ora a disposizione una nuova scheda, **la personalizzazione**.  Seleziona questa scheda
* Modificare **i colori** in **modalità scura (anteprima)**
