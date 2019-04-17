---
title: Sviluppare un'estensione dello strumento
description: Sviluppare un'estensione dello strumento Windows Admin Center SDK (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 7dd213f7032ab77021bbfcbdc966c9c2307b86eb
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081058"
---
# Sviluppare un'estensione dello strumento

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Un'estensione dello strumento è il modo principale che gli utenti interagiscono con Windows Admin Center possa gestire una connessione, ad esempio un server o cluster. Quando fai clic su una connessione nella schermata iniziale di Windows Admin Center e connettersi, verrà visualizzato un elenco degli strumenti nel riquadro di spostamento a sinistra. Quando fai clic su uno strumento, l'estensione dello strumento viene caricata e visualizzata nel riquadro destro.

Quando un'estensione dello strumento viene caricata, può eseguire chiamate WMI o script di PowerShell in un cluster o server di destinazione e visualizzare informazioni nell'interfaccia utente o eseguire comandi in base all'input dell'utente. Lo strumento estensioni definiscono le soluzioni per cui deve essere visualizzata, generando un diverso set di strumenti per ciascuna soluzione.

> [!NOTE]
> Non hanno familiarità con i tipi di estensione diversa? Altre informazioni sui [tipi di architettura e l'estensione di estendibilità](understand-extensions.md).

## Preparazione dell'ambiente

Se hai già fatto, [preparare l'ambiente](prepare-development-environment.md) per l'installazione di dipendenze e globale prerequisito necessario per tutti i progetti.

## Creare una nuova estensione dello strumento con l'interfaccia CLI di Windows Admin Center ##

Dopo aver creato tutte le dipendenze installate, sei pronto per creare la nuova estensione dello strumento.  Creare o passa a una cartella che contiene i file di progetto, Apri un prompt dei comandi e impostare tale cartella come directory di lavoro.  Usando l'interfaccia CLI centro di amministrazione di Windows che è stato installato in precedenza, crea una nuova estensione con la sintassi seguente:

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

## Aggiungere contenuti alla tua estensione

Ora che hai creato un'estensione con l'interfaccia CLI di Windows Admin Center, sei pronto per personalizzare il contenuto.  Per esempi di cosa puoi fare, vedi queste guide:

- Aggiungere un [modulo vuoto](guides\add-module.md)
- Aggiungere un [iFrame](guides\add-iframe.md)
 
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