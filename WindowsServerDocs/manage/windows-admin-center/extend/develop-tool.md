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
ms.openlocfilehash: 31d8dbd3df4c44b6e0a3780b022dfbd9fffdffec
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452584"
---
# <a name="develop-a-tool-extension"></a>Sviluppare un'estensione dello strumento

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

Un'estensione degli strumenti è il modo principale che gli utenti interagiscono con Windows Admin Center per gestire una connessione, ad esempio un server o cluster. Quando si fa clic su una connessione nella schermata principale del Windows Admin Center, la connessione verrà quindi presentato con un elenco degli strumenti nel riquadro di spostamento a sinistra. Quando fa clic su uno strumento, l'estensione degli strumenti è caricato e visualizzato nel riquadro di destra.

Quando viene caricata un'estensione per strumento, può eseguire le chiamate a WMI o script di PowerShell in un server di destinazione o il cluster e visualizzare le informazioni nell'interfaccia utente o eseguire i comandi basati su input dell'utente. Le estensioni di strumenti definiscono quali soluzioni deve essere visualizzato, risultante in un diverso set di strumenti per ogni soluzione.

> [!NOTE]
> Non hanno familiarità con i tipi di estensione diversi? Altre informazioni sul [tipi di architettura e l'estensione di estendibilità](understand-extensions.md).

## <a name="prepare-your-environment"></a>Preparazione dell'ambiente

Se non già [preparare l'ambiente](prepare-development-environment.md) installando le dipendenze e globali prerequisiti obbligatori per tutti i progetti.

## <a name="create-a-new-tool-extension-with-the-windows-admin-center-cli"></a>Creare una nuova estensione per strumento con l'interfaccia CLI di Windows Admin Center ##

Dopo aver creato tutte le dipendenze installate, si è pronti per creare la nuova estensione degli strumenti.  Creare o selezionare una cartella che contiene i file di progetto, aprire un prompt dei comandi e impostare tale cartella come directory di lavoro.  Tramite la CLI di Windows Admin Center che è stato installato in precedenza, creare una nuova estensione con la sintassi seguente:

``` cmd
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| Value | Spiegazione | Esempio |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Il nome della società (spazi inclusi) | ```Contoso Inc``` |
| ```{!Tool Name}``` | Il nome dello strumento (spazi inclusi) | ```Manage Foo Works``` |

Ecco un esempio di utilizzo:

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

In questo modo viene creata una nuova cartella all'interno della directory di lavoro corrente usando il nome è specificato per il tuo strumento di copia tutti i file modello necessari nel progetto e configura i file con il nome della società e strumento.  

Successivamente, passare alla cartella appena creata, quindi installare le dipendenze locali necessari eseguendo il comando seguente:

``` cmd
npm install
```

Al termine, aver configurato tutto il che necessario per caricare la nuova estensione Windows Admin Center. 

## <a name="add-content-to-your-extension"></a>Aggiungere contenuto all'estensione

Ora che è stata creata un'estensione con l'interfaccia CLI di Windows Admin Center, si è pronti per personalizzare il contenuto.  Per esempi di operazioni possibili, vedere le guide seguenti:

- Aggiungere un [modulo vuoto](guides/add-module.md)
- Aggiungere un [iFrame](guides/add-iframe.md)
 
Sono disponibili anche altri esempi nostri [sito GitHub SDK](https://aka.ms/wacsdk):
-  [Strumenti di sviluppo](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/windows-admin-center-developer-tools) è un'estensione completamente funzionante che può essere caricate in sideload in Windows Admin Center e contiene un'ampia raccolta di esempi di funzionalità e lo strumento di esempio che è possibile esplorare e utilizzare in un'estensione personalizzata.

## <a name="customize-your-extensions-icon"></a>Personalizzare l'icona dell'estensione

È possibile personalizzare l'icona che mostra per l'estensione nell'elenco degli strumenti.  A tale scopo, modificare tutte ```icon``` voci ```manifest.json``` per l'estensione:

``` json
"icon": "{!icon-uri}",
```

| Value | Spiegazione | Uri di esempio |
| ----- | ----------- | ------- |
| ```{!icon-uri}``` | Il percorso della risorsa di icona | ```assets/foo-icon.svg``` |

NOTA: Attualmente, icone personalizzate non sono visibili quando lato caricano l'estensione in modalità di sviluppo.  In alternativa, rimuovere il contenuto dei ```target``` come indicato di seguito:

``` json
"target": "",
```

Questa configurazione è valida solo per il caricamento laterale in modalità di sviluppo, pertanto è importante mantenere il valore contenuto nella ```target``` e quindi ripristinarlo prima di pubblicare l'estensione.

## <a name="build-and-side-load-your-extension"></a>Compilazione e sul lato caricare l'estensione

Successivamente, compilazione e sul lato, caricare l'estensione Windows Admin Center.  Aprire una finestra di comando, passare alla directory di origine, quindi è possibile passare alla compilazione.

* Crea e gestisci con gulp:

    ``` cmd
    gulp build
    gulp serve -p 4201
    ```

Devi scegliere una porta attualmente libera. Non tentare di utilizzare la porta su cui è in esecuzione Windows Admin Center.

Il progetto può essere trasferito localmente in un'istanza locale Windows Admin Center per scopi di test collegando il progetto gestito localmente in Windows Admin Center.

* Avvia Windows Admin Center in un Web browser
* Apri il debugger (F12)
* Apri la console e digita il comando seguente:

    ``` cmd
    MsftSme.sideLoad("http://localhost:4201")
    ```

*   Aggiorna il Web browser

Il progetto sarà ora visibile nell'elenco Strumenti con (trasferito localmente) accanto al nome.

## <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>Destinazione una versione diversa di Windows Admin Center SDK

L'estensione di stare al passo con le modifiche a SDK e piattaforma è facile.  Ottenere informazioni su come [destinare una versione diversa](target-sdk-version.md) Windows Admin Center SDK.