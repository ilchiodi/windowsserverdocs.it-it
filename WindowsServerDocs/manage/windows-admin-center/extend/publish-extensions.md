---
title: Pubblicazione di estensioni per Windows Admin Center
description: Pubblicazione di estensioni per Windows Admin Center (progetto Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 762bd4613fa8ad6cdfb5b44745a7ce78b331499d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820422"
---
# <a name="publishing-extensions"></a>Pubblicazione di estensioni

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

Dopo aver sviluppato l'estensione, è consigliabile per pubblicarla e renderla disponibile ad altri utenti per testare o utilizzare. A seconda di destinatari e lo scopo della pubblicazione, esistono alcune opzioni di cui viene presentata di seguito insieme ai passaggi e i requisiti per la pubblicazione.

## <a name="publishing-options"></a>Opzioni di pubblicazione

Esistono tre opzioni principali per le origini pacchetto configurabile che supporta Windows Admin Center:
* Microsoft pubblica Windows Admin Center feed NuGet
* Il proprio feed NuGet privati
* Locale o condivisione file di rete

### <a name="publishing-to-the-windows-admin-center-extension-feed"></a>Pubblicazione dell'estensione di Windows Admin Center feed

Per impostazione predefinita, Windows Admin Center è connesso al pacchetto NuGet feed gestito dal team di prodotto Windows Admin Center presso Microsoft. Le prime versioni di anteprima delle nuove estensioni sviluppate da Microsoft e possono essere pubblicati in questo feed e disponibili per gli utenti Windows Admin Center. Gli sviluppatori esterni prevede di compilare e rilasciare pubblicamente le estensioni possono anche [inviare una richiesta](#publishing-your-extension-to-the-windows-admin-center-feed) per pubblicare in questo feed.

### <a name="publishing-to-a-different-nuget-feed"></a>Pubblicazione in un diverso NuGet feed

È anche possibile creare il proprio feed NuGet per pubblicare estensioni in usando uno dei numerosi [diverse opzioni per configurare un'origine privata o tramite un servizio di hosting NuGet](https://docs.microsoft.com/nuget/hosting-packages/overview). Il feed NuGet deve supportare l'API v2 di NuGet. Poiché Windows Admin Center non supporta attualmente l'autenticazione di feed, il feed deve essere configurato per consentire l'accesso in lettura a tutti gli utenti.

### <a name="publishing-to-a-file-share"></a>La pubblicazione in una condivisione file

Per limitare l'accesso dell'estensione per l'organizzazione o a un gruppo limitato di persone, è possibile usare una condivisione file SMB come un'estensione del feed. In questo caso, è verranno applicate le autorizzazioni di condivisione e la cartella di file per consentire l'accesso al feed.

## <a name="preparing-your-extension-for-release"></a>Preparazione dell'estensione per il rilascio

Assicurarsi di leggere e prendere in considerazione i seguenti argomenti di sviluppo:

- [Controllare la visibilità dello strumento](guides/dynamic-tool-display.md)
- [Le stringhe e la localizzazione](guides/strings-localization.md)

### <a name="consider-releasing-as-a-preview-release"></a>Provare a rilasciare come una versione di anteprima

Se si sta rilasciando una versione di anteprima dell'estensione per scopi di valutazione, è consigliabile che è:

- Aggiungere "(anteprima)" alla fine del titolo dell'estensione nel file con estensione nuspec
- Illustrano le limitazioni nella descrizione dell'estensione nel file con estensione nuspec

## <a name="creating-an-extension-package"></a>Creazione di un pacchetto di estensione

Windows Admin Center Usa i pacchetti NuGet e i feed per distribuzione e il download delle estensioni.  Affinché il pacchetto da spedire, dovrai generare un pacchetto NuGet che contiene il plug-in ed estensioni.  Un unico pacchetto può contenere un'estensione dell'interfaccia utente oltre a un plug-in Gateway e la sezione seguente illustra il processo.

### <a name="1-build-your-extension"></a>1. L'estensione di compilazione

Non appena si è pronti per avviare Creazione del pacchetto dell'estensione, creare una nuova directory nel file system, aprire una console e recapito Continuo al suo interno.  Questa sarà la directory radice che utilizzeremo per contenere tutte le nuspec e contenuto Directory che costituiranno il pacchetto.  Questa cartella si farà riferimento come "Pacchetto NuGet" per la durata di questo documento.

#### <a name="ui-extensions"></a>Estensioni dell'interfaccia utente

Per iniziare il processo in tutto il contenuto necessario per un'estensione dell'interfaccia utente di raccolta, eseguire "compilazione gulp" sullo strumento e assicurarsi che la compilazione ha esito positivo.  Pacchetti in questo processo tutti i componenti insieme in una cartella denominata "del bundle" che si trovano nella directory radice dell'estensione (allo stesso livello della directory src).  Copiare la directory e tutte le contenuto nella cartella "NuGet Package".

#### <a name="gateway-plugins"></a>Plug-in gateway

Usa l'infrastruttura di compilazione (può essere semplice come l'apertura di Visual Studio e fare clic sul pulsante di compilazione), compilare e compilare il plug-in.  Aprire la directory di output di compilazione e copiare le DLL che rappresentano il plug-in e inserirli in una nuova cartella all'interno della directory "NuGet Package" denominata "package".  Non devi copiare la dll FeatureInterface, solo le DLL che rappresentano il codice.

### <a name="2-create-the-nuspec-file"></a>2. Creare il file con estensione nuspec

Per creare il pacchetto NuGet, è necessario creare innanzitutto un file con estensione nuspec. Un file con estensione nuspec è un manifesto XML che contiene i metadati del pacchetto NuGet. Questo manifesto viene usato per compilare il pacchetto e per fornire informazioni ai consumer.  Inserire questo file nella radice della cartella "NuGet Package".

Di seguito è riportato un esempio di file con estensione nuspec e l'elenco di proprietà necessari o consigliate. Per lo schema completo, vedere la [riferimento file con estensione nuspec](https://docs.microsoft.com/nuget/reference/nuspec). Salvare il file con estensione nuspec alla cartella radice del progetto con un nome di file di propria scelta.

> [!IMPORTANT]
> Il ```<id>``` valore nel file con estensione nuspec deve corrispondere il ```"name"``` valore del progetto ```manifest.json``` file o, in caso contrario, le estensioni pubblicate non verranno caricato correttamente in Windows Admin Center.

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

#### <a name="required-or-recommended-properties"></a>Richieste o consigliate delle proprietà

| Nome proprietà | Obbligatorio / consigliato | Descrizione |
| ---- | ---- | ---- |
| packageType | Obbligatorio | Usare "WindowsAdminCenterExtension", ovvero il tipo di pacchetto NuGet definito per le estensioni Windows Admin Center. |
| ID | Obbligatorio | Identificatore univoco del pacchetto all'interno del feed. Questo valore deve corrispondere al valore "name" nel file manifest del progetto.  Visualizzare [scelta di un identificatore univoco del pacchetto](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) per materiale sussidiario. |
| title | Obbligatorio per la pubblicazione per il Windows Admin Center feed | Nome descrittivo per il pacchetto che viene visualizzato in Gestione estensioni a colori Windows Admin Center. |
| version | Obbligatorio | Versione dell'estensione. Usando [Versionamento semantico (SemVer convenzione)](http://semver.org/spec/v1.0.0.html) è consigliata ma non richiesta. |
| Autori | Obbligatorio | Se la pubblicazione per conto dell'azienda, usare il nome della società. |
| description | Obbligatorio | Fornire una descrizione delle funzionalità dell'estensione. |
| iconUrl | Consigliato quando si pubblica il Windows Admin Center feed | URL per l'icona da visualizzare in Gestione estensioni. |
| projectUrl | Obbligatorio per la pubblicazione per il Windows Admin Center feed | URL al sito Web dell'estensione. Se non si dispone di un sito Web separato, usare l'URL per la pagina Web del pacchetto nel feed NuGet. |
| licenseUrl | Obbligatorio per la pubblicazione per il Windows Admin Center feed | URL al contratto di licenza dell'estensione utente finale. |
| file | Obbligatorio | Queste due impostazioni configurare la struttura di cartelle che si aspetta che Windows Admin Center per le estensioni dell'interfaccia utente e i plug-in Gateway. |

### <a name="3-build-the-extension-nuget-package"></a>3. Compilare il pacchetto NuGet di estensione

Usa il file con estensione nuspec creato in precedenza, si creerà ora il file con estensione nupkg del pacchetto NuGet che è possibile caricare e pubblicare il feed NuGet.

1. Scaricare lo strumento della riga di comando nuget.exe il [sito Web di strumenti client NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).
2. Eseguire "nuget.exe pack [nome file con estensione nuspec]" per creare il file con estensione nupkg.

### <a name="4-signing-your-extension-nuget-package"></a>4. Firma il pacchetto NuGet di estensione

Tutti i file con estensione dll inclusi nella propria estensione sono deve essere firmato con un certificato da un'autorità di certificazione attendibile (CA). Per impostazione predefinita, i file DLL senza segno non potranno in esecuzione al momento Windows Admin Center è in esecuzione in modalità di produzione.

È anche consigliabile che si firma il pacchetto NuGet di estensione per assicurare l'integrità del pacchetto, ma questo non è un passaggio obbligatorio.

### <a name="5-test-your-extension-nuget-package"></a>5. Testare il pacchetto NuGet di estensione

Pacchetto di estensione è ora pronto per il test. Caricare il file con estensione nupkg a un feed NuGet oppure copiarlo in una condivisione file. Per visualizzare e scaricare i pacchetti da un feed diverso o una condivisione di file, dovrai [modificare la configurazione del feed](../configure/using-extensions.md#installing-extensions-from-a-different-feed) per puntare al feed di NuGet o condivisione file. Durante il test, assicurarsi che le proprietà vengono visualizzate correttamente in Gestione estensioni ed è stato possibile installare e disinstallare l'estensione.

## <a name="publishing-your-extension-to-the-windows-admin-center-feed"></a>Pubblicazione dell'estensione per il Windows Admin Center feed

Tramite la pubblicazione del feed di di Windows Admin Center, è possibile rendere disponibile l'estensione a tutti gli utenti Windows Admin Center. Poiché il SDK di Windows Admin Center è ancora in anteprima, vorremmo a collaborare con te per risolvere i problemi di sviluppo e, assicurarsi di che essere in grado di offrire un prodotto di qualità ed esperienza agli utenti.

Prima di rilasciare la versione iniziale dell'estensione, è consigliabile inviare una richiesta di revisione di estensione a Microsoft almeno 2-3 settimane prima della versione in modo da garantire il tempo sufficiente per esaminare e consente di apportare modifiche all'estensione, se necessario. Una volta pronta per la pubblicazione dell'estensione, è necessario inviarlo a Microsoft per la revisione e se approvata, si sarà pubblicarla al feed per l'utente.

In un secondo momento, se si desidera rilasciare un aggiornamento per l'estensione, è necessario inviare un'altra richiesta di revisione. Mentre in base all'ambito di modifica, il tempo di turnaround per le revisioni di aggiornamento deve essere in genere più breve.

### <a name="submit-an-extension-review-request-to-microsoft"></a>Inviare una richiesta di revisione di estensione a Microsoft

Per inviare una richiesta di revisione di estensione, immettere le informazioni seguenti e inviare un messaggio di posta elettronica [ wacextensionrequest@microsoft.com ](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Review%20Request). Messaggio di posta elettronica risponderemo entro una settimana.

```
Windows Admin Center Extension Review Request
1. Name and email address of extension owner/developer (up to 3 users). If you will be releasing an extension on behalf of your company, provide your company email address.
2. Company name (Only required if you are releasing an extension on behalf of your company):
3. Extension name:
4. Release target date (estimate):
5. For new extension submission - Extension description (early design wire frames, screen mockups or product screenshots are highly recommended):
6. For extension update review – Description of changes (include product screenshots if UI has been significantly changed):
```

### <a name="submit-your-extension-package-for-review-and-publishing"></a>Inviare il pacchetto di estensione per la revisione e la pubblicazione

Assicurarsi di seguire le istruzioni precedenti relative [creazione di un pacchetto di estensione](#creating-an-extension-package) e il file con estensione nuspec sia definito correttamente e i file sono firmati. Si consiglia inoltre di avere un sito Web del progetto, incluse le seguenti:

- Descrizione dettagliata dell'estensione inclusi screenshot o video
- Funzionalità di indirizzo o il sito Web tramite posta elettronica per ricevere commenti e suggerimenti o domande

Quando si è pronti per pubblicare l'estensione, inviare un messaggio di posta elettronica [ wacextensionrequest@microsoft.com ](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Package%20Review) e Microsoft fornirà le istruzioni su come inviare il pacchetto di estensione. Una volta che si riceve il pacchetto, Microsoft esaminerà e se approvato, pubblica per il Windows Admin Center feed.