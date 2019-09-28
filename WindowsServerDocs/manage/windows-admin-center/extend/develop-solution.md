---
title: Sviluppare un'estensione della soluzione
description: Sviluppare un'estensione di soluzione Windows Admin Center SDK (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 6ac9c6296fdf9159c9f50a1304dd345932052ac9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357151"
---
# <a name="develop-a-solution-extension"></a>Sviluppare un'estensione della soluzione

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Le soluzioni definiscono principalmente un tipo di oggetto univoco che si vuole gestire tramite l'interfaccia di amministrazione di Windows.  Per impostazione predefinita, tali soluzioni/tipi di connessione sono inclusi nell'interfaccia di amministrazione di Windows:

* Connessioni di Windows Server
* Connessioni PC Windows
* Connessioni cluster di failover
* Connessioni cluster iperconvergenti

Quando si seleziona una connessione dalla pagina di connessione dell'interfaccia di amministrazione di Windows, viene caricata l'estensione della soluzione per il tipo di tale connessione e il centro di amministrazione di Windows tenterà di connettersi al nodo di destinazione. Se la connessione ha esito positivo, l'interfaccia utente dell'estensione della soluzione verrà caricata e il centro di amministrazione di Windows visualizzerà gli strumenti per tale soluzione nel riquadro di spostamento a sinistra.

Se si desidera creare un'interfaccia utente grafica di gestione per i servizi non definiti dai tipi di connessione predefiniti precedenti, ad esempio un commutire di rete o un altro hardware non individuabile in base al nome del computer, è possibile creare un'estensione della soluzione personalizzata.

> [!NOTE]
> Non si ha familiarità con i diversi tipi di estensione? Altre informazioni sull' [architettura di estendibilità e i tipi di estensione](understand-extensions.md).

## <a name="prepare-your-environment"></a>Preparazione dell'ambiente

Se non è già stato fatto, [preparare l'ambiente](prepare-development-environment.md) installando le dipendenze e i prerequisiti globali necessari per tutti i progetti.

## <a name="create-a-new-solution-extension-with-the-windows-admin-center-cli"></a>Creare una nuova estensione della soluzione con l'interfaccia della riga di comando di Windows Admin Center ##

Una volta installate tutte le dipendenze, si è pronti per creare la nuova estensione della soluzione.  Creare o selezionare una cartella contenente i file di progetto, aprire un prompt dei comandi e impostare tale cartella come directory di lavoro.  Usando l'interfaccia della riga di comando dell'interfaccia di amministrazione di Windows installata in precedenza, creare una nuova estensione con la sintassi seguente:

```
wac create --company "{!Company Name}" --solution "{!Solution Name}" --tool "{!Tool Name}"
```

| Value | Spiegazione | Esempio |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nome della società (con spazi) | ```Contoso Inc``` |
| ```{!Solution Name}``` | Nome della soluzione (con spazi) | ```Contoso Foo Works Suite``` |
| ```{!Tool Name}``` | Nome dello strumento (con spazi) | ```Manage Foo Works``` |

Ecco un esempio di utilizzo:

```
wac create --company "Contoso Inc" --solution "Contoso Foo Works Suite" --tool "Manage Foo Works"
```

Viene creata una nuova cartella nella directory di lavoro corrente usando il nome specificato per la soluzione, vengono copiati tutti i file di modello necessari nel progetto e vengono configurati i file con la società, la soluzione e il nome dello strumento.  

Successivamente, passare alla cartella appena creata, quindi installare le dipendenze locali necessarie eseguendo il comando seguente:

```
npm install
```

Al termine dell'operazione, è stato configurato tutto il necessario per caricare la nuova estensione nell'interfaccia di amministrazione di Windows. 

## <a name="add-content-to-your-extension"></a>Aggiungere contenuto all'estensione

Ora che è stata creata un'estensione con l'interfaccia della riga di comando di Windows Admin Center, si è pronti per personalizzare il contenuto.  Per esempi delle operazioni possibili, vedere le guide seguenti:

- Aggiungere un [modulo vuoto](guides/add-module.md)
- Aggiungere un [IFRAME](guides/add-iframe.md)
- Creazione di un [provider di connessione personalizzato](guides/create-connection-provider.md)
- Modificare il [comportamento di navigazione radice](guides/modify-root-navigation.md)
 
Altri esempi sono reperibili nel [sito di GITHUB SDK](https://aka.ms/wacsdk):
-  [Strumenti di sviluppo](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/windows-admin-center-developer-tools) è un'estensione completamente funzionante che può essere caricata a lato nell'interfaccia di amministrazione di Windows e contiene una raccolta completa di esempi di funzionalità e strumenti che è possibile esplorare e utilizzare nella propria estensione.

## <a name="build-and-side-load-your-extension"></a>Compilazione e caricamento laterale dell'estensione

Successivamente, compilare e caricare la propria estensione nell'interfaccia di amministrazione di Windows.  Aprire una finestra di comando, passare alla directory di origine, quindi si è pronti per la compilazione.

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

## <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>Specificare come destinazione una versione diversa di Windows Admin Center SDK

L'aggiornamento dell'estensione con le modifiche apportate all'SDK e le modifiche apportate alla piattaforma è facile.  Leggere le informazioni su come [assegnare una versione diversa](target-sdk-version.md) di Windows Admin Center SDK.