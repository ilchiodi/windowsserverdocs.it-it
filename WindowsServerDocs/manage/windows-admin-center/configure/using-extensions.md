---
title: Installare e gestire le estensioni
description: Installare e gestire le estensioni nell'interfaccia di amministrazione di Windows (progetto Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: d49e25591c705afa217b2332ee48eb42c5c2f7ab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357236"
---
# <a name="install-and-manage-extensions"></a>Installare e gestire le estensioni

>Si applica a: Windows Admin Center, Windows Admin Center Preview

L'interfaccia di amministrazione di Windows è costituita da una piattaforma estendibile in cui ogni tipo di connessione e strumento è un'estensione che è possibile installare, disinstallare e aggiornare singolarmente. È possibile cercare le nuove estensioni pubblicate da Microsoft e da altri sviluppatori e installarle e aggiornarle singolarmente senza dover aggiornare l'intera installazione di Windows Admin Center. È anche possibile configurare un feed NuGet separato o una condivisione file e distribuire le estensioni da usare internamente all'interno dell'organizzazione.

## <a name="installing-an-extension"></a>Installazione di un'estensione

L'interfaccia di amministrazione di Windows mostrerà le estensioni disponibili dal feed NuGet specificato. Per impostazione predefinita, il centro di amministrazione di Windows punta al feed NuGet ufficiale Microsoft che ospita le estensioni pubblicate da Microsoft e da altri sviluppatori.

1. Fare clic sul pulsante **Settings (impostazioni** ) in alto a > destra nel riquadro a sinistra e fare clic su **Extensions (estensioni**). 
2. Nella scheda **estensioni disponibili** sono elencate le estensioni sul feed disponibili per l'installazione.
3. Fare clic su un'estensione per visualizzare la descrizione dell'estensione, la versione, il server di pubblicazione e altre informazioni nel riquadro dei **Dettagli** .
4. Fare clic su **Installa** per installare un'estensione. Se il gateway deve essere eseguito in modalità con privilegi elevati per apportare questa modifica, verrà visualizzata una richiesta di elevazione del controllo dell'account utente. Al termine dell'installazione, il browser verrà aggiornato automaticamente e il centro di amministrazione di Windows verrà ricaricato con la nuova estensione installata. Se l'estensione che si sta provando a installare è un aggiornamento di un'estensione installata in precedenza, è possibile fare clic sul pulsante **Aggiorna a più recente** per installare l'aggiornamento. È anche possibile passare alla scheda **estensioni installate** per visualizzare le estensioni installate e verificare se nella colonna **stato** è disponibile un aggiornamento.

## <a name="installing-extensions-from-a-different-feed"></a>Installazione di estensioni da un feed diverso

L'interfaccia di amministrazione di Windows supporta più feed ed è possibile visualizzare e gestire i pacchetti da più di un feed alla volta. Tutti i feed NuGet che supportano le API NuGet v2 o una condivisione file possono essere aggiunti all'interfaccia di amministrazione di Windows per l'installazione delle estensioni da.

1. Fare clic sul pulsante **Settings (impostazioni** ) in alto a > destra nel riquadro a sinistra e fare clic su **Extensions (estensioni**).
2. Nel riquadro destro fare clic sulla scheda **feed** .
3. Fare clic sul pulsante **Aggiungi** per aggiungere un altro feed. Per un feed NuGet, immettere l'URL del feed NuGet V2. Il provider di feed NuGet o l'amministratore deve essere in grado di fornire le informazioni sull'URL. Per una condivisione file, immettere il percorso completo della condivisione file in cui sono archiviati i file del pacchetto di estensione (nupkg).
4. Fai clic su **Aggiungi**. Se il gateway deve essere eseguito in modalità con privilegi elevati per apportare questa modifica, verrà visualizzata una richiesta di elevazione del controllo dell'account utente.

Nell'elenco **estensioni disponibili** vengono visualizzate le estensioni di tutti i feed registrati. È possibile verificare quale feed di ogni estensione viene utilizzato usando la colonna del **feed del pacchetto** .

## <a name="uninstalling-an-extension"></a>Disinstallazione di un'estensione

È possibile disinstallare le estensioni installate in precedenza o disinstallare tutti gli strumenti preinstallati nell'ambito dell'installazione di Windows Admin Center.

1. Fare clic sul pulsante **Settings (impostazioni** ) in alto a > destra nel riquadro a sinistra e fare clic su **Extensions (estensioni**). 
2. Fare clic sulla scheda **estensioni installate** per visualizzare tutte le estensioni installate.
3. Scegliere un'estensione da disinstallare, quindi fare clic su **Disinstalla**.

Al termine della disinstallazione, il browser verrà aggiornato automaticamente e il centro di amministrazione di Windows verrà ricaricato con l'estensione rimossa. Se è stato disinstallato uno strumento pre-installato come parte dell'interfaccia di amministrazione di Windows, lo strumento sarà disponibile per la reinstallazione nella scheda **estensioni disponibili** .

## <a name="installing-extensions-on-a-computer-without-internet-connectivity"></a>Installazione di estensioni in un computer senza connettività Internet

Se l'interfaccia di amministrazione di Windows è installata in un computer che non è connesso a Internet o si trova dietro un proxy, potrebbe non essere in grado di accedere e installare le estensioni dal feed dell'interfaccia di amministrazione di Windows. È possibile scaricare i pacchetti di estensione manualmente o con uno script di PowerShell e configurare l'interfaccia di amministrazione di Windows per recuperare i pacchetti da una condivisione file o da un'unità locale.

### <a name="manually-downloading-extension-packages"></a>Download manuale dei pacchetti di estensione

1. In un altro computer con connettività Internet, aprire un Web browser e passare all'URL seguente: [https://msft-sme.myget.org/gallery/windows-admin-center-feed](https://msft-sme.myget.org/gallery/windows-admin-center-feed) 

   * Potrebbe essere necessario creare un account in msft-sme.myget.org e accedere per visualizzare i pacchetti di estensione.

2. Fare clic sul nome del pacchetto che si desidera installare per visualizzare la pagina dei dettagli del pacchetto.
3. Fare clic sul collegamento **download** nel riquadro destro della pagina dei dettagli del pacchetto e scaricare il file con estensione nupkg per l'estensione.
4. Ripetere i passaggi 2 e 3 per tutti i pacchetti che si desidera scaricare.
5. Copiare i file del pacchetto in una condivisione file a cui è possibile accedere dal computer in cui è installato l'interfaccia di amministrazione di Windows o nel disco locale del computer.
6. [Seguire le istruzioni per installare le estensioni da un feed diverso](#installing-extensions-from-a-different-feed).

### <a name="downloading-packages-with-a-powershell-script"></a>Download di pacchetti con uno script di PowerShell

Sono disponibili molti script in Internet per il download di pacchetti NuGet da un feed NuGet. Si userà lo [script fornito da Jon Galloway](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), Senior Program Manager di Microsoft.

1. Come descritto nel [post di Blog](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), installare lo script come pacchetto NuGet oppure copiare e incollare lo script in PowerShell ISE.
2. Modificare la prima riga dello script nell'URL V2 del feed NuGet. Se si scaricano pacchetti dal feed ufficiale di Windows Admin Center, usare l'URL seguente.

```powershell
$feedUrlBase = "https://aka.ms/sme-extension-feed"
```

3. Eseguire lo script per scaricare tutti i pacchetti NuGet dal feed alla seguente cartella locale:%USERPROFILE%\Documents\NuGetLocal
4. [Seguire le istruzioni per installare le estensioni da un feed diverso](#installing-extensions-from-a-different-feed).

## <a name="manage-extensions-with-powershell"></a>Gestire le estensioni con PowerShell

>Si applica a: Windows Admin Center, Windows Admin Center Preview

L'anteprima dell'interfaccia di amministrazione di Windows include un modulo PowerShell per gestire le estensioni del gateway.

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

### <a name="learn-more-about-building-an-extension-with-the-windows-admin-center-sdkextendextensibility-overviewmd"></a>[Altre informazioni sulla creazione di un'estensione con Windows Admin Center SDK](../extend/extensibility-overview.md).