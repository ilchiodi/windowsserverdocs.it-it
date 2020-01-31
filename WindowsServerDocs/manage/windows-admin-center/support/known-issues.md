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
ms.openlocfilehash: 4a91d09d6824795a21a9a7cdc7695c407aa70756
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822704"
---
# <a name="windows-admin-center-known-issues"></a>Problemi noti di Windows Admin Center

> Si applica a: Windows Admin Center, Windows Admin Center Preview

Se si verifica un problema non descritto in questa pagina, [faccelo sapere](https://aka.ms/WACfeedback).

## <a name="installer"></a>Programma di installazione

- Durante l'installazione di Windows Admin Center utilizzando il proprio certificato, occorre tenere presente che se si copia l'identificazione personale dallo strumento MMC Gestione certificati, [contiene un carattere non valido all'inizio.](https://support.microsoft.com/help/2023835/certificate-thumbprint-displayed-in-mmc-certificate-snap-in-has-extra) In alternativa, digita il primo carattere dell'identificazione personale, quindi copia e incolla il resto.

- L'uso della porta inferiore a 1024 non è supportato. In modalità servizio è eventualmente possibile configurare la porta 80 per il reindirizzamento alla porta specificata.

## <a name="general"></a>Generale

- Se l'interfaccia di amministrazione di Windows è installata come gateway in **Windows Server 2016** con un utilizzo intensivo, il servizio potrebbe arrestarsi in modo anomalo con un errore nel registro eventi che contiene ```Faulting application name: sme.exe``` e ```Faulting module name: WsmSvc.dll```. Ciò è dovuto a un bug che è stato risolto in Windows Server 2019. La patch per Windows Server 2016 includeva l'aggiornamento cumulativo di febbraio 2019, [KB4480977](https://www.catalog.update.microsoft.com/Search.aspx?q=4480977).

- Se il centro di amministrazione di Windows è installato come gateway e l'elenco di connessioni risulta danneggiato, seguire questa procedura:

   > [!WARNING]
   >Questa operazione eliminerà l'elenco di connessioni e le impostazioni per tutti gli utenti di Windows Admin Center sul gateway.

  1. Disinstallare Windows Admin Center
  2. Eliminare la cartella **Server Management Experience** in **C:\Windows\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft**
  3. Reinstallare Windows Admin Center

- Se lasci lo strumento aperto e inattivo per un lungo periodo di tempo, potresti ottenere diversi errori del tipo **Error: The runspace state is not valid for this operation**. In tal caso, aggiorna il browser. Se si verifica questo problema, [inviare commenti e suggerimenti](https://aka.ms/WACfeedback).

- Potrebbe esserci una varianza secondaria tra i numeri di versione di OSS in esecuzione nei moduli dell'interfaccia di amministrazione di Windows e ciò che è elencato nell'avviso software di terze parti.

### <a name="extension-manager"></a>Gestione estensioni

- Quando si aggiorna l'interfaccia di amministrazione di Windows, è necessario reinstallare le estensioni.
- Se si aggiunge un feed di estensione non accessibile, non viene visualizzato alcun avviso. [14412861]

## <a name="browser-specific-issues"></a>Problemi specifici del browser

### <a name="microsoft-edge"></a>Microsoft Edge

- Se l'interfaccia di amministrazione di Windows è distribuita come servizio e si usa Microsoft Edge come browser, la connessione del gateway ad Azure potrebbe non riuscire dopo la generazione di una nuova finestra del browser. Provare a risolvere questo problema aggiungendo https://login.microsoftonline.com , https://login.live.com e l'URL del gateway come siti attendibili e siti consentiti per le impostazioni di blocco popup sul browser lato client. Per ulteriori informazioni sulla correzione di questo problema, fare [riferimento alla guida alla risoluzione dei problemi](troubleshooting.md#azure-features-dont-work-properly-in-edge). [17990376]

### <a name="google-chrome"></a>Google Chrome

- Prima della versione 70 (rilasciata nel tardo ottobre 2018) Chrome presentava un [bug](https://bugs.chromium.org/p/chromium/issues/detail?id=423609) relativo al protocollo WebSockets e all'autenticazione NTLM. Questo interessa i seguenti strumenti: Eventi, PowerShell, Desktop remoto.

- Chrome potrebbe visualizzare diverse richieste di credenziali, soprattutto durante l'esperienza di aggiunta di una connessione in un ambiente **gruppo di lavoro** (non di dominio).

- Se l'interfaccia di amministrazione di Windows è distribuita come servizio, i popup dall'URL del gateway devono essere abilitati per il funzionamento di qualsiasi funzionalità di integrazione di Azure.

### <a name="mozilla-firefox"></a>Mozilla Firefox

Windows Admin Center non è stato testato con Mozilla Firefox, ma la maggior parte delle funzionalità dovrebbe funzionare.

- Installazione di Windows 10: Mozilla Firefox ha il proprio archivio certificati, quindi è necessario importare il certificato ```Windows Admin Center Client``` in Firefox per usare l'interfaccia di amministrazione di Windows in Windows 10.

## <a name="websocket-compatibility-when-using-a-proxy-service"></a>Compatibilità di WebSocket quando si usa un servizio proxy

I moduli Remote Desktop, PowerShell ed Eventi in Windows Admin Center utilizzano il protocollo WebSocket, spesso non supportato quando si utilizza un servizio proxy. Il supporto per WebSocket nella compatibilità proxy dell'App Azure AD è disponibile nell'[anteprima](https://blogs.technet.microsoft.com/applicationproxyblog/2018/03/28/limited-websocket-support-now-in-public-preview/) o cercando il feedback sulla compatibilità.

## <a name="support-for-windows-server-versions-before-2016-2012-r2-2012-2008-r2"></a>Supporto per le versioni di Windows Server precedenti alla 2016 (2012 R2, 2012, 2008 R2)

> [!NOTE]
> L'interfaccia di amministrazione di Windows richiede le funzionalità di PowerShell che non sono incluse in Windows Server 2012 R2, 2012 o 2008 R2. Se si intende gestire Windows Server con l'interfaccia di amministrazione di Windows, sarà necessario installare WMF versione 5,1 o successiva in questi server.

Digita `$PSVersiontable` in PowerShell per verificare che WMF 5.1 o versione successiva sia installato.

Se non è installato, puoi [scaricare e installare WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616).

## <a name="role-based-access-control-rbac"></a>Controllo degli accessi in base al ruolo (RBAC)

- La distribuzione di Controllo degli accessi in base al ruolo non verrà completata sui computer configurati per utilizzare Controllo di applicazioni di Windows Defender (WDAC, in precedenza noto come integrità del codice) [16568455]

- Per utilizzare Controllo degli accessi in base al ruolo in un cluster, devi distribuire la configurazione per ogni nodo del membro singolarmente.

- Quando Controllo degli accessi in base al ruolo viene distribuito, potresti ottenere errori non autorizzati erroneamente attribuiti alla configurazione di Controllo degli accessi in base al ruolo. [16369238]

## <a name="server-manager-solution"></a>Soluzione Server Manager

### <a name="certificates"></a>Certificati

- Impossibile importare il certificato crittografato .PFX nell'archivio utente corrente. [11818622]

### <a name="events"></a>Eventi

- Gli eventi sono interessati dalla [compatibilità websocket quando si utilizza un servizio proxy.](#websocket-compatibility-when-using-a-proxy-service)

- Potresti ottenere un errore che fa riferimento alle "dimensione del pacchetto" durante l'esportazione di file di registro di grandi dimensioni.

  - Per risolvere questo problema, usare il comando seguente in un prompt dei comandi con privilegi elevati nel computer del gateway: ```winrm set winrm/config @{MaxEnvelopeSizekb="8192"}```

### <a name="files"></a>File

- Il caricamento o download di file di grandi dimensioni non è ancora supportato. (limite di\~MB) [12524234]

### <a name="powershell"></a>PowerShell

- PowerShell è interessato dalla [compatibilità websocket quando si utilizza un servizio proxy](#websocket-compatibility-when-using-a-proxy-service)

- L'operazione Incolla facendo clic una sola volta con il pulsante destro del mouse nella console PowerShell desktop non funziona. In realtà verrà visualizzato il menu contestuale del browser, in cui è possibile selezionare Incolla. È possibile utilizzare anche CTRL+V.

- Il comando CTRL+C per copiare non funziona, in quanto invia sempre il comando di interruzione CTRL+C alla console. Il comando Copia del menu contestuale visualizzato facendo clic con il pulsante destro del mouse funziona.

- Quando riduci le dimensioni della finestra di Windows Admin Center, verrà effettuato l'adattamento dinamico del contenuto, ma quando viene ingrandita di nuovo, il contenuto potrebbe non tornare allo stato precedente. Se il contenuto diventa confuso, puoi provare il comando Clear-Host o disconnetterti e riconnetterti utilizzando il pulsante sopra il terminale.

### <a name="registry-editor"></a>Editor del Registro di sistema

- La funzionalità di ricerca non è implementata. [13820009]

### <a name="remote-desktop"></a>Desktop remoto

- Quando l'interfaccia di amministrazione di Windows viene distribuita come servizio, il Desktop remoto strumento potrebbe non riuscire a caricare dopo l'aggiornamento del servizio centro di amministrazione di Windows a una nuova versione. Per ovviare a questo problema, cancellare la cache del browser.   [23824194]

- Lo strumento Desktop remoto potrebbe non riuscire a connettersi durante la gestione di Windows Server 2012. [20258278]

- Quando si usa il Desktop remoto per connettersi a un computer che non è aggiunto a un dominio, è necessario immettere l'account nel formato ```MACHINENAME\USERNAME```.

- Alcune configurazioni possono bloccare il client desktop remoto del centro di amministrazione di Windows con criteri di gruppo. Se si verifica questo problema, abilitare ```Allow users to connect remotely by using Remote Desktop Services``` in ```Computer Configuration/Policies/Administrative Templates/Windows Components/Remote Desktop Services/Remote Desktop Session Host/Connections```

- Desktop remoto viene applicata la [compatibilità di WebSocket.](#websocket-compatibility-when-using-a-proxy-service)

- Lo strumento Desktop remoto non supporta attualmente alcuna operazione di copia/incolla di testo, immagini o file tra il desktop locale e la sessione remota.

- Per eseguire l'operazione di copia/ incolla all'interno della sessione remota, puoi copiare normalmente (fai clic con il pulsante destro + copia o CTRL+C), ma per incollare devi fare clic con il pulsante destro + incollare (CTRL+V NON funziona)

- Non è possibile inviare i seguenti comandi chiave alla sessione remota
  - ALT+TAB
  - Tasti funzione
  - Tasto Windows
  - STAMP

### <a name="roles-and-features"></a>Ruoli e funzionalità

- Quando si selezionano ruoli o funzionalità con origini non disponibili per l'installazione, vengono ignorati. [12946914]

- Se scegli di non riavviare automaticamente dopo l'installazione del ruolo, la richiesta non verrà visualizzata nuovamente. [13098852]

- Se scegli il riavvio automatico, questo si verificherà prima che lo stato venga aggiornato al 100%. [13098852]

### <a name="storage"></a>Archiviazione:

- Livello inferiore: le unità CD/DVD/Floppy non vengono visualizzate come volumi di livello inferiore.

- Livello inferiore: alcune proprietà in dischi e volumi non sono disponibili al livello inferiore in modo che vengano visualizzate come sconosciute o vuote nel riquadro dei dettagli.

- Livello inferiore: durante la creazione di un nuovo volume, ReFS supporta solo una dimensione unità di allocazione di 64 KB sui computer Windows 2012 e 2012 R2. Se si crea un volume ReFS con una dimensione di unità di allocazione più piccola sulle relative destinazioni di livello inferiore, la formattazione del file system non riesce. Il nuovo volume non sarà utilizzabile. La soluzione consiste nell'eliminare il volume e utilizzare una dimensione dell'unità di allocazione di 64 KB.

### <a name="updates"></a>Aggiornamenti

- Dopo l'installazione degli aggiornamenti, lo stato dell'installazione potrebbe essere memorizzato nella cache e richiedere un aggiornamento del browser.

- È possibile che si verifichi l'errore: "keyset non esiste" durante il tentativo di configurare la gestione degli aggiornamenti di Azure. In questo caso, provare a eseguire la procedura di correzione seguente nel nodo gestito-
    1. Arrestare il servizio "servizi di crittografia".
    2. Modificare le opzioni della cartella per visualizzare i file nascosti (se necessario).
    3. Si è arrivati alla cartella "%allusersprofile%\Microsoft\Crypto\RSA\S-1-5-18" ed è stato eliminato tutto il contenuto.
    4. Riavviare il servizio "servizi di crittografia".
    5. Ripetere la configurazione di Gestione aggiornamenti con l'interfaccia di amministrazione di Windows

### <a name="virtual-machines"></a>Macchine virtuali

- Quando si gestiscono le macchine virtuali in un host Windows Server 2012, lo strumento di connessione della macchina virtuale nel browser non riesce a connettersi alla macchina virtuale. Il download del file con estensione RDP per connettersi alla macchina virtuale dovrebbe continuare a funzionare. [20258278]

- Azure Site Recovery: se ASR è configurato nell'host esterno a WAC, non sarà possibile proteggere una macchina virtuale dall'interno di WAC [18972276]

- La funzionalità avanzate disponibili in Hyper-V Manager, ad esempio Gestione SAN virtuale, Sposta macchina virtuale, Esporta macchina virtuale, Replica macchina virtuale non sono attualmente supportate.

### <a name="virtual-switches"></a>Switch virtuali

- Switch Embedded Teaming (SET): durante l'aggiunta di schede di rete a un team, devono essere nella stessa subnet.

## <a name="computer-management-solution"></a>Soluzione di Gestione computer

La soluzione Gestione computer contiene un sottoinsieme degli strumenti della soluzione Gestione server, pertanto si applicano gli stessi problemi noti, nonché i seguenti problemi specifici della soluzione Gestione computer:

- Se si usa un account Microsoft ([MSA](https://account.microsoft.com/account/)) o si usa Azure Active Directory (AAD) per accedere al computer Windows 10, è necessario usare "Manage-As" per fornire le credenziali per un account amministratore locale [16568455]

- Quando provi a gestire il localhost, ti verrà richiesto di elevare il processo di gateway. Se si fa clic su **No** nella finestra popup controllo dell'account utente che segue, è necessario annullare il tentativo di connessione e ricominciare.

- Per impostazione predefinita, Windows 10 non dispone di comunicazione remota WinRM/PowerShell.
  
  - Per abilitare la gestione di Windows 10 Client, devi eseguire il comando ```Enable-PSRemoting``` da un prompt dei comandi di PowerShell con privilegi elevati.

  - Potrebbe essere necessario anche aggiornare il firewall per consentire le connessioni dall'esterno della subnet locale con ```Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any```. Per gli scenari di reti più restrittivi, fai riferimento a [questa documentazione](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-5.1).

## <a name="failover-cluster-manager-solution"></a>Soluzione Gestione cluster di failover

- Durante la gestione di un cluster (iperconvergente o tradizionale) potrebbe verificarsi un errore del tipo **shell non trovata**. In questo caso ricarica il browser o esci per accedere a un altro strumento e torna indietro. [13882442]

- Un problema può verificarsi durante la gestione di un cluster di livello inferiore (Windows Server 2012 o 2012 R2) che non è stato configurato completamente. La soluzione di questo problema consiste nel verificare che la funzionalità di Windows **RSAT-Clustering-PowerShell** sia stata installata e attivata su **ogni nodo del membro** del cluster. Per ottenere questo risultato in PowerShell, digita il comando `Install-WindowsFeature -Name RSAT-Clustering-PowerShell` in tutti i nodi del cluster. [12524664]

- Potrebbe essere necessario aggiungere il cluster con l'intero nome di dominio completo per essere individuato correttamente.

- Durante la connessione a un cluster tramite Windows Admin Center installato come gateway e fornendo un nome utente e una password espliciti per l'autenticazione, devi selezionare **Usa queste credenziali per tutte le connessioni** in modo che le credenziali siano disponibili per la query ai nodi del membro.

## <a name="hyper-converged-cluster-manager-solution"></a>Soluzione Hyper-Converged Cluster Manager

- Alcuni comandi, ad esempio **Drives - Update firmware**, **Drives - Update firmware** e **Volumes - Open** sono disabilitati e al momento non supportati.

## <a name="azure-services"></a>Servizi di Azure

### <a name="azure-file-sync-permissions"></a>Autorizzazioni Sincronizzazione file di Azure

Sincronizzazione file di Azure richiede le autorizzazioni in Azure che l'interfaccia di amministrazione di Windows non ha fornito prima della versione 1910. Se il gateway dell'interfaccia di amministrazione di Windows è stato registrato con Azure usando una versione precedente alla versione 1910 dell'interfaccia di amministrazione di Windows, sarà necessario aggiornare l'applicazione Azure Active Directory per ottenere le autorizzazioni corrette per usare Sincronizzazione file di Azure nella versione più recente di Interfaccia di amministrazione di Windows. L'autorizzazione aggiuntiva consente Sincronizzazione file di Azure di eseguire la configurazione automatica dell'accesso all'account di archiviazione, come descritto in questo articolo: [assicurarsi che sincronizzazione file di Azure abbia accesso all'account di archiviazione](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#tabpanel_CeZOj-G++Q-5_azure-portal).

Per aggiornare l'app Azure Active Directory, è possibile eseguire una delle due operazioni seguenti:
1. Passare a **impostazioni** > **Azure** > **annullare la registrazione**, quindi registrare l'interfaccia di amministrazione di Windows con Azure, assicurandosi di scegliere di creare una nuova applicazione Azure Active Directory. 
2. Passare all'applicazione Azure Active Directory e aggiungere manualmente l'autorizzazione necessaria per l'app Azure Active Directory esistente registrata nell'interfaccia di amministrazione di Windows. A tale scopo, passare a **impostazioni** >  Visualizzazione > **di Azure in Azure**. Dal pannello **registrazione app** in Azure passare a **autorizzazioni API**e selezionare **Aggiungi un'autorizzazione**. Scorrere verso il basso fino a selezionare **Azure Active Directory grafico**, selezionare **autorizzazioni delegate**, espandere **directory**e selezionare **Directory. AccessAsUser. All**. Fare clic su **Aggiungi autorizzazioni** per salvare gli aggiornamenti nell'app.

### <a name="options-for-setting-up-azure-management-services"></a>Opzioni per la configurazione dei servizi di gestione di Azure

I servizi di gestione di Azure, tra cui monitoraggio di Azure, Azure Gestione aggiornamenti e il Centro sicurezza di Azure, usano lo stesso agente per un server locale: il Microsoft Monitoring Agent. Azure Gestione aggiornamenti dispone di un set più limitato di aree supportate e richiede che l'area di lavoro Log Analytics sia collegata a un account di automazione di Azure. A causa di questa limitazione, se si vuole configurare più servizi nell'interfaccia di amministrazione di Windows, è necessario configurare prima Azure Gestione aggiornamenti, quindi il Centro sicurezza di Azure o il monitoraggio di Azure. Se sono stati configurati tutti i servizi di gestione di Azure che usano il Microsoft Monitoring Agent e quindi si prova a configurare Azure Gestione aggiornamenti usando l'interfaccia di amministrazione di Windows, l'interfaccia di amministrazione di Windows consentirà solo di configurare Azure Gestione aggiornamenti se il le risorse collegate al Microsoft Monitoring Agent supportano Gestione aggiornamenti di Azure. In caso contrario, sono disponibili due opzioni:

1. Passare al pannello di controllo > Microsoft Monitoring Agent per [disconnettere il server dalle soluzioni di gestione di Azure esistenti](https://docs.microsoft.com/azure/azure-monitor/platform/log-faq#q-how-do-i-stop-an-agent-from-communicating-with-log-analytics) , ad esempio monitoraggio di Azure o il Centro sicurezza di Azure. Configurare quindi Gestione aggiornamenti di Azure nell'interfaccia di amministrazione di Windows. Successivamente, è possibile tornare indietro per configurare le altre soluzioni di gestione di Azure tramite l'interfaccia di amministrazione di Windows senza problemi.
2. È possibile [configurare manualmente le risorse di Azure necessarie per gestione aggiornamenti di Azure](https://docs.microsoft.com/azure/automation/automation-update-management) e quindi [aggiornare manualmente il Microsoft Monitoring Agent](https://docs.microsoft.com/azure/azure-monitor/platform/agent-manage#adding-or-removing-a-workspace) (al di fuori dell'interfaccia di amministrazione di Windows) per aggiungere la nuova area di lavoro corrispondente alla soluzione Gestione aggiornamenti che si vuole usare.
