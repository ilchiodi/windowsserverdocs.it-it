---
title: Sviluppare un plug-in del gateway
description: Sviluppare un plug-in gateway Windows Admin Center SDK (progetto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 66e36a349fc6bd38a77ccf4f00d380788ea4b422
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445948"
---
# <a name="develop-a-gateway-plugin"></a>Sviluppare un plug-in del gateway

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

Un plug-in Windows Admin Center gateway abilita la comunicazione API dall'interfaccia utente del tuo strumento o soluzione a un nodo di destinazione.  Windows Admin Center ospita un servizio gateway che inoltra i comandi e script dal plug-in gateway deve essere eseguito nei nodi di destinazione. Il servizio gateway può essere esteso per includere i plug-in gateway personalizzati che supportano protocolli diversi da quelli predefiniti.

Questi plug-in gateway sono inclusi per impostazione predefinita con Windows Admin Center:

* Plug-in gateway di PowerShell
* Plug-in gateway WMI

Se si desidera comunicare con un protocollo diverso da PowerShell o WMI, ad esempio con REST, è possibile creare un gateway plug-in.  I plug-in gateway vengono caricati in un AppDomain separato dal processo del gateway esistente, ma usare lo stesso livello di elevazione dei privilegi per i diritti.

> [!NOTE]
> Non hanno familiarità con i tipi di estensione diversi? Altre informazioni sul [tipi di architettura e l'estensione di estendibilità](understand-extensions.md).

## <a name="prepare-your-environment"></a>Preparazione dell'ambiente

Se non già [preparare l'ambiente](prepare-development-environment.md) installando le dipendenze e globali prerequisiti obbligatori per tutti i progetti.

## <a name="create-a-gateway-plugin-c-library"></a>Creare un plug-in gateway (C# libreria)

Per creare un plug-in gateway personalizzato, creare un nuovo C# classe che implementa il ```IPlugIn``` dell'interfaccia dal ```Microsoft.ManagementExperience.FeatureInterfaces``` dello spazio dei nomi.  

> [!NOTE]
> Il ```IFeature``` interfaccia, disponibile nelle versioni precedenti del SDK, sono ora contrassegnato come obsoleto.  Sviluppo di plug-in tutti i gateway deve usare IPlugIn (o, facoltativamente, la classe astratta HttpPlugIn).

### <a name="download-sample-from-github"></a>Scaricare l'esempio da GitHub

Per iniziare rapidamente con un plug-in gateway personalizzato, è possibile clonare o scaricare una copia del nostro [esempio C# plug-in project](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin) da Windows Admin Center SDK [sito GitHub](https://aka.ms/wacsdk).

### <a name="add-content"></a>Aggiungi contenuto

Aggiungere nuovo contenuto alla copia clonata del [esempio C# plug-in project](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin) file di progetto (o il proprio progetto) per contenere le API personalizzate e quindi compilare il plug-in gateway personalizzato DLL per essere usato nei passaggi successivi.

### <a name="deploy-plugin-for-testing"></a>Distribuire plug-in per test

Testare il plug-in gateway personalizzato DLL caricandola nel processo del gateway Windows Admin Center.

Windows Admin Center esegue la ricerca di tutti i plug-in un ```plugins``` nella cartella dei dati dell'applicazione dei macchina corrente (usando il valore CommonApplicationData dell'enumerazione Environment. SpecialFolder). In Windows 10 in questo percorso è ```C:\ProgramData\Server Management Experience```.  Se il ```plugins``` cartella non esiste ancora, è possibile creare la cartella manualmente.

> [!NOTE]
> È possibile sostituire il percorso del plug-in una build di debug, aggiornando il valore di configurazione "StaticsFolder". Se sta eseguendo il debug in locale, questa impostazione è nell'app. config della soluzione Desktop. 

All'interno della cartella plugins (in questo esempio ```C:\ProgramData\Server Management Experience\plugins```)

* Creare una nuova cartella con lo stesso nome il ```Name``` valore della proprietà il ```Feature``` del plug-in gateway personalizzato DLL (nel progetto di esempio, il ```Name``` è "Esempio di Uno")
* Copiare il file DLL plug-in di gateway personalizzato in questa nuova cartella
* Riavviare il processo di Windows Admin Center

Dopo aver riavviato il processo di amministrazione di Windows, sarà possibile esercitare le API del plug-in gateway personalizzato DLL eseguendo un'operazione GET, PUT, PATCH, DELETE o POST per ```http(s)://{domain|localhost}/api/nodes/{node}/features/{feature name}/{identifier}```

### <a name="optional-attach-to-plugin-for-debugging"></a>Facoltativo: Collegare al plug-in per il debug

In Visual Studio 2017, dal menu Debug, selezionare "Connetti a processo". Nella finestra successiva, scorrere l'elenco processi disponibili e selezionare SMEDesktop.exe, quindi fare clic su "Collega". Una volta, verrà avviato il debugger è possibile inserire un punto di interruzione nel codice di funzionalità e quindi esercizio tramite il formato dell'URL precedente. Per questo progetto di esempio (nome della funzionalità: Per esempio "Uno") l'URL è: "<http://localhost:6516/api/nodes/fake-server.my.domain.com/features/Sample%20Uno>"

## <a name="create-a-tool-extension-with-the-windows-admin-center-cli"></a>Creare un'estensione degli strumenti con la CLI di Windows Admin Center ##

A questo punto è necessario creare un'estensione degli strumenti da cui è possibile chiamare il plug-in gateway personalizzato.  Creare o selezionare una cartella in cui si desidera archiviare i file di progetto, aprire un prompt dei comandi e impostare tale cartella come cartella di lavoro.  Tramite la CLI di Windows Admin Center che è stato installato in precedenza, creare una nuova estensione con la sintassi seguente:

```
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| Value | Spiegazione | Esempio |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Il nome della società (spazi inclusi) | ```Contoso Inc``` |
| ```{!Tool Name}``` | Il nome dello strumento (spazi inclusi) | ```Manage Foo Works``` |

Ecco un esempio di utilizzo:

```
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

In questo modo viene creata una nuova cartella all'interno della directory di lavoro corrente usando il nome è specificato per il tuo strumento di copia tutti i file modello necessari nel progetto e configura i file con il nome della società e strumento.  

Successivamente, passare alla cartella appena creata, quindi installare le dipendenze locali necessari eseguendo il comando seguente:

```
npm install
```

Al termine, aver configurato tutto il che necessario per caricare la nuova estensione Windows Admin Center. 

## <a name="connect-your-tool-extension-to-your-custom-gateway-plugin"></a>Connettere l'estensione degli strumenti per il plug-in gateway personalizzato

Ora che è stata creata un'estensione con l'interfaccia CLI di Windows Admin Center, si è pronti per la connessione dell'estensione degli strumenti per il plug-in gateway personalizzato, seguendo questa procedura:

- Aggiungere un [modulo vuoto](guides/add-module.md)
- Usare il [plug-in gateway personalizzato](guides/use-custom-gateway-plugin.md) nella propria estensione degli strumenti
 
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