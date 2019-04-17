---
title: Sviluppare un plug-in gateway
description: Sviluppare un plug-in di gateway Windows Admin Center SDK (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 93cee5b8e3611a264119947103d22d9aa3b9a56b
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081158"
---
# Sviluppare un plug-in gateway

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Un plug-in gateway Windows Admin Center consente di comunicazione API dall'interfaccia utente della tua strumento o una soluzione a un nodo di destinazione.  Windows Admin Center ospita un servizio gateway che inoltra comandi e script da plug-in gateway per essere eseguiti sui nodi di destinazione. Il servizio gateway può essere esteso per includere i plug-in gateway personalizzati che supportano protocolli diversi da quelli predefiniti.

Questi plug-in gateway sono inclusi per impostazione predefinita con Windows Admin Center:

* Plug-in gateway di PowerShell
* Plug-in gateway WMI

Se si desidera comunicare con un protocollo diverso da PowerShell o WMI, ad esempio con REST, puoi creare il tuo plug-in gateway.  Plug-in gateway vengono caricati in un AppDomain separato dal processo di gateway esistenti, ma usa lo stesso livello di elevazione dei diritti.

> [!NOTE]
> Non hanno familiarità con i tipi di estensione diversa? Altre informazioni sui [tipi di architettura e l'estensione di estendibilità](understand-extensions.md).

## Preparazione dell'ambiente

Se hai già fatto, [preparare l'ambiente](prepare-development-environment.md) per l'installazione di dipendenze e globale prerequisito necessario per tutti i progetti.

## Creare un plug-in di gateway (libreria c#)

Per creare un plug-in gateway personalizzato, creare una nuova classe c# che implementa il ```IPlugIn``` interfaccia dal ```Microsoft.ManagementExperience.FeatureInterfaces``` dello spazio dei nomi.  

> [!NOTE]
> Il ```IFeature``` interfaccia, disponibile nelle versioni precedenti del SDK, ora sono contrassegnata come obsoleto.  Lo sviluppo di plug-in di gateway deve usare IPlugIn (o facoltativamente astratta HttpPlugIn).

### Scarica l'esempio da GitHub

Per iniziare rapidamente con un plug-in gateway personalizzato, puoi clonare o scaricare una copia del [progetto di plug-in di esempio c#](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin) dal nostro [sito GitHub](https://aka.ms/wacsdk)di Windows Admin Center SDK.

### Aggiungere contenuto

Aggiungere nuovo contenuto la copia clonato di progetto [c# plug-in del progetto di esempio](https://github.com/Microsoft/windows-admin-center-sdk/tree/master/GatewayPluginExample/Plugin) (o il tuo progetto) per contenere l'API personalizzate, quindi compila il file DLL di plug-in gateway personalizzato per l'uso nei passaggi successivi.

### Distribuire plug-in di test

Testare plug-in gateway personalizzato DLL da caricare nel processo di gateway Windows Admin Center.

Windows Admin Center ha un aspetto per tutti i plug-in un ```plugins``` cartella nella cartella di dati dell'applicazione del computer corrente (con il valore CommonApplicationData dell'enumerazione SpecialFolder). In Windows 10 questo percorso è ```C:\ProgramData\Server Management Experience```.  Se il ```plugins``` cartella non esiste ancora, è possibile creare manualmente la cartella.

> [!NOTE]
> È possibile ignorare la posizione di plug-in una build di debug aggiornando il valore di configurazione "StaticsFolder". Se si esegue il debug in locale, questa impostazione è in App della soluzione Desktop. 

All'interno della cartella di plug-in (in questo esempio, ```C:\ProgramData\Server Management Experience\plugins```)

* Crea una nuova cartella con lo stesso nome il ```Name``` valore della proprietà il ```Feature``` nella DLL plug-in gateway personalizzato (del progetto di esempio, il ```Name``` è "Esempio Uno")
* Copia il file DLL di plug-in gateway personalizzato in questa nuova cartella
* Riavviare il processo di Windows Admin Center

Dopo aver riavviato il processo di amministrazione di Windows, sarai in grado di esercitare le API in plug-in gateway personalizzato DLL inviando un GET, PUT, PATCH, eliminare o pubblicare ```http(s)://{domain|localhost}/api/nodes/{node}/features/{feature name}/{identifier}```

### Facoltativo: Connetti a plug-in per eseguire il debug

In Visual Studio 2017, dal menu Debug, seleziona "Connetti a processo". Nella finestra del successiva, scorrere l'elenco di processi disponibili e selezionare SMEDesktop.exe, quindi fai clic su "Connetti". Dopo l'avvio del debugger, è possibile inserire un punto di interruzione nel codice di funzionalità e quindi esercizio tramite il formato dell'URL sopra. Per il progetto di esempio (nome della funzionalità: "Esempio Uno") è l'URL: "http://localhost:6516/api/nodes/fake-server.my.domain.com/features/Sample%20Uno"

## Creare un'estensione dello strumento con l'interfaccia CLI di Windows Admin Center ##

Ora dobbiamo creare un'estensione dello strumento da cui puoi chiamare plug-in gateway personalizzati.  Crea o seleziona una cartella in cui si desidera archiviare i file di progetto, Apri un prompt dei comandi e impostare tale cartella come directory di lavoro.  Con Windows Admin Center CLI che è stato installato in precedenza, crea una nuova estensione con la sintassi seguente:

```
wac create --company "{!Company Name}" --tool "{!Tool Name}"
```

| Valore | Spiegazione | Esempio |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Il nome della società (con spazi) | ```Contoso Inc``` |
| ```{!Tool Name}``` | Il nome dello strumento (con spazi) | ```Manage Foo Works``` |

Ecco un esempio di utilizzo:

```
wac create --company "Contoso Inc" --tool "Manage Foo Works"
```

Ciò crea una nuova cartella all'interno di directory di lavoro corrente usando il nome specificato per il tuo strumento, copia tutti i file di modello necessario nel tuo progetto e configura i file con il nome della società e lo strumento.  

Successivamente, modifica directory nella cartella appena creata e quindi installa dipendenze locali obbligatorie eseguendo il comando seguente:

```
npm install
```

Una volta completata l'attivazione, hai impostato su tutto il che necessario per caricare la nuova estensione in Windows Admin Center. 

## Connettere l'estensione dello strumento di plug-in gateway personalizzati

Ora che hai creato un'estensione con l'interfaccia CLI di Windows Admin Center, sei pronto per la connessione l'estensione dello strumento di plug-in gateway personalizzato, seguendo questi passaggi:

- Aggiungere un [modulo vuoto](guides\add-module.md)
- Usa il [plug-in gateway personalizzati](guides\use-custom-gateway-plugin.md) nell'estensione dello strumento
 
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