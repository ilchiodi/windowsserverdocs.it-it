---
title: Sviluppare un plug-in del gateway
description: Sviluppare un plug-in del gateway Windows Admin Center SDK (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 2b096b226190ad1ca3fd07c38b7b939d019ee30f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406932"
---
# <a name="develop-a-gateway-plugin"></a>Sviluppare un plug-in del gateway

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Un plug-in del gateway di Windows Admin Center consente la comunicazione API dall'interfaccia utente dello strumento o della soluzione a un nodo di destinazione.  L'interfaccia di amministrazione di Windows ospita un servizio gateway che inoltra comandi e script dai plug-in di gateway da eseguire nei nodi di destinazione. Il servizio gateway può essere esteso in modo da includere plug-in del gateway personalizzati che supportano protocolli diversi da quelli predefiniti.

Questi plug-in del gateway sono inclusi per impostazione predefinita con l'interfaccia di amministrazione di Windows:

* Plug-in del gateway PowerShell
* Plug-in gateway WMI

Se si desidera comunicare con un protocollo diverso da PowerShell o WMI, ad esempio con REST, è possibile creare un plug-in del gateway personalizzato.  I plug-in del gateway vengono caricati in un AppDomain separato dal processo del gateway esistente, ma utilizzano lo stesso livello di elevazione dei diritti.

> [!NOTE]
> Non si ha familiarità con i diversi tipi di estensione? Altre informazioni sull' [architettura di estendibilità e i tipi di estensione](understand-extensions.md).

## <a name="prepare-your-environment"></a>Preparazione dell'ambiente

Se non è già stato fatto, [preparare l'ambiente](prepare-development-environment.md) installando le dipendenze e i prerequisiti globali necessari per tutti i progetti.

## <a name="create-a-gateway-plugin-c-library"></a>Creazione di un plug-C# in del gateway (libreria)

Per creare un plug-in del gateway personalizzato, C# creare una nuova classe che implementi l'interfaccia ```IPlugIn``` dallo spazio dei nomi ```Microsoft.ManagementExperience.FeatureInterfaces```.  

> [!NOTE]
> L'interfaccia ```IFeature```, disponibile nelle versioni precedenti dell'SDK, è ora contrassegnata come obsoleta.  Lo sviluppo di tutti i plug-in del gateway deve usare IPlugIn (o facoltativamente la classe astratta HttpPlugIn).

### <a name="download-sample-from-github"></a>Scaricare l'esempio da GitHub

Per iniziare rapidamente a usare un plug-in per gateway personalizzato, è possibile clonare o scaricare una copia del [progetto di C# plug](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin) -in di esempio dal [sito Web](https://aka.ms/wacsdk)di Windows Admin Center SDK.

### <a name="add-content"></a>Aggiungi contenuto

Aggiungere nuovo contenuto alla copia clonata del progetto di [progetto C# di plug](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin) -in di esempio (o del progetto) per contenere le API personalizzate, quindi compilare il file dll del plug-in del gateway personalizzato da usare nei passaggi successivi.

### <a name="deploy-plugin-for-testing"></a>Distribuire il plug-in per il test

Testare la DLL del plug-in del gateway personalizzato caricando il plug-in nel processo gateway di amministrazione di Windows.

L'interfaccia di amministrazione di Windows Cerca tutti i plug-in in una cartella ```plugins``` nella cartella Application Data del computer corrente (usando il valore CommonApplicationData dell'enumerazione Environment. SpecialFolder). In Windows 10 questo percorso è ```C:\ProgramData\Server Management Experience```.  Se la cartella ```plugins``` non esiste ancora, è possibile creare la cartella manualmente.

> [!NOTE]
> È possibile eseguire l'override del percorso del plug-in in una build di debug aggiornando il valore di configurazione "StaticsFolder". Se si sta eseguendo il debug in locale, questa impostazione si trova nel file app. config della soluzione desktop. 

All'interno della cartella plugins (in questo esempio ```C:\ProgramData\Server Management Experience\plugins```)

* Creare una nuova cartella con lo stesso nome del valore della proprietà ```Name``` della ```Feature``` nella DLL del plug-in del gateway personalizzato (nel progetto di esempio, il ```Name``` è "campione uno")
* Copiare il file DLL del plug-in del gateway personalizzato in questa nuova cartella
* Riavviare il processo dell'interfaccia di amministrazione di Windows

Una volta riavviato il processo di amministrazione di Windows, sarà possibile eseguire le API nella DLL del plug-in del gateway personalizzato inviando un'operazione GET, PUT, PATCH, DELETE o POST a ```http(s)://{domain|localhost}/api/nodes/{node}/features/{feature name}/{identifier}```

### <a name="optional-attach-to-plugin-for-debugging"></a>Facoltativo: Connetti a plug-in per il debug

In Visual Studio 2017 selezionare "Connetti a processo" dal menu debug. Nella finestra successiva scorrere l'elenco processi disponibili e selezionare SMEDesktop. exe, quindi fare clic su "Connetti". Una volta avviato il debugger, è possibile inserire un punto di interruzione nel codice della funzionalità e quindi esercitare il formato dell'URL precedente. Per il progetto di esempio (nome della funzionalità: "Campione uno") l'URL è: "<http://localhost:6516/api/nodes/fake-server.my.domain.com/features/Sample%20Uno>"

## <a name="create-a-tool-extension-with-the-windows-admin-center-cli"></a>Creare un'estensione dello strumento con l'interfaccia della riga di comando di Windows Admin Center ##

A questo punto è necessario creare un'estensione dello strumento da cui è possibile chiamare il plug-in del gateway personalizzato.  Creare o selezionare una cartella in cui archiviare i file di progetto, aprire un prompt dei comandi e impostare tale cartella come directory di lavoro.  Usando l'interfaccia della riga di comando dell'interfaccia di amministrazione di Windows installata in precedenza, creare una nuova estensione con la sintassi seguente:

```
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| Value | Spiegazione | Esempio |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nome della società (con spazi) | ```Contoso Inc``` |
| ```{!Tool Name}``` | Nome dello strumento (con spazi) | ```Manage Foo Works``` |

Ecco un esempio di utilizzo:

```
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

Viene creata una nuova cartella nella directory di lavoro corrente usando il nome specificato per lo strumento, vengono copiati tutti i file di modello necessari nel progetto e vengono configurati i file con il nome della società e dello strumento.  

Successivamente, passare alla cartella appena creata, quindi installare le dipendenze locali necessarie eseguendo il comando seguente:

```
npm install
```

Al termine dell'operazione, è stato configurato tutto il necessario per caricare la nuova estensione nell'interfaccia di amministrazione di Windows. 

## <a name="connect-your-tool-extension-to-your-custom-gateway-plugin"></a>Connettere l'estensione dello strumento al plug-in del gateway personalizzato

Ora che è stata creata un'estensione con l'interfaccia della riga di comando di Windows Admin Center, si è pronti per connettere l'estensione dello strumento al plug-in del gateway personalizzato attenendosi alla procedura seguente:

- Aggiungere un [modulo vuoto](guides/add-module.md)
- Usare il plug-in del [gateway personalizzato](guides/use-custom-gateway-plugin.md) nell'estensione dello strumento
 
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