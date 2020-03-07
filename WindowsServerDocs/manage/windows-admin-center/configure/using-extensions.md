---
title: Installare e gestire le estensioni
description: Installa e gestisci le estensioni in Windows Admin Center (progetto Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 7c1a70e36dfac9b23ded8f920ffcc8cccbfff023
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371119"
---
# <a name="install-and-manage-extensions"></a>Installare e gestire le estensioni

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Windows Admin Center è costituita da una piattaforma estendibile in cui ogni strumento o tipo di connessione è un'estensione che puoi installare, disinstallare e aggiornare singolarmente. Puoi cercare le nuove estensioni pubblicate da Microsoft e altri sviluppatori, nonché installarle e aggiornarle singolarmente senza dover aggiornare l'intera installazione di Windows Admin Center. Puoi anche configurare una condivisione file o un feed NuGet separato e distribuire le estensioni da usare all'interno della tua organizzazione.

## <a name="installing-an-extension"></a>Installazione di un'estensione

Windows Admin Center visualizza le estensioni disponibili nel feed NuGet specificato. Per impostazione predefinita, Windows Admin Center punta al feed NuGet ufficiale Microsoft che ospita le estensioni pubblicate da Microsoft e altri sviluppatori.

1. Fai clic sul pulsante **Impostazioni** in alto a destra e nel riquadro a sinistra fai clic su **Estensioni**. 
2. Nella scheda **Estensioni disponibili** verranno elencate le estensioni presenti nel feed disponibili per l'installazione.
3. Fai clic su un'estensione per visualizzarne la descrizione, la versione, l'autore e altri dati relativi a tale estensione nel riquadro **Dettagli**.
4. Fai clic su **Installa** per installare l'estensione. Se per apportare questa modifica il gateway deve essere eseguito con privilegi elevati, verrà visualizzata una richiesta di elevazione dei privilegi di Controllo account utente. Al termine dell'installazione, il browser verrà aggiornato automaticamente e Windows Admin Center verrà ricaricato con la nuova estensione installata. Se l'estensione che stai provando a installare è un aggiornamento di un'estensione installata in precedenza, puoi fare clic sul pulsante **Aggiorna alla versione più recente** per installare l'aggiornamento. Puoi anche passare alla scheda **Estensioni installate** per visualizzare le estensioni installate e verificare se è disponibile un aggiornamento nella colonna **Stato**.

## <a name="installing-extensions-from-a-different-feed"></a>Installazione di estensioni da un feed diverso

Windows Admin Center supporta più feed e puoi visualizzare e gestire i pacchetti da più feed contemporaneamente. Ogni feed NuGet che supporta una condivisione file o le API NuGet V2 può essere aggiunto a Windows Admin Center per l'installazione delle estensioni.

1. Fai clic sul pulsante **Impostazioni** in alto a destra e nel riquadro a sinistra fai clic su **Estensioni**.
2. Nel riquadro a destra fai clic sulla scheda **Feed**.
3. Fai clic sul pulsante **Aggiungi** per aggiungere un altro feed. Per un feed NuGet, immetti l'URL del feed NuGet V2. Il provider di feed NuGet o l'amministratore deve essere in grado di fornire le informazioni sull'URL. Per una condivisione file, immetti il percorso completo della condivisione in cui sono archiviati i file di pacchetto dell'estensione (NUPKG).
4. Fare clic su **Add**. Se per apportare questa modifica il gateway deve essere eseguito con privilegi elevati, verrà visualizzata una richiesta di elevazione dei privilegi di Controllo account utente.

L'elenco **Estensioni disponibili** visualizzerà le estensioni presenti in tutti i feed registrati. Puoi verificare il feed da cui proviene ogni estensione usando la colonna **Feed del pacchetto**.

## <a name="uninstalling-an-extension"></a>Disinstallazione di un'estensione

Puoi disinstallare tutte le estensioni installate in precedenza o addirittura tutti gli strumenti preinstallati durante l'installazione di Windows Admin Center.

1. Fai clic sul pulsante **Impostazioni** in alto a destra e nel riquadro a sinistra fai clic su **Estensioni**. 
2. Fai clic sulla scheda **Estensioni installate** per visualizzare tutte le estensioni installate.
3. Scegli un'estensione da disinstallare e quindi fai clic su **Disinstalla**.

Al termine della disinstallazione, il browser verrà aggiornato automaticamente e Windows Admin Center verrà ricaricato con l'estensione rimossa. Se hai disinstallato uno strumento preinstallato durante l'installazione di Windows Admin Center, lo strumento sarà disponibile per la reinstallazione nella scheda **Estensioni disponibili**.

## <a name="installing-extensions-on-a-computer-without-internet-connectivity"></a>Installazione di estensioni in un computer senza connettività Internet

Se Windows Admin Center è installato in un computer non connesso a Internet o dietro un proxy, tale computer potrebbe non essere in grado di eseguire l'accesso e installare le estensioni del feed di Windows Admin Center. Puoi scaricare i pacchetti delle estensioni manualmente o con uno script PowerShell e configurare Windows Admin Center per il recupero dei pacchetti da una condivisione file o un'unità locale.

### <a name="manually-downloading-extension-packages"></a>Download manuale dei pacchetti delle estensioni

1. In un altro computer con connettività Internet apri un Web browser e passa all'URL seguente: [https://msft-sme.myget.org/gallery/windows-admin-center-feed](https://msft-sme.myget.org/gallery/windows-admin-center-feed) 

   * Potresti aver bisogno di creare un account in msft-sme.myget.org ed eseguire l'accesso per visualizzare i pacchetti delle estensioni.

2. Fai clic sul nome del pacchetto che vuoi installare per visualizzare la pagina dei dettagli di pacchetto.
3. Fai clic sul collegamento **Download** nel riquadro a destra dei dettagli di pacchetto e scarica il file NUPKG dell'estensione.
4. Ripeti i passaggi 2 e 3 per tutti i pacchetti da scaricare.
5. Copia i file del pacchetto in una condivisione file a cui è possibile accedere dal computer in cui è installato Windows Admin Center oppure nel disco locale del computer.
6. [Segui le istruzioni per installare le estensioni da un feed diverso](#installing-extensions-from-a-different-feed).

### <a name="downloading-packages-with-a-powershell-script"></a>Download di pacchetti con uno script PowerShell

In Internet sono disponibili molti script per il download dei pacchetti NuGet da un feed NuGet. Useremo lo [script fornito da Jon Galloway](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), Senior Program Manager Microsoft.

1. Come illustrato nel [post di blog](https://weblogs.asp.net/jongalloway/downloading-a-local-nuget-repository-with-powershell), installa lo script come un pacchetto NuGet oppure copia e incolla lo script in PowerShell ISE.
2. Modifica la prima riga dello script nell'URL del feed NuGet v2. Se scarichi i pacchetti dal feed ufficiale di Windows Admin Center, usa l'URL seguente.

```powershell
$feedUrlBase = "https://aka.ms/sme-extension-feed"
```

3. Esegui lo script e tutti i pacchetti NuGet del feed verranno scaricati nella cartella locale seguente: %USERPROFILE%\Documents\NuGetLocal
4. [Segui le istruzioni per installare le estensioni da un feed diverso](#installing-extensions-from-a-different-feed).

## <a name="manage-extensions-with-powershell"></a>Gestire le estensioni con PowerShell

Windows Admin Center Preview include un modulo PowerShell per gestire le estensioni del gateway.

[!INCLUDE [ps-extensions](../includes/ps-extensions.md)]

### <a name="learn-more-about-building-an-extension-with-the-windows-admin-center-sdk"></a>[Scopri come creare un'estensione con Windows Admin Center SDK](../extend/extensibility-overview.md).