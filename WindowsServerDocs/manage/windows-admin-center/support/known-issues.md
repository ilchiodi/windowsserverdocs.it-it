---
title: Problemi noti di Windows Admin Center
description: Problemi noti di Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 06/07/2019
ms.openlocfilehash: a579d0274ff4b53a72c17760a6d53ef796625d3a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356906"
---
# <a name="windows-admin-center-known-issues"></a>Problemi noti di Windows Admin Center

> Si applica a: Windows Admin Center, Windows Admin Center Preview

Se si verifica un problema non descritto in questa pagina, [faccelo sapere](http://aka.ms/WACfeedback).

## <a name="lenovo-xclarity-integrator"></a>Lenovo XClarity Integrator

Il problema di incompatibilità precedentemente divulgato dell'estensione Lenovo XClarity Integrator e la versione 1904 dell'interfaccia di amministrazione di Windows è ora risolto con Windows Admin Center versione 1904,1. Si consiglia vivamente di eseguire l'aggiornamento alla versione supportata più recente dell'interfaccia di amministrazione di Windows.

- Lenovo XClarity Integrator Extension versione 1,1 è completamente compatibile con l'interfaccia di amministrazione di Windows 1904,1. Si consiglia vivamente di eseguire l'aggiornamento alla versione più recente dell'interfaccia di amministrazione di Windows e dell'estensione Lenovo.
- Per qualsiasi motivo, se è necessario continuare a usare l'interfaccia di amministrazione di Windows 1809,5 per il momento, è possibile usare XClarity Integrator 1.0.4, che sarà anche disponibile nel feed dell'estensione dell'interfaccia di amministrazione di Windows fino a quando l'interfaccia di amministrazione di Windows 1809,5 non è più supportata in base a [criteri di supporto](../support/index.md).

## <a name="installer"></a>Programma di installazione

- Durante l'installazione di Windows Admin Center utilizzando il proprio certificato, occorre tenere presente che se si copia l'identificazione personale dallo strumento MMC Gestione certificati, [contiene un carattere non valido all'inizio.](https://support.microsoft.com/help/2023835/certificate-thumbprint-displayed-in-mmc-certificate-snap-in-has-extra) In alternativa, digita il primo carattere dell'identificazione personale, quindi copia e incolla il resto.

- L'uso della porta inferiore a 1024 non è supportato. In modalità servizio è eventualmente possibile configurare la porta 80 per il reindirizzamento alla porta specificata.

- Se il servizio Windows Update (wuauserv) viene arrestato e disabilitato, il programma di installazione avrà esito negativo. [19100629]

### <a name="upgrade"></a>Aggiornamento

- Quando si aggiorna l'interfaccia di amministrazione di Windows in modalità servizio da una versione precedente, se si usa msiexec in modalità non interattiva, è possibile che si verifichi un problema a causa del quale viene eliminata la regola del firewall in ingresso per la porta centro di amministrazione di Windows.
  - Per ricreare la regola, eseguire il comando seguente da una console di PowerShell con privilegi \<elevati, sostituendo la porta > con la porta configurata per l'interfaccia di amministrazione di Windows (valore predefinito 443).

    ```powershell
    New-NetFirewallRule -DisplayName "SmeInboundOpenException" -Description "Windows Admin Center inbound port exception" -LocalPort <port> -RemoteAddress Any -Protocol TCP
    ```

## <a name="general"></a>Generale

- Se l'interfaccia di amministrazione di Windows è installata come gateway in **Windows Server 2016** con un utilizzo intensivo, il servizio potrebbe arrestarsi in modo anomalo con ```Faulting application name: sme.exe``` un ```Faulting module name: WsmSvc.dll```errore nel registro eventi che contiene e. Ciò è dovuto a un bug che è stato risolto in Windows Server 2019. La patch per Windows Server 2016 includeva l'aggiornamento cumulativo di febbraio 2019, [KB4480977](https://www.catalog.update.microsoft.com/Search.aspx?q=4480977).

- Se il centro di amministrazione di Windows è installato come gateway e l'elenco di connessioni risulta danneggiato, seguire questa procedura:

   > [!WARNING]
   >Questa operazione eliminerà l'elenco di connessioni e le impostazioni per tutti gli utenti di Windows Admin Center sul gateway.

  1. Disinstallare Windows Admin Center
  2. Eliminare la cartella **Server Management Experience** in **C:\Windows\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft**
  3. Reinstallare Windows Admin Center

- Se si lascia lo strumento aperto e inattivo per un lungo periodo di tempo, è possibile che **si verifichino diversi errori: Lo stato di spazio non è valido per gli** errori di questa operazione. In tal caso, aggiorna il browser. Se si verifica questo problema, [inviare commenti e suggerimenti](http://aka.ms/WACfeedback).

- Potresti riscontrare un **errore 500** durante l'aggiornamento di pagine con URL molto lunghi. [12443710]

- In alcuni strumenti, il controllo ortografico del browser può contrassegnare alcuni valori di campo come errati. [12425477]

- In alcuni strumenti, i pulsanti di comando potrebbero non riflettere le modifiche allo stato subito dopo aver fatto clic e l'interfaccia utente dello strumento potrebbe non riflettere automaticamente le modifiche a proprietà specifiche. Puoi fare clic su **Aggiorna** per recuperare lo stato più recente dal server di destinazione. [11445790]

- Filtraggio dei tag nell'elenco di connessioni: se si selezionano le connessioni usando le caselle di controllo MultiSelect, quindi si filtra l'elenco di connessioni in base ai tag, la selezione originale viene mantenute in modo che tutte le azioni selezionate verranno applicate a tutti i computer selezionati in precedenza. [18099259]

- Potrebbe esserci una varianza secondaria tra i numeri di versione di OSS in esecuzione nei moduli dell'interfaccia di amministrazione di Windows e ciò che è elencato nell'avviso software di terze parti.

### <a name="extension-manager"></a>Gestione estensioni

- Quando si aggiorna l'interfaccia di amministrazione di Windows, è necessario reinstallare le estensioni.
- Se si aggiunge un feed di estensione non accessibile, non viene visualizzato alcun avviso. [14412861]

## <a name="browser-specific-issues"></a>Problemi specifici del browser

### <a name="microsoft-edge"></a>Microsoft Edge

- In alcuni casi, possono verificarsi tempi di caricamento lungo quando si utilizza Microsoft Edge per accedere a un gateway Windows Admin Center tramite Internet. Ciò può verificarsi nelle macchine virtuali di Azure in cui il gateway di Windows Admin Center utilizza un certificato autofirmato. [13819912]

- Durante l'utilizzo di Azure Active Directory come provider di identità e Windows Admin Center è configurato con un certificato autofirmato o di altro tipo non attendibile, non puoi completare l'autenticazione AAD in Microsoft Edge.  [15968377]

- Se l'interfaccia di amministrazione di Windows è distribuita come servizio e si usa Microsoft Edge come browser, la connessione del gateway ad Azure potrebbe non riuscire dopo la generazione di una nuova finestra del browser. Provare a risolvere questo problema aggiungendo https://login.microsoftonline.com , https://login.live.com e l'URL del gateway come siti attendibili e siti consentiti per le impostazioni di blocco popup sul browser lato client. Per ulteriori informazioni sulla correzione di questo problema, fare [riferimento alla guida alla risoluzione dei problemi](troubleshooting.md#azure-features-dont-work-properly-in-edge). [17990376]

- Se l'interfaccia di amministrazione di Windows è installata in modalità desktop, nella scheda Esplorazione di Microsoft Edge non verrà visualizzata la favicon. [17665801]

### <a name="google-chrome"></a>Google Chrome

- Prima della versione 70 (rilasciata nel tardo ottobre 2018) Chrome presentava un [bug](https://bugs.chromium.org/p/chromium/issues/detail?id=423609) relativo al protocollo WebSockets e all'autenticazione NTLM. Questa operazione ha effetto sugli strumenti seguenti: Eventi, PowerShell, Desktop remoto.

- Chrome potrebbe visualizzare diverse richieste di credenziali, soprattutto durante l'esperienza di aggiunta di una connessione in un ambiente **gruppo di lavoro** (non di dominio).

- Se l'interfaccia di amministrazione di Windows è distribuita come servizio, i popup dall'URL del gateway devono essere abilitati per il funzionamento di qualsiasi funzionalità di integrazione di Azure. Questi servizi includono la scheda di rete di Azure, Gestione aggiornamenti di Azure e Azure Site Recovery.

### <a name="mozilla-firefox"></a>Mozilla Firefox

Windows Admin Center non è stato testato con Mozilla Firefox, ma la maggior parte delle funzionalità dovrebbe funzionare.

- Installazione di Windows 10: Mozilla Firefox ha il proprio archivio certificati, quindi è necessario importare il certificato ```Windows Admin Center Client``` in Firefox per usare l'interfaccia di amministrazione di Windows in Windows 10.

## <a name="websocket-compatibility-when-using-a-proxy-service"></a>Compatibilità di WebSocket quando si usa un servizio proxy

I moduli Remote Desktop, PowerShell ed Eventi in Windows Admin Center utilizzano il protocollo WebSocket, spesso non supportato quando si utilizza un servizio proxy. Il supporto per WebSocket nella compatibilità proxy dell'App Azure AD è disponibile nell'[anteprima](https://blogs.technet.microsoft.com/applicationproxyblog/2018/03/28/limited-websocket-support-now-in-public-preview/) o cercando il feedback sulla compatibilità.

## <a name="support-for-windows-server-versions-before-2016-2012-r2-2012-2008-r2"></a>Supporto per le versioni di Windows Server precedenti alla 2016 (2012 R2, 2012, 2008 R2)

> [!NOTE]
> L'interfaccia di amministrazione di Windows richiede le funzionalità di PowerShell che non sono incluse in Windows Server 2012 R2, 2012 o 2008 R2. Se si intende gestire Windows Server con l'interfaccia di amministrazione di Windows, sarà necessario installare WMF versione 5,1 o successiva in questi server.

Digita `$PSVersiontable` in PowerShell per verificare che WMF 5.1 o versione successiva sia installato.

Se non è installato, puoi [scaricare e installare WMF 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616).

## <a name="role-based-access-control-rbac"></a>Controllo degli accessi in base al ruolo (RBAC)

- La distribuzione di Controllo degli accessi in base al ruolo non verrà completata sui computer configurati per utilizzare Controllo di applicazioni di Windows Defender (WDAC, in precedenza noto come integrità del codice) [16568455]

- Per utilizzare Controllo degli accessi in base al ruolo in un cluster, devi distribuire la configurazione per ogni nodo del membro singolarmente.

- Quando Controllo degli accessi in base al ruolo viene distribuito, potresti ottenere errori non autorizzati erroneamente attribuiti alla configurazione di Controllo degli accessi in base al ruolo. [16369238]

## <a name="server-manager-solution"></a>Soluzione Server Manager

### <a name="server-settings"></a>Impostazioni server

- Se si modifica un'impostazione, provare a uscire senza salvare, nella pagina verranno segnalate le modifiche non salvate, ma si continuerà a uscire. È possibile che si verifichi uno stato in cui la scheda Impostazioni selezionata non corrisponde al contenuto della pagina. [19905798] [19905787]

### <a name="certificates"></a>Certificati

- Impossibile importare il certificato crittografato .PFX nell'archivio utente corrente. [11818622]

### <a name="devices"></a>Dispositivi

- Quando si passa alla tabella con la tastiera, la selezione può passare alla parte superiore del gruppo di tabelle. [16646059]

### <a name="events"></a>Events

- Gli eventi sono interessati dalla [compatibilità websocket quando si utilizza un servizio proxy.](#websocket-compatibility-when-using-a-proxy-service)

- Potresti ottenere un errore che fa riferimento alle "dimensione del pacchetto" durante l'esportazione di file di registro di grandi dimensioni. [16630279]

  - Per risolvere questo problema, usare il comando seguente in un prompt dei comandi con privilegi elevati nel computer del gateway:```winrm set winrm/config @{MaxEnvelopeSizekb="8192"}```

### <a name="files"></a>File

- Il caricamento o download di file di grandi dimensioni non è ancora supportato. (limite \~100mb) [12524234]

### <a name="powershell"></a>PowerShell

- PowerShell è interessato dalla [compatibilità websocket quando si utilizza un servizio proxy](#websocket-compatibility-when-using-a-proxy-service)

- L'operazione Incolla facendo clic una sola volta con il pulsante destro del mouse nella console PowerShell desktop non funziona. In realtà verrà visualizzato il menu contestuale del browser, in cui è possibile selezionare Incolla. È possibile utilizzare anche CTRL+V.

- Il comando CTRL+C per copiare non funziona, in quanto invia sempre il comando di interruzione CTRL+C alla console. Il comando Copia del menu contestuale visualizzato facendo clic con il pulsante destro del mouse funziona.

- Quando riduci le dimensioni della finestra di Windows Admin Center, verrà effettuato l'adattamento dinamico del contenuto, ma quando viene ingrandita di nuovo, il contenuto potrebbe non tornare allo stato precedente. Se il contenuto diventa confuso, puoi provare il comando Clear-Host o disconnetterti e riconnetterti utilizzando il pulsante sopra il terminale.

### <a name="registry-editor"></a>Editor del Registro di sistema

- La funzionalità di ricerca non è implementata. [13820009]

### <a name="remote-desktop"></a>Desktop remoto

- Lo strumento Desktop remoto potrebbe non riuscire a connettersi durante la gestione di Windows Server 2012. [20258278]

- Quando si usa il desktop remoto per connettersi a un computer che non è aggiunto a un dominio, è necessario immettere l' ```MACHINENAME\USERNAME``` account nel formato.

- Alcune configurazioni possono bloccare il client desktop remoto del centro di amministrazione di Windows con criteri di gruppo. Se si verifica questo problema, ```Allow users to connect remotely by using Remote Desktop Services``` abilitare in```Computer Configuration/Policies/Administrative Templates/Windows Components/Remote Desktop Services/Remote Desktop Session Host/Connections```

- Desktop remoto viene applicata la [compatibilità di WebSocket.](#websocket-compatibility-when-using-a-proxy-service)

- Lo strumento Desktop remoto non supporta attualmente alcuna operazione di copia/incolla di testo, immagini o file tra il desktop locale e la sessione remota.

- Per eseguire l'operazione di copia/ incolla all'interno della sessione remota, puoi copiare normalmente (fai clic con il pulsante destro + copia o CTRL+C), ma per incollare devi fare clic con il pulsante destro + incollare (CTRL+V NON funziona)

- Non è possibile inviare i seguenti comandi chiave alla sessione remota
  - ALT+TAB
  - Tasti funzione
  - Tasto Windows
  - STAMP

- App remota: dopo aver abilitato lo strumento app Remote da Desktop remoto impostazioni, lo strumento potrebbe non essere visualizzato nell'elenco degli strumenti quando si gestisce un server con esperienza desktop. [18906904]

### <a name="roles-and-features"></a>Ruoli e funzionalità

- Quando si selezionano ruoli o funzionalità con origini non disponibili per l'installazione, vengono ignorati. [12946914]

- Se scegli di non riavviare automaticamente dopo l'installazione del ruolo, la richiesta non verrà visualizzata nuovamente. [13098852]

- Se scegli il riavvio automatico, questo si verificherà prima che lo stato venga aggiornato al 100%. [13098852]

### <a name="storage"></a>Archiviazione

- Il recupero delle informazioni sulle quote potrebbe non riuscire senza una notifica di errore (si verificherà comunque un errore nella console del browser) [18962274]

- Livello inferiore: Le unità DVD/CD/floppy non vengono visualizzate come volumi in un livello inferiore.

- Livello inferiore: Alcune proprietà in volumi e dischi non sono disponibili a livello inferiore, quindi appaiono sconosciute o vuote nel pannello dei dettagli.

- Livello inferiore: Quando si crea un nuovo volume, ReFS supporta solo una dimensione dell'unità di allocazione di 64K nei computer Windows 2012 e 2012 R2. Se si crea un volume ReFS con una dimensione di unità di allocazione più piccola sulle relative destinazioni di livello inferiore, la formattazione del file system non riesce. Il nuovo volume non sarà utilizzabile. La soluzione consiste nell'eliminare il volume e utilizzare una dimensione dell'unità di allocazione di 64 KB.

### <a name="updates"></a>Aggiornamenti

- Dopo l'installazione degli aggiornamenti, lo stato dell'installazione potrebbe essere memorizzato nella cache e richiedere un aggiornamento del browser.

- È possibile che si verifichi l'errore: "Keyset non esiste" durante il tentativo di configurare la gestione degli aggiornamenti di Azure. In questo caso, provare a eseguire la procedura di correzione seguente nel nodo gestito-
    1. Arrestare il servizio "servizi di crittografia".
    2. Modificare le opzioni della cartella per visualizzare i file nascosti (se necessario).
    3. Si è arrivati alla cartella "%allusersprofile%\Microsoft\Crypto\RSA\S-1-5-18" ed è stato eliminato tutto il contenuto.
    4. Riavviare il servizio "servizi di crittografia".
    5. Ripetere la configurazione di Gestione aggiornamenti con l'interfaccia di amministrazione di Windows

### <a name="virtual-machines"></a>Macchine virtuali

- Quando si gestiscono le macchine virtuali in un host Windows Server 2012, lo strumento di connessione della macchina virtuale nel browser non riesce a connettersi alla macchina virtuale. Il download del file con estensione RDP per connettersi alla macchina virtuale dovrebbe continuare a funzionare. [20258278]

- Azure Site Recovery: se ASR è configurato nell'host esterno a WAC, non sarà possibile proteggere una macchina virtuale dall'interno di WAC [18972276]

- La funzionalità avanzate disponibili in Hyper-V Manager, ad esempio Gestione SAN virtuale, Sposta macchina virtuale, Esporta macchina virtuale, Replica macchina virtuale non sono attualmente supportate.

### <a name="virtual-switches"></a>Commutatori virtuali

- Switch Embedded Teaming (SET): Quando si aggiungono NIC a un team, è necessario che si trovino nella stessa subnet.

## <a name="computer-management-solution"></a>Soluzione di Gestione computer

La soluzione Gestione computer contiene un sottoinsieme degli strumenti della soluzione Gestione server, pertanto si applicano gli stessi problemi noti, nonché i seguenti problemi specifici della soluzione Gestione computer:

- Se si usa un account Microsoft ([MSA](https://account.microsoft.com/account/)) o si usa Azure Active Directory (AAD) per accedere al computer Windows 10, è necessario specificare le credenziali "Gestisci come" per gestire il computer locale [16568455]

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