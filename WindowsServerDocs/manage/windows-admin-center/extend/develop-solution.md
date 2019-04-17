---
title: Sviluppare un'estensione della soluzione
description: Sviluppare un'estensione della soluzione Windows Admin Center SDK (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ed5ecddbaef91f127846825e408a9a6ec65ff741
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081068"
---
# Sviluppare un'estensione della soluzione

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Soluzioni principalmente definiscono un tipo di oggetto che si desidera gestire tramite Windows Admin Center univoco.  Questi tipi di soluzioni/connessione sono inclusi con Windows Admin Center per impostazione predefinita:

* Connessioni di Windows Server
* Connessioni a PC Windows
* Connessioni cluster di failover
* Connessioni cluster iperconvergente

Quando selezioni una connessione dalla pagina di connessione di Windows Admin Center, viene caricata l'estensione della soluzione per tipo di connessione e Windows Admin Center tenterà di connettersi al nodo di destinazione. Se la connessione ha esito positivo, la soluzione di caricamento dell'interfaccia utente dell'estensione e Windows Admin Center visualizzerà gli strumenti per la soluzione nel riquadro di spostamento a sinistra.

Se vuoi compilare un'interfaccia di gestione per i servizi non è definito per i tipi di connessione predefinito sopra, un commutatore di rete o per altri componenti hardware non individuabile dal nome del computer, puoi creare il tuo estensione della soluzione.

> [!NOTE]
> Non hanno familiarità con i tipi di estensione diversa? Altre informazioni sui [tipi di architettura e l'estensione di estendibilità](understand-extensions.md).

## Preparazione dell'ambiente

Se hai già fatto, [preparare l'ambiente](prepare-development-environment.md) per l'installazione di dipendenze e globale prerequisito necessario per tutti i progetti.

## Creare una nuova estensione della soluzione con l'interfaccia CLI di Windows Admin Center ##

Dopo aver creato tutte le dipendenze installate, sei pronto per creare la nuova estensione della soluzione.  Creare o passa a una cartella che contiene i file di progetto, Apri un prompt dei comandi e impostare tale cartella come directory di lavoro.  Usando l'interfaccia CLI centro di amministrazione di Windows che è stato installato in precedenza, crea una nuova estensione con la sintassi seguente:

```
wac create --company "{!Company Name}" --solution "{!Solution Name}" --tool "{!Tool Name}"
```

| Valore | Spiegazione | Esempio |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Il nome della società (con spazi) | ```Contoso Inc``` |
| ```{!Solution Name}``` | Nome della soluzione (con spazi) | ```Contoso Foo Works Suite``` |
| ```{!Tool Name}``` | Il nome dello strumento (con spazi) | ```Manage Foo Works``` |

Ecco un esempio di utilizzo:

```
wac create --company "Contoso Inc" --solution "Contoso Foo Works Suite" --tool "Manage Foo Works"
```

Ciò crea una nuova cartella all'interno di directory di lavoro corrente usando il nome specificato per la soluzione, copia tutti i file di modello necessario nel tuo progetto e configura i file con la società, soluzioni e nome dello strumento.  

Successivamente, modifica directory nella cartella appena creata e quindi installa dipendenze locali obbligatorie eseguendo il comando seguente:

```
npm install
```

Una volta completata l'attivazione, hai impostato su tutto il che necessario per caricare la nuova estensione in Windows Admin Center. 

## Aggiungere contenuti alla tua estensione

Ora che hai creato un'estensione con l'interfaccia CLI di Windows Admin Center, sei pronto per personalizzare il contenuto.  Per esempi di cosa puoi fare, vedi queste guide:

- Aggiungere un [modulo vuoto](guides\add-module.md)
- Aggiungere un [iFrame](guides\add-iframe.md)
- Creare un [provider di connessione personalizzata](guides\create-connection-provider.md)
- Modificare il [comportamento di spostamento principale](guides\modify-root-navigation.md)
 
Sono disponibili anche altri esempi nostro [sito SDK GitHub](https://aka.ms/wacsdk):
-  [Strumenti di sviluppo](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/windows-admin-center-developer-tools) è un'estensione completamente funzionante che può essere trasferita localmente in Windows Admin Center e contiene un insieme completo di funzionalità di esempio ed esempi strumento che puoi cercare e utilizzare in un'estensione personalizzata.

## Crea e Trasferisci localmente estensione

Successivamente, crea e Trasferisci localmente l'estensione in Windows Admin Center.  Apri una finestra di comando, passare alla directory di origine, quindi sei pronto per creare.

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

## Fare riferimento a una versione diversa di Windows Admin Center SDK

Mantenere l'estensione aggiornato con le modifiche SDK e platform è facile.  Informazioni su come [destinazione una versione diversa](target-sdk-version.md) di Windows Admin Center SDK.