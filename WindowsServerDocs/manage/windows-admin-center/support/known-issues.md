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
ms.openlocfilehash: 7bf23c5af5620241574864babd07fd852115a450
ms.sourcegitcommit: 39ab8041d166e6817a95417d6aa30bc7abeeef54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/28/2019
ms.locfileid: "66260262"
---
# <a name="windows-admin-center-known-issues"></a>Problemi noti di Windows Admin Center

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

Se si verifica un problema non descritto in questa pagina, [faccelo sapere](http://aka.ms/WACfeedback).

## <a name="lenovo-xclarity-integrator"></a>Lenovo XClarity Integrator
Il problema di incompatibilità in precedenza divulgata dell'estensione di Lenovo XClarity Integrator e Windows Admin Center versione 1904 è stato risolto con Windows Admin Center versione 1904.1. Si consiglia di aggiornare la versione supportata più recente di Windows Admin Center.

- Lenovo XClarity Integrator versione 1.1 dell'estensione è completamente compatibile con Windows Admin Center 1904.1. È consigliabile aggiornare alla versione più recente di Windows Admin Center e l'estensione di Lenovo.
- Per qualsiasi motivo, se è necessario continuare a utilizzare Windows Admin Center 1809.5 per il momento, è possibile usare Integrator XClarity 1.0.4 che saranno disponibili anche in Windows Admin Center estensione Feed fino a quando non Windows Admin Center 1809.5 non è più supportata in base nostri [criteri di supporto](../support/index.md).

## <a name="installer"></a>Programma di installazione

- Durante l'installazione di Windows Admin Center utilizzando il proprio certificato, occorre tenere presente che se si copia l'identificazione personale dallo strumento MMC Gestione certificati, [contiene un carattere non valido all'inizio.](https://support.microsoft.com/help/2023835/certificate-thumbprint-displayed-in-mmc-certificate-snap-in-has-extra) In alternativa, digita il primo carattere dell'identificazione personale, quindi copia e incolla il resto.

- Non è supportata tramite la porta inferiori a 1024. In modalità servizio, è possibile facoltativamente configurare la porta 80 per reindirizzare la porta specificata.

- Se il servizio di Windows Update (wuauserv) è stato arrestato e disabilitato, il programma di installazione avrà esito negativo. [19100629]

### <a name="upgrade"></a>Aggiornamento

- Quando si aggiorna Windows Admin Center in modalità di servizio da una versione precedente, se si usa msiexec in modalità non interattiva, è possibile riscontrare un problema in cui viene eliminata la regola del firewall in ingresso per la porta di Windows Admin Center.
  - Per ricreare la regola, eseguire il comando seguente da una console di PowerShell con privilegi elevata, sostituendo \<porta > con la porta configurata per Windows Admin Center (impostazione predefinita 443).

    ```powershell
    New-NetFirewallRule -DisplayName "SmeInboundOpenException" -Description "Windows Admin Center inbound port exception" -LocalPort <port> -RemoteAddress Any -Protocol TCP
    ```

## <a name="general"></a>Generale

- Se si dispone di Windows Admin Center installato come gateway sul **Windows Server 2016** con un uso massiccio, il servizio potrebbe bloccarsi con un errore nel registro eventi contenente ```Faulting application name: sme.exe``` e ```Faulting module name: WsmSvc.dll```. Ciò è dovuto a un bug che è stato risolto in Windows Server 2019. La patch per Windows Server 2016 è stata inclusa l'aggiornamento cumulativo di febbraio 2019, [KB4480977](https://www.catalog.update.microsoft.com/Search.aspx?q=4480977).

- Se si ha Windows Admin Center installato come gateway e l'elenco delle connessioni sembra essere danneggiata, procedere come segue:

>[!WARNING]
>Questa operazione eliminerà l'elenco delle connessioni e le impostazioni per tutti gli utenti di Windows Admin Center nel gateway.

  1. Disinstallare Windows Admin Center
  2. Eliminare la cartella **Server Management Experience** in **C:\Windows\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft**
  3. Reinstallare Windows Admin Center

- Se si lascia aperta lo strumento e inattive per un lungo periodo di tempo, si potrebbero ottenere diversi **errore: Lo stato dello spazio di esecuzione non è valido per questa operazione** errori. In tal caso, aggiorna il browser. Se questa operazione, si verifica [inviare commenti e suggerimenti](http://aka.ms/WACfeedback).

- Potresti riscontrare un **errore 500** durante l'aggiornamento di pagine con URL molto lunghi. [12443710]

- In alcuni strumenti, il controllo ortografico del browser può contrassegnare alcuni valori di campo come errati. [12425477]

- In alcuni strumenti, i pulsanti di comando potrebbero non riflettere le modifiche allo stato subito dopo aver fatto clic e l'interfaccia utente dello strumento potrebbe non riflettere automaticamente le modifiche a proprietà specifiche. Puoi fare clic su **Aggiorna** per recuperare lo stato più recente dal server di destinazione. [11445790]

- Tag del filtro su elenco di connessioni - se si seleziona le connessioni utilizzando le caselle di controllo di selezione multipla, filtra l'elenco di connessioni per i tag, la selezione originale mantiene in modo che qualsiasi operazione selezionata verrà applicata a tutti i computer selezionati in precedenza. [18099259]

- Potrebbe esserci una minore varianza tra i numeri di versione del software Open Source in esecuzione in moduli di Windows Admin Center, e quanto elencato all'interno di notifica di Software di terze parti 3rd.

### <a name="extension-manager"></a>Extension Manager

- Quando si aggiorna Windows Admin Center, è necessario reinstallare le estensioni.
- Se si aggiunge un'estensione di feed che non è accessibile, non vi è alcun messaggio di avviso. [14412861]

## <a name="browser-specific-issues"></a>Problemi specifici del browser

### <a name="microsoft-edge"></a>Microsoft Edge

- In alcuni casi, possono verificarsi tempi di caricamento lungo quando si utilizza Microsoft Edge per accedere a un gateway Windows Admin Center tramite Internet. Ciò può verificarsi nelle macchine virtuali di Azure in cui il gateway di Windows Admin Center utilizza un certificato autofirmato. [13819912]

- Durante l'utilizzo di Azure Active Directory come provider di identità e Windows Admin Center è configurato con un certificato autofirmato o di altro tipo non attendibile, non puoi completare l'autenticazione AAD in Microsoft Edge.  [15968377]

- Se si dispone di Windows Admin Center distribuito come servizio e si usa Microsoft Edge come browser, ci si connette il gateway in Azure potrebbe non riuscire dopo la generazione di una nuova finestra del browser. Provare a risolvere il problema aggiungendo https://login.microsoftonline.com, https://login.live.com, e l'URL del gateway come siti attendibili e siti consentiti per le impostazioni di blocco popup nel browser sul lato client. Per altre indicazioni sulla risoluzione di questo in errore la [Guida alla risoluzione dei](troubleshooting.md#azlogin). [17990376]

- Se hai installato in modalità desktop di Windows Admin Center, la scheda del browser Microsoft Edge non verrà visualizzato il favicon. [17665801]

### <a name="google-chrome"></a>Google Chrome

- Prima della versione 70 (rilasciato alla fine del 2018, ottobre) Chrome aveva una [bug](https://bugs.chromium.org/p/chromium/issues/detail?id=423609) per quanto riguarda il protocollo di websockets e l'autenticazione NTLM. Ciò ha effetto gli strumenti seguenti: Gli eventi, PowerShell, Desktop remoto.

- Chrome potrebbe visualizzare diverse richieste di credenziali, soprattutto durante l'esperienza di aggiunta di una connessione in un ambiente **gruppo di lavoro** (non di dominio).

- Se si dispone di Windows Admin Center distribuito come servizio, i popup provenienti dall'URL del gateway necessario deve essere abilitato per qualsiasi uso della funzionalità integrazione di Azure. Questi servizi includono una scheda di rete di Azure, gestione degli aggiornamenti di Azure e Azure Site Recovery.

### <a name="mozilla-firefox"></a>Mozilla Firefox

Windows Admin Center non è stato testato con Mozilla Firefox, ma la maggior parte delle funzionalità dovrebbe funzionare.

- Installazione di Windows 10: Mozilla Firefox ha proprio archivio certificati, quindi è necessario importare il ```Windows Admin Center Client``` certificato in Firefox per l'uso di Windows Admin Center on Windows 10.

<a id="websockets"></a>

## <a name="websocket-compatibility-when-using-a-proxy-service"></a>Compatibilità di WebSocket quando si usa un servizio proxy

I moduli Remote Desktop, PowerShell ed Eventi in Windows Admin Center utilizzano il protocollo WebSocket, spesso non supportato quando si utilizza un servizio proxy. Il supporto per WebSocket nella compatibilità proxy dell'App Azure AD è disponibile nell'[anteprima](https://blogs.technet.microsoft.com/applicationproxyblog/2018/03/28/limited-websocket-support-now-in-public-preview/) o cercando il feedback sulla compatibilità.

## <a name="support-for-windows-server-versions-before-2016-2012-r2-2012-2008-r2"></a>Supporto per le versioni di Windows Server 2016 (2012 R2, 2012, 2008 R2) prima di

> [!NOTE]
> Windows Admin Center richiede alcune funzionalità di PowerShell che non sono inclusi in Windows Server 2012 R2, 2012 o 2008 R2. Se si gestisce Windows Server con Windows Admin Center, dovrai installare WMF 5.1 o versione successiva in tali server.

Digita `$PSVersiontable` in PowerShell per verificare che WMF 5.1 o versione successiva sia installato.

Se non è installato, puoi [scaricare e installare WMF 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616).

<a id="rbacknownissues"></a>

## <a name="role-based-access-control-rbac"></a>In base al ruolo (RBAC) di controllo di accesso

- La distribuzione di Controllo degli accessi in base al ruolo non verrà completata sui computer configurati per utilizzare Controllo di applicazioni di Windows Defender (WDAC, in precedenza noto come integrità del codice) [16568455]

- Per utilizzare Controllo degli accessi in base al ruolo in un cluster, devi distribuire la configurazione per ogni nodo del membro singolarmente.

- Quando Controllo degli accessi in base al ruolo viene distribuito, potresti ottenere errori non autorizzati erroneamente attribuiti alla configurazione di Controllo degli accessi in base al ruolo. [16369238]

## <a name="server-manager-solution"></a>Soluzione Server Manager

### <a name="server-settings"></a>Impostazioni del server

- Se si modifica un'impostazione e si prova a uscire senza salvare le modifiche, la pagina verrà avvisare in caso di modifiche non salvate, ma continuare a uscire. Si potrebbe incorrere in uno stato in cui la scheda Impostazioni selezionato corrisponde al contenuto della pagina. [19905798] [19905787]

### <a name="certificates"></a>Certificati

- Impossibile importare il certificato crittografato .PFX nell'archivio utente corrente. [11818622]

### <a name="devices"></a>Dispositivi

- Durante lo spostamento tra la tabella con la tastiera, la selezione potrebbe passare alla parte superiore del gruppo di tabella. [16646059]

### <a name="events"></a>Eventi

- Gli eventi sono interessati dalla [compatibilità websocket quando si utilizza un servizio proxy.](#websockets)

- Potresti ottenere un errore che fa riferimento alle "dimensione del pacchetto" durante l'esportazione di file di registro di grandi dimensioni. [16630279]

  - Per risolvere questo problema, usare il comando seguente in un prompt dei comandi con privilegi elevati nel computer del gateway: ```winrm set winrm/config @{MaxEnvelopeSizekb="8192"}```

### <a name="files"></a>File

- Il caricamento o download di file di grandi dimensioni non è ancora supportato. (\~limite di 100 mb) [12524234]

### <a name="powershell"></a>PowerShell

- PowerShell è interessato dalla [compatibilità websocket quando si utilizza un servizio proxy](#websockets)

- L'operazione Incolla facendo clic una sola volta con il pulsante destro del mouse nella console PowerShell desktop non funziona. In realtà verrà visualizzato il menu contestuale del browser, in cui è possibile selezionare Incolla. È possibile utilizzare anche CTRL+V.

- Il comando CTRL+C per copiare non funziona, in quanto invia sempre il comando di interruzione CTRL+C alla console. Il comando Copia del menu contestuale visualizzato facendo clic con il pulsante destro del mouse funziona.

- Quando riduci le dimensioni della finestra di Windows Admin Center, verrà effettuato l'adattamento dinamico del contenuto, ma quando viene ingrandita di nuovo, il contenuto potrebbe non tornare allo stato precedente. Se il contenuto diventa confuso, puoi provare il comando Clear-Host o disconnetterti e riconnetterti utilizzando il pulsante sopra il terminale.

### <a name="registry-editor"></a>Editor del Registro di sistema

- La funzionalità di ricerca non è implementata. [13820009]

### <a name="remote-desktop"></a>Desktop remoto

- Lo strumento di Desktop remoto potrebbe non riuscire a connettersi durante la gestione di Windows Server 2012. [20258278]

- Quando si usa Desktop remoto per connettersi a un computer non aggiunti a un dominio, è necessario immettere il proprio account nel ```MACHINENAME\USERNAME``` formato.

- Alcune configurazioni possono bloccare i client desktop remoto di Windows Admin Center con criteri di gruppo. Se si verifica ciò, abilitare ```Allow users to connect remotely by using Remote Desktop Services``` sotto ```Computer Configuration/Policies/Administrative Templates/Windows Components/Remote Desktop Services/Remote Desktop Session Host/Connections```

- Desktop remoto è effettuato da [compatibilità websocket.](#websockets)

- Lo strumento Desktop remoto non supporta attualmente alcuna operazione di copia/incolla di testo, immagini o file tra il desktop locale e la sessione remota.

- Per eseguire l'operazione di copia/ incolla all'interno della sessione remota, puoi copiare normalmente (fai clic con il pulsante destro + copia o CTRL+C), ma per incollare devi fare clic con il pulsante destro + incollare (CTRL+V NON funziona)

- Non è possibile inviare i seguenti comandi chiave alla sessione remota
  - ALT+TAB
  - Tasti funzione
  - Tasto Windows
  - STAMP

- App remote: dopo aver abilitato lo strumento Remote App dalle impostazioni di Desktop remoto, lo strumento potrebbe non vengono visualizzati nell'elenco degli strumenti quando si gestisce un Server con esperienza Desktop. [18906904]

### <a name="roles-and-features"></a>Ruoli e funzionalità

- Quando si selezionano ruoli o funzionalità con origini non disponibili per l'installazione, vengono ignorati. [12946914]

- Se scegli di non riavviare automaticamente dopo l'installazione del ruolo, la richiesta non verrà visualizzata nuovamente. [13098852]

- Se scegli il riavvio automatico, questo si verificherà prima che lo stato venga aggiornato al 100%. [13098852]

### <a name="storage"></a>Archiviazione

- Il recupero di informazioni sulle quote può non riuscire senza una notifica di errore (è comunque presente un errore nella console del browser) [18962274]

- Livello inferiore: Unità CD/DVD o Floppy non vengono visualizzati come volumi nel livello inferiore.

- Livello inferiore: Alcune proprietà in volumi e dischi non sono disponibili legacy in modo che vengano visualizzati sconosciuto o lasciare vuoto nel riquadro dei dettagli.

- Livello inferiore: Quando si crea un nuovo volume, ReFS supporta solo una dimensione di unità di allocazione di 64K in Windows 2012 e 2012 R2 macchine. Se si crea un volume ReFS con una dimensione di unità di allocazione più piccola sulle relative destinazioni di livello inferiore, la formattazione del file system non riesce. Il nuovo volume non sarà utilizzabile. La soluzione consiste nell'eliminare il volume e utilizzare una dimensione dell'unità di allocazione di 64 KB.

### <a name="updates"></a>Aggiornamenti

- Dopo aver installato gli aggiornamenti, stato dell'installazione potrebbe essere memorizzato nella cache e richiede un aggiornamento del browser.

- È possibile riscontrare l'errore: "Keyset non esistente" durante il tentativo di impostare la gestione degli aggiornamenti di Azure. In questo caso, provare la procedura di correzione seguente nel nodo gestito-
    1. Arrestare il servizio 'Servizi di crittografia'.
    2. Modificare le opzioni di cartella per visualizzare i file nascosti (se richiesto).
    3. Ottenuto alla cartella "% allusersprofile%\Microsoft\Crypto\RSA\S-1-5-18" ed eliminare tutto il relativo contenuto.
    4. Riavviare il servizio 'Servizi di crittografia'.
    5. Ripetere il configurazione di gestione degli aggiornamenti con Windows Admin Center

### <a name="virtual-machines"></a>Macchine virtuali

- Quando si gestiscono le macchine virtuali in un host Windows Server 2012, la macchina virtuale nel browser connettersi lo strumento riusciranno a connettersi alla macchina virtuale. Download del file con estensione rdp per connettersi alla macchina virtuale dovrebbe funzionare comunque. [20258278]

- Azure Site Recovery: se Azure Site Recovery è configurato nell'host all'esterno di WAC, non sarà possibile proteggere una macchina virtuale dall'interno WAC [18972276]

- La funzionalità avanzate disponibili in Hyper-V Manager, ad esempio Gestione SAN virtuale, Sposta macchina virtuale, Esporta macchina virtuale, Replica macchina virtuale non sono attualmente supportate.

### <a name="virtual-switches"></a>Commutatori virtuali

- Switch Embedded Teaming (SET): Durante l'aggiunta di schede di rete a un team, devono essere nella stessa subnet.

## <a name="computer-management-solution"></a>Soluzione di Gestione computer

La soluzione Gestione computer contiene un sottoinsieme degli strumenti della soluzione Gestione server, pertanto si applicano gli stessi problemi noti, nonché i seguenti problemi specifici della soluzione Gestione computer:

- Se si usa un Account Microsoft ([MSA](https://account.microsoft.com/account/)) oppure se si usa Azure Active Directory (AAD) per accedere al computer Windows 10, è necessario specificare "Gestisci-come" delle credenziali per gestire il computer locale [16568455]

- Quando provi a gestire il localhost, ti verrà richiesto di elevare il processo di gateway. Se fai clic su **no** nel popup Controllo dell'account utente che segue, Windows Admin Center non sarà in grado di visualizzarlo nuovamente. In questo caso, chiudi il processo di gateway facendo clic con il pulsante destro del mouse sull'icona di Windows Admin Center nella barra delle applicazioni e scegli Esci, quindi riavvia Windows Admin Center dal menu Start.

- Windows 10 non dispone di PowerShell/WinRM in remoto per impostazione predefinita
  
  - Per abilitare la gestione di Windows 10 Client, devi eseguire il comando ```Enable-PSRemoting``` da un prompt dei comandi di PowerShell con privilegi elevati.

  - Potrebbe essere necessario anche aggiornare il firewall per consentire le connessioni dall'esterno della subnet locale con ```Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any```. Per gli scenari di reti più restrittivi, fai riferimento a [questa documentazione](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-5.1).

## <a name="failover-cluster-manager-solution"></a>Soluzione Gestione cluster di failover

- Durante la gestione di un cluster (iperconvergente o tradizionale) potrebbe verificarsi un errore del tipo **shell non trovata**. In questo caso ricarica il browser o esci per accedere a un altro strumento e torna indietro. [13882442]

- Un problema può verificarsi durante la gestione di un cluster di livello inferiore (Windows Server 2012 o 2012 R2) che non è stato configurato completamente. La soluzione di questo problema consiste nel verificare che la funzionalità di Windows **RSAT-Clustering-PowerShell** sia stata installata e attivata su **ogni nodo del membro** del cluster. Per ottenere questo risultato in PowerShell, digita il comando `Install-WindowsFeature -Name RSAT-Windows-PowerShell` in tutti i nodi del cluster. [12524664]

- Potrebbe essere necessario aggiungere il cluster con l'intero nome di dominio completo per essere individuato correttamente.

- Durante la connessione a un cluster tramite Windows Admin Center installato come gateway e fornendo un nome utente e una password espliciti per l'autenticazione, devi selezionare **Usa queste credenziali per tutte le connessioni** in modo che le credenziali siano disponibili per la query ai nodi del membro.

## <a name="hyper-converged-cluster-manager-solution"></a>Soluzione Hyper-Converged Cluster Manager

- Alcuni comandi, ad esempio **Drives - Update firmware**, **Drives - Update firmware** e **Volumes - Open** sono disabilitati e al momento non supportati.