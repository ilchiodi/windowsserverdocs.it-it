---
title: Problemi noti di Windows Admin Center
description: Problemi noti di Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 04/12/2019
ms.openlocfilehash: b0159d88251c7f4f6422dffd8f1e1414e1f30f33
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297044"
---
# Problemi noti di Windows Admin Center

>Si applica a: Windows Admin Center, Anteprima di Windows Admin Center

Se si verifica un problema non descritto in questa pagina, [faccelo sapere](http://aka.ms/WACfeedback).

## Lenovo XClarity Integrator
Non esiste un problema di incompatibilità tra una combinazione di versione specifica di Lenovo XClarity Integrator e Windows Admin Center. Se attualmente usi o prevede di usare l'estensione di Lenovo XClarity Integrator in Windows Admin Center, ecco ciò che devi conoscere:

- Lenovo XClarity Integrator versione dell'estensione 1.0.4 è completamente compatibile con Windows Admin Center 1809.5.
- Lenovo XClarity Integrator versione dell'estensione 1.0.4 ha esposto un problema di compatibilità in Windows Admin Center 1904. Tecnici di Microsoft e Lenovo siano esaminando attivamente i insieme, e speriamo offrire una soluzione appena possibile. Tutti gli aggiornamenti verranno pubblicati qui sul sito della documentazione di Windows Admin Center e puoi anche fare riferimento alla [pagina di supporto del Lenovo](https://support.lenovo.com/solutions/ht507549) per riferimento.
- Se sei un utente di funzionalità XClarity del Lenovo frequente, è possibile entrambi Mantieniti su Windows Admin Center 1809.5 per continuare a usare XClarity Integrator 1.0.4 o puoi eseguire l'aggiornamento a Windows Admin Center 1904 e utilizzare separatamente autonomo XClarity amministratore software come soluzione per il momento.


## Programma di installazione

- Durante l'installazione di Windows Admin Center utilizzando il proprio certificato, occorre tenere presente che se si copia l'identificazione personale dallo strumento MMC Gestione certificati, [contiene un carattere non valido all'inizio.](https://support.microsoft.com/help/2023835/certificate-thumbprint-displayed-in-mmc-certificate-snap-in-has-extra) In alternativa, digita il primo carattere dell'identificazione personale, quindi copia e incolla il resto.

- Tramite la porta inferiori a 1024 non è supportato. In modalità di servizio, è possibile configurare facoltativamente la porta 80 per reindirizzare alla porta specificata.

- Se il servizio Windows Update (wuauserv) viene arrestato e disabilitato, il programma di installazione avrà esito negativo. [19100629]

### Aggiornamento di versione/Aggiornare la versione

- Durante l'aggiornamento di Windows Admin Center in modalità di servizio da una versione precedente, se usi msiexec in modalità non interattiva, che potrebbe verificarsi un problema in cui la regola del firewall in entrata per la porta Windows Admin Center viene eliminata.
  - Per ricreare la regola, eseguire il comando seguente da una console di PowerShell con privilegi elevata, sostituendo \<port> con la porta configurata per Windows Admin Center (impostazione predefinita 443).

    ```powershell
    New-NetFirewallRule -DisplayName "SmeInboundOpenException" -Description "Windows Admin Center inbound port exception" -LocalPort <port> -RemoteAddress Any -Protocol TCP
    ```

## Generale

- Se hai Windows Admin Center installato come gateway su **Windows Server 2016** in uso eccessivo, il servizio potrebbe arrestarsi in modo anomalo con un errore nel registro eventi che contiene ```Faulting application name: sme.exe``` e ```Faulting module name: WsmSvc.dll```. Ciò è dovuto a un bug che è stato risolto in Windows Server 2019. L'oggetto patch di Windows Server 2016 è stato incluso l'aggiornamento cumulativo di febbraio 2019 aggiornare, [KB4480977](https://www.catalog.update.microsoft.com/Search.aspx?q=4480977).

- Se hai Windows Admin Center installato come gateway e viene visualizzato l'elenco di connessione potrebbe essere danneggiato, eseguire i passaggi seguenti:

>[!WARNING]
>Ciò elimina le impostazioni per tutti gli utenti di Windows Admin Center sul gateway e un elenco di connessione.

  1. Disinstallare Windows Admin Center
  2. Eliminare la cartella **Server Management Experience** in **C:\Windows\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft**
  3. Reinstallare Windows Admin Center

- Se lasci lo strumento aperto e inattivo per un lungo periodo di tempo, potresti ottenere diversi errori del tipo **Error: The runspace state is not valid for this operation**. In tal caso, aggiorna il browser. Se si verifica, [inviare il feedback](http://aka.ms/WACfeedback).

- Potresti riscontrare un **errore 500** durante l'aggiornamento di pagine con URL molto lunghi. [12443710]

- In alcuni strumenti, il controllo ortografico del browser può contrassegnare alcuni valori di campo come errati. [12425477]

- In alcuni strumenti, i pulsanti di comando potrebbero non riflettere le modifiche allo stato subito dopo aver fatto clic e l'interfaccia utente dello strumento potrebbe non riflettere automaticamente le modifiche a proprietà specifiche. Puoi fare clic su **Aggiorna** per recuperare lo stato più recente dal server di destinazione. [11445790]

- Tag di filtro nell'elenco di connessione - se si seleziona connessioni utilizzando le caselle di controllo di selezione multipla, quindi filtrare l'elenco di connessione per i tag, la selezione originale mantiene in modo che qualsiasi azione che selezioni verrà applicate a tutti i computer selezionati in precedenza. [18099259]

- Potrebbe essere alcune minime differenze numeri di versione dei sistemi operativi in esecuzione nei moduli di Windows Admin Center e quelli elencate nel 3a notifica di Software di terze parti.

### Gestione estensioni

- Quando si aggiorna Windows Admin Center, è necessario reinstallare le estensioni.
- Se Aggiungi un'estensione feed che non è accessibile, non esiste alcun avviso. [14412861]

## Problemi specifici del browser

### Microsoft Edge

- In alcuni casi, possono verificarsi tempi di caricamento lungo quando si utilizza Microsoft Edge per accedere a un gateway Windows Admin Center tramite Internet. Ciò può verificarsi nelle macchine virtuali di Azure in cui il gateway di Windows Admin Center utilizza un certificato autofirmato. [13819912]

- Durante l'utilizzo di Azure Active Directory come provider di identità e Windows Admin Center è configurato con un certificato autofirmato o di altro tipo non attendibile, non puoi completare l'autenticazione AAD in Microsoft Edge.  [15968377]

- Se hai distribuito come servizio Windows Admin Center e si usa Microsoft Edge come browser, connette il gateway di Azure può avere esito negativo dopo la generazione di una nuova finestra del browser. Provare a risolvere questo problema, aggiungendo https://login.microsoftonline.com, https://login.live.com, e l'URL del gateway come siti attendibili e siti consentiti per le impostazioni di blocco popup del browser sul lato client. Per altre indicazioni per risolvere questo problema nella [Guida di risoluzione dei problemi](troubleshooting.md#azlogin). [17990376]

- Se hai installato in modalità desktop di Windows Admin Center, nella scheda del browser Microsoft Edge non visualizza la favicon. [17665801]

### Google Chrome

- Prima della versione 70 (rilasciato tardiva ottobre 2018) Chrome avuto un [bug](https://bugs.chromium.org/p/chromium/issues/detail?id=423609) riguardanti il protocollo WebSocket e l'autenticazione NTLM. Questo interessa i seguenti strumenti: Eventi, PowerShell, Desktop remoto.

- Chrome potrebbe visualizzare diverse richieste di credenziali, soprattutto durante l'esperienza di aggiunta di una connessione in un ambiente **gruppo di lavoro** (non di dominio).

- Se hai distribuito come servizio Windows Admin Center, popup dall'URL gateway necessario abilitare per qualsiasi funzionalità di integrazione di Azure a funzionare. Questi servizi includono scheda di rete di Azure, gestione degli aggiornamenti Azure e Azure Site Recovery.

### Mozilla Firefox

Windows Admin Center non è stato testato con Mozilla Firefox, ma la maggior parte delle funzionalità dovrebbe funzionare.

- Installazione su Windows 10: Mozilla Firefox ha un proprio archivio certificati, pertanto devi importare il certificato ```Windows Admin Center Client``` in Firefox per utilizzare Windows Admin Center in Windows 10.

<a id="websockets"></a>

## Compatibilità WebSocket quando si utilizza un servizio proxy

I moduli Remote Desktop, PowerShell ed Eventi in Windows Admin Center utilizzano il protocollo WebSocket, spesso non supportato quando si utilizza un servizio proxy. Il supporto per WebSocket nella compatibilità proxy dell'App Azure AD è disponibile nell'[anteprima](https://blogs.technet.microsoft.com/applicationproxyblog/2018/03/28/limited-websocket-support-now-in-public-preview/) o cercando il feedback sulla compatibilità.

## Supporto per le versioni di Windows Server prima della 2016 (2012 R2, 2012, 2008 R2)

> [!NOTE]
> Windows Admin Center richiede funzionalità PowerShell non incluse in Windows Server 2012 R2, 2012 o 2008 R2. Se dovrai gestire Windows Server con Windows Admin Center, devi installare WMF 5.1 o versione successiva in tali server.

Digita `$PSVersiontable` in PowerShell per verificare che WMF 5.1 o versione successiva sia installato.

Se non è installato, puoi [scaricare e installare WMF 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616).

<a id="rbacknownissues"></a>

## Controllo degli accessi in base al ruolo

- La distribuzione di Controllo degli accessi in base al ruolo non verrà completata sui computer configurati per utilizzare Controllo di applicazioni di Windows Defender (WDAC, in precedenza noto come integrità del codice) [16568455]

- Per utilizzare Controllo degli accessi in base al ruolo in un cluster, devi distribuire la configurazione per ogni nodo del membro singolarmente.

- Quando Controllo degli accessi in base al ruolo viene distribuito, potresti ottenere errori non autorizzati erroneamente attribuiti alla configurazione di Controllo degli accessi in base al ruolo. [16369238]

## Soluzione Server Manager

### Impostazioni del server

- Se modificare un'impostazione, prova a Esci senza salvare, la pagina genera avvisi in merito le modifiche non salvate, ma continuare a Esci. Potrebbero finire in uno stato in cui la scheda di impostazioni selezionato non corrisponde il contenuto della pagina. [19905798] [19905787]

### Certificati

- Impossibile importare il certificato crittografato .PFX nell'archivio utente corrente. [11818622]

### Dispositivi

- Durante lo spostamento tramite la tabella con la tastiera, la selezione potrebbe passare all'inizio del gruppo di tabella. [16646059]

### Eventi

- Gli eventi sono interessati dalla [compatibilità websocket quando si utilizza un servizio proxy.](#websockets)

- Potresti ottenere un errore che fa riferimento alle "dimensione del pacchetto" durante l'esportazione di file di registro di grandi dimensioni. [16630279]

  - Per risolvere questo problema, utilizza il comando seguente in un prompt dei comandi con privilegi elevati sul computer gateway: ```winrm set winrm/config @{MaxEnvelopeSizekb="8192"}```

### File

- Il caricamento o download di file di grandi dimensioni non è ancora supportato. (Limite \~100 mb) [12524234]

### PowerShell

- PowerShell è interessato dalla [compatibilità websocket quando si utilizza un servizio proxy](#websockets)

- L'operazione Incolla facendo clic una sola volta con il pulsante destro del mouse nella console PowerShell desktop non funziona. In realtà verrà visualizzato il menu contestuale del browser, in cui è possibile selezionare Incolla. È possibile utilizzare anche CTRL+V.

- Il comando CTRL+C per copiare non funziona, in quanto invia sempre il comando di interruzione CTRL+C alla console. Il comando Copia del menu contestuale visualizzato facendo clic con il pulsante destro del mouse funziona.

- Quando riduci le dimensioni della finestra di Windows Admin Center, verrà effettuato l'adattamento dinamico del contenuto, ma quando viene ingrandita di nuovo, il contenuto potrebbe non tornare allo stato precedente. Se il contenuto diventa confuso, puoi provare il comando Clear-Host o disconnetterti e riconnetterti utilizzando il pulsante sopra il terminale.

### Editor del Registro di sistema

- La funzionalità di ricerca non è implementata. [13820009]

### Desktop remoto

- Lo strumento Desktop remoto potrà avere esito negativo per la connessione durante la gestione di Windows Server 2012. [20258278]

- Quando il Desktop remoto per connettersi a un computer che non è aggiunto a un dominio, è necessario immettere l'account nel ```MACHINENAME\USERNAME``` formato.

- Alcune configurazioni di bloccare client desktop remoto a del Windows Admin Center con criteri di gruppo. Se si verifica, abilitare ```Allow users to connect remotely by using Remote Desktop Services``` in ```Computer Configuration/Policies/Administrative Templates/Windows Components/Remote Desktop Services/Remote Desktop Session Host/Connections```

- Desktop remoto è interessato dalla [compatibilità websocket.](#websockets)

- Lo strumento Desktop remoto non supporta attualmente alcuna operazione di copia/incolla di testo, immagini o file tra il desktop locale e la sessione remota.

- Per eseguire l'operazione di copia/ incolla all'interno della sessione remota, puoi copiare normalmente (fai clic con il pulsante destro + copia o CTRL+C), ma per incollare devi fare clic con il pulsante destro + incollare (CTRL+V NON funziona)

- Non è possibile inviare i seguenti comandi chiave alla sessione remota
  - ALT+TAB
  - Tasti funzione
  - Tasto Windows
  - STAMP

- App remota – dopo aver attivato lo strumento App remota dalle impostazioni di Desktop remoto, lo strumento potrebbe non essere visualizzate nell'elenco strumento durante la gestione di un Server con esperienza Desktop. [18906904]

### Ruoli e funzionalità

- Quando si selezionano ruoli o funzionalità con origini non disponibili per l'installazione, vengono ignorati. [12946914]

- Se scegli di non riavviare automaticamente dopo l'installazione del ruolo, la richiesta non verrà visualizzata nuovamente. [13098852]

- Se scegli il riavvio automatico, questo si verificherà prima che lo stato venga aggiornato al 100%. [13098852]

### Archiviazione

- Recupero delle informazioni di Quota può avere esito negativo senza una notifica di errore (comunque sarà presente un errore nella console del browser) [18962274]

- Livello inferiore: le unità CD/DVD/Floppy non vengono visualizzate come volumi di livello inferiore.

- Livello inferiore: alcune proprietà in dischi e volumi non sono disponibili al livello inferiore in modo che vengano visualizzate come sconosciute o vuote nel riquadro dei dettagli.

- Livello inferiore: durante la creazione di un nuovo volume, ReFS supporta solo una dimensione unità di allocazione di 64 KB sui computer Windows 2012 e 2012 R2. Se si crea un volume ReFS con una dimensione di unità di allocazione più piccola sulle relative destinazioni di livello inferiore, la formattazione del file system non riesce. Il nuovo volume non sarà utilizzabile. La soluzione consiste nell'eliminare il volume e utilizzare una dimensione dell'unità di allocazione di 64 KB.

### Aggiornamenti

- Dopo l'installazione degli aggiornamenti, lo stato installato potrebbe essere memorizzato nella cache e richiede l'aggiornamento di un browser.

- Che possono verificarsi l'errore: "Keyset non esiste" quando si tenta di impostare la gestione degli aggiornamenti di Azure. In questo caso, provare i seguenti passaggi di correzione sul nodo gestito-
    1. Arrestare il servizio 'Servizi di crittografia'.
    2. Opzioni di cartella cambia per mostrare i file nascosti (se richiesto).
    3. Recuperato nella cartella "% allusersprofile%\Microsoft\Crypto\RSA\S-1-5-18" ed elimina i relativi contenuti.
    4. Riavviare il servizio 'Servizi di crittografia'.
    5. Ripeti l'impostazione di gestione degli aggiornamenti con Windows Admin Center

### Macchine virtuali

- Quando si gestiscono le macchine virtuali in un host di Windows Server 2012, connettersi alla macchina virtuale nel browser strumento avrà esito negativo per connettersi alla macchina virtuale. Scaricare il file RDP per la connessione alla macchina virtuale dovrebbero comunque funzionare. [20258278]

- Azure Site Recovery – se ASR è installato nell'host di fuori di WAC, sarai in grado di proteggere una macchina virtuale all'interno di WAC [18972276]

- La funzionalità avanzate disponibili in Hyper-V Manager, ad esempio Gestione SAN virtuale, Sposta macchina virtuale, Esporta macchina virtuale, Replica macchina virtuale non sono attualmente supportate.

### Switch virtuali

- Switch Embedded Teaming (SET): durante l'aggiunta di schede di rete a un team, devono essere nella stessa subnet.

## Soluzione di Gestione computer

La soluzione Gestione computer contiene un sottoinsieme degli strumenti della soluzione Gestione server, pertanto si applicano gli stessi problemi noti, nonché i seguenti problemi specifici della soluzione Gestione computer:

- Se si utilizza un Account Microsoft ([MSA](https://account.microsoft.com/account/)) oppure se si utilizza Azure Active Directory (AAD) per accedere al computer Windows 10, devi specificare "gestire-come" le credenziali per gestire il computer locale [16568455]

- Quando provi a gestire il localhost, ti verrà richiesto di elevare il processo di gateway. Se fai clic su **no** nel popup Controllo dell'account utente che segue, Windows Admin Center non sarà in grado di visualizzarlo nuovamente. In questo caso, chiudi il processo di gateway facendo clic con il pulsante destro del mouse sull'icona di Windows Admin Center nella barra delle applicazioni e scegli Esci, quindi riavvia Windows Admin Center dal menu Start.

- Windows 10 non dispone di PowerShell/WinRM in remoto per impostazione predefinita
  
  - Per abilitare la gestione di Windows 10 Client, devi eseguire il comando ```Enable-PSRemoting``` da un prompt dei comandi di PowerShell con privilegi elevati.

  - Potrebbe essere necessario anche aggiornare il firewall per consentire le connessioni dall'esterno della subnet locale con ```Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any```. Per gli scenari di reti più restrittivi, fai riferimento a [questa documentazione](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-5.1).

## Soluzione Gestione cluster di failover

- Durante la gestione di un cluster (iperconvergente o tradizionale) potrebbe verificarsi un errore del tipo **shell non trovata**. In questo caso ricarica il browser o esci per accedere a un altro strumento e torna indietro. [13882442]

- Un problema può verificarsi durante la gestione di un cluster di livello inferiore (Windows Server 2012 o 2012 R2) che non è stato configurato completamente. La soluzione di questo problema consiste nel verificare che la funzionalità di Windows **RSAT-Clustering-PowerShell** sia stata installata e attivata su **ogni nodo del membro** del cluster. Per ottenere questo risultato in PowerShell, digita il comando `Install-WindowsFeature -Name RSAT-Windows-PowerShell` in tutti i nodi del cluster. [12524664]

- Potrebbe essere necessario aggiungere il cluster con l'intero nome di dominio completo per essere individuato correttamente.

- Durante la connessione a un cluster tramite Windows Admin Center installato come gateway e fornendo un nome utente e una password espliciti per l'autenticazione, devi selezionare **Usa queste credenziali per tutte le connessioni** in modo che le credenziali siano disponibili per la query ai nodi del membro.

## Soluzione Hyper-Converged Cluster Manager

- Alcuni comandi, ad esempio **Drives - Update firmware**, **Drives - Update firmware** e **Volumes - Open** sono disabilitati e al momento non supportati.