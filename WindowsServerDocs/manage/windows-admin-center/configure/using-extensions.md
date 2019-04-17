---
title: Installare e gestire le estensioni
description: Installare e gestire le estensioni di Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: c775dd5a3011115bbb031c0b9e4e24a8911d378e
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296703"
---
# Installare e gestire le estensioni

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Windows Admin Center è creato come una piattaforma estensibile in cui ogni tipo di connessione e lo strumento è un'estensione che è possibile installare, disinstallare e aggiornare singolarmente. È possibile cercare le nuove estensioni pubblicate da Microsoft e di altri sviluppatori e installare e aggiornare singolarmente senza la necessità di aggiornare l'intera installazione di Windows Admin Center. È anche possibile configurare un feed NuGet separato o condivisione file e distribuire le estensioni di usare internamente all'interno dell'organizzazione.

## L'installazione di un'estensione

Windows Admin Center ti mostrerà le estensioni disponibili da di NuGet specificato feed. Per impostazione predefinita, Windows Admin Center punta al NuGet ufficiali Microsoft feed che ospita estensioni pubblicate da Microsoft e di altri sviluppatori.

1. Fare clic sul pulsante **Impostazioni** di gt _ superiore destro nel riquadro sinistro, fai clic su **estensioni**. 
2. Scheda **Estensioni disponibili** verrà elencate le estensioni del feed disponibili per l'installazione.
3. Fai clic su un'estensione per visualizzare la descrizione dell'estensione, versione, autore e altre informazioni nel riquadro dei **Dettagli** .
4. Fai clic su **Installa** per installare un'estensione. Se il gateway deve essere eseguite in modalità con privilegi elevati per rendere questa modifica, verrà visualizzata con una richiesta di elevazione dei privilegi di controllo dell'account utente. Dopo l'installazione è stata completata, verrà aggiornato automaticamente il browser e verrà ricaricato Windows Admin Center con la nuova estensione installata. Se l'estensione che si sta tentando di installare un aggiornamento di un'estensione precedentemente installata, puoi fare clic sul pulsante di **aggiornamento alla versione più recente** per installare l'aggiornamento. Anche passare alla scheda **Estensioni installate** per visualizzare le estensioni installate e verificare se un aggiornamento è disponibile nella colonna **stato** .

## Installazione delle estensioni da un feed diverso

Windows Admin Center supporta più feed e puoi visualizzare e gestire i pacchetti da uno o più feed alla volta. Qualsiasi NuGet feed che supporta le API V2 NuGet o una condivisione file possa essere aggiunti a Windows Admin Center per l'installazione di estensioni da.

1. Fare clic sul pulsante **Impostazioni** di gt _ superiore destro nel riquadro sinistro, fai clic su **estensioni**.
2. Nel riquadro di destra, fare clic sulla scheda **feed** .
3. Fai clic sul pulsante **Aggiungi** per aggiungere un altro feed. Per un feed NuGet, Immetti il V2 NuGet l'URL del feed. Provider di feed di NuGet o amministratore deve essere in grado di fornire le informazioni sull'URL. Per una condivisione di file, Immetti il percorso completo del file di condividere in cui sono archiviati i file di pacchetto di estensione (.nupkg).
4. Fai clic su **Aggiungi**. Se il gateway deve essere eseguite in modalità con privilegi elevati per rendere questa modifica, verrà visualizzata con una richiesta di elevazione dei privilegi di controllo dell'account utente.

L'elenco di **Estensioni disponibili** mostrerà le estensioni da tutti i feed registrati. Puoi controllare quali feed ogni estensione è da utilizzando la colonna **Pacchetto Feed** .

## Disinstallazione di un'estensione

Puoi disinstallare tutte le estensioni che hanno installato in precedenza o anche disinstallare gli strumenti che sono stati pre-installati come parte dell'installazione di Windows Admin Center.

1. Fare clic sul pulsante **Impostazioni** di gt _ superiore destro nel riquadro sinistro, fai clic su **estensioni**. 
2. Fare clic sulla scheda **Estensioni installate** per visualizzare le estensioni installate tutti.
3. Scegli un'estensione per disinstallare e quindi fai clic su **disinstallazione**.

Dopo la disinstallazione è completato, verrà aggiornato automaticamente il browser e verrà ricaricato Windows Admin Center con l'estensione rimosso. Se è stato disinstallato uno strumento che è stato pre-installato come parte di Windows Admin Center, lo strumento sarà disponibile per la reinstallazione nella scheda **Estensioni disponibili** .

## Installazione delle estensioni in un computer senza connettività internet

Se è installato Windows Admin Center in un computer che non è connesso a internet o si trova dietro un proxy, potrebbe non essere in grado di accedere e installare le estensioni di Windows Admin Center feed. Puoi scaricare pacchetti di estensioni manualmente o con uno script di PowerShell e configurare Windows Admin Center per recuperare i pacchetti da una condivisione file o un'unità locale.

### Download manuale di pacchetti di estensione

1. In un altro computer che dispone di connettività internet, Apri un web browser e passa all'URL seguente:[https://msft-sme.myget.org/gallery/windows-admin-center-feed](https://msft-sme.myget.org/gallery/windows-admin-center-feed) 

  * Potrebbe essere necessario creare un account nel msft sme.myget.org ed eseguire l'accesso per visualizzare i pacchetti di estensione.

2. Fai clic sul nome del pacchetto che vuoi installare per visualizzare la pagina dei dettagli del pacchetto.
3. Fai clic sul collegamento **Download** nel riquadro di destra della pagina dei dettagli del pacchetto e scaricare il file .nupkg per l'estensione.
4. Ripeti i passaggi 2 e 3 per tutti i pacchetti da scaricare.
5. Copia i file del pacchetto in una condivisione file accessibile dal computer in cui è installato Windows Admin Center, o da disco locale del computer.
6. [Segui le istruzioni per installare le estensioni da un feed diverso](#installing-extensions-from-a-different-feed).

### Download di pacchetti con uno script di PowerShell

Sono disponibili molti script in Internet per il download dei pacchetti NuGet da un feed di NuGet. Usiamo il [script forniti dal DOM Galloway](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), Microsoft Senior Program Manager.

1. Come descritto nel [post di blog](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), installare lo script come un pacchetto NuGet o copia e Incolla lo script in PowerShell ISE.
2. Modifica che la prima riga dello script alla tua NuGet feed's v2 URL. Se scarichi i pacchetti da Windows Admin Center ufficiale feed, Usa l'URL di seguito.

```powershell
$feedUrlBase = "https://aka.ms/sme-extension-feed"
```

3. Esegui lo script e verrà scaricati tutti i pacchetti NuGet dal feed alla cartella locale seguente: %USERPROFILE%\Documents\NuGetLocal
4. [Segui le istruzioni per installare le estensioni da un feed diverso](#installing-extensions-from-a-different-feed).

## Gestire le estensioni con PowerShell

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Windows Admin Center Preview include un modulo di PowerShell per gestire le estensioni di gateway.

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

### [Ulteriori informazioni sulla creazione di un'estensione con Windows Admin Center SDK](../extend/extensibility-overview.md).