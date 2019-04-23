---
title: Panoramica di Aggiornamento compatibile con cluster
ms.topic: article
ms.prod: windows-server-threshold
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 08/06/2018
description: L'aggiornamento (compatibile con cluster) automatizza l'installazione degli aggiornamenti software nei cluster che esegue Windows Server.
ms.assetid: 3c2993b4-aa81-452b-a5c3-3724ad95d892
ms.openlocfilehash: 77ccfe7dc9a769d602ff069d5f1d8e77522aa001
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827422"
---
# <a name="cluster-aware-updating-overview"></a>Panoramica di Aggiornamento compatibile con cluster

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento viene fornita una panoramica del Cluster\-aggiornamento compatibile con \(aggiornamento compatibile con cluster\), una funzionalità che consente di automatizzare il processo di aggiornamento sul software dei server cluster mantenendone la disponibilità.

> [!NOTE]
> Quando si aggiornano [spazi di archiviazione diretta](../storage/storage-spaces/storage-spaces-direct-overview.md) cluster, è consigliabile usare aggiornamento compatibile con Cluster.
  
## <a name="BKMK_OVER"></a>Descrizione funzionalità  
Aggiornamento compatibile con cluster è una funzionalità automatizzata che consente di aggiornare i server in una [cluster di failover](failover-clustering-overview.md) minima o senza alcuna perdita di disponibilità durante il processo di aggiornamento. Durante un'operazione di aggiornamento, aggiornamento compatibile con Cluster esegue in modo trasparente le attività seguenti:  

1. Inserisce ogni nodo del cluster in modalità di manutenzione del nodo.
2. Consente di spostare i ruoli del cluster nel nodo.
3. Installa gli aggiornamenti e eventuali aggiornamenti dipendenti.
4. Esegue un riavvio se necessario.
5. Visualizza il nodo dalla modalità manutenzione.
6. Ripristina i ruoli del cluster nel nodo.
7. Passa all'aggiornamento del nodo successivo.

Per molti ruoli del cluster nel cluster, il processo di aggiornamento automatico attiva un failover pianificato. Ciò può causare un'interruzione del servizio temporanea per i client connessi. Tuttavia, nel caso di carichi di lavoro continuamente disponibili, ad esempio Hyper\-V con live migration o un file server con Failover trasparente SMB, aggiornamento compatibile con Cluster consente di coordinare gli aggiornamenti del cluster senza impatto per la disponibilità dei servizi.    
  
## <a name="practical-applications"></a>Applicazioni pratiche  
  
-   Aggiornamento compatibile con cluster riduce le interruzioni nei servizi del cluster, riducendo la necessità di aggiornare le soluzioni alternative manuali e lo rende l'estremità\-a\-cluster end più affidabili di processo di aggiornamento per l'amministratore. Quando la funzionalità aggiornamento compatibile con cluster viene usata in combinazione con carichi di lavoro cluster a disponibilità continua, ad esempio file server continuamente disponibili \(carico di lavoro di file server con Failover trasparente SMB\) Hyper o\-V, cluster gli aggiornamenti possono essere eseguiti senza impatto sulla disponibilità del servizio per i client.  
  
-   Con Aggiornamento compatibile con cluster risulta facilitata l'adozione di processi IT coerenti nell'intera organizzazione. L'aggiornamento di profili di esecuzione può essere creato per classi diverse di cluster di failover e quindi gestita a livello centrale in una condivisione file per garantire che le distribuzioni di aggiornamento compatibile con cluster nell'intera organizzazione IT applicano gli aggiornamenti in modo coerente, anche se i cluster sono gestiti da righe differenti \-di\-business o amministratori.  
  
-   Aggiornamento compatibile con cluster supporta la pianificazione delle operazioni di aggiornamento a intervalli regolari giornalieri, settimanali o mensili per agevolare il coordinamento degli aggiornamenti del cluster con altri processi di gestione IT.  
  
-   Aggiornamento compatibile con cluster fornisce un'architettura estensibile per aggiornare l'inventario software in un cluster\-modalità compatibile con. Utilizzabile dai server di pubblicazione per coordinare l'installazione degli aggiornamenti software che non vengono pubblicati in Windows Update o Microsoft Update oppure che non sono disponibili da Microsoft, ad esempio, gli aggiornamenti per non\-i driver di dispositivo di Microsoft.  
  
-   Aggiornamento compatibile con cluster self\-modalità di aggiornamento consente a un'appliance di "cluster in una casella" \(un set di computer fisici in cluster, generalmente assemblati in uno chassis\) aggiornarsi. Questi sistemi vengono in genere distribuiti in succursali con un supporto IT locale minimo per la gestione dei cluster. Self\-modalità di aggiornamento è particolarmente preziosa in questi scenari di distribuzione.  
  
## <a name="important-functionality"></a>Funzionalità importanti  
Di seguito è una descrizione delle funzionalità importanti di aggiornamento compatibile con Cluster:  
  
-   Un'interfaccia utente \(dell'interfaccia utente\) -finestra di aggiornamento compatibile con Cluster - e un set di cmdlet che è possibile usare per visualizzare in anteprima, applicarli, monitorarli e segnalare gli aggiornamenti  
  
-   Un'estremità\-al\-terminare l'automazione del cluster\-operazione di aggiornamento \(un'operazione di aggiornamento\), orchestrato da uno o più computer coordinatore dell'aggiornamento  
  
-   Un plug predefinito\-in cui si integra con l'agente di aggiornamento di Windows esistenti \(WUA\) e Windows Server Update Services \(WSUS\) infrastruttura in Windows Server per applicare importante Microsoft aggiornamenti  
  
-   Un secondo plug\-in cui può essere usato per applicare gli hotfix Microsoft e che possono essere personalizzati da applicare non\-aggiornamenti Microsoft  
  
-   Profili delle operazioni di aggiornamento configurabili con impostazioni per le opzioni delle operazioni di aggiornamento, come il numero massimo di tentativi di applicazione dell'aggiornamento per nodo. I profili delle operazioni di aggiornamento consentono di riutilizzare rapidamente le stesse impostazioni per più operazioni di aggiornamento e di condividere facilmente le impostazioni di aggiornamento con altri cluster di failover.  
  
-   Un'architettura estensibile che supporta i nuovi plug\-in fase di sviluppo per coordinare l'altro nodo\-l'aggiornamento degli strumenti in tutto il cluster, ad esempio i programmi di installazione software personalizzati, aggiornamento degli strumenti e una scheda di rete o una scheda bus host delBIOS\( HBA\) aggiornamento degli strumenti.  
  
Aggiornamento compatibile con cluster consente di coordinare l'operazione in due modalità di aggiornamento completa del cluster:  
  
-   **Self\-modalità di aggiornamento** per questa modalità, il ruolo del cluster aggiornamento compatibile con cluster è configurato come carico di lavoro nel cluster di failover che deve essere aggiornato e viene definita una pianificazione di aggiornamento associata. Il cluster si aggiorna automaticamente a intervalli pianificati tramite un profilo dell'operazione di aggiornamento predefinito o personalizzato. Nel corso dell'operazione di aggiornamento, il processo del coordinatore dell'aggiornamento di Aggiornamento compatibile con cluster viene avviato sul nodo proprietario del ruolo cluster di Aggiornamento compatibile con cluster e il processo esegue gli aggiornamenti in sequenza su ogni nodo del cluster. Per aggiornare il nodo del cluster corrente, il ruolo del cluster di Aggiornamento compatibile con cluster esegue il failover su un altro nodo del cluster e un nuovo processo del coordinatore dell'aggiornamento su tale nodo assume il controllo dell'operazione di aggiornamento. In self\-modalità di aggiornamento, aggiornamento compatibile con cluster può aggiornare il cluster di failover usando un completamente automatizzata, terminare\-a\-end processo di aggiornamento. Un amministratore può anche attivare gli aggiornamenti\-richiedono in questa modalità, o semplicemente utilizzare remoto\-aggiornamento approccio se si desidera. In self\-modalità di aggiornamento, un amministratore può ottenere le informazioni di riepilogo su un'operazione di aggiornamento in corso connettendosi al cluster ed eseguendo la **ottenere\-CauRun** cmdlet di Windows PowerShell.  
  
-   **Remote\-modalità di aggiornamento** per questa modalità, un computer remoto, noto come coordinatore dell'aggiornamento, viene configurato con gli strumenti di aggiornamento compatibile con cluster. Il coordinatore dell'aggiornamento non è un membro del cluster aggiornato nel corso dell'operazione di aggiornamento. Dal computer remoto, l'amministratore può attivare un su\-richiedono l'aggiornamento eseguito con un valore predefinito o un profilo operazione di aggiornamento personalizzato. Remote\-modalità di aggiornamento è utile per monitorare reale\-ora lo stato di avanzamento durante l'operazione di aggiornamento e per i cluster eseguiti nelle installazioni Server Core.  
  
## <a name="hardware-and-software-requirements"></a>Requisiti hardware e software  

Aggiornamento compatibile con cluster è utilizzabile in tutte le edizioni di Windows Server, comprese le installazioni Server Core. Per informazioni dettagliate sui requisiti, vedere [requisiti di aggiornamento compatibile con Cluster e le procedure consigliate](cluster-aware-updating-requirements.md).

### <a name="installing-cluster-aware-updating"></a>Installazione di aggiornamento compatibile con Cluster
Per usare aggiornamento compatibile con cluster, installare la funzionalità Clustering di Failover in Windows Server e creare un cluster di failover. I componenti che supportano la funzionalità di Aggiornamento compatibile con cluster vengono automaticamente installati in ogni nodo del cluster.

Per installare la funzionalità Clustering di failover, è possibile usare gli strumenti seguenti:
- Aggiunta guidata ruoli e funzionalità in Server Manager
- [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/Install-WindowsFeature?view=winserver2012r2-ps&viewFallbackFrom=win10-ps) cmdlet di Windows PowerShell
- Strumento da riga di comando di Gestione e manutenzione immagini distribuzione

Per altre informazioni, vedere [installare la funzionalità Clustering di Failover](create-failover-cluster.md#install-the-failover-clustering-feature).

È anche necessario installare gli strumenti per Clustering di Failover, che fanno parte di strumenti di amministrazione remota del Server e vengono installati per impostazione predefinita quando si installa la funzionalità Clustering di Failover in Server Manager. Gli strumenti per Clustering di Failover includono l'interfaccia utente di aggiornamento compatibile con Cluster e cmdlet di PowerShell. 

È necessario installare gli Strumenti per Clustering di failover come indicato di seguito per poter supportare le diverse modalità di aggiornamento di Aggiornamento compatibile con cluster:

- Per usare aggiornamento compatibile con cluster in self\-aggiornamento modalità, installare gli strumenti per Clustering di Failover in ogni nodo del cluster.   
  
- Per abilitare remoto\-aggiornamento modalità, installare gli strumenti per Clustering di Failover in un computer che disponga della connettività di rete al cluster di failover.  
  
> [!NOTE]  
> -   È possibile utilizzare gli strumenti per Clustering di Failover in Windows Server 2012 per gestire l'aggiornamento compatibile con Cluster in una versione più recente di Windows Server. 
> -   Per usare aggiornamento compatibile con cluster solo in remoto\-modalità di aggiornamento, installazione di strumenti per Clustering di Failover nei nodi del cluster non è obbligatorio. Certe funzionalità di Aggiornamento compatibile con cluster, tuttavia, non saranno disponibili. Per altre informazioni, vedere [requisiti e procedure consigliate per il Cluster\-aggiornamento compatibile con](cluster-aware-updating-requirements.md).  
> -   A meno che non si usa aggiornamento compatibile con cluster solo nella self\-modalità di aggiornamento, il computer in cui sono installati gli strumenti di aggiornamento compatibile con cluster e che coordina gli aggiornamenti non può essere un membro del cluster di failover.  
  
### <a name="enabling-self-updating-mode"></a>Abilitazione della modalità di aggiornamento automatico
Per abilitare la modalità di aggiornamento automatico, è necessario aggiungere il ruolo del cluster aggiornamento compatibile con Cluster al cluster di failover. A tale scopo, usare uno dei metodi seguenti:
- In Server Manager, selezionare **degli strumenti** > **aggiornamento compatibile con Cluster**, quindi nella finestra di aggiornamento compatibile con Cluster, selezionare **cluster ConfiguraOpzionidiaggiornamentoautomatico**. 
- In una sessione di PowerShell, eseguire la [Add-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole?view=win10-ps) cmdlet.  
  
Per disinstallare aggiornamento compatibile con cluster, disinstallare la funzionalità Clustering di Failover o strumenti per Clustering di Failover tramite Server Manager, il [Uninstall-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/Uninstall-WindowsFeature?view=win10-ps) cmdlet, o il comando DISM\-strumenti della riga.  
  
### <a name="additional-requirements-and-best-practices"></a>Requisiti aggiuntivi e procedure consigliate  

Per assicurarsi che sia possibile aggiornare correttamente i nodi del cluster con Aggiornamento compatibile con cluster e per indicazioni aggiuntive per la configurazione dell'ambiente cluster di failover per l'utilizzo di Aggiornamento compatibile con cluster, è possibile eseguire Best Practices Analyzer.  
  
Per informazioni dettagliate sui requisiti e le procedure consigliate per aggiornamento compatibile con cluster e informazioni sull'esecuzione di Best Practices Analyzer di aggiornamento compatibile con cluster, vedere [requisiti e procedure consigliate per il Cluster\-aggiornamento compatibile con](cluster-aware-updating-requirements.md).  
  
### <a name="starting-cluster-aware-updating"></a>Avvio aggiornamento compatibile con Cluster  
  
##### <a name="to-start-cluster-aware-updating-from-server-manager"></a>Per avviare l'aggiornamento compatibile con Cluster da Server Manager  
  
1.  Avviare Server Manager.  
  
2.  Effettua una delle seguenti operazioni:  
  
    -   Nel **degli strumenti** menu, fare clic su **Cluster\-aggiornamento compatibile con**.  
  
    -   Se uno o più nodi del cluster o il cluster, viene aggiunto a Server Manager, sul **tutti i server** pagina, a destra\-fare clic sul nome di un nodo \(oppure il nome del cluster\)e quindi fare clic su  **Aggiorna Cluster**.  
  
## <a name="see-also"></a>Vedere anche  
I collegamenti seguenti forniscono altre informazioni sull'utilizzo di aggiornamento compatibile con Cluster.  
  
-   [Requisiti e procedure consigliate per Cluster\-aggiornamento compatibile con](cluster-aware-updating.md)  
  
-   [Cluster\-aggiornamento compatibile con: Domande frequenti](cluster-aware-updating-faq.md)  
  
-   [Le opzioni avanzate e profili di esecuzione con l'aggiornamento per aggiornamento compatibile con cluster](cluster-aware-updating-options.md)  
  
-   [Come collegare aggiornamento compatibile con cluster\-dei componenti aggiuntivi](cluster-aware-updating-plug-ins.md)  
  
-   [Cluster\-cmdlet in Windows PowerShell per aggiornamento compatibile con](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps&viewFallbackFrom=winserverr2-ps)  
  
-   [Cluster\-Plug per aggiornamento compatibile con\-nel riferimento](https://docs.microsoft.com/previous-versions/windows/desktop/mscs/cluster-aware-update-plug-in-interfaces-and-classes)  
  

