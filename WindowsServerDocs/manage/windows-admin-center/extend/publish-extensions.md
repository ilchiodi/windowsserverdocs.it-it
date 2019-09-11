---
title: Estensioni di pubblicazione per l'interfaccia di amministrazione di Windows
description: Estensioni di pubblicazione per l'interfaccia di amministrazione di Windows (Project Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d2bb97fb65e3fbf5c7809317a8565ff7051d0447
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869701"
---
# <a name="publishing-extensions"></a>Pubblicazione di estensioni

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Dopo aver sviluppato l'estensione, è necessario pubblicarla e renderla disponibile ad altri utenti per il test o l'utilizzo. A seconda del pubblico e dello scopo della pubblicazione, sono disponibili alcune opzioni che verranno presentate di seguito insieme ai passaggi e ai requisiti per la pubblicazione.

## <a name="publishing-options"></a>Opzioni di pubblicazione

Sono disponibili tre opzioni principali per le origini dei pacchetti configurabili supportate dal centro di amministrazione di Windows:
* Feed NuGet del centro di amministrazione di Windows pubblico Microsoft
* Feed NuGet privato
* Condivisione file locale o di rete

### <a name="publishing-to-the-windows-admin-center-extension-feed"></a>Pubblicazione nel feed di estensione dell'interfaccia di amministrazione di Windows

Per impostazione predefinita, l'interfaccia di amministrazione di Windows è connessa a un feed NuGet gestito dal team del prodotto Windows Admin Center in Microsoft. Le prime versioni di anteprima delle nuove estensioni sviluppate da Microsoft possono essere pubblicate in questo feed e disponibili per gli utenti del centro di amministrazione di Windows. Gli sviluppatori esterni che pianificano di compilare e rilasciare le estensioni pubblicamente possono [inviare anche una richiesta](#publishing-your-extension-to-the-windows-admin-center-feed) di pubblicazione in questo feed.

### <a name="publishing-to-a-different-nuget-feed"></a>Pubblicazione in un feed NuGet diverso

È anche possibile creare un feed NuGet personalizzato per pubblicare le estensioni in usando una delle numerose [opzioni disponibili per configurare un'origine privata o usare un servizio di hosting NuGet](https://docs.microsoft.com/nuget/hosting-packages/overview). Il feed NuGet deve supportare l'API NuGet V2. Poiché l'interfaccia di amministrazione di Windows non supporta attualmente l'autenticazione del feed, il feed deve essere configurato per consentire l'accesso in lettura a chiunque.

### <a name="publishing-to-a-file-share"></a>Pubblicazione in una condivisione file

Per limitare l'accesso dell'estensione all'organizzazione o a un gruppo limitato di utenti, è possibile usare una condivisione file SMB come feed di estensione. In questo caso, verranno applicate le autorizzazioni per la condivisione file e la cartella per consentire l'accesso al feed.

## <a name="preparing-your-extension-for-release"></a>Preparazione dell'estensione per la versione

Assicurarsi di leggere e considerare gli argomenti di sviluppo seguenti:

- [Controllare la visibilità dello strumento](guides/dynamic-tool-display.md)
- [Stringhe e localizzazione](guides/strings-localization.md)

### <a name="consider-releasing-as-a-preview-release"></a>Provare a rilasciare come versione di anteprima

Se si sta rilasciando una versione di anteprima dell'estensione a scopo di valutazione, è consigliabile:

- Aggiungere "(Preview)" alla fine del titolo dell'estensione nel file. NuSpec
- Illustrare le limitazioni nella descrizione dell'estensione nel file nuspec

## <a name="creating-an-extension-package"></a>Creazione di un pacchetto di estensione

L'interfaccia di amministrazione di Windows usa pacchetti NuGet e feed per la distribuzione e il download delle estensioni.  Per poter spedire il pacchetto, è necessario generare un pacchetto NuGet contenente i plug-in e le estensioni.  Un singolo pacchetto può contenere sia un'estensione dell'interfaccia utente che un plug-in del gateway. nella sezione seguente viene illustrato il processo.

### <a name="1-build-your-extension"></a>1. Compilare l'estensione

Non appena si è pronti per iniziare a creare il pacchetto dell'estensione, creare una nuova directory nella file system, aprire una console e un CD al suo interno.  Si tratta della directory radice che verrà usata per contenere tutte le directory NuSpec e Content che compongono il pacchetto.  Per la durata del documento verrà fatto riferimento a questa cartella come "pacchetto NuGet".

#### <a name="ui-extensions"></a>Estensioni dell'interfaccia utente

Per iniziare il processo di raccolta di tutto il contenuto necessario per un'estensione dell'interfaccia utente, eseguire "Gulp Build" nello strumento e assicurarsi che la compilazione abbia esito positivo.  Questo processo raggruppa tutti i componenti in una cartella denominata "bundle", che si trova nella directory radice dell'estensione, allo stesso livello della directory src.  Copiare questa directory e tutto il relativo contenuto nella cartella "pacchetto NuGet".

#### <a name="gateway-plugins"></a>Plug-in del gateway

Usando l'infrastruttura di compilazione (potrebbe essere semplice come aprire Visual Studio e fare clic sul pulsante Compila), compilare e compilare il plug-in.  Aprire la directory di output di compilazione e copiare le dll che rappresentano il plug-in e inserirle in una nuova cartella all'interno della directory "pacchetto NuGet" denominata "pacchetto".  Non è necessario copiare la dll FeatureInterface, bensì solo le dll che rappresentano il codice.

### <a name="2-create-the-nuspec-file"></a>2. Creare il file con estensione NuSpec

Per creare il pacchetto NuGet, è prima necessario creare un file con estensione NuSpec. Un file con estensione NuSpec è un manifesto XML che contiene i metadati del pacchetto NuGet. Questo manifesto viene utilizzato sia per compilare il pacchetto che per fornire informazioni ai consumer.  Inserire questo file nella directory radice della cartella "NuGet Package".

Ecco un esempio di file con estensione NuSpec e l'elenco delle proprietà obbligatorie o consigliate. Per lo schema completo, vedere il [riferimento. NuSpec](https://docs.microsoft.com/nuget/reference/nuspec). Salvare il file con estensione NuSpec nella cartella radice del progetto con il nome di file desiderato.

> [!IMPORTANT]
> Il ```<id>``` valore nel file. NuSpec deve corrispondere al ```"name"``` ```manifest.json``` valore nel file del progetto. in caso contrario, l'estensione pubblicata non verrà caricata correttamente nell'interfaccia di amministrazione di Windows.

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

#### <a name="required-or-recommended-properties"></a>Proprietà obbligatorie o consigliate

| Nome proprietà | Obbligatorio/consigliato | Descrizione |
| ---- | ---- | ---- |
| PackageType | Obbligatoria | Usare "WindowsAdminCenterExtension", che è il tipo di pacchetto NuGet definito per le estensioni dell'interfaccia di amministrazione di Windows. |
| id | Obbligatoria | Identificatore univoco del pacchetto all'interno del feed. Questo valore deve corrispondere al valore "Name" nel file manifest. JSON del progetto.  Per informazioni aggiuntive, vedere [scelta di un identificatore univoco del pacchetto](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number) . |
| title | Obbligatorio per la pubblicazione nel feed dell'interfaccia di amministrazione di Windows | Nome descrittivo per il pacchetto visualizzato in Gestione estensioni di Windows Admin Center. |
| version | Obbligatoria | Versione dell'estensione. L'uso del [controllo delle versioni semantico (convenzione SemVer)](http://semver.org/spec/v1.0.0.html) è consigliato ma non obbligatorio. |
| Autori | Obbligatoria | Se si pubblica per conto dell'azienda, usare il nome della società. |
| description | Obbligatoria | Fornire una descrizione delle funzionalità dell'estensione. |
| iconUrl | Consigliato per la pubblicazione nel feed dell'interfaccia di amministrazione di Windows | URL dell'icona da visualizzare in Gestione estensioni. |
| projectUrl | Obbligatorio per la pubblicazione nel feed dell'interfaccia di amministrazione di Windows | URL del sito Web dell'estensione. Se non si dispone di un sito Web separato, usare l'URL della pagina Web del pacchetto nel feed NuGet. |
| licenseUrl | Obbligatorio per la pubblicazione nel feed dell'interfaccia di amministrazione di Windows | URL del contratto di licenza con l'utente finale dell'estensione. |
| files | Obbligatoria | Queste due impostazioni configurano la struttura di cartelle prevista dal centro di amministrazione di Windows per le estensioni dell'interfaccia utente e i plug-in del gateway. |

### <a name="3-build-the-extension-nuget-package"></a>3. Compilare il pacchetto NuGet di estensione

Usando il file con estensione NuSpec creato in precedenza, si creerà il file Package. nupkg di NuGet che è possibile caricare e pubblicare nel feed NuGet.

1. Scaricare lo strumento dell'interfaccia della riga di comando NuGet. exe dal [sito Web degli strumenti client di NuGet](https://docs.microsoft.com/nuget/install-nuget-client-tools).
2. Eseguire "NuGet. exe Pack [. NuSpec file name]" per creare il file. nupkg.

### <a name="4-signing-your-extension-nuget-package"></a>4. Firma del pacchetto NuGet di estensione

Qualsiasi file con estensione dll incluso nell'estensione deve essere firmato con un certificato di un'autorità di certificazione (CA) attendibile. Per impostazione predefinita, i file dll non firmati non vengono eseguiti quando l'interfaccia di amministrazione di Windows è in esecuzione in modalità di produzione.

Si consiglia inoltre di firmare il pacchetto NuGet di estensione per garantire l'integrità del pacchetto, ma questo non è un passaggio obbligatorio.

### <a name="5-test-your-extension-nuget-package"></a>5. Testare il pacchetto NuGet di estensione

Il pacchetto di estensione è ora pronto per il test. Caricare il file con estensione nupkg in un feed NuGet o copiarlo in una condivisione file. Per visualizzare e scaricare i pacchetti da un feed o una condivisione file diversa, è necessario [modificare la configurazione del feed](../configure/using-extensions.md#installing-extensions-from-a-different-feed) in modo che punti al feed NuGet o alla condivisione file. Durante il test, verificare che le proprietà siano visualizzate correttamente in Gestione estensioni ed è possibile installare e disinstallare l'estensione.

## <a name="publishing-your-extension-to-the-windows-admin-center-feed"></a>Pubblicazione dell'estensione nel feed dell'interfaccia di amministrazione di Windows

Pubblicando il feed dell'interfaccia di amministrazione di Windows, è possibile rendere disponibile l'estensione per qualsiasi utente di Windows Admin Center. Poiché il Windows Admin Center SDK è ancora in fase di anteprima, vorremmo collaborare con te per contribuire alla risoluzione dei problemi di sviluppo e garantire che tu sia in grado di offrire un prodotto di qualità e un'esperienza agli utenti.

Prima di rilasciare la versione iniziale dell'estensione, è consigliabile inviare una richiesta di revisione dell'estensione a Microsoft almeno 2-3 settimane prima del rilascio per assicurarsi che il tempo necessario per la revisione sia sufficiente per apportare modifiche all'estensione, se necessario. Quando l'estensione è pronta per la pubblicazione, sarà necessario inviarla a Microsoft per la revisione e, se approvata, verrà pubblicata nel feed.

Successivamente, se si desidera rilasciare un aggiornamento per l'estensione, sarà necessario inviare un'altra richiesta di revisione. A seconda dell'ambito di modifica, il tempo di inversione per le verifiche degli aggiornamenti dovrebbe essere in genere più breve.

### <a name="submit-an-extension-review-request-to-microsoft"></a>Inviare una richiesta di revisione dell'estensione a Microsoft

Per inviare una richiesta di revisione dell'estensione, immettere le informazioni seguenti e inviare un messaggio [wacextensionrequest@microsoft.com](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Review%20Request)di posta elettronica a. Ti invieremo una risposta alla tua posta elettronica entro una settimana.

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

Assicurarsi di seguire le istruzioni riportate in precedenza per la [creazione di un pacchetto di estensione](#creating-an-extension-package) e il file con estensione NuSpec viene definito correttamente e i file vengono firmati. Si consiglia inoltre di disporre di un sito Web di progetto che includa quanto segue:

- Descrizione dettagliata dell'estensione, incluse schermate o video
- Indirizzo di posta elettronica o funzionalità del sito Web per ricevere commenti e suggerimenti

Quando si è pronti per pubblicare l'estensione, inviare un messaggio di [wacextensionrequest@microsoft.com](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Package%20Review) posta elettronica a e verranno fornite istruzioni su come inviare il pacchetto di estensione. Una volta ricevuto il pacchetto, si verificherà e, se approvato, la pubblicazione nel feed dell'interfaccia di amministrazione di Windows.