---
title: Desktop remoto - confrontare le app client
description: Scopri come diverse App desktop remoto confrontare quando si tratta di funzioni e funzionalità supportate.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 12efe858-6b76-4e08-9f72-b9603aceb0fc
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/22/2018
ms.localizationpriority: medium
ms.openlocfilehash: fc20d1a2c51abddb8ae014efc621f4f0b36c3677
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297410"
---
# Confrontare le app client

>Si applica a: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Ci stiamo spesso richiesto come confrontare con diverse App client Desktop remoto. È tutti la stessa operazione? Ecco le risposte a queste domande.

## Supporto di reindirizzamento

Le tabelle seguenti confrontare il supporto per dispositivi e altri reindirizzamenti su app connessione Desktop remoto, un'app universale, app Android, iOS app, client web e app macOS. Queste tabelle descrivono i reindirizzamenti di cui è possibile accedere una sola volta in una sessione remota. 

Se si remoto in desktop personali, ci sono reindirizzamenti aggiuntivi che è possibile configurare le **Impostazioni aggiuntive** per la sessione. Se il desktop remoto o l'App viene gestita dall'organizzazione, l'amministratore può abilitare o disabilitare reindirizzamenti tramite le impostazioni di criteri di gruppo.

### Reindirizzamento dell'input

| Reindirizzamento | Desktop remoto<br> Connessione | Universale | Android | iOS | macOS | client Web |
|-------------|-------------------------------|-----------|---------|-----|-------|------------|
| Tastiera    | X                             | X         | X       | X   | X     | X          |
| Mouse       | X                             | X         | X       | X *    | X     | X          |
| Touch screen       | X                             | X         | X       | X   |       |            |
| Other       | Penna                           |           |         |     |       |            |
* Visualizza l' [elenco dei dispositivi di input supportati per il client di Desktop remoto iOS Beta](remote-desktop-ios.md#supported-input-devices).

### Reindirizzamento delle porte   

| Reindirizzamento | Desktop remoto <br>Connessione | Universale | Android | iOS | macOS | Client Web |
|-------------|-------------------------------|-----------|---------|-----|-------|------------|
| Porta seriale | X                             |           |         |     |       |            |
| USB         | X                             |           |         |     |       |            |

Quando abiliti il reindirizzamento delle porte USB, le periferiche USB collegate alla porta USB vengono riconosciute automaticamente nella sessione remota.

### Altri reindirizzamento (i dispositivi, e così via)



| Reindirizzamento         | Connessione Desktop remoto | Universale   | Android | iOS         | macOS                                    | Client Web    |
|---------------------|---------------------------|-------------|---------|-------------|------------------------------------------|---------------|
| Fotocamere             | X                         |             |         |             |                                          |               |
| Appunti           | X                         | testo, immagini | testo    | testo, immagini | X                                        | testo          |
| Unità/archiviazione locale | X                         |             | X       |             | x                                        |               |
| Posizione            | X                         |             |         |             |                                          |               |
| Microfoni         | X                         |X            |         |             | X                                        |               |
| Printers            | X                         |             |         |             | X (solo TAZZE)                            | Stampa PDF     |
| Scanner            | X                         |             |         |             |                                          |               |
| Smart card         | X                         |             |         |             | X (autenticazione di Windows non è supportata) |               |
| Speakers            | X                         | X           | X       | X           | X                                        | X (tranne Internet Explorer) |

* Per il reindirizzamento della stampante - app macOS supporta il driver della stampante Publisher Imagesetter per impostazione predefinita. Non supportano il reindirizzamento driver della stampante nativo.

### Impostazioni RDP supportati
Questa tabella include l'elenco delle impostazioni di file supportate RDP che può essere utilizzato con i client Windows e il codice HTML. Una "x" nella colonna piattaforma indica che l'impostazione è supportata. Tieni presente che questo non è un elenco completo delle impostazioni supportate per i client Windows e HTML5. Continueremo a questa tabella per includere più supportate le impostazioni RDP per i client Windows e HTML5, nonché il macOS, iOS e Android client di aggiornamento.

| Impostazione RDP                        | Descrizione            | Valori                 | Valore predefinito          | Windows Desktop virtuale | Windows | HTML5   |
|------------------------------------|------------------------|------------------------|:----------------------:|:-----------------------:|:-------:|:-------:|
| l'indirizzo completo alternativo: s:value | Specifica un nome alternativo o un indirizzo IP del computer remoto. | Qualsiasi nome valido o l'indirizzo IP del computer remoto, ad esempio, "10.10.15.15" | | x | x | x |
| shell: s:value alternativo        | Determina se un programma viene avviato automaticamente quando ti Connetti a RDP. Questa impostazione corrisponde alla programma percorso e nome casella nella scheda programmi del client connessione Desktop remoto. Per specificare una shell alternativa, Immetti un percorso valido per un file eseguibile per il valore, ad esempio "" C:\ProgramFiles\Office\word.exe"". Questa impostazione determina anche il percorso o l'alias dell'applicazione remota per essere avviato in fase di connessione se RemoteApplicationMode è abilitato. | ad esempio, "C:\ProgramFiles\Office\word.exe" || x | x | x |
| audiocapturemode:i:value | Indica se è abilitato il reindirizzamento di input/output audio. Questa impostazione corrisponde alla casella Audio remota dei client connessione Desktop remoto. | Acquisizione di audio disable (0) dal dispositivo locale; (1) Abilita acquisizione audio dal dispositivo locale e reindirizzamento a un'applicazione audio nella sessione remota | 0 | x | x | |
| audiomode:i:value | Determina quali computer (ad esempio, locale o remoto) riproduce l'audio. | (0) suoni del computer locale (Riproduci nel computer); (1) riprodurre i suoni nel computer remoto (Riproduci nel computer remoto); (2) non vengono riprodotti suoni (non vengono riprodotti) | 0 | x | x | x |
| livello di autenticazione: i:value | Definisce le impostazioni a livello di autenticazione server. | (0) se l'autenticazione server non riesce, connettersi al computer senza avviso (Connect e avvisare); (1) se l'autenticazione server non riesce, non stabilire una connessione (non si connettono); (2) se l'autenticazione server non riesce, Mostra un messaggio di avviso e Consenti la connessione o rifiuta la connessione (avvisa); (3) Nessun requisito di autenticazione è specificato. | 3 | x | x ||
| autoreconnection abilitato: i:value | Questa impostazione determina se il computer client tenterà automaticamente di riconnettersi al computer remoto, se la connessione viene eliminata; ad esempio, quando è presente un'interruzione della connettività di rete. This setting corresponds to the Reconnect if the connection is dropped check box on the Experience tab under Options in Remote Desktop Connection (RDC).| I computer client (0) non tenta di riconnettersi; automaticamente (1) computer client tenta di riconnettersi automaticamente| 1 | x | x | x |
| bandwidthautodetect:i:value | Determina se è abilitato il rilevamento automatico della rete tipo | Rilevamento di tipo rete automatica disable (0); (1) attivare il rilevamento di tipo di rete automatica | 1 | x | x | x |
| compressione: i:value | Questa impostazione determina se è abilitata la compressione in blocco quando vengono trasmesse da RDP al computer locale.|Compressione in blocco disabilitare RDP (0); (1) Abilita RDP in blocco compressione | 1 | x | x | x |
| id: i:value dimensione del desktop | Specifica le dimensioni del desktop sessione remota da un set di opzioni predefinite. Questa impostazione viene ignorata se vengono specificati desktopheight o desktopwidth.| (0) 640 x 480; (1) 800 x 600; (2) 1024 x 768; (3) 1280 x 1024; (4) 1600 x 1200 | 0 | x | x | x |
| desktopheight:i:value | This setting determines the resolution height (in pixels) on the remote computer when you connect by using Remote Desktop Connection (RDC). Questa impostazione corrisponde alla selezione nella configurationslider Shaded su underOptions theDisplaytab in connessione desktop remoto. | Valore numerico compreso tra 200 e 2048 | Il valore predefinito è impostato per la risoluzione del computer locale | x | x | x |
| desktopwidth:i:value | This setting determines the resolution width (in pixels) on the remote computer when you connect by using Remote Desktop Connection (RDC). Questa impostazione corrisponde alla selezione nella configurationslider Shaded su underOptions theDisplaytab in connessione desktop remoto. | Valore numerico compreso tra 200 e 4096 | Il valore predefinito è impostato per la risoluzione del computer locale | x | x | x |
| disableclipboardredirection:i:value | Questa impostazione determina se è abilitato il reindirizzamento degli Appunti quando ci si connette al computer remoto. | È abilitato il reindirizzamento degli Appunti (0); (1) il reindirizzamento degli Appunti non è abilitato | x | x | x |
| disableconnectionsharing:i:value | Determina se il client desktop remoto si riconnetterà a connessioni Apri esistente o avviare una nuova connessione quando viene avviato un desktop o RemoteApp | Riconnessione (0) per qualsiasi sessione esistente. (1) avviare la nuova connessione | 0 | x | x | x |
| disableprinterredirection:i:value | Questa impostazione determina se è abilitato il reindirizzamento della stampante stampa facile quando ci si connette al computer remoto. | È abilitato il reindirizzamento della stampante di stampa semplice (0); (1) il reindirizzamento della stampante di stampa facile è disabilitato | x | x | x |
| dominio: s:value | This setting specifies the name of the domain in which the user account that will be used to log on to the remote computer by using Remote Desktop Connection (RDC) is located. The value of this setting, along with the value of the username setting, will appear in theUser nametext box on theGeneraltab underOptionsin RDC. | Un nome di dominio valido, ad esempio, "CONTOSO" | Nessun valore predefinito | x | x | x |
| drivestoredirect:s:value | Determina quali unità disco locale nel computer client verrà reindirizzato e sarà disponibile nella sessione remota. | Nessun valore specificato - reindirizzare qualsiasi unità. * - Reindirizzare tutte le unità disco, tra cui unità collegate in un secondo momento; DynamicDrives - reindirizzare eventuali unità in cui sono connessi in un secondo momento; L'unità e le etichette per le unità di uno o più - reindirizzare i rigidi specificati.| Nessun valore specificato - il reindirizzamento delle unità | x | x    | |
| enablecredsspsupport:i:value | Questa impostazione determina se RDP utilizzerà il Provider di supporto di protezione di credenziali (CredSSP) per l'autenticazione se disponibile. | RDP (0) non userà CredSSP, anche se il sistema operativo supporta CredSSP; (1) RDP userà CredSSP se il sistema operativo supportano CredSSP | 1 | x | x | |
| indirizzo: s:value completo | Questa impostazione specifica il nome o indirizzo IP del computer remoto che si desidera connettersi | Un nome computer valido, indirizzo IPv4 o IPv6 indirizzo - specifica del computer remoto a cui si desidera connettersi. | | x | x | x |
| GatewayCredentialsSource:i:value | Specifica o recupera il metodo di autenticazione Gateway Desktop remoto. | Chiedi (0) per la password (NTLM); (1) utilizza smart card; (4) consentono all'utente di selezionare in un secondo momento | 0 | x | x | x |
| gatewayhostname:s:value | Specifica il nome host Gateway Desktop remoto. | Indirizzo del server gateway valido. ||x|x|x|
| gatewayprofileusagemethod:i:value | Specifica se utilizzare le impostazioni predefinite Gateway Desktop remoto | (0) usa la modalità di profilo predefinito, come specificato dall'amministratore; (1) usa le impostazioni esplicite, come specificato dall'utente. | 0 | x | x | x |
| gatewayusagemethod:i:value | Specifica quando usare il server Gateway Desktop remoto | (0) non utilizzare un server Gateway Desktop remoto. (1) sempre usare un server Gateway Desktop remoto. (2) usa un server Gateway Desktop remoto se non può essere effettuata una connessione diretta all'Host sessione Desktop remoto. (3) usa le impostazioni predefinite di server Gateway Desktop remoto; (4) non usare un Gateway Desktop remoto, Ignora server per gli indirizzi locali. Impostazione di questo valore di proprietà a 0 o 4 sono equivale in modo efficace, ma impostando questa proprietà su 4 abilitato l'opzione di ignorare gli indirizzi locali. | | x | x | x |
| networkautodetect:i:value | Determina se usare il rilevamento della larghezza di banda di rete automatica o No. È necessario impostare il optionbandwidthautodetectto e correlato withconnection tipo 7. | (0) non utilizzano il rilevamento della larghezza di banda di rete automatica; (1) usare il rilevamento della larghezza di banda di rete automatica | 1 | x ||x|
| PromptCredentialOnce:i:value | Determina se le credenziali dell'utente vengono salvate e utilizzate per il Gateway Desktop remoto e il computer remoto.|Sessione remota (0) non utilizzerà le stesse credenziali; (1) sessione remota utilizzerà le stesse credenziali|1|x|x||
| redirectclipboard:i:value | Questa impostazione determina se gli Appunti nel computer locale saranno reindirizzate ed è disponibile nella sessione remota. This setting corresponds to theClipboardcheck box on theLocal Resourcestab underOptionsin RDC. | Appunti (0) nel computer locale non sono disponibile nella sessione remota. (1) Appunti nel computer locale sono disponibili nella sessione remota|1|x|x|x|
| redirectdrives:i:value | Questa impostazione determina se le unità nel computer client saranno reindirizzate ed è disponibile nella sessione remota. This setting corresponds to the selections forDrivesunderMoreon theLocal Resourcestab underOptionsin RDC.|(0) le unità nel computer locale non sono disponibili nella sessione remota; (1) sono disponibili nella sessione remota le unità nel computer locale|0|x|x| |
| redirectprinters:i:value | This setting determines whether printers configured on the client computer will be redirected and available in the remote session when you connect to a remote computer by using Remote Desktop Connection (RDC). This setting corresponds to the selection in thePrinterscheck box on theLocal Resourcestab underOptionsin RDC. | (0) le stampanti nel computer locale non sono disponibili nella sessione remota; (1) sono disponibili nella sessione remota le stampanti nel computer locale|1|x|x|x|
| redirectsmartcards:i:value | This setting determines whether smart card devices on the client computer will be redirected and available in the remote session when you connect to a remote computer by using Remote Desktop Connection (RDC). This setting corresponds to the selection in theSmart cardscheck box underMore on theLocal Resourcestab underOptionsin RDC.|(0) lo smart card nel computer locale non è disponibile nella sessione remota; (1) lo smart card nel computer locale è disponibile nella sessione remota|1|x|x||
| remoteapplicationcmdline:s:value | Parametri della riga di comando facoltativo per RemoteApp.||x|x|x|
| remoteapplicationexpandcmdline:i:value| Determina se le variabili di ambiente contenute nel parametro RemoteApp riga di comando devono essere espanso in locale o in remoto.|Le variabili di ambiente (0) devono essere espanso per i valori del computer locale. (1) le variabili di ambiente devono essere espanso sul computer remoto per i valori del computer remoto||x|x|x|
| remoteapplicationexpandworkingdir | Determina se le variabili di ambiente contenute nel parametro RemoteApp working directory devono essere espanso in locale o in remoto. | Le variabili di ambiente (0) devono essere espanso per i valori del computer locale. (1) le variabili di ambiente devono essere espanso sul computer remoto per i valori del computer remoto. Nota: La directory di lavoro RemoteApp viene specificata tramite il parametro di directory di lavoro della shell.||x|x|x|
|remoteapplicationfile:s:value | Specifica un file deve essere aperto nel computer remoto da RemoteApp. Nota: Per l'apertura di file locali, devi anche abilitare il reindirizzamento delle unità per l'unità di origine.||x|x|x|
|remoteapplicationicon:s:value | Specifica il file di icona da visualizzare nell'interfaccia utente del client durante l'avvio di un RemoteApp. Se non viene specificato alcun nome di file, il client userà l'icona di Desktop remoto standard. Sono supportati solo i file "con estensione ico".||x|x|x|
|remoteapplicationmode:i:value | Determina se una connessione RemoteApp viene avviata una sessione RemoteApp.| (0) non avviare una sessione RemoteApp. (1) avviare una sessione RemoteApp|1|x|x|x|
|remoteapplicationname:s:value | Specifica il nome del RemoteApp nell'interfaccia del client durante l'avvio RemoteApp.| ad esempio, "Excel 2016"|x|x|x|
|remoteapplicationprogram:s:value | Specifica il nome dell'alias o file eseguibile di RemoteApp. | ad esempio, "EXCEL" |x|x|x|
|schermata modalità id: i:value | This setting determines whether the remote session window appears full screen when you connect to the remote computer by using Remote Desktop Connection (RDC). This setting corresponds to the selection in theDisplay configurationslider on theDisplaytab underOptionsin RDC.|(1) la sessione remota verrà visualizzato in una finestra. (2) la sessione remota verrà visualizzato a schermo intero|2|x|x|x|
|ridimensionamento: i:value smart | Questa impostazione determina o meno il computer client può ridimensionare il contenuto sul computer remoto in base alle dimensioni della finestra del computer client.|(0) della visualizzazione della finestra client non essere ridimensionata quando ridimensionato; (1) visualizzazione della finestra il client verrà ridimensionata quando ridimensionato|0|x|x||
| usare multimon:i:value | This setting configures multiple monitor support when you connect to the remote computer by using Remote Desktop Connection (RDC).|(0) non abilita il supporto di più monitor. (1) abilitare il supporto di più monitor|0|x|x||
| UserName:s:value | This setting specifies the name of the user account that will be used to log on to the remote computer by using Remote Desktop Connection (RDC). The value of this setting, along with the value of the domain setting, will appear in theUser namebox on theGeneraltab underOptionsin RDC.| Qualsiasi nome utente valido. ||x|x|x|
| videoplaybackmode:i:value| This setting determines if Remote Desktop Connection (RDC) will use RDP efficient multimedia streaming for video playback.|(0) non utilizzare RDP efficiente flussi multimediali per la riproduzione video; (1) usare RDP efficiente flussi multimediali per la riproduzione video quando possibile|1|x|x||
| workspaceid:s:value | Questa impostazione definisce il RemoteApp e Desktop ID associato il file RDP che contiene l'impostazione di questo. | Un valido RemoteApp e l'ID di connessione Desktop|x|x||