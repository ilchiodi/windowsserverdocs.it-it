---
ms.assetid: 75cc1d24-fa2f-45bd-8f3b-1bbd4a1aead0
title: Aggiornamento compatibile con cluster requisiti e procedure consigliate
ms.prod: windows-server-threshold
ms.topic: article
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 4/28/2017
description: Requisiti per l'utilizzo di aggiornamento compatibile con Cluster per installare gli aggiornamenti nei cluster che esegue Windows Server.
ms.openlocfilehash: 8517466d58345077af446c1b2c1e2aeb3b17aaa1
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-aware-updating-requirements-and-best-practices"></a>Aggiornamento compatibile con cluster requisiti e procedure consigliate

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questa sezione vengono descritti i requisiti e le dipendenze necessarie per usare [aggiornamento compatibile con Cluster](cluster-aware-updating.md) (aggiornamento compatibile con cluster) per applicare aggiornamenti a un cluster di failover che esegue Windows Server.
  
> [!NOTE]  
> Potrebbe essere necessario verificare in modo indipendente che l'ambiente cluster sia pronto per applicare gli aggiornamenti se si utilizza un plug-in diverso da **Microsoft. windowsupdateplugin**. Se si utilizza un fornitori non Microsoft plug-in, contattare il server di pubblicazione per ulteriori informazioni. Per ulteriori informazioni sull'installazione di plug-aggiuntivi, vedere [funzionamento installazione di plug-in](cluster-aware-updating-plug-ins.md).   
  
## <a name="BKMK_REQ_CLUS"></a>Installare la funzionalità Clustering di Failover e strumenti per Clustering di Failover  
Aggiornamento compatibile con cluster richiede l'installazione della funzionalità Clustering di Failover e strumenti per Clustering di Failover. Strumenti per Clustering di Failover includono il \(clusterawareupdating.dll\) strumenti aggiornamento compatibile con cluster, i cmdlet di Clustering di Failover e altri componenti necessari per le operazioni di aggiornamento compatibile con cluster. Per la procedura installare la funzionalità Clustering di Failover, vedere [installazione della funzionalità Clustering di Failover e strumenti](http://go.microsoft.com/fwlink/p/?LinkId=253342).  
  
Requisiti di installazione esatto strumenti per Clustering di Failover variano a seconda se aggiornamento compatibile con cluster coordina gli aggiornamenti come un ruolo del cluster nel cluster di failover \ (utilizzando l'aggiornamento e mode\) o da un computer remoto. La modalità di aggiornamento e di aggiornamento compatibile con cluster richiede inoltre l'installazione del ruolo del cluster aggiornamento compatibile con cluster nel cluster di failover utilizzando gli strumenti di aggiornamento compatibile con cluster.    
  
Nella tabella seguente vengono riepilogati i requisiti di installazione della funzionalità aggiornamento compatibile con cluster per le due modalità di aggiornamento di aggiornamento compatibile con cluster.  
  
|Componente installato|Modalità di aggiornamento e|Modalità di aggiornamento Remote\|  
|-----------------------|-----------------------|-------------------------|  
|Funzionalità Clustering di failover|Obbligatori su tutti i nodi del cluster|Obbligatori su tutti i nodi del cluster|  
|Strumenti per Clustering di failover|Obbligatori su tutti i nodi del cluster|-Necessarie nel computer di aggiornamento remote\<br />-Obbligatori su tutti i nodi del cluster per eseguire il [Save-CauDebugTrace](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/save-caudebugtrace) cmdlet|  
|Ruolo del cluster|Obbligatorio|Non è necessario|  
  
## <a name="obtain-an-administrator-account"></a>Ottenere un account amministratore  
I seguenti requisiti amministratore sono necessari per usare le funzionalità di aggiornamento compatibile con cluster.  
  
-   Per visualizzare l'anteprima o applicare azioni di aggiornamento tramite l'interfaccia utente di aggiornamento compatibile con cluster \(UI\) o i cmdlet di aggiornamento compatibile con Cluster, è necessario utilizzare un account di dominio che dispone di autorizzazioni e diritti di amministratore locale su tutti i nodi del cluster. Se l'account non dispone di privilegi sufficienti su ogni nodo, viene chiesto nella finestra di aggiornamento compatibile con Cluster per fornire le credenziali necessarie quando si eseguono queste azioni. Per utilizzare il [aggiornamento compatibile con Cluster](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating) cmdlet, è possibile fornire le credenziali necessarie come parametro del cmdlet.  
  
-   Se si utilizza aggiornamento compatibile con cluster in modalità di aggiornamento remote\ quando si è connessi con un account che non dispone delle autorizzazioni e diritti di amministratore locale sui nodi del cluster, è necessario eseguire gli strumenti di aggiornamento compatibile con cluster come un amministratore utilizzando un account amministratore locale nel computer coordinatore dell'aggiornamento oppure utilizzando un account che dispone di **rappresenta un client dopo l'autenticazione** diritto utente. 
  
-   Per eseguire l'aggiornamento compatibile con cluster Best Practices Analyzer, è necessario utilizzare un account con privilegi amministrativi sui nodi del cluster e privilegi di amministratore locale nel computer in cui viene utilizzato per eseguire il [Test-CauSetup](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/test-causetup) cmdlet o per analizzare la disponibilità tramite la finestra di aggiornamento compatibile con Cluster di aggiornamento del cluster. Per ulteriori informazioni, vedere [Test cluster disponibilità all'aggiornamento](#BKMK_BPA).  
  
## <a name="verify-the-cluster-configuration"></a>Verificare la configurazione del cluster  
Di seguito sono requisiti generali per un cluster di failover supportare gli aggiornamenti tramite aggiornamento compatibile con cluster. Requisiti di configurazione aggiuntivi per la gestione remota sui nodi sono elencati [configurare i nodi per la gestione remota](#BKMK_NODE_CONFIG) più avanti in questo argomento.  
  
-   Sufficiente nodi del cluster devono essere online in modo che il cluster dispone del quorum.  
  
-   Tutti i nodi del cluster devono essere nello stesso dominio Active Directory.  
  
-   È necessario risolvere il nome del cluster nella rete tramite DNS.  
  
-   Se aggiornamento compatibile con cluster viene usato in modalità di aggiornamento remote\, il computer coordinatore dell'aggiornamento deve disporre di connettività di rete ai nodi del cluster di failover e deve essere nello stesso dominio Active Directory come cluster di failover.  
  
-   Il servizio Cluster deve essere in esecuzione in tutti i nodi del cluster. Per impostazione predefinita questo servizio viene installato in tutti i nodi del cluster e viene configurato per avviarsi automaticamente.  
  
-   Per utilizzare gli script di pre-aggiornamento o post\ PowerShell durante un'operazione di aggiornamento, assicurarsi che gli script siano installati in tutti i nodi del cluster o che siano accessibili a tutti i nodi, ad esempio, in una condivisione di file di rete a disponibilità elevata. Se gli script sono salvati in una condivisione di file di rete, configurare la cartella per l'autorizzazione di lettura per Everyone gruppo.  
  
## <a name="BKMK_NODE_CONFIG"></a>Configurare i nodi per la gestione remota  
Per usare aggiornamento compatibile con Cluster, tutti i nodi del cluster devono essere configurati per la gestione remota. Per impostazione predefinita, l'unica attività da eseguire per configurare i nodi per la gestione remota è [abilitare una regola firewall per consentire i riavvii automatici](#BKMK_FW). 

Nella tabella seguente vengono elencati i requisiti di gestione remota completo, nel caso in cui l'ambiente differisce dalle impostazioni predefinite.

Questi requisiti si aggiungono ai requisiti di installazione di [installare la funzionalità Clustering di Failover e strumenti per Clustering di Failover](#BKMK_REQ_CLUS) e generale clustering requisiti descritti nelle sezioni precedenti di questo argomento.  
  
|Requisito|Stato predefinito|Modalità di aggiornamento e|Modalità di aggiornamento Remote\|  
|---------------|---|-----------------------|-------------------------|  
|[Abilitare una regola firewall per consentire i riavvii automatici](#BKMK_FW)|Disabilitato|Obbligatorio su tutti i nodi del cluster se un firewall è in uso|Obbligatorio su tutti i nodi del cluster se un firewall è in uso|  
|[Abilitare Strumentazione gestione Windows](#BKMK_WMI)|Abilitato|Obbligatori su tutti i nodi del cluster|Obbligatori su tutti i nodi del cluster|  
|[Abilitare Windows PowerShell 3.0 o 4.0 e comunicazione remota di Windows PowerShell](#BKMK_PS)|Abilitato|Obbligatori su tutti i nodi del cluster|Obbligatori su tutti i nodi del cluster per eseguire le operazioni seguenti:<br /><br />-Le [Save-CauDebugTrace](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/save-caudebugtrace) cmdlet<br />-Gli script di pre-aggiornamento e post\ PowerShell durante un'operazione di aggiornamento<br />-Test di disponibilità tramite la finestra di aggiornamento compatibile con Cluster di aggiornamento del cluster o [test-CauSetup](http://go.microsoft.com/fwlink/p/?LinkId=242384) cmdlet di Windows PowerShell|  
|[Installare .NET Framework 4.6 o 4.5](#BKMK_NET)|Abilitato|Obbligatori su tutti i nodi del cluster|Obbligatori su tutti i nodi del cluster per eseguire le operazioni seguenti:<br /><br />-Le [Save-CauDebugTrace](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/save-caudebugtrace) cmdlet<br />-Gli script di pre-aggiornamento e post\ PowerShell durante un'operazione di aggiornamento<br />-Test di disponibilità tramite la finestra di aggiornamento compatibile con Cluster di aggiornamento del cluster o [test-CauSetup](http://go.microsoft.com/fwlink/p/?LinkId=242384) cmdlet di Windows PowerShell|  

### <a name="BKMK_FW"></a>Abilitare una regola firewall per consentire i riavvii automatici  
Per consentire i riavvii automatici dopo l'applicazione degli aggiornamenti \ (se l'installazione di un aggiornamento richiede l'una Restart), se Windows Firewall o un firewall Microsoft è in uso nei nodi del cluster, è necessario abilitare una regola del firewall su ogni nodo che consente il traffico seguenti:  
  
-   Protocollo: TCP  
  
-   Direzione: in ingresso  
  
-   Programma: wininit.exe  
  
-   Porte: Porte dinamiche RPC  
  
-   Profilo: dominio  
  
Se viene utilizzato Windows Firewall sui nodi del cluster, è possibile farlo abilitando il **arresto remoto** gruppo di regole Windows Firewall in ogni nodo del cluster. Quando si utilizza la finestra di aggiornamento compatibile con Cluster per applicare gli aggiornamenti e per configurare le opzioni di aggiornamento e, di **arresto remoto** gruppo di regole Windows Firewall viene abilitato automaticamente in ogni nodo del cluster.  
  
> [!NOTE]  
> Il **arresto remoto** gruppo di regole Windows Firewall non può essere abilitata quando questo crea conflitti con le impostazioni di criteri di gruppo che sono configurate per Windows Firewall.    
  
Il **arresto remoto** gruppo di regole firewall è abilitato anche specificando il **– EnableFirewallRules** parametro quando si esegue il cmdlet di aggiornamento compatibile con cluster seguenti: [Add-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole), [Invoke-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-causcan), e [SetCauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/set-cauclusterrole).  
  
L'esempio di PowerShell seguente mostra un metodo aggiuntivo per abilitare i riavvii automatici in un nodo del cluster.  
  
```PowerShell  
Set-NetFirewallRule -Group "@firewallapi.dll,-36751" -Profile Domain -Enabled true  
```  

### <a name="BKMK_WMI"></a>Abilitare Strumentazione gestione Windows (WMI) 
Tutti i nodi del cluster devono essere configurati per la gestione remota mediante Strumentazione gestione Windows \(WMI\). Questo è abilitato per impostazione predefinita.  
  
Per abilitare la gestione remota manualmente, eseguire le operazioni seguenti:  
  
1.  Nella console servizi, avviare il **gestione remota Windows** service e impostare il tipo di avvio **automatica**.  
  
2.  Eseguire il [Set-WSManQuickConfig](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.wsman.management/set-wsmanquickconfig) prompt cmdlet oppure eseguire il comando seguente da un prompt dei comandi:  
  
    ```PowerShell  
    winrm quickconfig -q  
    ```  
  
Per supportare la comunicazione remota di WMI, se Windows Firewall è in uso nei nodi del cluster, della regola del firewall in entrata per **gestione remota Windows \(HTTP\-In\)** deve essere abilitata su ciascun nodo.  Per impostazione predefinita, questa regola è abilitata.  
  
### <a name="BKMK_PS"></a>Abilitare Windows PowerShell e comunicazione remota di Windows PowerShell  
Per abilitare la modalità di aggiornamento e e alcune funzionalità di aggiornamento compatibile con cluster in modalità di aggiornamento remote\, PowerShell deve essere installato e abilitato per eseguire comandi remoti su tutti i nodi del cluster. Per impostazione predefinita, PowerShell è installato e abilitato alla comunicazione remota.  
  
Per abilitare la comunicazione remota di PowerShell, utilizzare uno dei metodi seguenti:  
  
-   Eseguire il [Enable-PSRemoting](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/enable-psremoting) cmdlet.  
  
-   Configurare un'impostazione di criteri di gruppo a livello di domain \ per gestione remota Windows \(WinRM\).  
  
Per ulteriori informazioni sull'abilitazione della comunicazione remota di PowerShell, vedere [about\_Remote\_Requirements](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/about/about_remote_requirements).  
  
### <a name="BKMK_NET"></a>Installare .NET Framework 4.6 o 4.5  
Per abilitare la modalità di aggiornamento e e alcune funzionalità di aggiornamento compatibile con cluster in modalità di aggiornamento remote\, .NET Framework 4.6 o .NET Framework 4.5 (in Windows Server 2012 R2) deve essere installato in tutti i nodi del cluster. Per impostazione predefinita, viene installato NET Framework.  

Per installare .NET Framework 4.6 o 4.5 mediante PowerShell se non è già installato, utilizzare il comando seguente:

```PowerShell
Install-WindowsFeature -Name NET-Framework-45-Core
```

## <a name="BKMK_BEST_PRAC"></a>Procedure consigliate per l'utilizzo di aggiornamento compatibile con Cluster 
  
### <a name="BKMK_BP_WUA"></a>Consigli per l'applicazione di aggiornamenti Microsoft

È consigliabile che quando si inizia ad utilizzare aggiornamento compatibile con cluster per applicare gli aggiornamenti con il valore predefinito **Microsoft. windowsupdateplugin** plug-in un cluster, si interrompe l'utilizzo di altri metodi per installare gli aggiornamenti software da Microsoft nei nodi del cluster.  
  
> [!CAUTION]  
> La combinazione di aggiornamento compatibile con cluster con metodi che aggiornano automaticamente i singoli nodi \ (in tempi pianificati \ un periodo di tempo fisso) può causare risultati imprevedibili, inclusi interruzioni del servizio e tempi di inattività non pianificato.  
  
Ti consigliamo di seguire queste linee guida:  
  
-   Per ottenere risultati ottimali, si consiglia di disattivare impostazioni sui nodi del cluster per l'aggiornamento automatico, ad esempio, tramite le impostazioni di aggiornamenti automatici nel Pannello di controllo o in impostazioni configurate mediante criteri di gruppo.  
  
    > [!CAUTION]  
    > Installazione automatica degli aggiornamenti sui nodi del cluster può interferire con l'installazione di aggiornamenti tramite aggiornamento compatibile con cluster e può causare errori di aggiornamento compatibile con cluster.  
  
    Se sono necessarie, le impostazioni seguenti gli aggiornamenti automatici sono compatibili con aggiornamento compatibile con cluster, poiché l'amministratore può controllare le tempistiche di installazione degli aggiornamenti:  
  
    -   Impostazioni con notifica prima il download degli aggiornamenti e prima dell'installazione  
  
    -   Impostazioni per scaricare automaticamente gli aggiornamenti e prima dell'installazione  
  
    Tuttavia, se gli aggiornamenti automatici sta scaricando aggiornamenti contemporaneamente come un'operazione di aggiornamento, l'operazione di aggiornamento potrebbe richiedere più tempo.  
  
-   Non si configura un sistema di aggiornamento, ad esempio Windows Server Update Services \(WSUS\) per applicare gli aggiornamenti automaticamente \ (in tempi pianificati \ un periodo di tempo fisso) per i nodi del cluster.  
  
-   Tutti i nodi del cluster devono essere configurati in modo uniforme per usare la stessa origine di aggiornamento, ad esempio, un Windows Server Update Services server, Windows Update o Microsoft Update.  
  
-   Se si utilizza un sistema di Gestione configurazione per applicare gli aggiornamenti software ai computer della rete, escludere i nodi del cluster da tutti gli aggiornamenti richiesti o automatici. Microsoft System Center Configuration Manager 2007 e Microsoft System Center Virtual Machine Manager 2008 sono esempi di sistemi di gestione della configurazione.  
  
-   Se il server di distribuzione software interni \ (ad esempio, WSUS servers\) vengono utilizzati per contenere e distribuire gli aggiornamenti, assicurarsi che tali server identifichino correttamente gli aggiornamenti approvati per i nodi del cluster.  
  
#### <a name="BKMK_PROXY"></a>Applicare aggiornamenti Microsoft in scenari di succursale  
Per scaricare gli aggiornamenti Microsoft da Microsoft Update o Windows Update per i nodi del cluster in determinati scenari di succursale, è necessario configurare le impostazioni proxy per l'account sistema locale su ciascun nodo. Ad esempio, si potrebbe essere necessario eseguire questa operazione se i cluster della succursale accedere a Microsoft Update o Windows Update per scaricare gli aggiornamenti tramite un server proxy locale.  
  
Se necessario, configurare le impostazioni proxy WinHTTP su ogni nodo per specificare un server proxy locale e configurare eccezioni per indirizzi locali \ (ovvero, un elenco di esclusione per addresses\ locale). A tale scopo, è possibile eseguire il comando seguente in ogni nodo del cluster da un prompt dei comandi con privilegi elevati:  
  
```  
netsh winhttp set proxy <ProxyServerFQDN >:<port> "<local>"  
```  
  
Dove <*ProxyServerFQDN*> è il nome di dominio completo del server proxy e <*porta*> è la porta su cui si desidera comunicare (in genere la porta 443).  
  
Ad esempio, per configurare le impostazioni proxy WinHTTP per l'account sistema locale specificando il server proxy *MyProxy.CONTOSO.com*, con la porta 443 ed eccezioni per indirizzi locali, digitare il comando seguente:  
  
```  
netsh winhttp set proxy MyProxy.CONTOSO.com:443 "<local>"  
```  
  
### <a name="BKMK_BP_HF"></a>Suggerimenti per l'uso di Microsoft. hotfixplugin  
  
-   Si consiglia di configurare le autorizzazioni nella cartella radice degli aggiornamenti rapidi e file di configurazione degli aggiornamenti rapidi per limitare l'accesso in scrittura ai soli amministratori locali nei computer che vengono usati per archiviare questi file. Ciò consente di prevenire la manomissione di questi file da utenti non autorizzati che potrebbero compromettere il funzionamento del cluster di failover quando vengono applicati gli aggiornamenti rapidi.  
  
-   Per garantire l'integrità dei dati per le connessioni server message block \(SMB\) che vengono utilizzati per accedere alla cartella radice degli aggiornamenti rapidi, è necessario configurare la crittografia SMB nella cartella SMB condivisa, se è possibile configurarla. Il **Microsoft. hotfixplugin** richiede che la firma SMB o crittografia SMB sia configurata per garantire l'integrità dei dati per le connessioni SMB. 
  
    Per ulteriori informazioni, vedere [limitare l'accesso per la cartella radice degli aggiornamenti rapidi e i file di configurazione degli aggiornamenti rapidi](cluster-aware-updating-plug-ins.md#BKMK_ACL).
  
### <a name="additional-recommendations"></a>Indicazioni aggiuntive  
  
-   Per evitare di interferire con un'operazione di aggiornamento che possono essere pianificati allo stesso tempo, non pianificare le modifiche delle password per oggetti nome cluster e gli oggetti computer virtuale durante le finestre di manutenzione pianificata.  
  
-   È necessario impostare le autorizzazioni appropriate sugli script di pre-aggiornamento e post\-aggiornamento salvati in cartelle condivise in rete per impedire potenziali manomissioni dei file da utenti non autorizzati.  
  
-   Per configurare aggiornamento compatibile con cluster in modalità di aggiornamento e, un oggetto computer virtuale \(VCO\) per il ruolo del cluster aggiornamento compatibile con cluster deve essere creato in Active Directory. Aggiornamento compatibile con cluster può creare questo oggetto automaticamente nel momento in cui viene aggiunto il ruolo del cluster aggiornamento compatibile con cluster, se il cluster di failover disponga di autorizzazioni sufficienti. A causa dei criteri di sicurezza in alcune organizzazioni, tuttavia, potrebbe essere necessario pre-installare l'oggetto in Active Directory. Per una procedura eseguire questa operazione, vedere [passaggi per la pre-installazione di un account per un ruolo del cluster](http://go.microsoft.com/fwlink/p/?LinkId=237624).  
  
-   Per salvare e riutilizzare le impostazioni di operazione di aggiornamento su cluster di failover con esigenze dell'organizzazione IT, di aggiornamento analoghe è possibile creare profili delle operazioni di aggiornamento. Inoltre, a seconda della modalità di aggiornamento, è possibile salvare e gestire l'aggiornamento dei profili di esecuzione in una condivisione file accessibile a tutti i computer coordinatore dell'aggiornamento remoti o i cluster di failover. Per ulteriori informazioni, vedere [opzioni avanzate e aggiornamento dei profili di esecuzione per aggiornamento compatibile con cluster](cluster-aware-updating-options.md).  
  
## <a name="BKMK_BPA"></a>Cluster di test preparazione per l'aggiornamento  
È possibile eseguire il modello \(BPA\) aggiornamento compatibile con cluster Best Practices Analyzer per verificare se un cluster di failover e l'ambiente di rete soddisfano molti dei requisiti necessari per gli aggiornamenti software applicati da aggiornamento compatibile con cluster. Molti dei test controllare l'ambiente per la preparazione applicare aggiornamenti Microsoft usando il valore predefinito plug-in, **Microsoft. windowsupdateplugin**.  
  
> [!NOTE]  
> Potrebbe essere necessario verificare in modo indipendente che l'ambiente cluster sia pronto per applicare gli aggiornamenti software utilizzando un plug-in diverso **Microsoft. windowsupdateplugin**. Se si utilizza un fornitori non Microsoft plug-in, ad esempio un fornito dal produttore dell'hardware, contattare il server di pubblicazione per ulteriori informazioni.  
  
È possibile eseguire BPA in due modi seguenti:  
  
1.  Selezionare **Analyze cluster disponibilità all'aggiornamento** nella console di aggiornamento compatibile con cluster. Dopo il completamento dei test di disponibilità, viene visualizzato un report di test. Se vengono rilevati problemi nei nodi del cluster, in modo da consentire di eseguire azioni correttive vengono identificati i problemi specifici e i nodi in cui vengono visualizzati i problemi. I test possono richiedere diversi minuti.  
  
2.  Eseguire il [Test-CauSetup](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/test-causetup) cmdlet. È possibile eseguire il cmdlet in un computer locale o remoto in cui è installato il Failover Clustering modulo per Windows PowerShell (parte di strumenti per Clustering di Failover). È anche possibile eseguire il cmdlet in un nodo del cluster di failover.  
  
> [!NOTE]  
> -   È necessario utilizzare un account con privilegi amministrativi sui nodi del cluster e privilegi di amministratore locale nel computer in cui viene utilizzato per eseguire il **test-CauSetup** cmdlet o per analizzare la disponibilità tramite la finestra di aggiornamento compatibile con Cluster di aggiornamento del cluster. Per eseguire i test tramite la finestra di aggiornamento compatibile con Cluster, è necessario essere connessi al computer con le credenziali necessarie.  
> -   I test si presuppone che gli strumenti di aggiornamento compatibile con cluster che vengono usati per visualizzare l'anteprima e applicare gli aggiornamenti software eseguite dallo stesso computer e con le stesse credenziali utente come vengono utilizzati per testare la disponibilità di aggiornamento del cluster.  
  
> [!IMPORTANT]  
> Ti consigliamo di testare il cluster per l'aggiornamento readiness nelle situazioni seguenti:  
>   
> -   Prima di utilizzare aggiornamento compatibile con cluster per la prima volta per applicare gli aggiornamenti software.  
> -   Dopo aver aggiunto un nodo al cluster o eseguire altre modifiche hardware del cluster che richiedono l'esecuzione della convalida guidata Cluster.  
> -   Dopo che si modifica un'origine degli aggiornamenti, o aggiornamento delle impostazioni o configurazioni \(other than CAU\) che possono influire sull'applicazione di aggiornamenti sui nodi.  
  
### <a name="tests-for-cluster-updating-readiness"></a>Test per la preparazione di aggiornamento del cluster  
La tabella seguente elenca i test di disponibilità, alcuni problemi comuni e la risoluzione dei problemi di aggiornamento del cluster.  
  
|Test|Possibili problemi ed effetti|Risoluzione dei problemi|  
|--------|-------------------------------|--------------------|  
|Il cluster di failover deve essere disponibile|Impossibile risolvere che il nome del cluster di failover o uno o più nodi del cluster non è possibile accedere. BPA non può eseguire i test di disponibilità del cluster.|-Controllare l'ortografia del nome del cluster specificato durante l'esecuzione di BPA.<br />-Verificare che tutti i nodi del cluster siano online e in esecuzione.<br />-Verificare che la convalida guidata configurazione possa essere eseguita correttamente sul cluster di failover.|  
|I nodi del cluster di failover devono essere abilitati per la gestione remota tramite WMI|Uno o più nodi del cluster di failover non sono abilitati per la gestione remota mediante Strumentazione gestione Windows \(WMI\). Aggiornamento compatibile con cluster non può aggiornare i nodi del cluster se i nodi non sono configurati per la gestione remota.|Assicurarsi che tutti i nodi del cluster di failover siano abilitati per la gestione remota tramite WMI. Per ulteriori informazioni, vedere [configurare i nodi per la gestione remota](#BKMK_NODE_CONFIG) in questo argomento.|  
|Comunicazione remota di PowerShell deve essere abilitata su ogni nodo del cluster di failover|PowerShell non è installato o non è abilitato alla comunicazione remota su uno o più nodi del cluster di failover. Aggiornamento compatibile con cluster non può essere configurato per la modalità di aggiornamento e o usare determinate funzionalità in modalità di aggiornamento remote\.|Assicurarsi che PowerShell sia installato in tutti i nodi del cluster ed è abilitata per la comunicazione remota.<br /><br />Per ulteriori informazioni, vedere [configurare i nodi per la gestione remota](#BKMK_NODE_CONFIG) in questo argomento.|  
|Versione del cluster di failover|Non eseguono uno o più nodi nel cluster di failover Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012. Aggiornamento compatibile con cluster non può aggiornare il cluster di failover.|Verificare che il cluster di failover specificato durante l'esecuzione di BPA esegua Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.<br /><br />Per ulteriori informazioni, vedere [verificare la configurazione del cluster](#BKMK_REQ_CLUS) in questo argomento.|  
|Delle versioni richieste di .NET Framework e Windows PowerShell devono essere installate in tutti i nodi del cluster di failover|Non è installato .NET framework 4.6 4.5 o Windows PowerShell in uno o più nodi del cluster. Alcune funzionalità di aggiornamento compatibile con cluster potrebbero non funzionare.|Assicurarsi che siano installati .NET Framework 4.6 o 4.5 e Windows PowerShell in tutti i nodi del cluster, se sono necessarie.<br /><br />Per ulteriori informazioni, vedere [configurare i nodi per la gestione remota](#BKMK_NODE_CONFIG) in questo argomento.|  
|Il servizio Cluster deve essere in esecuzione in tutti i nodi del cluster|Il servizio Cluster non è in esecuzione in uno o più nodi. Aggiornamento compatibile con cluster non può aggiornare il cluster di failover.|-Verificare che il \(clussvc\) servizio Cluster viene avviato in tutti i nodi del cluster, che è configurato per l'avvio automatico.<br />-Verificare che la convalida guidata configurazione possa essere eseguita correttamente sul cluster di failover.<br /><br />Per ulteriori informazioni, vedere [verificare la configurazione del cluster](#BKMK_REQ_CLUS) in questo argomento.|  
|Aggiornamenti automatici non devono essere configurato per installare automaticamente gli aggiornamenti sui nodi del cluster di failover|In almeno un nodo del cluster di failover, aggiornamenti automatici è configurato per installare automaticamente gli aggiornamenti di Microsoft su tale nodo. La combinazione di aggiornamento compatibile con cluster con altri metodi di aggiornamento può comportare tempi di inattività imprevisti o a risultati imprevedibili.|Se la funzionalità di Windows Update è configurata per gli aggiornamenti automatici su uno o più nodi del cluster, assicurarsi che gli aggiornamenti automatici non è configurato per installare automaticamente gli aggiornamenti.<br /><br />Per ulteriori informazioni, vedere [consigli per l'applicazione di aggiornamenti Microsoft](#BKMK_BP_WUA).|  
|I nodi del cluster di failover devono usare la stessa origine di aggiornamento|Uno o più nodi del cluster di failover sono configurati per utilizzare un'origine degli aggiornamenti per gli aggiornamenti Microsoft diversa dal resto dei nodi. Gli aggiornamenti non vengano applicati in modo uniforme nei nodi del cluster da aggiornamento compatibile con cluster.|Assicurarsi che ogni nodo del cluster sia configurato per usare la stessa origine di aggiornamento, ad esempio, un server WSUS, Windows Update o Microsoft Update.<br /><br />Per ulteriori informazioni, vedere [consigli per l'applicazione di aggiornamenti Microsoft](#BKMK_BP_WUA).|  
|Una regola firewall che consenta l'arresto remoto deve essere abilitata su ogni nodo nel cluster di failover|Uno o più nodi del cluster di failover non sono abilitata una regola del firewall che consenta l'arresto remoto, o un'impostazione di criteri di gruppo che impedisce l'abilitazione di questa regola. Un'operazione di aggiornamento che applica aggiornamenti che richiedono il riavvio automatico dei nodi potrebbe non essere completata correttamente.|Se Windows Firewall o un firewall Microsoft è in uso nei nodi del cluster, configurare una regola firewall che consenta l'arresto remoto.<br /><br />Per ulteriori informazioni, vedere [abilitare una regola firewall per consentire i riavvii automatici](#BKMK_FW) in questo argomento.|  
|L'impostazione di server proxy su ogni nodo del cluster di failover deve essere impostato su un server proxy locale|Uno o più nodi del cluster di failover dispongano di una configurazione del server proxy non corretta.<br /><br />Se un server proxy locale è in uso, le impostazioni del server proxy su ogni nodo devono essere configurata correttamente per il cluster per accedere a Microsoft Update o Windows Update.|Assicurarsi che le impostazioni proxy WinHTTP su ogni nodo del cluster siano impostate su un server proxy locale, se necessario. Se un server proxy non è in uso nell'ambiente in uso, questo avviso può essere ignorato.<br /><br />Per ulteriori informazioni, vedere [applicare gli aggiornamenti in scenari di succursale](#BKMK_PROXY) in questo argomento.|  
|Il ruolo del cluster aggiornamento compatibile con cluster deve essere installato nel cluster di failover per abilitare la modalità di aggiornamento e|Il ruolo del cluster aggiornamento compatibile con cluster non è installato nel cluster di failover. Questo ruolo è necessario per l'aggiornamento e cluster.|Per usare aggiornamento compatibile con cluster in modalità di aggiornamento e, aggiungere il ruolo del cluster aggiornamento compatibile con cluster nel cluster di failover in uno dei modi seguenti:<br /><br />-Eseguire il [Add-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole) cmdlet di PowerShell.<br />-Seleziona il **configurare le opzioni di aggiornamento e cluster** azione nella finestra di aggiornamento compatibile con Cluster.|  
|Il ruolo del cluster aggiornamento compatibile con cluster deve essere abilitato nel cluster di failover per abilitare la modalità di aggiornamento e|Il ruolo del cluster aggiornamento compatibile con cluster è disabilitato. Ad esempio, il ruolo del cluster aggiornamento compatibile con cluster non è installato o è stato disabilitato usando il [Disable-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/disable-cauclusterrole) cmdlet di PowerShell. Questo ruolo è necessario per l'aggiornamento e cluster.|Per usare aggiornamento compatibile con cluster in modalità di aggiornamento e, abilitare il ruolo del cluster aggiornamento compatibile con cluster nel cluster di failover in uno dei modi seguenti:<br /><br />-Eseguire il [Enable-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/enable-cauclusterrole) cmdlet di PowerShell.<br />-Seleziona il **configurare le opzioni di aggiornamento e cluster** azione nella finestra di aggiornamento compatibile con Cluster.|  
|L'aggiornamento compatibile con cluster configurato plug-in per la modalità di aggiornamento e deve essere registrato in tutti i nodi del cluster di failover|Il ruolo del cluster aggiornamento compatibile con cluster in uno o più nodi del cluster di failover non può accedere il modulo di plug-in aggiornamento compatibile con cluster è configurato nelle opzioni di aggiornamento e. Un e-operazioni di aggiornamento automatico potrebbe non riuscire.|-Verificare che l'aggiornamento compatibile con cluster configurato plug-in sia installato in tutti i nodi del cluster seguendo la procedura di installazione per il prodotto che fornisce l'aggiornamento compatibile con cluster plug-in.<br />-Eseguire il [Register-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) cmdlet PowerShell per registrare il plug-nei nodi del cluster necessarie.|  
|Tutti i nodi del cluster di failover devono avere lo stesso set di installazione di plug-in di aggiornamento compatibile con cluster registrati|Un e-operazioni di aggiornamento automatico potrebbe non riuscire se l'installazione di plug-in che è configurato per essere utilizzato in un'operazione di aggiornamento viene modificato in uno che non è disponibile in tutti i nodi del cluster.|-Verificare che l'aggiornamento compatibile con cluster configurato plug-in sia installato in tutti i nodi del cluster seguendo la procedura di installazione per il prodotto che fornisce l'aggiornamento compatibile con cluster plug-in.<br />-Eseguire il [Register-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) cmdlet PowerShell per registrare il plug-nei nodi del cluster necessarie.|  
|Le opzioni di operazione di aggiornamento configurate devono essere valide|La pianificazione e aggiornamento e opzioni dell'operazione di aggiornamento configurate per il cluster di failover sono incomplete o non valide. Un e-operazioni di aggiornamento automatico potrebbe non riuscire.|Configurare una pianificazione di aggiornamento e valido e di un set di opzioni dell'aggiornamento. Ad esempio, è possibile utilizzare il [set-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/set-cauclusterrole) cmdlet PowerShell per configurare l'aggiornamento compatibile con cluster ruolo.|  
|Almeno due nodi del cluster di failover devono essere proprietari del ruolo del cluster aggiornamento compatibile con cluster|Un'operazione di aggiornamento avviate in modalità di aggiornamento e avrà esito negativo perché non dispone di un nodo proprietario per spostare il ruolo del cluster aggiornamento compatibile con cluster.|Utilizzare gli strumenti per Clustering di Failover per verificare che tutti i nodi del cluster siano configurati come possibili proprietari di aggiornamento compatibile con cluster ruolo. Questa è la configurazione predefinita.||  
|Tutti i nodi del cluster di failover devono essere in grado di accedere agli script di Windows PowerShell|Non tutti i possibili nodi proprietari del ruolo del cluster aggiornamento compatibile con cluster possono accedere gli script pre-aggiornamento e post\-aggiornamento di Windows PowerShell configurati. Un e-operazioni di aggiornamento automatico non riuscirà.|Assicurarsi che tutti i possibili nodi proprietari del ruolo del cluster aggiornamento compatibile con cluster dispongano delle autorizzazioni per accedere agli script pre-aggiornamento e post\ PowerShell configurati.|  
|Tutti i nodi del cluster di failover devono usare script di Windows PowerShell identici|Non tutti i possibili nodi proprietari del ruolo del cluster aggiornamento compatibile con cluster usano la stessa copia degli script di pre-aggiornamento e post\ Windows PowerShell specificato. Esecuzione di un aggiornamento e potrebbe non riuscire o presentare comportamenti imprevisti.|Assicurarsi che tutti i possibili nodi proprietari del ruolo del cluster aggiornamento compatibile con cluster usino gli script di pre-aggiornamento e post\ PowerShell stessi.|  
|L'impostazione WarnAfter specificato per l'operazione di aggiornamento deve essere minore dell'impostazione StopAfter|I valori di timeout specificati operazione di aggiornamento verifica il timeout dell'avviso inefficace. Un'operazione di aggiornamento potrebbe venire annullata prima di generare un registro eventi di avviso.|Nelle opzioni di operazione di aggiornamento, configurare un **WarnAfter** valore dell'opzione inferiore al valore **StopAfter** valore dell'opzione.|  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Panoramica di aggiornamento compatibile con cluster](cluster-aware-updating.md)
  
