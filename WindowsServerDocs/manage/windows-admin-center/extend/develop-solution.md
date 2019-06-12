---
title: Sviluppare un'estensione della soluzione
description: Sviluppare un'estensione di soluzioni Windows Admin Center SDK (progetto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 268a7d2833f73e9fab006501e9b3dc261d1b1d9e
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452570"
---
# <a name="develop-a-solution-extension"></a>Sviluppare un'estensione della soluzione

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

Soluzioni definiscono essenzialmente un tipo univoco dell'oggetto da gestire tramite Windows Admin Center.  Questi tipi di soluzioni/connessione sono inclusi con Windows Admin Center, per impostazione predefinita:

* Connessioni al Server di Windows
* Connessioni di PC Windows
* Connessioni del cluster di failover
* Connessioni al cluster iperconvergente

Quando si seleziona una connessione dalla pagina di connessione Windows Admin Center, l'estensione di soluzioni per il tipo della connessione viene caricata e Windows Admin Center tenterà di connettersi al nodo di destinazione. Se la connessione ha esito positivo, la soluzione dell'interfaccia utente dell'estensione verrà caricato e Windows Admin Center visualizzerà gli strumenti per la soluzione nel riquadro di spostamento a sinistra.

Se si vorrebbe compilare una GUI di gestione per i servizi non è definito per i tipi di connessione predefinita di sopra, uno switch di rete o altro hardware non individuabile in base al nome di computer, è possibile creare soluzioni un'estensione personalizzata.

> [!NOTE]
> Non hanno familiarità con i tipi di estensione diversi? Altre informazioni sul [tipi di architettura e l'estensione di estendibilità](understand-extensions.md).

## <a name="prepare-your-environment"></a>Preparazione dell'ambiente

Se non già [preparare l'ambiente](prepare-development-environment.md) installando le dipendenze e globali prerequisiti obbligatori per tutti i progetti.

## <a name="create-a-new-solution-extension-with-the-windows-admin-center-cli"></a>Creare una nuova estensione di soluzioni con la CLI di Windows Admin Center ##

Dopo aver creato tutte le dipendenze installate, si è pronti per creare la nuova estensione di soluzioni.  Creare o selezionare una cartella che contiene i file di progetto, aprire un prompt dei comandi e impostare tale cartella come directory di lavoro.  Tramite la CLI di Windows Admin Center che è stato installato in precedenza, creare una nuova estensione con la sintassi seguente:

```
wac create --company "{!Company Name}" --solution "{!Solution Name}" --tool "{!Tool Name}"
```

| Value | Spiegazione | Esempio |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Il nome della società (spazi inclusi) | ```Contoso Inc``` |
| ```{!Solution Name}``` | Il nome della soluzione (con spazi) | ```Contoso Foo Works Suite``` |
| ```{!Tool Name}``` | Il nome dello strumento (spazi inclusi) | ```Manage Foo Works``` |

Ecco un esempio di utilizzo:

```
wac create --company "Contoso Inc" --solution "Contoso Foo Works Suite" --tool "Manage Foo Works"
```

In questo modo viene creata una nuova cartella all'interno della directory di lavoro corrente usando il nome specificato per la soluzione, consente di copiare tutti i file modello necessari nel progetto e configura i file con l'azienda, di soluzione e nome dello strumento.  

Successivamente, passare alla cartella appena creata, quindi installare le dipendenze locali necessari eseguendo il comando seguente:

```
npm install
```

Al termine, aver configurato tutto il che necessario per caricare la nuova estensione Windows Admin Center. 

## <a name="add-content-to-your-extension"></a>Aggiungere contenuto all'estensione

Ora che è stata creata un'estensione con l'interfaccia CLI di Windows Admin Center, si è pronti per personalizzare il contenuto.  Per esempi di operazioni possibili, vedere le guide seguenti:

- Aggiungere un [modulo vuoto](guides/add-module.md)
- Aggiungere un [iFrame](guides/add-iframe.md)
- Creare un [provider di connessione personalizzata](guides/create-connection-provider.md)
- Modificare [radice comportamento di navigazione](guides/modify-root-navigation.md)
 
Sono disponibili anche altri esempi nostri [sito GitHub SDK](https://aka.ms/wacsdk):
-  [Strumenti di sviluppo](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/windows-admin-center-developer-tools) è un'estensione completamente funzionante che può essere caricate in sideload in Windows Admin Center e contiene un'ampia raccolta di esempi di funzionalità e lo strumento di esempio che è possibile esplorare e utilizzare in un'estensione personalizzata.

## <a name="build-and-side-load-your-extension"></a>Compilazione e sul lato caricare l'estensione

Successivamente, compilazione e sul lato, caricare l'estensione Windows Admin Center.  Aprire una finestra di comando, passare alla directory di origine, quindi è possibile passare alla compilazione.

* Crea e gestisci con gulp:

    ```
    gulp build
    gulp serve -p 4201
    ```

Devi scegliere una porta attualmente libera. Non tentare di utilizzare la porta su cui è in esecuzione Windows Admin Center.

Il progetto può essere trasferito localmente in un'istanza locale Windows Admin Center per scopi di test collegando il progetto gestito localmente in Windows Admin Center.

* Avvia Windows Admin Center in un Web browser
* Apri il debugger (F12)
* Apri la console e digita il comando seguente:

    ```
    MsftSme.sideLoad("http://localhost:4201")
    ```

*   Aggiorna il Web browser

Il progetto sarà ora visibile nell'elenco Strumenti con (trasferito localmente) accanto al nome.

## <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>Destinazione una versione diversa di Windows Admin Center SDK

L'estensione di stare al passo con le modifiche a SDK e piattaforma è facile.  Ottenere informazioni su come [destinare una versione diversa](target-sdk-version.md) Windows Admin Center SDK.