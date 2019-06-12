---
ms.assetid: 75cc1d24-fa2f-45bd-8f3b-1bbd4a1aead0
title: Requisiti di aggiornamento compatibile con cluster e le procedure consigliate
ms.prod: windows-server-threshold
ms.topic: article
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 08/06/2018
description: Requisiti per l'utilizzo di aggiornamento compatibile con Cluster per installare gli aggiornamenti nei cluster che esegue Windows Server.
ms.openlocfilehash: 32fa77ca2c24d75347d61f9bdeb4c5c93a20d89d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439687"
---
# <a name="cluster-aware-updating-requirements-and-best-practices"></a>Requisiti di aggiornamento compatibile con cluster e le procedure consigliate

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questa sezione vengono descritti i requisiti e dipendenze necessari per utilizzare [aggiornamento compatibile con Cluster](cluster-aware-updating.md) (aggiornamento compatibile con cluster) per applicare gli aggiornamenti a un cluster di failover che esegue Windows Server.

> [!NOTE]  
> Potrebbe essere necessario verificare in modo indipendente che l'ambiente cluster sia pronto per applicare gli aggiornamenti se si usa un plug\-negli altri rispetto **Microsoft. windowsupdateplugin**. Se si usa un non\-plug Microsoft\-, contattare il server di pubblicazione per ulteriori informazioni. Per altre informazioni sui plug\-i componenti aggiuntivi, vedere [come collegare\-aggiuntivi funzionano](cluster-aware-updating-plug-ins.md).   

## <a name="BKMK_REQ_CLUS"></a>Installare la funzionalità Clustering di Failover e strumenti per Clustering di Failover  
Aggiornamento compatibile con cluster richiede l'installazione della funzionalità Clustering di failover e degli Strumenti per Clustering di failover. Strumenti per Clustering di Failover includono gli strumenti di aggiornamento compatibile con cluster \(clusterawareupdating\), i cmdlet Clustering di Failover e altri componenti necessari per le operazioni di aggiornamento compatibile con cluster. Per i passaggi da eseguire per installare la funzionalità Clustering di failover, vedere l'articolo relativo all' [installazione della funzionalità Clustering di failover e dei relativi strumenti](create-failover-cluster.md#install-the-failover-clustering-feature).  

I requisiti di installazione esatto per gli strumenti per Clustering di Failover variano a seconda se aggiornamento compatibile con cluster coordina gli aggiornamenti come un ruolo del cluster nel cluster di failover \(mediante self\-modalità di aggiornamento\) o da un computer remoto. Il self\-inoltre alla modalità di aggiornamento compatibile con cluster di aggiornamento richiede l'installazione del ruolo del cluster aggiornamento compatibile con cluster nel cluster di failover usando gli strumenti di aggiornamento compatibile con cluster.    

Nella tabella seguente vengono riepilogati i requisiti di installazione della funzionalità Aggiornamento compatibile con cluster per le due modalità di aggiornamento.  

|Componente installato|Self\-modalità di aggiornamento|Remote\-modalità di aggiornamento|  
|-----------------------|-----------------------|-------------------------|  
|Funzionalità Clustering di failover|Obbligatori su tutti i nodi del cluster|Obbligatori su tutti i nodi del cluster|  
|Strumenti per Clustering di failover|Obbligatori su tutti i nodi del cluster|-Obbligatorio in remoto\-l'aggiornamento di computer<br />-Obbligatori su tutti i nodi del cluster per eseguire la [Save-CauDebugTrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps) cmdlet|  
|Ruolo del cluster di Aggiornamento compatibile con cluster|Obbligatorio|Non obbligatorio|  

## <a name="obtain-an-administrator-account"></a>Ottenere un account amministratore  
Per usare le funzionalità di Aggiornamento compatibile con cluster sono necessari i requisiti amministrativi seguenti.  

-   Visualizzare in anteprima o applicare azioni di aggiornamento usando l'interfaccia utente di aggiornamento compatibile con cluster \(dell'interfaccia utente\) o i cmdlet di aggiornamento compatibile con Cluster, è necessario usare un account di dominio che dispone di autorizzazioni e diritti di amministratore locale su tutti i nodi del cluster. Se l'account non ha privilegi sufficienti in ogni nodo, richiesto nella finestra di aggiornamento compatibile con Cluster per fornire le credenziali necessarie quando si eseguono queste azioni. Usare la [aggiornamento compatibile con Cluster](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps) cmdlet, è possibile fornire le credenziali necessarie come parametro del cmdlet.  

-   Se si usa aggiornamento compatibile con cluster in remoto\-modalità di aggiornamento quando si è connessi con un account che non ha le autorizzazioni e diritti di amministratore locale nei nodi del cluster, è necessario eseguire gli strumenti di aggiornamento compatibile con cluster come amministratore usando un account amministratore locale di Computer coordinatore dell'aggiornamento oppure usando un account con il **rappresenta un client dopo l'autenticazione** diritto utente. 

-   Per eseguire Best Practices Analyzer di aggiornamento compatibile con cluster, è necessario usare un account con privilegi amministrativi sui nodi del cluster e privilegi di amministratore locale nel computer in cui viene usato per eseguire la [Test-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) cmdlet o per analizzare cluster di disponibilità per l'uso della finestra di aggiornamento compatibile con Cluster. Per altre informazioni, vedere [Test della disponibilità del cluster per l'aggiornamento](#BKMK_BPA).  

## <a name="verify-the-cluster-configuration"></a>Verificare la configurazione del cluster  
Di seguito vengono riportati i requisiti generali di un cluster di failover che supporti aggiornamenti applicati mediante Aggiornamento compatibile con cluster. Un elenco di requisiti di configurazione aggiuntivi per la gestione remota dei nodi è fornito nella sezione [Configurare i nodi per la gestione remota](#BKMK_NODE_CONFIG) più avanti in questo argomento.  

-   Affinché il cluster disponga di quorum, deve trovarsi online un numero sufficiente di nodi del cluster.  

-   Tutti i nodi del cluster devono trovarsi nello stesso dominio Active Directory.  

-   La risoluzione del nome del cluster nella rete deve avvenire mediante DNS.  

-   Se aggiornamento compatibile con cluster viene usato in remoto\-modalità di aggiornamento, il computer coordinatore dell'aggiornamento deve avere connettività di rete ai nodi del cluster di failover e deve essere nello stesso dominio Active Directory come cluster di failover.  

-   Il servizio cluster deve essere in esecuzione in tutti i nodi del cluster. Per impostazione predefinita, il servizio è installato in tutti i nodi del cluster ed è configurato per essere avviato automaticamente.  

-   Usare PowerShell pre\-aggiornare o inserire\-gli script di aggiornamento durante un'operazione di aggiornamento, assicurarsi che gli script vengono installati in tutti i nodi del cluster o che siano accessibili da tutti i nodi, ad esempio, in una condivisione file di rete a disponibilità elevata. Se gli script sono salvati in una condivisione file di rete, configurare la relativa cartella per concedere l'autorizzazione di lettura al gruppo Everyone.  

## <a name="BKMK_NODE_CONFIG"></a>Configurare i nodi per la gestione remota  
Per usare aggiornamento compatibile con Cluster, tutti i nodi del cluster devono essere configurati per la gestione remota. Per impostazione predefinita, l'unica attività da eseguire per configurare i nodi di gestione remota consiste [abilitare una regola del firewall consentire i riavvii automatici](#BKMK_FW). 

Nella tabella seguente elenca i requisiti di gestione remoto completa, nel caso in cui l'ambiente differisce da quella predefinita.

Tali requisiti si aggiungono ai requisiti di installazione descritti in [Installare la funzionalità Clustering di failover e gli Strumenti per Clustering di failover](#BKMK_REQ_CLUS) e ai requisiti generali del cluster descritti nelle sezioni precedenti di questo argomento.  

|Requisito|Stato predefinito|Self\-modalità di aggiornamento|Remote\-modalità di aggiornamento|  
|---------------|---|-----------------------|-------------------------|  
|[Abilitare una regola del firewall consentire i riavvii automatici](#BKMK_FW)|Disabled|Obbligatorio in tutti i nodi del cluster se un firewall è in uso|Obbligatorio in tutti i nodi del cluster se un firewall è in uso|  
|[Abilitare la Strumentazione gestione Windows](#BKMK_WMI)|Enabled|Obbligatori su tutti i nodi del cluster|Obbligatori su tutti i nodi del cluster|  
|[Abilitare Windows PowerShell 3.0 o 4.0 e comunicazione remota di Windows PowerShell](#BKMK_PS)|Enabled|Obbligatori su tutti i nodi del cluster|Obbligatorio su tutti i nodi del cluster per l'esecuzione dei componenti seguenti:<br /><br />-il [Save-CauDebugTrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps) cmdlet<br />-Pre PowerShell\-aggiornamento e post\-durante un'operazione di aggiornamento<br />-I test di disponibilità tramite la finestra di aggiornamento compatibile con Cluster di aggiornamento del cluster o il [Test\-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) cmdlet di Windows PowerShell|  
|[Installare .NET Framework 4.6 o 4.5](#BKMK_NET)|Enabled|Obbligatori su tutti i nodi del cluster|Obbligatorio su tutti i nodi del cluster per l'esecuzione dei componenti seguenti:<br /><br />-il [Save-CauDebugTrace](https://docs.microsoft.com/powershell/module/clusterawareupdating/Save-CauDebugTrace?view=win10-ps) cmdlet<br />-Pre PowerShell\-aggiornamento e post\-durante un'operazione di aggiornamento<br />-I test di disponibilità tramite la finestra di aggiornamento compatibile con Cluster di aggiornamento del cluster o il [Test\-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup?view=win10-ps) cmdlet di Windows PowerShell|  

### <a name="BKMK_FW"></a>Abilitare una regola del firewall consentire i riavvii automatici  
Per consentire i riavvii automatici dopo gli aggiornamenti vengono applicati \(se l'installazione di un aggiornamento richiede un riavvio\), se Windows Firewall o un non\-firewall Microsoft è in uso nei nodi del cluster, deve essere abilitata una regola firewall ogni nodo che consenta il traffico seguente:  

-   Protocol: TCP  

-   Direzione: in ingresso  

-   Programma: wininit.exe  

-   Porte: Porte dinamiche RPC  

-   Profilo: Dominio  

Se Windows Firewall viene usato sui nodi del cluster, l'operazione può essere eseguita abilitando il gruppo di regole **Arresto remoto** di Windows Firewall su ogni nodo del cluster. Quando si usa la finestra di aggiornamento compatibile con Cluster per applicare gli aggiornamenti e per configurare self\-aggiornamento delle opzioni, il **Remote Shutdown** gruppo di regole Windows Firewall viene abilitato automaticamente in ogni nodo del cluster.  

> [!NOTE]  
> Non è possibile abilitare il gruppo di regole **Remote Shutdown** di Windows Firewall se questo crea conflitti con le impostazioni di Criteri di gruppo configurate per Windows Firewall.    

Il **Remote Shutdown** gruppo di regole firewall è abilitato anche specificando la **– EnableFirewallRules** parametro quando si esegue il cmdlet di aggiornamento compatibile con cluster seguenti: [Aggiungere-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole?view=win10-ps), [Invoke-CauRun](https://docs.microsoft.com/powershell/module/clusterawareupdating/Invoke-CauRun?view=win10-ps), e [SetCauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Set-CauClusterRole?view=win10-ps).  

Nell'esempio di PowerShell seguente illustra un metodo aggiuntivo per abilitare i riavvii automatici in un nodo del cluster.  

```PowerShell  
Set-NetFirewallRule -Group "@firewallapi.dll,-36751" -Profile Domain -Enabled true  
```  

### <a name="BKMK_WMI"></a>Abilitare la Strumentazione gestione Windows (WMI) 
Tutti i nodi del cluster devono essere configurati per la gestione remota mediante Strumentazione gestione Windows \(WMI\). L'impostazione è abilitata per impostazione predefinita.  

Per abilitare la gestione remota manualmente, procedere come segue:  

1.  Nella console Servizi, avviare il servizio **Gestione remota Windows** e impostare il tipo di avvio su **Automatico**.  

2.  Eseguire la [Set-WSManQuickConfig](https://docs.microsoft.com/powershell/module/Microsoft.WsMan.Management/Set-WSManQuickConfig?view=powershell-6) prompt cmdlet, o eseguire il comando seguente da un comando con privilegi elevato:  

    ```PowerShell  
    winrm quickconfig -q  
    ```  

Per supportare la comunicazione remota WMI, se Windows Firewall è in uso nei nodi del cluster, regola del firewall in ingresso per **gestione remota Windows \(HTTP\-nelle\)**  deve essere abilitata su ciascun nodo.  Questa regola è abilitata per impostazione predefinita.  

### <a name="BKMK_PS"></a>Abilitare Windows PowerShell e comunicazione remota di Windows PowerShell  
Per abilitare self\-l'aggiornamento di alcune funzionalità di aggiornamento compatibile con cluster in remoto e modalità\-modalità di aggiornamento, PowerShell deve essere installato e abilitato per eseguire comandi remoti su tutti i nodi del cluster. Per impostazione predefinita, PowerShell viene installato e abilitato alla comunicazione remota.  

Per abilitare la comunicazione remota di PowerShell, usare uno dei metodi seguenti:  

-   Eseguire la [Enable-PSRemoting](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Core/Enable-PSRemoting) cmdlet.  

-   Configurare un dominio\-impostazione di criteri di gruppo per la gestione remota Windows di livello \(WinRM\).  

Per altre informazioni sull'abilitazione della comunicazione remota di PowerShell, vedere [About_remote_requirements](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_remote_requirements?view=powershell-6).  

### <a name="BKMK_NET"></a>Installare .NET Framework 4.6 o 4.5  
Per abilitare self\-l'aggiornamento di alcune funzionalità di aggiornamento compatibile con cluster in remoto e modalità\-aggiornamento modalità, .NET Framework 4.6 o .NET Framework 4.5 (in Windows Server 2012 R2) deve essere installato in tutti i nodi del cluster. Per impostazione predefinita, viene installato .NET Framework.  

Per installare .NET Framework 4.6 (o 4.5) tramite PowerShell, se non è già installato, usare il comando seguente:

```PowerShell
Install-WindowsFeature -Name NET-Framework-45-Core
```

## <a name="BKMK_BEST_PRAC"></a>Procedure consigliate per l'uso di aggiornamento compatibile con Cluster 

### <a name="BKMK_BP_WUA"></a>Consigli per l'applicazione degli aggiornamenti Microsoft

È consigliabile che, quando si inizia a usare aggiornamento compatibile con cluster per applicare gli aggiornamenti con il valore predefinito **Microsoft. windowsupdateplugin** collegare\-in un cluster, deve essere arrestato utilizzando altri metodi di installazione degli aggiornamenti software da Microsoft nel cluster nodi.  

> [!CAUTION]  
> La combinazione di aggiornamento compatibile con cluster con i metodi che aggiornano automaticamente i singoli nodi \(in una pianificazione temporale fissa\) può causare risultati imprevedibili, inclusi interruzioni del servizio e tempi di inattività non pianificato.  

Si consiglia di attenersi alle seguenti linee guida:  

-   Per risultati ottimali, è consigliabile disabilitare le impostazioni relative all'aggiornamento automatico sui nodi del cluster, ad esempio tramite le impostazioni sull'aggiornamento automatico presenti nel Pannello di controllo oppure tramite le impostazioni configurate mediante Criteri di gruppo.  

    > [!CAUTION]  
    > L'installazione automatica di aggiornamenti nei nodi del cluster può interferire con l'installazione di aggiornamenti tramite Aggiornamento compatibile con cluster e determinarne la mancata riuscita.  

    Se gli aggiornamenti automatici sono indispensabili, le seguenti impostazioni sono compatibili con Aggiornamento compatibile con cluster, in quanto l'amministratore è in grado di controllare le tempistiche di installazione degli aggiornamenti.  

    -   Impostazioni con notifica prima del download e prima dell'installazione  

    -   Impostazioni con download automatico ma con notifica prima dell'installazione  

    Se la funzionalità Aggiornamenti automatici sta scaricando aggiornamenti contemporaneamente a un'operazione di aggiornamento di Aggiornamento compatibile con cluster, tuttavia, il completamento di tale operazione potrebbe richiedere più tempo del previsto.  

-   Non si configura un sistema di aggiornamento, ad esempio Windows Server Update Services \(WSUS\) per applicare gli aggiornamenti automaticamente \(in una pianificazione temporale fissa\) ai nodi del cluster.  

-   Tutti i nodi del cluster devono essere configurati in modo uniforme per usare la stessa origine di aggiornamento, ad esempio un server WSUS, Windows Update o Microsoft Update.  

-   Se si utilizza un sistema di gestione della configurazione per applicare gli aggiornamenti software ai computer sulla rete, escludere i nodi cluster da tutti gli aggiornamenti richiesti o automatici. Tra gli esempi di sistemi di gestione della configurazione sono inclusi Microsoft System Center Configuration Manager 2007 e Microsoft System Center Virtual Machine Manager 2008.  

-   Se il server di distribuzione software interni \(ad esempio, i server WSUS\) vengono utilizzati per contenere e distribuire gli aggiornamenti, assicurarsi che tali server identifichino correttamente gli aggiornamenti approvati per i nodi del cluster.  

#### <a name="BKMK_PROXY"></a>Applicare aggiornamenti Microsoft in scenari di succursale  
Per scaricare aggiornamenti da Microsoft Update o Windows Update sui nodi del cluster in determinati scenari di succursale, può essere necessario configurare impostazioni proxy per l'account di sistema locale su ciascun nodo. Ciò potrebbe rendersi necessario se i cluster della succursale, ad esempio, accedono a Microsoft Update o Windows Update per scaricare gli aggiornamenti usando un server proxy locale.  

Se necessario, configurare le impostazioni proxy WinHTTP su ogni nodo per specificare un server proxy locale e configurare eccezioni per indirizzi locali \(, ovvero un elenco di esclusione per gli indirizzi locali\). A questo scopo, è possibile eseguire il comando seguente su ciascun nodo del cluster da un prompt dei comandi con privilegi elevati:  

```  
netsh winhttp set proxy <ProxyServerFQDN >:<port> "<local>"  
```  

dove <*ProxyServerFQDN*> è il nome di dominio completo per il server proxy e <*porta*> è la porta su cui comunicare (in genere la porta 443).  

Ad esempio, per configurare le impostazioni proxy WinHTTP per l'account sistema locale specificando il server proxy *MyProxy.CONTOSO.com*, con la porta 443 ed eccezioni per indirizzi locali, digitare il comando seguente:  

```  
netsh winhttp set proxy MyProxy.CONTOSO.com:443 "<local>"  
```  

### <a name="BKMK_BP_HF"></a>Suggerimenti per l'uso di Microsoft. hotfixplugin  

-   È consigliabile configurare le autorizzazioni nella cartella radice e nel file di configurazione degli aggiornamenti rapidi in modo da limitare l'accesso in scrittura ai soli amministratori locali dei computer in cui sono archiviati tali file. Ciò consente di prevenire la manomissione di questi file da parte di utenti non autorizzati che potrebbero compromettere il funzionamento del cluster di failover quando vengono applicati gli aggiornamenti rapidi.  

-   Al fine di garantire l'integrità dei dati per server message block \(SMB\) le connessioni utilizzate per accedere alla cartella radice degli aggiornamenti rapidi, è necessario configurare la crittografia SMB nella cartella SMB condivisa, se è possibile configurarla. Per **Microsoft.HotfixPlugin** è necessaria la configurazione della firma SMB o della crittografia SMB affinché l'integrità dei dati delle connessioni SMB sia garantita. 

    Per altre informazioni, vedere [limitare l'accesso alla cartella radice degli aggiornamenti rapidi e al file di configurazione degli aggiornamenti rapidi](cluster-aware-updating-plug-ins.md#BKMK_ACL).

### <a name="additional-recommendations"></a>Suggerimenti aggiuntivi  

-   Per evitare di interferire con un'operazione di Aggiornamento compatibile con cluster che potrebbe essere stata pianificata, non programmare modifiche di password per oggetti nome cluster od oggetti computer virtuale durante le finestre di manutenzione pianificate.  

-   È necessario impostare le autorizzazioni appropriate su pre\-aggiornamento e post\-gli script di aggiornamento che vengono salvati nella rete condivisi cartelle per impedire eventuali manomissioni potenziale di questi file da utenti non autorizzati.  

-   Configurare aggiornamento compatibile con cluster in self\-modalità, un oggetto computer virtuale di aggiornamento \(VCO\) di aggiornamento compatibile con cluster ruolo del cluster deve essere creato in Active Directory. Aggiornamento compatibile con cluster può creare questo oggetto automaticamente nel momento in cui viene aggiunto il relativo ruolo del cluster, purché il cluster di failover disponga delle sufficienti autorizzazioni. In alcune organizzazioni, tuttavia, a causa dei criteri di sicurezza in uso, potrebbe rendersi necessario pre-installare l'oggetto in Active Directory. Per informazioni su come eseguire questa operazione, vedere [Passaggi per eseguire la pre-installazione di un account per un ruolo del cluster](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731002\(v=ws.10\)#steps-for-prestaging-the-cluster-name-account).  

-   Per salvare e riutilizzare le impostazioni dell'operazione di aggiornamento su cluster di failover con esigenze di aggiornamento analoghe nell'organizzazione IT, è possibile creare profili delle operazioni di aggiornamento. Inoltre, in base alla modalità di aggiornamento, è possibile salvare e gestire i profili delle operazioni di aggiornamento in una condivisione file accessibile a tutti i computer con funzione di coordinatore dell'aggiornamento remoto o ai cluster di failover. Per altre informazioni, vedere [opzioni avanzate e l'aggiornamento di profili di esecuzione per aggiornamento compatibile con cluster](cluster-aware-updating-options.md).  

## <a name="BKMK_BPA"></a>Aggiornamento dello stato di preparazione cluster di test  
È possibile eseguire l'aggiornamento compatibile con cluster Best Practices Analyzer \(Best Practices Analyzer\) modello per verificare se un cluster di failover e l'ambiente di rete soddisfano molti dei requisiti necessari per gli aggiornamenti software applicati da aggiornamento compatibile con cluster. Molti di questi test verificano l'ambiente per la conformità applicare aggiornamenti Microsoft usando l'impostazione predefinita plug\-, in **Microsoft. windowsupdateplugin**.  

> [!NOTE]  
> Potrebbe essere necessario verificare in modo indipendente che l'ambiente cluster sia pronto per applicare gli aggiornamenti software usando un plug\-negli altri rispetto **Microsoft. windowsupdateplugin**. Se si usa un non\-plug Microsoft\-, ad esempio quella fornita dal produttore dell'hardware, contattare il server di pubblicazione per ulteriori informazioni.  

BPA può essere eseguito nelle due modalità seguenti:  

1.  Selezionare **Analisi disponibilità del cluster per l'aggiornamento** nella console di Aggiornamento compatibile con cluster. Dopo il completamento dei test di disponibilità, verrà visualizzato un report di test. Se vengono rilevati problemi nei nodi del cluster, queste vengono identificate insieme ai nodi coinvolti per consentire azioni correttive. Per il completamento dei test potrebbero essere necessari diversi minuti.  

2.  Eseguire la [Test-CauSetup](https://docs.microsoft.com/powershell/module/clusterawareupdating/Test-CauSetup) cmdlet. È possibile eseguire il cmdlet in un computer locale o remoto in cui è installato il Failover Clustering modulo per Windows PowerShell (parte di strumenti per Clustering di Failover). Il cmdlet può inoltre essere eseguito in un nodo del cluster di failover.  

> [!NOTE]  
> -   È necessario usare un account con privilegi amministrativi sui nodi del cluster e privilegi di amministratore locale nel computer in cui viene usato per eseguire la **Test\-CauSetup** cmdlet o per analizzare la conformità di aggiornamento del cluster uso della finestra di aggiornamento compatibile con Cluster. Per eseguire i test tramite la finestra di aggiornamento compatibile con Cluster, è necessario essere connessi al computer con le credenziali necessarie.  
> -   Per i test si presuppone che gli strumenti di Aggiornamento compatibile con cluster usati per l'anteprima e l'applicazione degli aggiornamenti software siano eseguiti con le stesse credenziali utente e dallo stesso computer usato per il test della disponibilità del cluster per l'aggiornamento.  

> [!IMPORTANT]  
> Il test del cluster per la disponibilità all'aggiornamento è fortemente consigliato nelle situazioni seguenti:  
>   
> -   Prima di usare Aggiornamento compatibile con cluster la prima volta per l'applicazione di aggiornamenti software.  
> -   Dopo l'aggiunta di un nodo al cluster o l'introduzione in esso di altre modifiche che richiedono l'esecuzione della procedura guidata di convalida del cluster.  
> -   Dopo che si modifica un'origine degli aggiornamenti o modificare le impostazioni di aggiornamento o le configurazioni \(diverso da aggiornamento compatibile con cluster\) che possono interessare l'applicazione degli aggiornamenti nei nodi.  

### <a name="tests-for-cluster-updating-readiness"></a>Test di disponibilità del cluster per l'aggiornamento  
Nella tabella seguente sono elencati i test di disponibilità del cluster per l'aggiornamento, alcuni problemi comuni e le procedure per risolverli.  


|                                                      Testa                                                      |                                                                                                                                               Possibili problemi ed effetti                                                                                                                                               |                                                                                                                                                                                         Passaggi di risoluzione                                                                                                                                                                                         |
|----------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                     Il cluster di failover deve essere disponibile                                     |                                                                                       Non è possibile risolvere il nome del cluster di failover o non è possibile accedere a uno o più nodi del cluster. BPA non è in grado di eseguire i test di disponibilità del cluster.                                                                                        |                                                                   -Controllare l'ortografia del nome del cluster specificato durante l'esecuzione di BPA.<br />-Assicurarsi che tutti i nodi del cluster siano online e in esecuzione.<br />-Verificare che la convalida guidata configurazione possibile eseguire nel cluster di failover.                                                                    |
|                    I nodi del cluster di failover devono essere abilitati per la gestione remota tramite WMI                    |                                                Uno o più nodi del cluster non sono abilitati per la gestione remota mediante Strumentazione gestione Windows \(WMI\). Aggiornamento compatibile con cluster non può aggiornare i nodi del cluster se questi non sono configurati per la gestione remota.                                                 |                                                                                                  Assicurarsi che tutti i nodi del cluster di failover siano abilitati per la gestione remota tramite WMI. Per altre informazioni, vedere [Configurare i nodi per la gestione remota](#BKMK_NODE_CONFIG) in questo argomento.                                                                                                   |
|                      Comunicazione remota di PowerShell deve essere abilitata in ogni nodo del cluster di failover                       |                                                           PowerShell non è installato o non è abilitato per la comunicazione remota su uno o più nodi di cluster di failover. Aggiornamento compatibile con cluster non può essere configurato per l'oggetto corrente\-modalità di aggiornamento o usare determinate funzionalità in remoto\-modalità di aggiornamento.                                                            |                                                                                             Verificare che PowerShell sia installato in tutti i nodi del cluster e viene abilitata per la comunicazione remota.<br /><br />Per altre informazioni, vedere [Configurare i nodi per la gestione remota](#BKMK_NODE_CONFIG) in questo argomento.                                                                                             |
|                                            Versione del cluster di failover                                            |                                                                            Non vengono eseguiti uno o più nodi del cluster di failover Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012. Aggiornamento compatibile con cluster non può aggiornare il cluster di failover.                                                                             |                                                                   Verificare che il cluster di failover specificato durante l'esecuzione di BPA esegua Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.<br /><br />Per altre informazioni, vedere [Verificare la configurazione del cluster](#BKMK_REQ_CLUS) in questo argomento.                                                                   |
| Le versioni richieste di .NET Framework e Windows PowerShell devono essere installate su tutti i nodi del cluster di failover |                                                                                              .NET framework 4.6, 4.5 o Windows PowerShell non è installato in uno o più nodi del cluster. Alcune funzionalità di Aggiornamento compatibile con cluster potrebbero non funzionare.                                                                                              |                                                                            Assicurarsi che siano installati .NET Framework 4.6 o 4.5 e Windows PowerShell in tutti i nodi del cluster, se sono necessarie.<br /><br />Per altre informazioni, vedere [Configurare i nodi per la gestione remota](#BKMK_NODE_CONFIG) in questo argomento.                                                                             |
|                           Il servizio cluster deve essere in esecuzione in tutti i nodi del cluster                           |                                                                                                            Il servizio cluster non è in esecuzione in uno o più nodi del cluster. Aggiornamento compatibile con cluster non può aggiornare il cluster di failover.                                                                                                             |                        -Verificare che il servizio Cluster \(clussvc\) viene avviato in tutti i nodi del cluster, ed è configurato per l'avvio automatico.<br />-Verificare che la convalida guidata configurazione possibile eseguire nel cluster di failover.<br /><br />Per altre informazioni, vedere [Verificare la configurazione del cluster](#BKMK_REQ_CLUS) in questo argomento.                         |
|     Aggiornamenti automatici non deve essere configurato per installare automaticamente aggiornamenti in nessuno dei nodi del cluster di failover.     |                                           Almeno su un nodo del cluster di failover, Aggiornamenti automatici è configurato per installare aggiornamenti Microsoft automaticamente su quel nodo. La combinazione di Aggiornamenti compatibili con cluster con altri metodi di aggiornamento può dare origine a tempi di inattività imprevisti o a risultati imprevedibili.                                            |                                                     Se la funzionalità Windows Update è configurata per gli aggiornamenti automatici su uno o più nodi del cluster, assicurarsi che Aggiornamenti automatici non sia configurato per l'installazione automatica degli aggiornamenti.<br /><br />Per altre informazioni, vedere [Consigli per l'applicazione degli aggiornamenti Microsoft](#BKMK_BP_WUA).                                                     |
|                          I nodi del cluster di failover devono usare la stessa origine degli aggiornamenti                          |                                                    Uno o più nodi del cluster di failover sono configurati per usare un'origine di aggiornamento per gli aggiornamenti Microsoft diversa da quella usata dal resto dei nodi. L'applicazione degli aggiornamenti sui nodi del cluster da parte di Aggiornamento compatibile con cluster potrebbe non essere uniforme.                                                    |                                                                        Assicurarsi che ogni nodo del cluster sia configurato per usare la stessa origine di aggiornamento, ad esempio un server WSUS, Windows Update o Microsoft Update.<br /><br />Per altre informazioni, vedere [Consigli per l'applicazione degli aggiornamenti Microsoft](#BKMK_BP_WUA).                                                                         |
|       È necessario che in ogni nodo del cluster di failover sia configurata una regola del firewall che consenta l'arresto remoto       |                 In uno o più nodi del cluster non è presente o abilitata una regola del firewall che consenta l'arresto remoto, oppure un'impostazione di Criteri di gruppo impedisce l'abilitazione di tale regola. Un'operazione di aggiornamento che applica aggiornamenti che richiedono il riavvio automatico dei nodi potrebbe non essere completata correttamente.                  |                                                                    Se Windows Firewall o un non\-firewall Microsoft è in uso nei nodi del cluster, configurare una regola del firewall che consenta l'arresto remoto.<br /><br />Per altre informazioni, vedere [Abilitare una regola del firewall per consentire i riavvii automatici](#BKMK_FW) in questo argomento.                                                                    |
|          L'impostazione relativa al server proxy in ognuno dei nodi di failover deve essere impostata su un server proxy locale          |                             Uno o più nodi del cluster di failover presentano una configurazione di server proxy non corretta.<br /><br />Se è in uso un server proxy locale, le relative impostazioni su ogni nodo devono essere configurate correttamente per l'accesso del cluster a Microsoft Update o Windows Update.                              |                                            Assicurarsi che le impostazioni proxy WinHTTP su ogni nodo del cluster siano configurate su un server proxy locale, se necessario. Se nell'ambiente non è in uso un server proxy, questo avviso può essere ignorato.<br /><br />Per altre informazioni, vedere [Applicare aggiornamenti Microsoft in scenari di succursale](#BKMK_PROXY) in questo argomento.                                            |
|        Il ruolo del cluster aggiornamento compatibile con cluster deve essere installato nel cluster di failover per Abilita Self-Service\-modalità di aggiornamento        |                                                                                                   Il ruolo del cluster di Aggiornamento compatibile con cluster non è installato nel cluster di failover. Questo ruolo è richiesto per cluster self\-l'aggiornamento.                                                                                                   |      Per usare aggiornamento compatibile con cluster in self\-aggiornamento modalità, aggiungere il ruolo del cluster aggiornamento compatibile con cluster nel cluster di failover in uno dei modi seguenti:<br /><br />-Eseguire la [Add-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole) cmdlet di PowerShell.<br />-Selezionare il **Configura cluster self\-opzioni di aggiornamento** azione nella finestra di aggiornamento compatibile con Cluster.      |
|         Il ruolo del cluster aggiornamento compatibile con cluster deve essere abilitato nel cluster di failover per Abilita Self-Service\-modalità di aggiornamento         | Il ruolo del cluster di Aggiornamento compatibile con cluster è disabilitato. Ad esempio, il ruolo del cluster aggiornamento compatibile con cluster non è installato o è stata disabilitata mediante il [disabilitare\-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole) cmdlet di PowerShell. Questo ruolo è richiesto per cluster self\-l'aggiornamento. | Per usare aggiornamento compatibile con cluster in self\-aggiornamento modalità, abilitare il ruolo del cluster aggiornamento compatibile con cluster nel cluster di failover in uno dei modi seguenti:<br /><br />-Eseguire la [Enable-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole) cmdlet di PowerShell.<br />-Selezionare il **Configura cluster self\-opzioni di aggiornamento** azione nella finestra di aggiornamento compatibile con Cluster. |
|      La spina di aggiornamento compatibile con cluster configurata\-di profilo utente\-modalità di aggiornamento deve essere registrata in tutti i nodi del cluster di failover      |                                                              Il ruolo del cluster aggiornamento compatibile con cluster in uno o più nodi del cluster di failover non può accedere la spina di aggiornamento compatibile con cluster\-nel modulo che è configurato in self\-opzioni di aggiornamento. Un self\-operazioni di aggiornamento potrebbe non riuscire.                                                              |           -Verificare che la spina di aggiornamento compatibile con cluster configurata\-in viene installato in tutti i nodi del cluster seguendo la procedura di installazione per il prodotto che fornisce la spina di aggiornamento compatibile con cluster\-in.<br />-Eseguire la [registrare\-CauPlugin](https://docs.microsoft.com/powershell/module/clusterawareupdating/Register-CauPlugin) cmdlet di PowerShell per registrare il plug\-nei nodi del cluster necessarie.           |
|                Tutti i nodi del cluster devono avere lo stesso set di plug di aggiornamento compatibile con cluster registrati\-aggiuntivi                 |                                                                             Un self\-operazioni di aggiornamento potrebbe non riuscire se la spina\-in cui è configurato per essere usata in un'operazione di aggiornamento viene modificata in modo che non è disponibile in tutti i nodi del cluster.                                                                              |           -Verificare che la spina di aggiornamento compatibile con cluster configurata\-in viene installato in tutti i nodi del cluster seguendo la procedura di installazione per il prodotto che fornisce la spina di aggiornamento compatibile con cluster\-in.<br />-Eseguire la [registrare\-CauPlugin](https://docs.microsoft.com/powershell/module/clusterawareupdating/Register-CauPlugin) cmdlet di PowerShell per registrare il plug\-nei nodi del cluster necessarie.           |
|                               Le opzioni relative all'operazione di aggiornamento configurate devono essere valide                                |                                                                          Il self\-aggiornamento pianificazione e l'operazione di aggiornamento opzioni configurate per il cluster di failover sono incomplete o non valide. Un self\-operazioni di aggiornamento potrebbe non riuscire.                                                                           |                                                            Configurare un valido\-aggiornamento pianificazione e set di opzioni dell'aggiornamento. Ad esempio, è possibile usare la [impostata\-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Set-CauClusterRole) cmdlet di PowerShell per configurare l'aggiornamento compatibile con cluster ruolo.                                                            |
|                  È necessario che almeno due nodi del cluster di failover siano proprietari del ruolo del cluster di Aggiornamento compatibile con cluster.                  |                                                                                        Un'operazione di aggiornamento avviate in self\-modalità di aggiornamento avrà esito negativo perché il ruolo del cluster aggiornamento compatibile con cluster non dispone di un nodo proprietario possibile in cui spostarsi.                                                                                         |                                                                                                                Usare Strumenti per Clustering di failover per assicurarsi che tutti i nodi del cluster siano configurati come possibili proprietari del ruolo del cluster di Aggiornamento compatibile con cluster. Questa è la configurazione predefinita.                                                                                                                |
|                  Tutti i nodi del cluster di failover devono poter accedere agli script di Windows PowerShell                  |                                                                        Non tutti i possibili nodi proprietari del ruolo del cluster aggiornamento compatibile con cluster possono accedere a configurato Windows PowerShell pre\-aggiornamento e post\-aggiornare gli script. Un self\-operazioni di aggiornamento avrà esito negativo.                                                                        |                                                                                                                    Assicurarsi che tutti i possibili nodi proprietari del ruolo del cluster aggiornamento compatibile con cluster dispongano delle autorizzazioni di accesso configurato PowerShell pre\-aggiornamento e post\-aggiornare gli script.                                                                                                                     |
|                   Tutti i nodi del cluster di failover devono usare script di Windows PowerShell identici                   |                                                     Non tutti i possibili nodi proprietari del ruolo del cluster aggiornamento compatibile con cluster usano la stessa copia della specificato Windows PowerShell pre\-aggiornamento e post\-aggiornare gli script. Un self\-operazioni di aggiornamento potrebbe non riuscire o presentare comportamenti imprevisti.                                                     |                                                                                                                                   Assicurarsi che tutti i possibili nodi proprietari del ruolo del cluster aggiornamento compatibile con cluster usino stesso PowerShell pre\-aggiornamento e post\-aggiornare gli script.                                                                                                                                   |
|         Il valore dell'impostazione WarnAfter specificato per l'operazione di aggiornamento deve essere minore di quello dell'impostazione StopAfter         |                                                                           I valori di timeout specificati per l'operazione di aggiornamento di Aggiornamento compatibile con cluster rendono il timeout dell'avviso inefficace. Un'operazione di aggiornamento potrebbe venire annullata prima che sia possibile generare un registro eventi di avviso.                                                                            |                                                                                                                                      Nelle opzioni relative all'operazione di aggiornamento, configurare un valore dell'opzione **WarnAfter** inferiore a quello per l'opzione **StopAfter** .                                                                                                                                       |

## <a name="see-also"></a>Vedere anche  

-   [Panoramica di aggiornamento compatibile con cluster](cluster-aware-updating.md)