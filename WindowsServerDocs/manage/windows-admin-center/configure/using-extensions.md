---
title: Installare e gestire le estensioni
description: Installare e gestire le estensioni di Windows Admin Center (progetto Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 6091edd7aa7f790f6029ca6b6ae402bf1b7e61ca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877022"
---
# <a name="install-and-manage-extensions"></a>Installare e gestire le estensioni

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

Windows Admin Center viene compilato come una piattaforma estensibile in cui ogni tipo di connessione e lo strumento è un'estensione che è possibile installare, disinstallare e aggiornare singolarmente. È possibile eseguire la ricerca di nuove estensioni pubblicate da Microsoft e altri sviluppatori e installare e aggiornare singolarmente senza dover aggiornare l'intera installazione di Windows Admin Center. È anche possibile configurare un feed NuGet separato o condivisione file e distribuire estensioni per uso interno all'interno dell'organizzazione.

## <a name="installing-an-extension"></a>Installazione di un'estensione

Windows Admin Center visualizzerà le estensioni disponibili da feed NuGet specificato. Per impostazione predefinita, Windows Admin Center punta al feed di NuGet ufficiale di Microsoft che ospita le estensioni pubblicate da Microsoft e altri sviluppatori.

1. Scegliere il **impostazioni** pulsante in alto a destra > nel riquadro sinistro, fare clic su **estensioni**. 
2. Il **estensioni disponibili** scheda elencherà le estensioni nel feed che è disponibile per l'installazione.
3. Fare clic su un'estensione per visualizzare la descrizione dell'estensione, versione, editore e altre informazioni nel **dettagli** riquadro.
4. Fare clic su **installare** installare un'estensione. Se il gateway deve essere eseguito con privilegi elevati per apportare questa modifica, verrà visualizzato con una richiesta di elevazione dei privilegi di controllo dell'account utente. Al termine dell'installazione, verrà aggiornato automaticamente il browser e verrà ricaricato Windows Admin Center con la nuova estensione installata. Se l'estensione si sta tentando di installare è un aggiornamento di un'estensione installata in precedenza, è possibile scegliere il **aggiornare alla versione più recente** pulsante per installare l'aggiornamento. È anche possibile passare al **estensioni installate** scheda per visualizzare installato le estensioni e vedere se è disponibile in un aggiornamento di **stato** colonna.

## <a name="installing-extensions-from-a-different-feed"></a>Installazione di estensioni di un feed diverso

Windows Admin Center supporta più feed di ed è possibile visualizzare e gestire i pacchetti da più di un feed in un momento. Qualsiasi NuGet feed che supporta le API V2 di NuGet o una condivisione file può essere aggiunto a Windows Admin Center per l'installazione di estensioni da.

1. Scegliere il **impostazioni** pulsante in alto a destra > nel riquadro sinistro, fare clic su **estensioni**.
2. Nel riquadro destro, scegliere il **feed** scheda.
3. Scegliere il **Add** pulsante per aggiungere un altro feed. Per un feed NuGet, immettere l'URL del feed di V2 di NuGet. Provider di feed di NuGet o l'amministratore deve essere in grado di fornire le informazioni sull'URL. Per una condivisione file, immettere il percorso completo del file di condivisione in cui sono archiviati i file di pacchetto di estensione (con estensione nupkg).
4. Fai clic su **Aggiungi**. Se il gateway deve essere eseguito con privilegi elevati per apportare questa modifica, verrà visualizzato con una richiesta di elevazione dei privilegi di controllo dell'account utente.

Il **estensioni disponibili** elenco verranno visualizzati le estensioni da tutti i feed registrati. È possibile controllare il feed ogni estensione è da tramite il **Feed di pacchetti** colonna.

## <a name="uninstalling-an-extension"></a>Disinstallazione di un'estensione

È possibile disinstallare le estensioni che è già stata installata, o persino disinstallare gli strumenti che sono stati preinstallati come parte dell'installazione di Windows Admin Center.

1. Scegliere il **impostazioni** pulsante in alto a destra > nel riquadro sinistro, fare clic su **estensioni**. 
2. Scegliere il **estensioni installate** scheda per visualizzare tutte le estensioni installate.
3. Scegliere un'estensione da disinstallare, quindi fare clic su **Disinstalla**.

Dopo la disinstallazione è completata, verrà aggiornato automaticamente il browser e verrà ricaricato Windows Admin Center con l'estensione rimosso. Se è stato disinstallato uno strumento che è stato pre-installato come parte del Windows Admin Center, lo strumento saranno disponibile per la reinstallazione dopo il **estensioni disponibili** scheda.

## <a name="installing-extensions-on-a-computer-without-internet-connectivity"></a>Installazione delle estensioni in un computer senza connettività internet

Se Windows Admin Center viene installato in un computer che non è connesso a internet o si trova dietro un proxy, potrebbe non essere in grado di accedere e installare le estensioni dal Windows Admin Center feed. È possibile scaricare i pacchetti di estensione manualmente o tramite uno script di PowerShell e configurare Windows Admin Center per recuperare i pacchetti da una condivisione file o un'unità locale.

### <a name="manually-downloading-extension-packages"></a>Scaricare manualmente i pacchetti di estensione

1. Un altro computer con connettività internet, aprire un web browser e passare all'URL seguente: [https://msft-sme.myget.org/gallery/windows-admin-center-feed](https://msft-sme.myget.org/gallery/windows-admin-center-feed) 

  * Potrebbe essere necessario creare un account sul msft-sme.myget.org ed eseguire l'accesso per visualizzare i pacchetti di estensione.

2. Fare clic sul nome del pacchetto da installare per visualizzare la pagina dei dettagli del pacchetto.
3. Fare clic sui **scaricare** collegamento nel riquadro di destra della pagina dei dettagli del pacchetto e scaricare il file di pacchetto. nupkg per l'estensione.
4. Ripetere i passaggi 2 e 3 per tutti i pacchetti di cui che si desidera scaricare.
5. Copiare i file del pacchetto in una condivisione file accessibile dal computer in che cui è installato Windows Admin Center o nel disco locale del computer.
6. [Seguire le istruzioni per installare le estensioni da un feed diverso](#installing-extensions-from-a-different-feed).

### <a name="downloading-packages-with-a-powershell-script"></a>Download di pacchetti con uno script di PowerShell

Sono disponibili molti script in Internet per scaricare i pacchetti NuGet da un feed NuGet. Si userà il [script forniti di Jon Galloway](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), Senior Program Manager presso Microsoft.

1. Come descritto nel [post di blog](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), installare lo script come pacchetto NuGet, o copiare e incollare lo script in PowerShell ISE.
2. Modifica che la prima riga dello script da NuGet feed URL di v2 del. Se si scaricano i pacchetti dal Windows Admin Center ufficiale del feed, usare l'URL seguente.

```powershell
$feedUrlBase = "https://aka.ms/sme-extension-feed"
```

3. Eseguire lo script e scaricherà tutti i pacchetti NuGet dal feed nella seguente cartella locale: %USERPROFILE%\Documents\NuGetLocal
4. [Seguire le istruzioni per installare le estensioni da un feed diverso](#installing-extensions-from-a-different-feed).

## <a name="manage-extensions-with-powershell"></a>Gestire le estensioni con PowerShell

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

Windows Admin Center Preview include un modulo di PowerShell per gestire le estensioni di gateway.

>[!IMPORTANT]
>Gestione delle estensioni di gateway con il modulo PowerShell è supportata solo quando Windows Admin Center viene distribuito come servizio gateway in Windows Server.

```powershell
# Add the module to the current session
Import-Module "$env:ProgramFiles\windows admin center\PowerShell\Modules\ExtensionTools"
# Available cmdlets: Get-Feed, Add-Feed, Remove-Feed, Get-Extension, Install-Extension, Uninstall-Extension, Update-Extension

# List feeds
Get-Feed "https://wac.contoso.com"

# Add a new extension feed
Add-Feed -GatewayEndpoint "https://wac.contoso.com" -Feed "\\WAC\our-private-extensions"

# Remove an extension feed
Remove-Feed -GatewayEndpoint "https://wac.contoso.com" -Feed "\\WAC\our-private-extensions"

# List all extensions
Get-Extension "https://wac.contoso.com"

# Install an extension (locate the latest version from all feeds and install it)
Install-Extension -GatewayEndpoint "https://wac.contoso.com" "msft.sme.containers"

# Install an extension (latest version from a specific feed, if the feed is not present, it will be added)
Install-Extension -GatewayEndpoint "https://wac.contoso.com" "msft.sme.containers" -Feed "https://aka.ms/sme-extension-feed"

# Install an extension (install a specific version)
Install-Extension "https://wac.contoso.com" "msft.sme.certificate-manager" "0.133.0"

# Uninstall-Extension
Uninstall-Extension "https://wac.contoso.com" "msft.sme.containers"

# Update-Extension
Update-Extension "https://wac.contoso.com" "msft.sme.containers"
```

### <a name="learn-more-about-building-an-extension-with-the-windows-admin-center-sdkextendextensibility-overviewmd"></a>[Altre informazioni sulla creazione di un'estensione con il SDK di Windows Admin Center](../extend/extensibility-overview.md).