---
title: Panoramica di Aggiornamento compatibile con cluster
ms.topic: article
ms.prod: windows-server
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 08/06/2018
description: Aggiornamento compatibile con cluster automatizza l'installazione degli aggiornamenti software nei cluster che eseguono Windows Server.
ms.assetid: 3c2993b4-aa81-452b-a5c3-3724ad95d892
ms.openlocfilehash: e96223e0b4b44e87ade9dc8eb875f9aa7104f451
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361248"
---
# <a name="cluster-aware-updating-overview"></a>Panoramica di Aggiornamento compatibile con cluster

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento viene fornita una panoramica di cluster @ no__t-0Aware che aggiorna \(CAU @ no__t-2, una funzionalità che consente di automatizzare il processo di aggiornamento del software nei server del cluster mantenendo la disponibilità.

> [!NOTE]
> Quando si aggiornano i cluster di [spazi di archiviazione diretta](../storage/storage-spaces/storage-spaces-direct-overview.md) , è consigliabile usare l'aggiornamento compatibile con cluster.
  
## <a name="BKMK_OVER"></a>Descrizione della funzionalità  
Aggiornamento compatibile con cluster è una funzionalità automatizzata che consente di aggiornare i server in un [cluster di failover](failover-clustering-overview.md) con una perdita di disponibilità minima o nulla durante il processo di aggiornamento. Durante un'operazione di aggiornamento, l'aggiornamento compatibile con cluster esegue in modo trasparente le attività seguenti:  

1. Inserisce ogni nodo del cluster in modalità di manutenzione del nodo.
2. Sposta i ruoli del cluster fuori dal nodo.
3. Installa gli aggiornamenti e gli eventuali aggiornamenti dipendenti.
4. Se necessario, esegue un riavvio.
5. Porta fuori dalla modalità di manutenzione del nodo.
6. Ripristina i ruoli del cluster nel nodo.
7. Consente di spostare l'aggiornamento del nodo successivo.

Per molti ruoli del cluster nel cluster, il processo di aggiornamento automatico attiva un failover pianificato. Ciò può causare un'interruzione del servizio temporanea per i client connessi. Tuttavia, nel caso di carichi di lavoro continuamente disponibili, ad esempio Hyper @ no__t-0V con migrazione in tempo reale o file server con failover trasparente SMB, aggiornamento compatibile con cluster consente di coordinare gli aggiornamenti del cluster senza alcun effetto sulla disponibilità del servizio.    
  
## <a name="practical-applications"></a>Applicazioni pratiche  
  
-   Aggiornamento compatibile con cluster riduce le interruzioni del servizio nei servizi cluster, riduce la necessità di soluzioni alternative per l'aggiornamento manuale e rende il processo di aggiornamento del cluster end @ no__t-0to @ no__t-1end più affidabile per l'amministratore. Quando la funzionalità aggiornamento compatibile con cluster viene utilizzata insieme ai carichi di lavoro del cluster continuamente disponibili, ad esempio i file server a disponibilità continua @no__t il carico di lavoro del server 0file con il failover trasparente SMB @ no__t-1 o Hyper @ no__t-2V, è possibile eseguire gli aggiornamenti del cluster con un effetto zero sulla disponibilità dei servizi per i client.  
  
-   Con Aggiornamento compatibile con cluster risulta facilitata l'adozione di processi IT coerenti nell'intera organizzazione. È possibile creare profili delle operazioni di aggiornamento per classi diverse di cluster di failover e quindi gestirli a livello centrale in una condivisione file per garantire che le distribuzioni di aggiornamento compatibile con cluster nell'intera organizzazione IT applichino gli aggiornamenti in modo coerente, anche se i cluster sono gestiti da diversi Lines @ no__t-0of @ no__t-1BUSINESS o Administrators.  
  
-   Aggiornamento compatibile con cluster supporta la pianificazione delle operazioni di aggiornamento a intervalli regolari giornalieri, settimanali o mensili per agevolare il coordinamento degli aggiornamenti del cluster con altri processi di gestione IT.  
  
-   Aggiornamento compatibile con cluster fornisce un'architettura estendibile per aggiornare l'inventario software del cluster in un cluster @ no__t-0aware Fashion. Questa operazione può essere utilizzata dai Publisher per coordinare l'installazione degli aggiornamenti software non pubblicati in Windows Update o Microsoft Update o che non sono disponibili da Microsoft, ad esempio aggiornamenti per i driver di dispositivo non @ no__t-0Microsoft.  
  
-   Aggiornamento compatibile con cluster in modalità self @ no__t-0updating Abilita un appliance "cluster in a box" \(a set di computer fisici in cluster, in genere incluso in un unico chassis @ no__t-2 per l'aggiornamento. Questi sistemi vengono in genere distribuiti in succursali con un supporto IT locale minimo per la gestione dei cluster. Self @ no__t-0updating Mode offre un ottimo valore in questi scenari di distribuzione.  
  
## <a name="important-functionality"></a>Funzionalità importanti  
Di seguito è riportata una descrizione di importanti funzionalità di aggiornamento compatibile con cluster:  
  
-   Interfaccia utente \(UI @ no__t-1-la finestra di aggiornamento compatibile con cluster e un set di cmdlet che è possibile usare per visualizzare in anteprima, applicare, monitorare e creare report sugli aggiornamenti  
  
-   A end @ no__t-0to @ no__t-1end Automation of the cluster @ no__t-2updating Operation \(AN Updating Run @ no__t-4, orchestrata da uno o più computer Coordinator dell'aggiornamento  
  
-   Un plug-in predefinito no__t-0cm che si integra con l'agente di Windows Update esistente \(WUA @ no__t-2 e Windows Server Update Services \(WSUS @ no__t-4 Infrastructure in Windows Server per applicare importanti aggiornamenti Microsoft  
  
-   Un secondo plug @ no__t-0cm che può essere usato per applicare gli hotfix Microsoft e che può essere personalizzato per applicare gli aggiornamenti non @ no__t-1Microsoft  
  
-   Profili delle operazioni di aggiornamento configurabili con impostazioni per le opzioni delle operazioni di aggiornamento, come il numero massimo di tentativi di applicazione dell'aggiornamento per nodo. I profili delle operazioni di aggiornamento consentono di riutilizzare rapidamente le stesse impostazioni per più operazioni di aggiornamento e di condividere facilmente le impostazioni di aggiornamento con altri cluster di failover.  
  
-   Un'architettura estendibile che supporta lo sviluppo di nuovi plug-in no__t-0cm per coordinare altri strumenti node @ no__t-1updating nel cluster, come programmi di installazione software personalizzati, strumenti per l'aggiornamento del BIOS e scheda di rete o scheda bus host \(HBA @ no__t-3 aggiornamento degli strumenti.  
  
Aggiornamento compatibile con cluster consente di coordinare l'operazione di aggiornamento del cluster completa in due modalità:  
  
-   **Modalità self @ no__t-1updating** Per questa modalità, il ruolo del cluster di aggiornamento compatibile con cluster viene configurato come carico di lavoro nel cluster di failover da aggiornare e viene definita una pianificazione di aggiornamento associata. Il cluster si aggiorna automaticamente a intervalli pianificati tramite un profilo dell'operazione di aggiornamento predefinito o personalizzato. Nel corso dell'operazione di aggiornamento, il processo del coordinatore dell'aggiornamento di Aggiornamento compatibile con cluster viene avviato sul nodo proprietario del ruolo cluster di Aggiornamento compatibile con cluster e il processo esegue gli aggiornamenti in sequenza su ogni nodo del cluster. Per aggiornare il nodo del cluster corrente, il ruolo del cluster di Aggiornamento compatibile con cluster esegue il failover su un altro nodo del cluster e un nuovo processo del coordinatore dell'aggiornamento su tale nodo assume il controllo dell'operazione di aggiornamento. In modalità self-no__t-0updating, aggiornamento compatibile con cluster può aggiornare il cluster di failover usando un processo di aggiornamento completamente automatizzato, end @ no__t-1per @ no__t-2end. Un amministratore può anche attivare gli aggiornamenti in @ no__t-0demand in questa modalità oppure utilizzare semplicemente l'approccio remoto @ no__t-1updating, se lo si desidera. In modalità self-no__t-0updating un amministratore può ottenere informazioni di riepilogo su un'operazione di aggiornamento in corso connettendosi al cluster ed eseguendo il cmdlet **Get @ no__t-2CauRun** di Windows PowerShell.  
  
-   **Remote @ no__t-modalità 1updating** Per questa modalità, un computer remoto, denominato Coordinatore dell'aggiornamento, viene configurato con gli strumenti di aggiornamento compatibile con cluster. Il coordinatore dell'aggiornamento non è un membro del cluster aggiornato nel corso dell'operazione di aggiornamento. Dal computer remoto l'amministratore attiva un'operazione di aggiornamento @ no__t-0demand usando un profilo dell'operazione di aggiornamento predefinito o personalizzato. Remote @ no__t-0updating Mode è utile per il monitoraggio dello stato di avanzamento reale di @ no__t-1time durante l'operazione di aggiornamento e per i cluster in esecuzione in installazioni dei componenti di base del server.  
  
## <a name="hardware-and-software-requirements"></a>Requisiti hardware e software  

Aggiornamento compatibile con cluster può essere usato in tutte le edizioni di Windows Server, incluse le installazioni Server Core. Per informazioni dettagliate sui requisiti, vedere [requisiti e procedure consigliate per l'aggiornamento compatibile con cluster](cluster-aware-updating-requirements.md).

### <a name="installing-cluster-aware-updating"></a>Installazione di aggiornamento compatibile con cluster
Per usare aggiornamento compatibile con cluster, installare la funzionalità clustering di failover in Windows Server e creare un cluster di failover. I componenti che supportano la funzionalità di Aggiornamento compatibile con cluster vengono automaticamente installati in ogni nodo del cluster.

Per installare la funzionalità Clustering di failover, è possibile usare gli strumenti seguenti:
- Aggiunta guidata ruoli e funzionalità in Server Manager
- Cmdlet di PowerShell [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/Install-WindowsFeature?view=winserver2012r2-ps&viewFallbackFrom=win10-ps) Windows
- Strumento da riga di comando di Gestione e manutenzione immagini distribuzione

Per ulteriori informazioni, vedere [installare la funzionalità clustering di failover](create-failover-cluster.md#install-the-failover-clustering-feature).

È inoltre necessario installare gli strumenti di clustering di failover, che fanno parte del Strumenti di amministrazione remota del server e vengono installati per impostazione predefinita quando si installa la funzionalità clustering di failover in Server Manager. Gli strumenti di clustering di failover includono l'interfaccia utente di aggiornamento compatibile con cluster e i cmdlet di PowerShell. 

È necessario installare gli Strumenti per Clustering di failover come indicato di seguito per poter supportare le diverse modalità di aggiornamento di Aggiornamento compatibile con cluster:

- Per usare aggiornamento compatibile con cluster in modalità self @ no__t-0updating, installare gli strumenti di clustering di failover in ogni nodo del cluster.   
  
- Per abilitare la modalità remota @ no__t-0updating, installare gli strumenti di clustering di failover in un computer con connettività di rete al cluster di failover.  
  
> [!NOTE]  
> -   Non è possibile usare gli strumenti di clustering di failover in Windows Server 2012 per gestire l'aggiornamento compatibile con cluster in una versione più recente di Windows Server. 
> -   Per usare aggiornamento compatibile con cluster solo nella modalità remota @ no__t-0updating, non è necessario installare gli strumenti di clustering di failover nei nodi del cluster. Certe funzionalità di Aggiornamento compatibile con cluster, tuttavia, non saranno disponibili. Per ulteriori informazioni, vedere [requisiti e procedure consigliate per l'aggiornamento di cluster @ no__t-1Aware](cluster-aware-updating-requirements.md).  
> -   A meno che non si usi aggiornamento compatibile con cluster solo in modalità self-no__t-0updating, il computer in cui sono installati gli strumenti di aggiornamento compatibile con cluster e che coordina gli aggiornamenti non può essere un membro del cluster di failover.  
  
### <a name="enabling-self-updating-mode"></a>Abilitazione della modalità di aggiornamento automatico
Per abilitare la modalità di aggiornamento automatico, è necessario aggiungere il ruolo del cluster di aggiornamento compatibile con cluster al cluster di failover. A tale scopo, usare uno dei metodi seguenti:
- In Server Manager selezionare **strumenti** > **aggiornamento compatibile con cluster**, quindi nella finestra aggiornamento compatibile con cluster selezionare **Configura opzioni di aggiornamento automatico del cluster**. 
- In una sessione di PowerShell eseguire il cmdlet [Add-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole?view=win10-ps) .  
  
Per disinstallare aggiornamento compatibile con cluster, disinstallare la funzionalità clustering di failover o gli strumenti di clustering di failover usando Server Manager, il cmdlet [Uninstall-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/Uninstall-WindowsFeature?view=win10-ps) o il comando DISM @ no__t-1line Tools.  
  
### <a name="additional-requirements-and-best-practices"></a>Requisiti aggiuntivi e procedure consigliate  

Per assicurarsi che sia possibile aggiornare correttamente i nodi del cluster con Aggiornamento compatibile con cluster e per indicazioni aggiuntive per la configurazione dell'ambiente cluster di failover per l'utilizzo di Aggiornamento compatibile con cluster, è possibile eseguire Best Practices Analyzer.  
  
Per informazioni dettagliate sui requisiti e le procedure consigliate per l'uso di aggiornamento compatibile con cluster e informazioni sull'esecuzione del Best Practices Analyzer di aggiornamento compatibile con cluster, vedere [requisiti e procedure consigliate per il cluster @ no__t-1Aware](cluster-aware-updating-requirements.md).  
  
### <a name="starting-cluster-aware-updating"></a>Avvio di aggiornamento compatibile con cluster  
  
##### <a name="to-start-cluster-aware-updating-from-server-manager"></a>Per avviare l'aggiornamento compatibile con cluster da Server Manager  
  
1.  Avviare Server Manager.  
  
2.  Effettua una delle seguenti operazioni:  
  
    -   Nel menu **strumenti** fare clic su **cluster @ No__t-2Aware Update**.  
  
    -   Se uno o più nodi del cluster, o il cluster, vengono aggiunti a Server Manager, nella pagina **tutti i server** , right @ no__t-1Click il nome di un nodo \(OR il nome del cluster @ no__t-3, quindi fare clic su **Aggiorna cluster**.  
  
## <a name="see-also"></a>Vedere anche  
I collegamenti seguenti forniscono ulteriori informazioni sull'utilizzo di aggiornamento compatibile con cluster.  
  
-   [Requisiti e procedure consigliate per l'aggiornamento di cluster @ no__t-1Aware](cluster-aware-updating.md)  
  
-   [Cluster @ no__t-1Aware-aggiornamento: domande frequenti](cluster-aware-updating-faq.md)  
  
-   [Opzioni avanzate e profili di esecuzione aggiornamento per aggiornamento compatibile con cluster](cluster-aware-updating-options.md)  
  
-   [Funzionamento del plug-in di aggiornamento compatibile con cluster @ no__t-1ins](cluster-aware-updating-plug-ins.md)  
  
-   [Cluster @ no__t-1Aware aggiornamento di cmdlet in Windows PowerShell](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps&viewFallbackFrom=winserverr2-ps)  
  
-   [Cluster @ no__t-1Aware aggiornamento plug @ no__t-2in riferimento](https://docs.microsoft.com/previous-versions/windows/desktop/mscs/cluster-aware-update-plug-in-interfaces-and-classes)  
  

