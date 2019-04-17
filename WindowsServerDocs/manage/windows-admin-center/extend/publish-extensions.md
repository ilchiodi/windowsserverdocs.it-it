---
title: Pubblicazione delle estensioni per Windows Admin Center
description: Pubblicazione delle estensioni per Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 762bd4613fa8ad6cdfb5b44745a7ce78b331499d
ms.sourcegitcommit: 3883eebbba70bfea0221e510863ee1a724a5f926
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/29/2018
ms.locfileid: "5783753"
---
# Pubblicazione delle estensioni

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Dopo aver sviluppato l'estensione, devi pubblicarlo e renderla disponibile ad altri utenti di test o usare. A seconda del gruppo di destinatari e scopo di pubblicazione, sono disponibili alcune opzioni che verrà presentiamo seguito insieme i passaggi e i requisiti per la pubblicazione.

## Opzioni per la pubblicazione

Sono disponibili tre opzioni principale per le origini pacchetto configurabile che supporta Windows Admin Center:
* Microsoft pubblica Windows Admin Center NuGet feed
* Il tuo feed NuGet privato
* Locale o condivisione file di rete

### Pubblicare l'estensione di Windows Admin Center feed

Per impostazione predefinita, Windows Admin Center è connesso a una NuGet feed gestita dal team prodotto Windows Admin Center in Microsoft. Versioni di anteprima anticipata di nuove estensioni sviluppate da Microsoft potrebbero essere pubblicato per il feed ed è disponibile agli utenti di Windows Admin Center. Esterni agli sviluppatori di pianificazione per la compilazione e di rilascio estensioni pubblicamente possono anche [inviare una richiesta](#publishing-your-extension-to-the-windows-admin-center-feed) per pubblicare il feed.

### La pubblicazione in una diversa NuGet feed

È anche possibile creare il tuo NuGet feed per pubblicare le estensioni per usando uno dei molti [opzioni diverse per impostare un'origine privata o usando una NuGet hosting del servizio](https://docs.microsoft.com/nuget/hosting-packages/overview). Il feed NuGet deve supportare l'API v2 NuGet. Dato che Windows Admin Center non supporta attualmente autenticazione feed, deve essere configurato per consentire l'accesso in lettura agli utenti il feed.

### La pubblicazione in una condivisione file

Per limitare l'accesso dell'estensione per l'organizzazione o a un gruppo limitato di persone, è possibile utilizzare una condivisione file SMB come un'estensione del feed. In questo caso, le autorizzazioni di condivisione e la cartella di file verranno applicate per consentire l'accesso al feed.

## Preparazione dell'estensione per il rilascio

Assicurati di leggere e prendere in considerazione i seguenti argomenti di sviluppo:

- [Controllare la visibilità dello strumento](guides/dynamic-tool-display.md)
- [Stringhe e localizzazione](guides/strings-localization.md)

### Prendi in considerazione il rilascio di una versione di anteprima

Se si rilascia una versione di anteprima dell'estensione per scopi di valutazione, ti consigliamo di che puoi:

- Aggiungi "(anteprima)" alla fine del titolo dell'estensione del file .nuspec
- Spiegare le limitazioni nella descrizione dell'estensione del file .nuspec

## Creazione di un pacchetto di estensione

Windows Admin Center utilizza pacchetti NuGet e feed per la distribuzione e scaricare le estensioni.  Affinché il pacchetto per la spedizione, dovrai generare un pacchetto NuGet contenente il plug-in e le estensioni.  Può contenere un singolo pacchetto di un'estensione dell'interfaccia utente oltre a un plug-in Gateway e la sezione seguente illustra il processo.

### 1. creare la tua estensione

Non appena sei pronto per iniziare a creare il pacchetto di estensione, crea una nuova directory nel tuo file system, aprire una console e CD in tale.  Questa sarà la directory radice che utilizzeremo per contenere tutte le nuspec e contenuto Directory in cui verranno costituiscono il pacchetto.  Abbiamo farà riferimento a questa cartella come "Pacchetto NuGet" per la durata di questo documento.

#### Estensioni dell'interfaccia utente

Per avviare il processo nella raccolta tutti i contenuti necessari per un'estensione dell'interfaccia utente, Esegui "gulp build" nel tuo strumento e assicurati che la build ha esito positivo.  Pacchetti di questo processo tutti i componenti insieme in una cartella denominata "bundle" che si trovano nella directory radice dell'estensione (allo stesso livello della directory src).  Copia questa directory e tutto lo del contenuto nella cartella "Pacchetto NuGet".

#### Plug-in gateway

Usando l'infrastruttura di compilazione (ciò potrebbe essere semplice come apertura di Visual Studio e facendo clic sul pulsante Build), compilare e compila il plug-in.  Aprire la directory di output di compilazione, copiare la DLL che rappresentano il plug-in e inserirli in una nuova cartella all'interno della directory "Pacchetto NuGet" denominata "pacchetti".  Non devi copiare la dll FeatureInterface, solo le DLL che rappresentano il codice.

### 2. creare il file .nuspec

Per creare il pacchetto NuGet, devi prima creare un file .nuspec. Un file .nuspec è un manifesto XML che contiene i metadati del pacchetto NuGet. Il manifesto viene usato per creare il pacchetto e di fornire informazioni per i consumatori.  Posiziona questo file nella radice della cartella "Pacchetto NuGet".

Ecco un esempio di file .nuspec e l'elenco delle proprietà necessarie o consigliate. Per lo schema completo, vedi il [riferimento .nuspec](https://docs.microsoft.com/nuget/reference/nuspec). Salva il file .nuspec alla cartella radice del progetto con un nome di file di tua scelta.

> [!IMPORTANT]
> Il ```<id>``` valore nel file .nuspec deve corrispondere il ```"name"``` valore nel tuo progetto ```manifest.json``` file, altrimenti l'estensione pubblicata non verrà caricati correttamente in Windows Admin Center.

```xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2011/08/nuspec.xsd">
  <metadata>
    <packageTypes>
      <packageType name="WindowsAdminCenterExtension" />
    </packageTypes>  
    <id>contoso.project.extension</id>
    <version>1.0.0</version>
    <title>Contoso Hello Extension</title>
    <authors>Contoso</authors>
    <owners>Contoso</owners>
    <requireLicenseAcceptance>false</requireLicenseAcceptance>
    <projectUrl>https://msft-sme.myget.org/feed/windows-admin-center-feed/package/nuget/contoso.sme.hello-extension</projectUrl>
    <licenseUrl>http://YourLicenseLink</licenseUrl>
    <iconUrl>http://YourLogoLink</iconUrl>
    <description>Hello World extension by Contoso</description>
    <copyright>(c) Contoso. All rights reserved.</copyright> 
    <tags></tags>
  </metadata>
  <files>
    <file src="bundle\**\*.*" target="ux" />
    <file src="package\**\*.*" target="gateway" />
  </files>
</package>
```

#### Necessario o consigliato proprietà

| Nome della proprietà | Obbligatorio / consigliato | Descrizione |
| ---- | ---- | ---- |
| packageType | Obbligatorio | Utilizzare "WindowsAdminCenterExtension" è il tipo di pacchetto NuGet definito per le estensioni di Windows Admin Center. |
| id | Obbligatorio | Identificatore univoco del pacchetto all'interno del feed. Questo valore deve corrispondere al valore "name" nel file manifest.json del progetto.  Per istruzioni, vedi [scelta di un identificatore univoco del pacchetto](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) . |
| title | Necessari per la pubblicazione in Windows Admin Center feed | Nome descrittivo per il pacchetto che viene visualizzato Gestione estensioni in Windows Admin Center. |
| version | Obbligatorio | Versione dell'estensione. Usando [Il controllo delle versioni semantica (SemVer convenzione)](http://semver.org/spec/v1.0.0.html) è consigliato ma non necessario. |
| autori | Obbligatorio | Se la pubblicazione per conto della società, Usa il nome dell'azienda. |
| description | Obbligatorio | Fornisci una descrizione delle funzionalità dell'estensione. |
| iconUrl | Consigliato per la pubblicazione in Windows Admin Center feed | URL per l'icona da visualizzare in Gestione estensioni. |
| projectUrl | Necessari per la pubblicazione in Windows Admin Center feed | URL del sito Web dell'estensione. Se non hai un sito Web separato, utilizzare l'URL per la pagina Web pacchetto sui feed di NuGet. |
| licenseUrl | Necessari per la pubblicazione in Windows Admin Center feed | URL contratto di licenza utente finale dell'estensione. |
| file | Obbligatorio | Queste due impostazioni consente di impostare la struttura di cartelle che prevede per le estensioni dell'interfaccia utente e plug-in Gateway Windows Admin Center. |

### 3. creare il pacchetto NuGet estensione

Utilizza il file .nuspec creato in precedenza, ora creerà il file .nupkg del pacchetto NuGet che puoi caricare e pubblicare il feed di NuGet.

1. Scarica lo strumento CLI nuget.exe dal [sito Web di strumenti di NuGet client](https://docs.microsoft.com/nuget/install-nuget-client-tools).
2. Eseguire "nuget.exe pack [nome del file .nuspec]" per creare il file .nupkg.

### 4. firmare il pacchetto NuGet dell'estensione

Qualsiasi file dll inclusi nell'estensione sono necessari per essere firmato con un certificato da un'autorità di certificazione (CA) attendibile. Per impostazione predefinita, i file DLL non firmati verranno bloccati l'esecuzione quando è in esecuzione Windows Admin Center in modalità di produzione.

È anche consigliabile che firmare il pacchetto NuGet di estensione per garantire l'integrità del pacchetto, ma non si tratta di un passaggio obbligatorio.

### 5. test del pacchetto NuGet dell'estensione

Il pacchetto di estensione è ora pronto per il test! Carica il file .nupkg a un feed NuGet o copiarla in una condivisione file. Per visualizzare e scaricare pacchetti da una condivisione file o un feed diversi, puoi sarà necessario per [modificare la configurazione del feed](../configure/using-extensions.md#installing-extensions-from-a-different-feed) in modo che punti al feed NuGet o file di condividere. Durante il test, assicurati che le proprietà vengono visualizzate correttamente in Gestione estensioni e puoi installare e disinstallare l'estensione correttamente.

## Pubblicare l'estensione di Windows Admin Center feed

Pubblicandole in Windows Admin Center feed, è possibile rendere disponibile l'estensione per qualsiasi utente di Windows Admin Center. Dato che Windows Admin Center SDK è ancora in anteprima, desideriamo lavorare a stretto contatto con te che consentono di risolvere i problemi di sviluppo e, assicurati che sono in grado di offrire un prodotto di qualità ed esperienza agli utenti.

Prima di rilasciare la versione iniziale di estensione, ti consigliamo di inviare una richiesta di recensione di estensione a Microsoft almeno 2-3 settimane prima del rilascio per garantire che dispone di tempo sufficiente per esaminare e puoi apportare modifiche alla tua estensione se necessario. Una volta che l'estensione è pronta per la pubblicazione, dovrai inviarle a Microsoft per la revisione e in caso di approvazione, ti verrà pubblicarla nel feed per te.

Successivamente, se vuoi rilascio di un aggiornamento per l'estensione, devi inviare la richiesta di un altro per la revisione. A seconda dell'ambito di modifica, allungare i tempi per recensioni di aggiornamento in genere devono essere più brevi.

### Inviare una richiesta di recensione di estensione a Microsoft

Per inviare una richiesta di recensione di estensione, immettere le informazioni seguenti e inviare come un messaggio e-mail da [wacextensionrequest@microsoft.com](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Review%20Request). Abbiamo risponderanno alla posta elettronica all'interno di una settimana.

```
Windows Admin Center Extension Review Request
1. Name and email address of extension owner/developer (up to 3 users). If you will be releasing an extension on behalf of your company, provide your company email address.
2. Company name (Only required if you are releasing an extension on behalf of your company):
3. Extension name:
4. Release target date (estimate):
5. For new extension submission - Extension description (early design wire frames, screen mockups or product screenshots are highly recommended):
6. For extension update review – Description of changes (include product screenshots if UI has been significantly changed):
```

### Inviare il pacchetto di estensione per la revisione e pubblicazione

Assicurati che seguire le istruzioni descritte sopra per la [creazione di un pacchetto di estensione](#creating-an-extension-package) e il file .nuspec è definito correttamente e i file vengono firmati. Ti consigliamo anche di disporre di un sito Web di progetto tra cui le seguenti:

- Descrizione dettagliata di estensione inclusi screenshot o un video
- Funzionalità sito Web o l'indirizzo di posta elettronica di ricevere feedback o domande

Quando sei pronto per pubblicare l'estensione, inviare un messaggio di posta elettronica a [wacextensionrequest@microsoft.com](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Package%20Review) e forniremo istruzioni su come inviare il tuo pacchetto di estensione. Una volta che viene visualizzato il pacchetto, esamineremo e in caso di approvazione, pubblicare il feed di Windows Admin Center.