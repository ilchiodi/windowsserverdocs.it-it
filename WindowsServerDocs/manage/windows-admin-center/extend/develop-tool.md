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
ms.openlocfilehash: de2cbf3a47771555eef02cd7d18f93b2b33227b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406901"
---
# <a name="develop-a-tool-extension"></a>Sviluppare un'estensione dello strumento

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Un'estensione dello strumento è la modalità principale con cui gli utenti interagiscono con l'interfaccia di amministrazione di Windows per gestire una connessione, ad esempio un server o un cluster. Quando si fa clic su una connessione nella schermata iniziale dell'interfaccia di amministrazione di Windows e si esegue la connessione, viene visualizzato un elenco di strumenti nel riquadro di spostamento a sinistra. Quando si fa clic su uno strumento, l'estensione dello strumento viene caricata e visualizzata nel riquadro di destra.

Quando viene caricata un'estensione dello strumento, è possibile eseguire chiamate WMI o script di PowerShell in un cluster o un server di destinazione e visualizzare le informazioni nell'interfaccia utente o eseguire comandi in base all'input dell'utente. Le estensioni degli strumenti definiscono le soluzioni per le quali deve essere visualizzato, ottenendo un set di strumenti diverso per ogni soluzione.

> [!NOTE]
> Non si ha familiarità con i diversi tipi di estensione? Altre informazioni sull' [architettura di estendibilità e i tipi di estensione](understand-extensions.md).

## <a name="prepare-your-environment"></a>Preparazione dell'ambiente

Se non è già stato fatto, [preparare l'ambiente](prepare-development-environment.md) installando le dipendenze e i prerequisiti globali necessari per tutti i progetti.

## <a name="create-a-new-tool-extension-with-the-windows-admin-center-cli"></a>Creare una nuova estensione dello strumento con l'interfaccia della riga di comando di Windows Admin Center ##

Una volta installate tutte le dipendenze, si è pronti per creare la nuova estensione dello strumento.  Creare o selezionare una cartella contenente i file di progetto, aprire un prompt dei comandi e impostare tale cartella come directory di lavoro.  Usando l'interfaccia della riga di comando dell'interfaccia di amministrazione di Windows installata in precedenza, creare una nuova estensione con la sintassi seguente:

``` cmd
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| Value | Spiegazione | Esempio |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nome della società (con spazi) | ```Contoso Inc``` |
| ```{!Tool Name}``` | Nome dello strumento (con spazi) | ```Manage Foo Works``` |

Ecco un esempio di utilizzo:

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

Viene creata una nuova cartella nella directory di lavoro corrente usando il nome specificato per lo strumento, vengono copiati tutti i file di modello necessari nel progetto e vengono configurati i file con il nome della società e dello strumento.  

Successivamente, passare alla cartella appena creata, quindi installare le dipendenze locali necessarie eseguendo il comando seguente:

``` cmd
npm install
```

Al termine dell'operazione, è stato configurato tutto il necessario per caricare la nuova estensione nell'interfaccia di amministrazione di Windows. 

## <a name="add-content-to-your-extension"></a>Aggiungere contenuto all'estensione

Ora che è stata creata un'estensione con l'interfaccia della riga di comando di Windows Admin Center, si è pronti per personalizzare il contenuto.  Per esempi delle operazioni possibili, vedere le guide seguenti:

- Aggiungere un [modulo vuoto](guides/add-module.md)
- Aggiungere un [IFRAME](guides/add-iframe.md)
 
Altri esempi sono reperibili nel [sito di GITHUB SDK](https://aka.ms/wacsdk):
-  [Strumenti di sviluppo](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/windows-admin-center-developer-tools) è un'estensione completamente funzionante che può essere caricata a lato nell'interfaccia di amministrazione di Windows e contiene una raccolta completa di esempi di funzionalità e strumenti che è possibile esplorare e utilizzare nella propria estensione.

## <a name="customize-your-extensions-icon"></a>Personalizzare l'icona dell'estensione

È possibile personalizzare l'icona visualizzata per l'estensione nell'elenco degli strumenti.  A tale scopo, modificare tutte le voci ```icon``` in ```manifest.json``` per l'estensione:

``` json
"icon": "{!icon-uri}",
```

| Value | Spiegazione | URI di esempio |
| ----- | ----------- | ------- |
| ```{!icon-uri}``` | Il percorso della risorsa icona | ```assets/foo-icon.svg``` |

NOTA: Attualmente, le icone personalizzate non sono visibili quando si carica l'estensione in modalità dev.  Come soluzione alternativa, rimuovere il contenuto di ```target``` come indicato di seguito:

``` json
"target": "",
```

Questa configurazione è valida solo per il caricamento laterale in modalità dev, quindi è importante mantenere il valore contenuto in ```target``` e quindi ripristinarlo prima di pubblicare l'estensione.

## <a name="build-and-side-load-your-extension"></a>Compilazione e caricamento laterale dell'estensione

Successivamente, compilare e caricare la propria estensione nell'interfaccia di amministrazione di Windows.  Aprire una finestra di comando, passare alla directory di origine, quindi si è pronti per la compilazione.

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

## <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>Specificare come destinazione una versione diversa di Windows Admin Center SDK

L'aggiornamento dell'estensione con le modifiche apportate all'SDK e le modifiche apportate alla piattaforma è facile.  Leggere le informazioni su come [assegnare una versione diversa](target-sdk-version.md) di Windows Admin Center SDK.