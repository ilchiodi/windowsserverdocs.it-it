---
ms.assetid: 75cc1d24-fa2f-45bd-8f3b-1bbd4a1aead0
title: Requisiti e procedure consigliate per l'aggiornamento compatibile con cluster
ms.prod: windows-server
ms.topic: article
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 08/06/2018
description: Requisiti per l'utilizzo di aggiornamento compatibile con cluster per installare gli aggiornamenti nei cluster che eseguono Windows Server.
ms.openlocfilehash: 501969fad2455195bca485bd8124911d6d75378e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361317"
---
# <a name="cluster-aware-updating-requirements-and-best-practices"></a>Requisiti e procedure consigliate per l'aggiornamento compatibile con cluster

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questa sezione descrive i requisiti e le dipendenze necessari per usare [aggiornamento compatibile con cluster](cluster-aware-updating.md) per applicare gli aggiornamenti a un cluster di failover che esegue Windows Server.

> [!NOTE]  
> Potrebbe essere necessario verificare in modo indipendente che l'ambiente cluster sia pronto per applicare gli aggiornamenti se si usa un plug @ no__t-0cm diverso da **Microsoft. WindowsUpdatePlugin**. Se si usa un plug-in non @ no__t-0Microsoft @ no__t-1in, contattare l'autore per ulteriori informazioni. Per ulteriori informazioni sul plug-in no__t-0ins, vedere la pagina relativa alla [modalità di funzionamento di plug @ no__t-2Ins](cluster-aware-updating-plug-ins.md).   

## <a name="BKMK_REQ_CLUS"></a>Installare la funzionalità clustering di failover e gli strumenti di clustering di failover  
Aggiornamento compatibile con cluster richiede l'installazione della funzionalità Clustering di failover e degli Strumenti per Clustering di failover. Gli strumenti di clustering di failover includono gli strumenti di aggiornamento compatibile con cluster @no__t -0clusterawareupdating. dll @ no__t-1, i cmdlet del clustering di failover e altri componenti necessari per le operazioni di aggiornamento compatibile con cluster. Per i passaggi da eseguire per installare la funzionalità Clustering di failover, vedere l'articolo relativo all' [installazione della funzionalità Clustering di failover e dei relativi strumenti](create-failover-cluster.md#install-the-failover-clustering-feature).  

Gli esatti requisiti di installazione per gli strumenti di clustering di failover variano a seconda che aggiornamento compatibile con cluster coordini gli aggiornamenti come ruolo del cluster nel cluster di failover \(BY usando self @ no__t-1updating Mode @ no__t-2 o da un computer remoto. La modalità self-no__t-0updating di aggiornamento compatibile con cluster richiede inoltre l'installazione del ruolo del cluster di aggiornamento compatibile con cluster nel cluster di failover usando gli strumenti di aggiornamento compatibile con cluster.    

Nella tabella seguente vengono riepilogati i requisiti di installazione della funzionalità Aggiornamento compatibile con cluster per le due modalità di aggiornamento.  

|Componente installato|Modalità self @ no__t-0updating|Remote @ no__t-modalità 0updating|  
|-----------------------|-----------------------|-------------------------|  
|Funzionalità Clustering di failover|Obbligatori su tutti i nodi del cluster|Obbligatori su tutti i nodi del cluster|  
|Strumenti per Clustering di failover|Obbligatori su tutti i nodi del cluster|-Obbligatorio in remoto @ no__t-computer 0updating<br />-Obbligatorio in tutti i nodi del cluster per eseguire il cmdlet [Save-CauDebugTrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps)|  
|Ruolo del cluster di Aggiornamento compatibile con cluster|Obbligatorio|Non obbligatorio|  

## <a name="obtain-an-administrator-account"></a>Ottenere un account amministratore  
Per usare le funzionalità di Aggiornamento compatibile con cluster sono necessari i requisiti amministrativi seguenti.  

-   Per visualizzare in anteprima o applicare azioni di aggiornamento usando l'interfaccia utente di aggiornamento compatibile con cluster \(UI @ no__t-1 o i cmdlet di aggiornamento compatibile con cluster, è necessario usare un account di dominio che disponga di diritti e autorizzazioni di amministratore locale su tutti i nodi del cluster. Se l'account non dispone di privilegi sufficienti su ogni nodo, nella finestra di aggiornamento compatibile con cluster verrà richiesto di fornire le credenziali necessarie quando si eseguono queste azioni. Per utilizzare i cmdlet di [aggiornamento compatibile con cluster](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps) , è possibile fornire le credenziali necessarie come parametro del cmdlet.  

-   Se si usa aggiornamento compatibile con cluster in modalità remota @ no__t-0updating quando è stato eseguito l'accesso con un account che non dispone di diritti e autorizzazioni di amministratore locale nei nodi del cluster, è necessario eseguire gli strumenti di aggiornamento compatibile con cluster come amministratore usando un account amministratore locale nell'aggiornamento Computer coordinatore o utilizzando un account che disponga del diritto utente rappresenta **un client dopo l'autenticazione** . 

-   Per eseguire il Best Practices Analyzer di aggiornamento compatibile con cluster, è necessario usare un account con privilegi amministrativi sui nodi del cluster e i privilegi di amministratore locale nel computer usato per eseguire il cmdlet [test-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) o per analizzare l'aggiornamento del cluster conformità con la finestra di aggiornamento compatibile con cluster. Per altre informazioni, vedere [Test della disponibilità del cluster per l'aggiornamento](#BKMK_BPA).  

## <a name="verify-the-cluster-configuration"></a>Verificare la configurazione del cluster  
Di seguito vengono riportati i requisiti generali di un cluster di failover che supporti aggiornamenti applicati mediante Aggiornamento compatibile con cluster. Un elenco di requisiti di configurazione aggiuntivi per la gestione remota dei nodi è fornito nella sezione [Configurare i nodi per la gestione remota](#BKMK_NODE_CONFIG) più avanti in questo argomento.  

-   Affinché il cluster disponga di quorum, deve trovarsi online un numero sufficiente di nodi del cluster.  

-   Tutti i nodi del cluster devono trovarsi nello stesso dominio Active Directory.  

-   La risoluzione del nome del cluster nella rete deve avvenire mediante DNS.  

-   Se aggiornamento compatibile con cluster viene usato in modalità @ no__t-0updating remota, il computer coordinatore dell'aggiornamento deve disporre di connettività di rete ai nodi del cluster di failover e deve trovarsi nello stesso dominio Active Directory del cluster di failover.  

-   Il servizio cluster deve essere in esecuzione in tutti i nodi del cluster. Per impostazione predefinita, il servizio è installato in tutti i nodi del cluster ed è configurato per essere avviato automaticamente.  

-   Per usare gli script pre @ no__t-0update o post @ no__t-1Update di PowerShell durante un'operazione di aggiornamento di aggiornamento compatibile con cluster, verificare che gli script siano installati in tutti i nodi del cluster o che siano accessibili a tutti i nodi, ad esempio in una condivisione di file di rete a disponibilità elevata. Se gli script sono salvati in una condivisione file di rete, configurare la relativa cartella per concedere l'autorizzazione di lettura al gruppo Everyone.  

## <a name="BKMK_NODE_CONFIG"></a>Configurare i nodi per la gestione remota  
Per usare aggiornamento compatibile con cluster, tutti i nodi del cluster devono essere configurati per la gestione remota. Per impostazione predefinita, l'unica attività che è necessario eseguire per configurare i nodi per la gestione remota consiste nell' [abilitare una regola del firewall per consentire i riavvii automatici](#BKMK_FW). 

La tabella seguente elenca i requisiti di gestione remota completi, nel caso in cui l'ambiente sia diverso dalle impostazioni predefinite.

Tali requisiti si aggiungono ai requisiti di installazione descritti in [Installare la funzionalità Clustering di failover e gli Strumenti per Clustering di failover](#BKMK_REQ_CLUS) e ai requisiti generali del cluster descritti nelle sezioni precedenti di questo argomento.  

|Requisito|Stato predefinito|Modalità self @ no__t-0updating|Remote @ no__t-modalità 0updating|  
|---------------|---|-----------------------|-------------------------|  
|[Abilitare una regola del firewall per consentire i riavvii automatici](#BKMK_FW)|Disabilitata|Obbligatorio in tutti i nodi del cluster se un firewall è in uso|Obbligatorio in tutti i nodi del cluster se un firewall è in uso|  
|[Abilita Strumentazione gestione Windows](#BKMK_WMI)|Enabled|Obbligatori su tutti i nodi del cluster|Obbligatori su tutti i nodi del cluster|  
|[Abilitare Windows PowerShell 3,0 o 4,0 e la comunicazione remota di Windows PowerShell](#BKMK_PS)|Enabled|Obbligatori su tutti i nodi del cluster|Obbligatorio su tutti i nodi del cluster per l'esecuzione dei componenti seguenti:<br /><br />-Cmdlet [Save-CauDebugTrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps)<br />-Script pre @ no__t-0update e post @ no__t-1Update durante un'operazione di aggiornamento<br />-Test della disponibilità del cluster per l'aggiornamento tramite la finestra di aggiornamento compatibile con cluster o il cmdlet di Windows PowerShell per il [test @ no__t-1CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps)|  
|[Installare .NET Framework 4,6 o 4,5](#BKMK_NET)|Enabled|Obbligatori su tutti i nodi del cluster|Obbligatorio su tutti i nodi del cluster per l'esecuzione dei componenti seguenti:<br /><br />-Cmdlet [Save-CauDebugTrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps)<br />-Script pre @ no__t-0update e post @ no__t-1Update durante un'operazione di aggiornamento<br />-Test della disponibilità del cluster per l'aggiornamento tramite la finestra di aggiornamento compatibile con cluster o il cmdlet di Windows PowerShell per il [test @ no__t-1CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps)|  

### <a name="BKMK_FW"></a>Abilitare una regola del firewall per consentire i riavvii automatici  
Per consentire i riavvii automatici dopo l'applicazione degli aggiornamenti \(If l'installazione di un aggiornamento richiede un riavvio @ no__t-1, se Windows Firewall o un firewall non @ no__t-2Microsoft è in uso nei nodi del cluster, è necessario abilitare una regola del firewall in ogni nodo che consente il traffico seguente:  

-   Protocol: TCP  

-   Direzione: in ingresso  

-   Programma: wininit.exe  

-   Porte: Porte dinamiche RPC  

-   Profilo: Domain  

Se Windows Firewall viene usato sui nodi del cluster, l'operazione può essere eseguita abilitando il gruppo di regole **Arresto remoto** di Windows Firewall su ogni nodo del cluster. Quando si usa la finestra di aggiornamento compatibile con cluster per applicare gli aggiornamenti e configurare le opzioni self @ no__t-0updating, il gruppo di regole Windows Firewall di **arresto remoto** viene abilitato automaticamente in ogni nodo del cluster.  

> [!NOTE]  
> Non è possibile abilitare il gruppo di regole **Remote Shutdown** di Windows Firewall se questo crea conflitti con le impostazioni di Criteri di gruppo configurate per Windows Firewall.    

Il gruppo di regole del firewall di **arresto remoto** viene abilitato anche specificando il parametro **– EnableFirewallRules** quando si eseguono i cmdlet di aggiornamento compatibile con cluster seguenti: [Add-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole?view=win10-ps), [Invoke-CauRun](https://docs.microsoft.com/powershell/module/clusterawareupdating/Invoke-CauRun?view=win10-ps)e [SetCauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Set-CauClusterRole?view=win10-ps).  

Nell'esempio di PowerShell seguente viene illustrato un metodo aggiuntivo per abilitare i riavvii automatici in un nodo del cluster.  

```PowerShell  
Set-NetFirewallRule -Group "@firewallapi.dll,-36751" -Profile Domain -Enabled true  
```  

### <a name="BKMK_WMI"></a>Abilita Strumentazione gestione Windows (WMI) 
Tutti i nodi del cluster devono essere configurati per la gestione remota tramite Strumentazione gestione Windows \(WMI @ no__t-1. L'impostazione è abilitata per impostazione predefinita.  

Per abilitare la gestione remota manualmente, procedere come segue:  

1.  Nella console Servizi, avviare il servizio **Gestione remota Windows** e impostare il tipo di avvio su **Automatico**.  

2.  Eseguire il cmdlet [Set-WSManQuickConfig](https://docs.microsoft.com/powershell/module/Microsoft.WsMan.Management/Set-WSManQuickConfig?view=powershell-6) oppure eseguire il comando seguente da un prompt dei comandi con privilegi elevati:  

    ```PowerShell  
    winrm quickconfig -q  
    ```  

Per supportare la comunicazione remota WMI, se Windows Firewall è in uso nei nodi del cluster, è necessario abilitare la regola del firewall in ingresso per **Gestione remota Windows \(HTTP @ no__t-2in @ no__t-3** in ogni nodo.  Questa regola è abilitata per impostazione predefinita.  

### <a name="BKMK_PS"></a>Abilitare Windows PowerShell e la comunicazione remota di Windows PowerShell  
Per abilitare la modalità self-no__t-0updating e alcune funzionalità di aggiornamento compatibile con cluster in modalità remota @ no__t-1updating, è necessario che PowerShell sia installato e abilitato per l'esecuzione di comandi remoti su tutti i nodi del cluster. Per impostazione predefinita, PowerShell è installato e abilitato per la comunicazione remota.  

Per abilitare la comunicazione remota di PowerShell, usare uno dei metodi seguenti:  

-   Eseguire il cmdlet [Enable-PSRemoting](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Enable-PSRemoting) .  

-   Configurare un'impostazione di Criteri di gruppo Domain @ no__t-0level per Gestione remota Windows \(WinRM @ no__t-2.  

Per ulteriori informazioni sull'abilitazione della comunicazione remota di PowerShell, vedere [About Remote requirements](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_remote_requirements?view=powershell-6).  

### <a name="BKMK_NET"></a>Installare .NET Framework 4,6 o 4,5  
Per abilitare la modalità self-no__t-0updating e alcune funzionalità di aggiornamento compatibile con cluster in modalità remota @ no__t-1updating,. NET Framework 4,6 o .NET Framework 4,5 (in Windows Server 2012 R2) deve essere installato in tutti i nodi del cluster. Per impostazione predefinita, .NET Framework è installato.  

Per installare .NET Framework 4,6 (o 4,5) con PowerShell, se non è già installato, usare il comando seguente:

```PowerShell
Install-WindowsFeature -Name NET-Framework-45-Core
```

## <a name="BKMK_BEST_PRAC"></a>Procedure consigliate per l'utilizzo di aggiornamento compatibile con cluster 

### <a name="BKMK_BP_WUA"></a>Suggerimenti per l'applicazione degli aggiornamenti Microsoft

Quando si inizia a usare aggiornamento compatibile con cluster per applicare gli aggiornamenti con il plug-in **Microsoft. WindowsUpdatePlugin** predefinito no__t-1in in un cluster, si smette di usare altri metodi per installare gli aggiornamenti software da Microsoft nei nodi del cluster.  

> [!CAUTION]  
> La combinazione di aggiornamento compatibile con cluster con metodi che aggiornano i singoli nodi automaticamente \(on una pianificazione a tempo fisso @ no__t-1 può causare risultati imprevedibili, tra cui interruzioni del servizio e tempi di inattività non pianificati.  

Si consiglia di attenersi alle seguenti linee guida:  

-   Per risultati ottimali, è consigliabile disabilitare le impostazioni relative all'aggiornamento automatico sui nodi del cluster, ad esempio tramite le impostazioni sull'aggiornamento automatico presenti nel Pannello di controllo oppure tramite le impostazioni configurate mediante Criteri di gruppo.  

    > [!CAUTION]  
    > L'installazione automatica di aggiornamenti nei nodi del cluster può interferire con l'installazione di aggiornamenti tramite Aggiornamento compatibile con cluster e determinarne la mancata riuscita.  

    Se gli aggiornamenti automatici sono indispensabili, le seguenti impostazioni sono compatibili con Aggiornamento compatibile con cluster, in quanto l'amministratore è in grado di controllare le tempistiche di installazione degli aggiornamenti.  

    -   Impostazioni con notifica prima del download e prima dell'installazione  

    -   Impostazioni con download automatico ma con notifica prima dell'installazione  

    Se la funzionalità Aggiornamenti automatici sta scaricando aggiornamenti contemporaneamente a un'operazione di aggiornamento di Aggiornamento compatibile con cluster, tuttavia, il completamento di tale operazione potrebbe richiedere più tempo del previsto.  

-   Non configurare un sistema di aggiornamento, ad esempio Windows Server Update Services \(WSUS @ no__t-1 per applicare gli aggiornamenti automaticamente \(on una pianificazione a tempo fisso @ no__t-3 nei nodi del cluster.  

-   Tutti i nodi del cluster devono essere configurati in modo uniforme per usare la stessa origine di aggiornamento, ad esempio un server WSUS, Windows Update o Microsoft Update.  

-   Se si utilizza un sistema di gestione della configurazione per applicare gli aggiornamenti software ai computer sulla rete, escludere i nodi cluster da tutti gli aggiornamenti richiesti o automatici. Tra gli esempi di sistemi di gestione della configurazione sono inclusi Microsoft System Center Configuration Manager 2007 e Microsoft System Center Virtual Machine Manager 2008.  

-   Se i server di distribuzione software interni @no__t 0per esempio, i server WSUS @ no__t-1 vengono usati per contenere e distribuire gli aggiornamenti, assicurarsi che tali server identifichino correttamente gli aggiornamenti approvati per i nodi del cluster.  

#### <a name="BKMK_PROXY"></a>Applicare gli aggiornamenti Microsoft negli scenari di succursale  
Per scaricare aggiornamenti da Microsoft Update o Windows Update sui nodi del cluster in determinati scenari di succursale, può essere necessario configurare impostazioni proxy per l'account di sistema locale su ciascun nodo. Ciò potrebbe rendersi necessario se i cluster della succursale, ad esempio, accedono a Microsoft Update o Windows Update per scaricare gli aggiornamenti usando un server proxy locale.  

Se necessario, configurare le impostazioni proxy WinHTTP su ogni nodo per specificare un server proxy locale e configurare le eccezioni degli indirizzi locali \(that è, un elenco di bypass per gli indirizzi locali @ no__t-1. A questo scopo, è possibile eseguire il comando seguente su ciascun nodo del cluster da un prompt dei comandi con privilegi elevati:  

```  
netsh winhttp set proxy <ProxyServerFQDN >:<port> "<local>"  
```  

dove <*ProxyServerFQDN*> è il nome di dominio completo per il server proxy e <*porta*> è la porta su cui comunicare (in genere la porta 443).  

Ad esempio, per configurare le impostazioni proxy WinHTTP per l'account di sistema locale specificando il server proxy *MyProxy.contoso.com*, con la porta 443 e le eccezioni degli indirizzi locali, digitare il comando seguente:  

```  
netsh winhttp set proxy MyProxy.CONTOSO.com:443 "<local>"  
```  

### <a name="BKMK_BP_HF"></a>Suggerimenti per l'uso di Microsoft. HotfixPlugin  

-   È consigliabile configurare le autorizzazioni nella cartella radice e nel file di configurazione degli aggiornamenti rapidi in modo da limitare l'accesso in scrittura ai soli amministratori locali dei computer in cui sono archiviati tali file. Ciò consente di prevenire la manomissione di questi file da parte di utenti non autorizzati che potrebbero compromettere il funzionamento del cluster di failover quando vengono applicati gli aggiornamenti rapidi.  

-   Per garantire l'integrità dei dati per le connessioni Server Message Block \(SMB @ no__t-1 utilizzate per accedere alla cartella radice degli aggiornamenti rapidi, è necessario configurare la crittografia SMB nella cartella condivisa SMB, se è possibile configurarla. Per **Microsoft.HotfixPlugin** è necessaria la configurazione della firma SMB o della crittografia SMB affinché l'integrità dei dati delle connessioni SMB sia garantita. 

    Per ulteriori informazioni, vedere [limitare l'accesso alla cartella radice degli aggiornamenti rapidi e al file di configurazione degli aggiornamenti rapidi](cluster-aware-updating-plug-ins.md#BKMK_ACL).

### <a name="additional-recommendations"></a>Suggerimenti aggiuntivi  

-   Per evitare di interferire con un'operazione di Aggiornamento compatibile con cluster che potrebbe essere stata pianificata, non programmare modifiche di password per oggetti nome cluster od oggetti computer virtuale durante le finestre di manutenzione pianificate.  

-   È necessario impostare le autorizzazioni appropriate sugli script pre @ no__t-0update e post @ no__t-1Update salvati in cartelle condivise di rete per impedire la possibile manomissione di questi file da parte di utenti non autorizzati.  

-   Per configurare aggiornamento compatibile con cluster in modalità self @ no__t-0updating, è necessario creare un oggetto computer virtuale \(VCO @ no__t-2 per il ruolo del cluster di aggiornamento compatibile con cluster nel Active Directory. Aggiornamento compatibile con cluster può creare questo oggetto automaticamente nel momento in cui viene aggiunto il relativo ruolo del cluster, purché il cluster di failover disponga delle sufficienti autorizzazioni. In alcune organizzazioni, tuttavia, a causa dei criteri di sicurezza in uso, potrebbe rendersi necessario pre-installare l'oggetto in Active Directory. Per informazioni su come eseguire questa operazione, vedere [Passaggi per eseguire la pre-installazione di un account per un ruolo del cluster](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731002\(v=ws.10\)#steps-for-prestaging-the-cluster-name-account).  

-   Per salvare e riutilizzare le impostazioni dell'operazione di aggiornamento su cluster di failover con esigenze di aggiornamento analoghe nell'organizzazione IT, è possibile creare profili delle operazioni di aggiornamento. Inoltre, in base alla modalità di aggiornamento, è possibile salvare e gestire i profili delle operazioni di aggiornamento in una condivisione file accessibile a tutti i computer con funzione di coordinatore dell'aggiornamento remoto o ai cluster di failover. Per altre informazioni, vedere [Opzioni avanzate e profili di esecuzione aggiornamento per aggiornamento compatibile con](cluster-aware-updating-options.md)cluster.  

## <a name="BKMK_BPA"></a>Verifica disponibilità aggiornamenti cluster  
È possibile eseguire il modello di aggiornamento compatibile con cluster Best Practices Analyzer \(BPA @ no__t-1 per verificare se un cluster di failover e l'ambiente di rete soddisfano molti dei requisiti per applicare gli aggiornamenti software da aggiornamento compatibile con cluster. Molti dei test verificano che l'ambiente sia idoneo per applicare gli aggiornamenti Microsoft usando il plug-in predefinito no__t-0cm, **Microsoft. WindowsUpdatePlugin**.  

> [!NOTE]  
> Potrebbe essere necessario verificare in modo indipendente che l'ambiente cluster sia pronto per applicare gli aggiornamenti software usando un plug @ no__t-0cm diverso da **Microsoft. WindowsUpdatePlugin**. Se si usa un plug-in non @ no__t-0Microsoft, ad esempio uno fornito dal produttore dell'hardware, contattare l'autore per ulteriori informazioni.  

BPA può essere eseguito nelle due modalità seguenti:  

1.  Selezionare **Analisi disponibilità del cluster per l'aggiornamento** nella console di Aggiornamento compatibile con cluster. Quando BPA completa i test di conformità, viene visualizzato un report di test. Se vengono rilevati problemi nei nodi del cluster, queste vengono identificate insieme ai nodi coinvolti per consentire azioni correttive. Per il completamento dei test potrebbero essere necessari diversi minuti.  

2.  Eseguire il cmdlet [test-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup) . È possibile eseguire il cmdlet in un computer locale o remoto in cui è installato il modulo clustering di failover per Windows PowerShell (parte degli strumenti di clustering di failover). Il cmdlet può inoltre essere eseguito in un nodo del cluster di failover.  

> [!NOTE]  
> -   È necessario utilizzare un account con privilegi amministrativi sui nodi del cluster e i privilegi di amministratore locale nel computer utilizzato per eseguire il cmdlet **test @ no__t-1CauSetup** o per analizzare la disponibilità del cluster per l'aggiornamento tramite il cluster-aware Aggiornamento della finestra. Per eseguire i test utilizzando la finestra aggiornamento compatibile con cluster, è necessario accedere al computer con le credenziali necessarie.  
> -   Per i test si presuppone che gli strumenti di Aggiornamento compatibile con cluster usati per l'anteprima e l'applicazione degli aggiornamenti software siano eseguiti con le stesse credenziali utente e dallo stesso computer usato per il test della disponibilità del cluster per l'aggiornamento.  

> [!IMPORTANT]  
> Il test del cluster per la disponibilità all'aggiornamento è fortemente consigliato nelle situazioni seguenti:  
>   
> -   Prima di usare Aggiornamento compatibile con cluster la prima volta per l'applicazione di aggiornamenti software.  
> -   Dopo l'aggiunta di un nodo al cluster o l'introduzione in esso di altre modifiche che richiedono l'esecuzione della procedura guidata di convalida del cluster.  
> -   Dopo la modifica di un'origine degli aggiornamenti o la modifica delle impostazioni o delle configurazioni di aggiornamento \(other di aggiornamento compatibile con cluster @ no__t-1 che può influire sull'applicazione degli aggiornamenti nei nodi.  

### <a name="tests-for-cluster-updating-readiness"></a>Test di disponibilità del cluster per l'aggiornamento  
Nella tabella seguente sono elencati i test di disponibilità del cluster per l'aggiornamento, alcuni problemi comuni e le procedure per risolverli.  


|                                                      Testa                                                      |                                                                                                                                               Possibili problemi ed effetti                                                                                                                                               |                                                                                                                                                                                         Passaggi di risoluzione                                                                                                                                                                                         |
|----------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                     Il cluster di failover deve essere disponibile                                     |                                                                                       Non è possibile risolvere il nome del cluster di failover o non è possibile accedere a uno o più nodi del cluster. BPA non è in grado di eseguire i test di disponibilità del cluster.                                                                                        |                                                                   -Controllare l'ortografia del nome del cluster specificato durante l'esecuzione di BPA.<br />-Assicurarsi che tutti i nodi del cluster siano online e in esecuzione.<br />-Verificare che la convalida guidata configurazione possa essere eseguita correttamente nel cluster di failover.                                                                    |
|                    I nodi del cluster di failover devono essere abilitati per la gestione remota tramite WMI                    |                                                Uno o più nodi del cluster di failover non sono abilitati per la gestione remota tramite Strumentazione gestione Windows \(WMI @ no__t-1. Aggiornamento compatibile con cluster non può aggiornare i nodi del cluster se questi non sono configurati per la gestione remota.                                                 |                                                                                                  Assicurarsi che tutti i nodi del cluster di failover siano abilitati per la gestione remota tramite WMI. Per altre informazioni, vedere [Configurare i nodi per la gestione remota](#BKMK_NODE_CONFIG) in questo argomento.                                                                                                   |
|                      La comunicazione remota di PowerShell deve essere abilitata in ogni nodo del cluster di failover                       |                                                           PowerShell non è installato o non è abilitato per la comunicazione remota in uno o più nodi del cluster di failover. Non è possibile configurare aggiornamento compatibile con cluster per la modalità self-no__t-0updating o usare determinate funzionalità in modalità remota @ no__t-1updating.                                                            |                                                                                             Verificare che PowerShell sia installato in tutti i nodi del cluster ed è abilitato per la comunicazione remota.<br /><br />Per altre informazioni, vedere [Configurare i nodi per la gestione remota](#BKMK_NODE_CONFIG) in questo argomento.                                                                                             |
|                                            Versione del cluster di failover                                            |                                                                            Uno o più nodi del cluster di failover non eseguono Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012. Aggiornamento compatibile con cluster non può aggiornare il cluster di failover.                                                                             |                                                                   Verificare che il cluster di failover specificato durante l'esecuzione di BPA esegua Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.<br /><br />Per altre informazioni, vedere [Verificare la configurazione del cluster](#BKMK_REQ_CLUS) in questo argomento.                                                                   |
| Le versioni richieste di .NET Framework e Windows PowerShell devono essere installate su tutti i nodi del cluster di failover |                                                                                              .NET Framework 4,6, 4,5 o Windows PowerShell non è installato in uno o più nodi del cluster. Alcune funzionalità di Aggiornamento compatibile con cluster potrebbero non funzionare.                                                                                              |                                                                            Verificare che .NET Framework 4,6 o 4,5 e Windows PowerShell siano installati in tutti i nodi del cluster, se necessario.<br /><br />Per altre informazioni, vedere [Configurare i nodi per la gestione remota](#BKMK_NODE_CONFIG) in questo argomento.                                                                             |
|                           Il servizio cluster deve essere in esecuzione in tutti i nodi del cluster                           |                                                                                                            Il servizio cluster non è in esecuzione in uno o più nodi del cluster. Aggiornamento compatibile con cluster non può aggiornare il cluster di failover.                                                                                                             |                        -Verificare che il Servizio cluster \(clussvc @ no__t-1 venga avviato in tutti i nodi del cluster e che sia configurato per l'avvio automatico.<br />-Verificare che la convalida guidata configurazione possa essere eseguita correttamente nel cluster di failover.<br /><br />Per altre informazioni, vedere [Verificare la configurazione del cluster](#BKMK_REQ_CLUS) in questo argomento.                         |
|     Aggiornamenti automatici non deve essere configurato per installare automaticamente aggiornamenti in nessuno dei nodi del cluster di failover.     |                                           Almeno su un nodo del cluster di failover, Aggiornamenti automatici è configurato per installare aggiornamenti Microsoft automaticamente su quel nodo. La combinazione di Aggiornamenti compatibili con cluster con altri metodi di aggiornamento può dare origine a tempi di inattività imprevisti o a risultati imprevedibili.                                            |                                                     Se la funzionalità Windows Update è configurata per gli aggiornamenti automatici su uno o più nodi del cluster, assicurarsi che Aggiornamenti automatici non sia configurato per l'installazione automatica degli aggiornamenti.<br /><br />Per altre informazioni, vedere [Consigli per l'applicazione degli aggiornamenti Microsoft](#BKMK_BP_WUA).                                                     |
|                          I nodi del cluster di failover devono usare la stessa origine degli aggiornamenti                          |                                                    Uno o più nodi del cluster di failover sono configurati per usare un'origine di aggiornamento per gli aggiornamenti Microsoft diversa da quella usata dal resto dei nodi. L'applicazione degli aggiornamenti sui nodi del cluster da parte di Aggiornamento compatibile con cluster potrebbe non essere uniforme.                                                    |                                                                        Assicurarsi che ogni nodo del cluster sia configurato per usare la stessa origine di aggiornamento, ad esempio un server WSUS, Windows Update o Microsoft Update.<br /><br />Per altre informazioni, vedere [Consigli per l'applicazione degli aggiornamenti Microsoft](#BKMK_BP_WUA).                                                                         |
|       È necessario che in ogni nodo del cluster di failover sia configurata una regola del firewall che consenta l'arresto remoto       |                 In uno o più nodi del cluster non è presente o abilitata una regola del firewall che consenta l'arresto remoto, oppure un'impostazione di Criteri di gruppo impedisce l'abilitazione di tale regola. Un'operazione di aggiornamento che applica aggiornamenti che richiedono il riavvio automatico dei nodi potrebbe non essere completata correttamente.                  |                                                                    Se Windows Firewall o un firewall non @ no__t-0Microsoft è in uso nei nodi del cluster, configurare una regola del firewall che consenta l'arresto remoto.<br /><br />Per altre informazioni, vedere [Abilitare una regola del firewall per consentire i riavvii automatici](#BKMK_FW) in questo argomento.                                                                    |
|          L'impostazione relativa al server proxy in ognuno dei nodi di failover deve essere impostata su un server proxy locale          |                             Uno o più nodi del cluster di failover presentano una configurazione di server proxy non corretta.<br /><br />Se è in uso un server proxy locale, le relative impostazioni su ogni nodo devono essere configurate correttamente per l'accesso del cluster a Microsoft Update o Windows Update.                              |                                            Assicurarsi che le impostazioni proxy WinHTTP su ogni nodo del cluster siano configurate su un server proxy locale, se necessario. Se nell'ambiente non è in uso un server proxy, questo avviso può essere ignorato.<br /><br />Per altre informazioni, vedere [Applicare aggiornamenti Microsoft in scenari di succursale](#BKMK_PROXY) in questo argomento.                                            |
|        Il ruolo del cluster di aggiornamento compatibile con cluster deve essere installato nel cluster di failover per abilitare la modalità self-no__t-0updating        |                                                                                                   Il ruolo del cluster di Aggiornamento compatibile con cluster non è installato nel cluster di failover. Questo ruolo è necessario per il cluster self @ no__t-0updating.                                                                                                   |      Per usare aggiornamento compatibile con cluster in modalità self @ no__t-0updating, aggiungere il ruolo del cluster di aggiornamento compatibile con cluster nel cluster di failover di in uno dei modi seguenti:<br /><br />-Eseguire il cmdlet di PowerShell [Add-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole) .<br />-Selezionare l'azione **Configura cluster self @ no__t-1updating Options** nella finestra di aggiornamento compatibile con cluster.      |
|         Per abilitare la modalità self-no__t-0updating, è necessario abilitare il ruolo del cluster di aggiornamento compatibile con cluster nel cluster di failover         | Il ruolo del cluster di Aggiornamento compatibile con cluster è disabilitato. Ad esempio, il ruolo del cluster di aggiornamento compatibile con cluster non è installato o è stato disabilitato usando il cmdlet di PowerShell [Disable @ no__t-1CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole) . Questo ruolo è necessario per il cluster self @ no__t-0updating. | Per usare aggiornamento compatibile con cluster in modalità self-no__t-0updating, abilitare il ruolo del cluster di aggiornamento compatibile con cluster in questo cluster di failover in uno dei modi seguenti:<br /><br />-Eseguire il cmdlet di PowerShell [Enable-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole) .<br />-Selezionare l'azione **Configura cluster self @ no__t-1updating Options** nella finestra di aggiornamento compatibile con cluster. |
|      Il plug-in di aggiornamento compatibile con cluster no__t-0cm per la modalità self-no__t-1updating deve essere registrato su tutti i nodi del cluster di failover      |                                                              Il ruolo del cluster di aggiornamento compatibile con cluster in uno o più nodi del cluster di failover non può accedere al modulo plug @ no__t-0cm di aggiornamento compatibile con cluster configurato nelle opzioni self @ no__t-1updating. Un'esecuzione self @ no__t-0updating potrebbe non riuscire.                                                              |           -Verificare che il plug-in di aggiornamento compatibile con cluster configurato per no__t-0cm sia installato in tutti i nodi del cluster seguendo la procedura di installazione del prodotto che fornisce il plug-in di aggiornamento compatibile con cluster @ no__t-1in.<br />-Eseguire il cmdlet [Register @ no__t-1CauPlugin](https://docs.microsoft.com/powershell/module/clusterawareupdating/Register-CauPlugin) di PowerShell per registrare il plug @ no__t-2in nei nodi del cluster richiesti.           |
|                Tutti i nodi del cluster di failover devono avere lo stesso set di plug-in di aggiornamento compatibile con cluster registrati @ no__t-0ins                 |                                                                             Un'esecuzione self @ no__t-0updating potrebbe non riuscire se il plug @ no__t-1in configurato per l'uso in un'operazione di aggiornamento viene modificato in uno non disponibile su tutti i nodi del cluster.                                                                              |           -Verificare che il plug-in di aggiornamento compatibile con cluster configurato per no__t-0cm sia installato in tutti i nodi del cluster seguendo la procedura di installazione del prodotto che fornisce il plug-in di aggiornamento compatibile con cluster @ no__t-1in.<br />-Eseguire il cmdlet [Register @ no__t-1CauPlugin](https://docs.microsoft.com/powershell/module/clusterawareupdating/Register-CauPlugin) di PowerShell per registrare il plug @ no__t-2in nei nodi del cluster richiesti.           |
|                               Le opzioni relative all'operazione di aggiornamento configurate devono essere valide                                |                                                                          Le opzioni di pianificazione e aggiornamento self-no__t-0updating configurate per questo cluster di failover sono incomplete o non valide. Un'esecuzione self @ no__t-0updating potrebbe non riuscire.                                                                           |                                                            Configurare una pianificazione self @ no__t-0updating valida e un set di opzioni per l'esecuzione dell'aggiornamento. Ad esempio, è possibile usare il cmdlet di PowerShell [set @ no__t-1CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Set-CauClusterRole) per configurare il ruolo del cluster di aggiornamento compatibile con cluster.                                                            |
|                  È necessario che almeno due nodi del cluster di failover siano proprietari del ruolo del cluster di Aggiornamento compatibile con cluster.                  |                                                                                        Un'operazione di aggiornamento avviata in modalità self-no__t-0updating avrà esito negativo perché il ruolo del cluster di aggiornamento compatibile con cluster non dispone di un nodo proprietario possibile a cui spostarsi.                                                                                         |                                                                                                                Usare Strumenti per Clustering di failover per assicurarsi che tutti i nodi del cluster siano configurati come possibili proprietari del ruolo del cluster di Aggiornamento compatibile con cluster. Questa è la configurazione predefinita.                                                                                                                |
|                  Tutti i nodi del cluster di failover devono poter accedere agli script di Windows PowerShell                  |                                                                        Non tutti i possibili nodi proprietari del ruolo del cluster di aggiornamento compatibile con cluster possono accedere agli script di Windows PowerShell pre @ no__t-0update e post @ no__t-1Update configurati. Un'esecuzione self @ no__t-0updating avrà esito negativo.                                                                        |                                                                                                                    Assicurarsi che tutti i possibili nodi proprietari del ruolo del cluster di aggiornamento compatibile con cluster dispongano delle autorizzazioni per accedere agli script pre @ no__t-0update e post @ no__t-1Update di PowerShell configurati.                                                                                                                     |
|                   Tutti i nodi del cluster di failover devono usare script di Windows PowerShell identici                   |                                                     Non tutti i possibili nodi proprietari del ruolo del cluster di aggiornamento compatibile con cluster usano la stessa copia degli script pre @ no__t-0update e post @ no__t-1Update specificati di Windows PowerShell. Un'esecuzione self @ no__t-0updating potrebbe non riuscire o mostrare un comportamento imprevisto.                                                     |                                                                                                                                   Assicurarsi che tutti i possibili nodi proprietari del ruolo del cluster di aggiornamento compatibile con cluster usino gli stessi script di PowerShell pre @ no__t-0update e post @ no__t-1Update.                                                                                                                                   |
|         Il valore dell'impostazione WarnAfter specificato per l'operazione di aggiornamento deve essere minore di quello dell'impostazione StopAfter         |                                                                           I valori di timeout specificati per l'operazione di aggiornamento di Aggiornamento compatibile con cluster rendono il timeout dell'avviso inefficace. Un'operazione di aggiornamento potrebbe venire annullata prima che sia possibile generare un registro eventi di avviso.                                                                            |                                                                                                                                      Nelle opzioni relative all'operazione di aggiornamento, configurare un valore dell'opzione **WarnAfter** inferiore a quello per l'opzione **StopAfter** .                                                                                                                                       |

## <a name="see-also"></a>Vedere anche  

-   [Panoramica di aggiornamento compatibile con cluster](cluster-aware-updating.md)