---
title: Problemi noti dell'interfaccia di amministrazione di Windows
description: Problemi noti dell'interfaccia di amministrazione di Windows (progetto Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 06/07/2019
ms.openlocfilehash: d7dc3455c5d7c6b00940008ceea646436b40bed0
ms.sourcegitcommit: e51dd9dabec82c59e805e7a04c27e56c83773857
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/30/2020
ms.locfileid: "82613733"
---
# <a name="windows-admin-center-known-issues"></a>Problemi noti dell'interfaccia di amministrazione di Windows

> Si applica a: Windows Admin Center, Windows Admin Center Preview

Se si verifica un problema non descritto in questa pagina, è [possibile segnalarlo.](https://aka.ms/WACfeedback)

## <a name="installer"></a>Programma di installazione

- Quando si installa l'interfaccia di amministrazione di Windows con il proprio certificato, tenere presente che, se si copia l'identificazione personale dallo strumento MMC di gestione certificati, quest'area conterrà [un carattere non valido all'inizio.](https://support.microsoft.com/help/2023835/certificate-thumbprint-displayed-in-mmc-certificate-snap-in-has-extra) Come soluzione alternativa, digitare il primo carattere dell'identificazione digitale e copiare e incollare il resto.

- L'uso della porta inferiore a 1024 non è supportato. In modalità servizio è eventualmente possibile configurare la porta 80 per il reindirizzamento alla porta specificata.

## <a name="general"></a>Generale

- Nella versione 1910,2 dell'interfaccia di amministrazione di Windows, potrebbe non essere possibile connettersi ai server Hyper-V su hardware specifico. Se il problema persiste, [scaricare la compilazione precedente](https://aka.ms/wacprevious). 

- Se l'interfaccia di amministrazione di Windows è installata come gateway in **Windows Server 2016** con un utilizzo intensivo, il servizio potrebbe arrestarsi in modo anomalo con ```Faulting application name: sme.exe``` un ```Faulting module name: WsmSvc.dll```errore nel registro eventi che contiene e. Ciò è dovuto a un bug che è stato risolto in Windows Server 2019. La patch per Windows Server 2016 includeva l'aggiornamento cumulativo di febbraio 2019, [KB4480977](https://www.catalog.update.microsoft.com/Search.aspx?q=4480977).

- Se il centro di amministrazione di Windows è installato come gateway e l'elenco di connessioni risulta danneggiato, seguire questa procedura:

   > [!WARNING]
   >Questa operazione eliminerà l'elenco di connessioni e le impostazioni per tutti gli utenti di Windows Admin Center sul gateway.

  1. Disinstalla interfaccia di amministrazione di Windows
  2. Eliminare la cartella **esperienza di gestione server** in **C:\Windows\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft**
  3. Reinstallare l'interfaccia di amministrazione di Windows

- Se lo strumento viene lasciato aperto e inattivo per un lungo periodo di tempo, è possibile che si verifichino diversi errori **: lo stato di spazio non è valido per questa operazione** . In tal caso, aggiornare il browser. Se si verifica questo problema, [inviare commenti e suggerimenti](https://aka.ms/WACfeedback).

- Potrebbe esserci una varianza secondaria tra i numeri di versione di OSS in esecuzione nei moduli dell'interfaccia di amministrazione di Windows e ciò che è elencato nell'avviso software di terze parti.

### <a name="extension-manager"></a>Gestione estensioni

- Quando si aggiorna l'interfaccia di amministrazione di Windows, è necessario reinstallare le estensioni.
- Se si aggiunge un feed di estensione non accessibile, non viene visualizzato alcun avviso. [14412861]

## <a name="browser-specific-issues"></a>Problemi specifici del browser

### <a name="microsoft-edge"></a>Microsoft Edge

- Se l'interfaccia di amministrazione di Windows è distribuita come servizio e si usa Microsoft Edge come browser, la connessione del gateway ad Azure potrebbe non riuscire dopo la generazione di una nuova finestra del browser. Provare a risolvere questo problema aggiungendo https://login.microsoftonline.com, https://login.live.come l'URL del gateway come siti attendibili e siti consentiti per le impostazioni di blocco popup sul browser lato client. Per ulteriori informazioni sulla correzione di questo problema, fare [riferimento alla guida alla risoluzione dei problemi](troubleshooting.md#azure-features-dont-work-properly-in-edge). [17990376]

### <a name="google-chrome"></a>Google Chrome

- Prima della versione 70 (rilasciata nel tardo ottobre 2018) Chrome presentava un [bug](https://bugs.chromium.org/p/chromium/issues/detail?id=423609) relativo al protocollo WebSockets e all'autenticazione NTLM. Questa operazione ha effetto sui seguenti strumenti: eventi, PowerShell Desktop remoto.

- Chrome può visualizzare più richieste di credenziali, soprattutto durante l'aggiunta di un'esperienza di connessione in un ambiente di **gruppo** di lavoro (non di dominio).

- Se l'interfaccia di amministrazione di Windows è distribuita come servizio, i popup dall'URL del gateway devono essere abilitati per il funzionamento di qualsiasi funzionalità di integrazione di Azure.

### <a name="mozilla-firefox"></a>Mozilla Firefox

L'interfaccia di amministrazione di Windows non è stata testata con Mozilla Firefox, ma la maggior parte delle funzionalità dovrebbe funzionare.

- Installazione di Windows 10: Mozilla Firefox ha il proprio archivio certificati, quindi è necessario importare il ```Windows Admin Center Client``` certificato in Firefox per usare l'interfaccia di amministrazione di Windows in Windows 10.

## <a name="websocket-compatibility-when-using-a-proxy-service"></a>Compatibilità di WebSocket quando si usa un servizio proxy

I moduli Desktop remoto, PowerShell ed Events nell'interfaccia di amministrazione di Windows usano il protocollo WebSocket, che spesso non è supportato quando si usa un servizio proxy. 

## <a name="support-for-windows-server-versions-before-2016-2012-r2-2012-2008-r2"></a>Supporto per le versioni di Windows Server precedenti alla 2016 (2012 R2, 2012, 2008 R2)

> [!NOTE]
> L'interfaccia di amministrazione di Windows richiede le funzionalità di PowerShell che non sono incluse in Windows Server 2012 R2, 2012 o 2008 R2. Se si intende gestire Windows Server con l'interfaccia di amministrazione di Windows, sarà necessario installare WMF versione 5,1 o successiva in questi server.

Per verificare che WMF 5.1 o versione successiva sia installato, digita `$PSVersiontable` in PowerShell.

In caso contrario, puoi [scaricare e installare WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616).

## <a name="role-based-access-control-rbac"></a>Controllo degli accessi in base al ruolo

- La distribuzione RBAC non avrà esito positivo nei computer configurati per l'uso del controllo delle applicazioni di Windows Defender (WDAC, noto in precedenza come integrità del codice). [16568455]

- Per usare il controllo degli accessi in base al ruolo in un cluster, è necessario distribuire singolarmente la configurazione in ogni nodo membro.

- Quando si distribuisce il controllo degli accessi in base al ruolo, è possibile che si verifichino errori non autorizzati erroneamente attribuiti alla configurazione RBAC. [16369238]

## <a name="server-manager-solution"></a>Soluzione Server Manager

### <a name="certificates"></a>Certificati

- Non è possibile importare. Certificato PFX crittografato nell'archivio utente corrente. [11818622]

### <a name="events"></a>Events

- Gli eventi sono effettivi sulla [compatibilità di WebSocket quando si usa un servizio proxy.](#websocket-compatibility-when-using-a-proxy-service)

- È possibile che venga ricevuto un errore che fa riferimento a "dimensioni pacchetto" durante l'esportazione di file di log di grandi dimensioni.

  - Per risolvere questo problema, usare il comando seguente in un prompt dei comandi con privilegi elevati nel computer del gateway:```winrm set winrm/config @{MaxEnvelopeSizekb="8192"}```

### <a name="files"></a>File

- Il caricamento o il download di file di grandi dimensioni non è ancora supportato. (\~limite di 100 MB) [12524234]

### <a name="powershell"></a>PowerShell

- PowerShell viene applicato dalla [compatibilità di WebSocket quando si usa un servizio proxy](#websocket-compatibility-when-using-a-proxy-service)

- Incollare con un solo clic con il pulsante destro del mouse come nella console di PowerShell per desktop non funziona. Si otterrà invece il menu di scelta rapida del browser, in cui è possibile selezionare Incolla. Anche CTRL-V funziona.

- CTRL-C per la copia non funziona, ma invierà sempre il comando CTRL-C break alla console. Viene eseguita una copia dal menu di scelta rapida con il pulsante destro del mouse.

- Quando si imposta la finestra dell'interfaccia di amministrazione di Windows su un numero inferiore, il contenuto del terminale viene riflusso, ma quando lo si fa nuovamente, il contenuto potrebbe non tornare allo stato precedente. Se si verificano confusione, è possibile provare Clear-host oppure disconnettersi e riconnettersi usando il pulsante sopra il terminale.

### <a name="registry-editor"></a>Editor del Registro di sistema

- Funzionalità di ricerca non implementata. [13820009]

### <a name="remote-desktop"></a>Desktop remoto

- Quando l'interfaccia di amministrazione di Windows viene distribuita come servizio, il Desktop remoto strumento potrebbe non riuscire a caricare dopo l'aggiornamento del servizio centro di amministrazione di Windows a una nuova versione. Per ovviare a questo problema, cancellare la cache del browser.   [23824194]

- Lo strumento Desktop remoto potrebbe non riuscire a connettersi durante la gestione di Windows Server 2012. [20258278]

- Quando si usa il Desktop remoto per connettersi a un computer che non è aggiunto a un dominio, è necessario immettere l' ```MACHINENAME\USERNAME``` account nel formato.

- Alcune configurazioni possono bloccare il client desktop remoto del centro di amministrazione di Windows con criteri di gruppo. Se si verifica questo problema, ```Allow users to connect remotely by using Remote Desktop Services``` abilitare in```Computer Configuration/Policies/Administrative Templates/Windows Components/Remote Desktop Services/Remote Desktop Session Host/Connections```

- Desktop remoto viene applicata la [compatibilità di WebSocket.](#websocket-compatibility-when-using-a-proxy-service)

- Lo strumento Desktop remoto attualmente non supporta la copia e incolla di testo, immagine o file tra il desktop locale e la sessione remota.

- Per eseguire qualsiasi operazione di copia e incolla all'interno della sessione remota, è possibile copiarla normalmente (facendo clic con il pulsante destro del mouse + copia o CTRL + C), ma è necessario fare clic con il pulsante destro del mouse e incollare (CTRL + V non funziona)

- Non è possibile inviare i comandi chiave seguenti alla sessione remota
  - ALT + TAB
  - Tasti funzione
  - Tasto Windows
  - Stamp

### <a name="roles-and-features"></a>Ruoli e funzionalità

- Quando si selezionano ruoli o funzionalità con origini non disponibili per l'installazione, queste vengono ignorate. [12946914]

- Se si sceglie di non riavviare automaticamente dopo l'installazione del ruolo, non verrà più richiesto. [13098852]

- Se si sceglie di riavviare automaticamente, il riavvio verrà eseguito prima che lo stato venga aggiornato al 100%. [13098852]

### <a name="storage"></a>Archiviazione

- Livello inferiore: le unità DVD/CD/floppy non vengono visualizzate come volumi in un livello inferiore.

- Livello inferiore: alcune proprietà nei volumi e nei dischi non sono disponibili a livello inferiore, quindi appaiono sconosciute o vuote nel pannello dei dettagli.

- Livello inferiore: quando si crea un nuovo volume, ReFS supporta solo una dimensione dell'unità di allocazione di 64K nei computer Windows 2012 e 2012 R2. Se viene creato un volume ReFS con una dimensione dell'unità di allocazione più piccola sulle destinazioni di livello inferiore, file system la formattazione avrà esito negativo. Il nuovo volume non sarà utilizzabile. La risoluzione consiste nell'eliminare il volume e utilizzare le dimensioni dell'unità di allocazione 64K.

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

- Le funzionalità avanzate disponibili nella console di gestione di Hyper-V, ad esempio Virtual SAN Manager, Sposta macchina virtuale, Esporta macchina virtuale e replica VM, non sono attualmente supportate.

### <a name="virtual-switches"></a>Commutatori virtuali

- Switch Embedded Teaming (SET): quando si aggiungono NIC a un team, è necessario che si trovino nella stessa subnet.

## <a name="computer-management-solution"></a>Soluzione Gestione computer

La soluzione Gestione computer contiene un subset degli strumenti della soluzione Server Manager, quindi si applicano gli stessi problemi noti, nonché i problemi specifici della soluzione di gestione dei computer seguenti:

- Se si usa un account Microsoft ([MSA](https://account.microsoft.com/account/)) o si usa Azure Active Directory (AAD) per accedere al computer Windows 10, è necessario usare "Manage-As" per fornire le credenziali per un account amministratore locale [16568455]

- Quando si tenta di gestire il localhost, verrà richiesto di elevare il processo del gateway. Se si fa clic su **No** nella finestra popup controllo dell'account utente che segue, è necessario annullare il tentativo di connessione e ricominciare.

- Per impostazione predefinita, Windows 10 non dispone di comunicazione remota WinRM/PowerShell.
  
  - Per abilitare la gestione del client Windows 10, è necessario eseguire il comando ```Enable-PSRemoting``` da un prompt di PowerShell con privilegi elevati.

  - Potrebbe anche essere necessario aggiornare il firewall per consentire le connessioni dall'esterno della subnet locale con ```Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any```. Per gli scenari con reti più restrittive, fare riferimento a [questa documentazione](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-5.1).

## <a name="failover-cluster-manager-solution"></a>Soluzione Gestione cluster di failover

- Quando si gestisce un cluster (iperconvergente o tradizionale?) potrebbe verificarsi un errore di **Shell non trovato** . In tal caso, è possibile ricaricare il browser o passare a un altro strumento e viceversa. [13882442]

- Un problema può verificarsi quando si gestisce un cluster di livello inferiore (Windows Server 2012 o 2012 R2) che non è stato configurato completamente. La correzione di questo problema consiste nel garantire che la funzionalità Windows- **clustering-PowerShell** sia stata installata e abilitata in **ogni nodo membro** del cluster. Per eseguire questa operazione con PowerShell, immettere il `Install-WindowsFeature -Name RSAT-Clustering-PowerShell` comando in tutti i nodi del cluster. [12524664]

- Potrebbe essere necessario aggiungere il cluster con l'intero FQDN per individuarlo correttamente.

- Quando ci si connette a un cluster usando l'interfaccia di amministrazione di Windows installato come gateway e fornendo nome utente/password esplicita per l'autenticazione, è necessario selezionare **Usa queste credenziali per tutte le connessioni** in modo che le credenziali siano disponibili per eseguire query sui nodi membro.

## <a name="hyper-converged-cluster-manager-solution"></a>Soluzione Gestione cluster iperconvergente

- Alcuni comandi, ad esempio le **unità, firmware di aggiornamento**, server, **Rimuovi** e **volumi aperti** sono disabilitati e attualmente non supportati.

## <a name="azure-services"></a>Servizi di Azure

### <a name="azure-file-sync-permissions"></a>Autorizzazioni Sincronizzazione file di Azure

Sincronizzazione file di Azure richiede le autorizzazioni in Azure che l'interfaccia di amministrazione di Windows non ha fornito prima della versione 1910. Se il gateway dell'interfaccia di amministrazione di Windows è stato registrato con Azure usando una versione precedente alla versione 1910 dell'interfaccia di amministrazione di Windows, sarà necessario aggiornare l'applicazione Azure Active Directory per ottenere le autorizzazioni corrette per usare Sincronizzazione file di Azure nella versione più recente dell'interfaccia di amministrazione di Windows. L'autorizzazione aggiuntiva consente Sincronizzazione file di Azure di eseguire la configurazione automatica dell'accesso all'account di archiviazione, come descritto in questo articolo: [assicurarsi che sincronizzazione file di Azure abbia accesso all'account di archiviazione](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#tabpanel_CeZOj-G++Q-5_azure-portal).

Per aggiornare l'app Azure Active Directory, è possibile eseguire una delle due operazioni seguenti:
1. Passare a **Impostazioni** > **Azure** > **Annulla registrazione**di Azure, quindi registrare l'interfaccia di amministrazione di Windows con Azure, assicurandosi di aver scelto di creare una nuova applicazione Azure Active Directory. 
2. Passare all'applicazione Azure Active Directory e aggiungere manualmente l'autorizzazione necessaria per l'app Azure Active Directory esistente registrata nell'interfaccia di amministrazione di Windows. A tale scopo, passare a **Impostazioni** > **Azure** > **visualizzazione di Azure in Azure**. Dal pannello **registrazione app** in Azure passare a **autorizzazioni API**e selezionare **Aggiungi un'autorizzazione**. Scorrere verso il basso fino a selezionare **Azure Active Directory grafico**, selezionare **autorizzazioni delegate**, espandere **directory**e selezionare **Directory. AccessAsUser. All**. Fare clic su **Aggiungi autorizzazioni** per salvare gli aggiornamenti nell'app.

### <a name="options-for-setting-up-azure-management-services"></a>Opzioni per la configurazione dei servizi di gestione di Azure

I servizi di gestione di Azure, tra cui monitoraggio di Azure, Azure Gestione aggiornamenti e il Centro sicurezza di Azure, usano lo stesso agente per un server locale: il Microsoft Monitoring Agent. Azure Gestione aggiornamenti dispone di un set più limitato di aree supportate e richiede che l'area di lavoro Log Analytics sia collegata a un account di automazione di Azure. A causa di questa limitazione, se si vuole configurare più servizi nell'interfaccia di amministrazione di Windows, è necessario configurare prima Azure Gestione aggiornamenti, quindi il Centro sicurezza di Azure o il monitoraggio di Azure. Se sono stati configurati tutti i servizi di gestione di Azure che usano il Microsoft Monitoring Agent e quindi si prova a configurare Azure Gestione aggiornamenti usando l'interfaccia di amministrazione di Windows, l'interfaccia di amministrazione di Windows consentirà solo di configurare Gestione aggiornamenti di Azure se le risorse esistenti collegate al Microsoft Monitoring Agent supportano Azure Gestione aggiornamenti. In caso contrario, sono disponibili due opzioni:

1. Passare al pannello di controllo > Microsoft Monitoring Agent per [disconnettere il server dalle soluzioni di gestione di Azure esistenti](https://docs.microsoft.com/azure/azure-monitor/platform/log-faq#q-how-do-i-stop-an-agent-from-communicating-with-log-analytics) , ad esempio monitoraggio di Azure o il Centro sicurezza di Azure. Configurare quindi Gestione aggiornamenti di Azure nell'interfaccia di amministrazione di Windows. Successivamente, è possibile tornare indietro per configurare le altre soluzioni di gestione di Azure tramite l'interfaccia di amministrazione di Windows senza problemi.
2. È possibile [configurare manualmente le risorse di Azure necessarie per gestione aggiornamenti di Azure](https://docs.microsoft.com/azure/automation/automation-update-management) e quindi [aggiornare manualmente il Microsoft Monitoring Agent](https://docs.microsoft.com/azure/azure-monitor/platform/agent-manage#adding-or-removing-a-workspace) (al di fuori dell'interfaccia di amministrazione di Windows) per aggiungere la nuova area di lavoro corrispondente alla soluzione Gestione aggiornamenti che si vuole usare.
