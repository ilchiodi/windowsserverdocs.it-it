---
title: Panoramica di aggiornamento compatibile con cluster
ms.topic: article
ms.prod: windows-server-threshold
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 3/23/2017
description: L'aggiornamento (compatibile con cluster) automatizza l'installazione dell'aggiornamento software nei cluster che esegue Windows Server.
ms.assetid: 3c2993b4-aa81-452b-a5c3-3724ad95d892
ms.openlocfilehash: 68336741accabd6e4cb0da5dbd708be707f68df6
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-aware-updating-overview"></a>Panoramica di aggiornamento compatibile con cluster

> Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento viene fornita una panoramica di aggiornamento compatibile con Cluster\ \(CAU\), una funzionalità che consente di automatizzare il processo nel server del cluster di aggiornamento, mantenendo la disponibilità di software.

> [!NOTE]
> Quando si aggiorna [spazi di archiviazione diretta](../storage/storage-spaces/storage-spaces-direct-overview.md) cluster, ti consigliamo di usare aggiornamento compatibile con Cluster.
  
## <a name="BKMK_OVER"></a>Descrizione delle funzionalità  
Aggiornamento compatibile con cluster è una funzionalità automatizzata che consente di aggiornare i server in un [cluster di failover](failover-clustering-overview.md) con senza alcuna perdita di disponibilità durante il processo di aggiornamento. Durante un'operazione di aggiornamento, aggiornamento compatibile con Cluster in modo trasparente esegue le attività seguenti:  

1. Inserisce ogni nodo del cluster in modalità di manutenzione del nodo.
2. Sposta i ruoli del cluster dal nodo.
3. Installa gli aggiornamenti e eventuali aggiornamenti dipendenti.
4. Esegue un riavvio se necessario.
5. Porta il nodo dalla modalità manutenzione.
6. Ripristina i ruoli del cluster nel nodo.
7. Passa all'aggiornamento del nodo successivo.

Per molti ruoli del cluster nel cluster, il processo di aggiornamento automatico attiva un failover pianificato. Questo può causare un'interruzione temporanea dei servizi per i client connessi. Tuttavia, in caso di carichi di lavoro continuamente disponibili, ad esempio Hyper\-V con live migration o file server con Failover trasparente SMB, aggiornamento compatibile con Cluster consente di coordinare gli aggiornamenti del cluster senza impatto per la disponibilità dei servizi.    
  
## <a name="practical-applications"></a>Applicazioni pratiche  
  
-   Aggiornamento compatibile con cluster riduce interruzioni nei servizi del cluster, riduce la necessità di soluzioni di aggiornamento manuale e rende più affidabile processo di aggiornamento per l'amministratore del cluster di end-to-end. Quando la funzionalità aggiornamento compatibile con cluster viene utilizzata in combinazione con carichi di lavoro cluster a disponibilità continua, come file server continuamente disponibili \ (file server carico di lavoro con Failover\ trasparente SMB) o Hyper\-V, il cluster aggiornamenti possono essere eseguiti senza impatto sulla disponibilità del servizio per i client.  
  
-   Aggiornamento compatibile con cluster risulta facilitata l'adozione di processi IT coerenti nell'azienda. Profili delle operazioni di aggiornamento può essere creato per classi diverse di cluster di failover e quindi gestito centralmente in una condivisione file per assicurarsi che le distribuzioni di aggiornamento compatibile con cluster nell'intera organizzazione IT applicano gli aggiornamenti in modo coerente, anche in caso di cluster gestiti da righe \-of\-business o amministratori diversi.  
  
-   Aggiornamento compatibile con cluster è possibile pianificare l'aggiornamento viene eseguito in base a intervalli regolari giornaliere, settimanale o mensile per consentire di coordinare gli aggiornamenti del cluster con altri processi di gestione IT.  
  
-   Aggiornamento compatibile con cluster fornisce un'architettura estendibile per aggiornare l'inventario software di cluster in modo compatibile con cluster\. Può essere utilizzato dagli editori per coordinare l'installazione degli aggiornamenti software non pubblicati in Windows Update o Microsoft Update o che non sono disponibili da Microsoft, ad esempio, gli aggiornamenti per i driver di dispositivo fornitori non Microsoft.  
  
-   La modalità di aggiornamento e aggiornamento compatibile con cluster consente a un accessorio "cluster in una casella" \ (un set di computer fisici in cluster, in genere inserite in pacchetti in uno chassis\) ad aggiornare se stesso. In genere, questi dispositivi sono distribuiti in succursali con il supporto IT locale minimo per gestire i cluster. Modalità di aggiornamento e particolarmente preziosa in questi scenari di distribuzione.  
  
## <a name="important-functionality"></a>Funzionalità importanti  
Di seguito è una descrizione delle funzionalità importanti di aggiornamento compatibile con Cluster:  
  
-   Un'interfaccia utente \(UI\) - finestra di aggiornamento compatibile con Cluster - e un set di cmdlet che è possibile utilizzare per visualizzare l'anteprima, applicarli, monitorarli e segnalare gli aggiornamenti  
  
-   Un'automazione end-to-end dell'operazione di aggiornamento cluster\ \ (un aggiornamento di debug), orchestrata da uno o più computer coordinatore dell'aggiornamento  
  
-   Installazione di plug-in predefinito che si integra con l'esistente \(WUA\) agente di Windows Update e l'infrastruttura di Windows Server Update Services \(WSUS\) in Windows Server per applicare aggiornamenti importanti di Microsoft  
  
-   Un secondo plug-in che può essere utilizzato per applicare gli hotfix Microsoft e che può essere personalizzato per applicare gli aggiornamenti di fornitori non Microsoft  
  
-   Aggiornamento dei profili di esecuzione configurato con le impostazioni per le opzioni di operazione di aggiornamento, ad esempio il numero massimo di volte in cui l'aggiornamento verrà ripetuta per ogni nodo. Profili delle operazioni di aggiornamento consentono di riutilizzare rapidamente le stesse impostazioni più operazioni di aggiornamento e condividere facilmente le impostazioni di aggiornamento con altri cluster di failover.  
  
-   Un'architettura estensibile che supporta lo sviluppo nuova installazione di plug-in per coordinare altri strumenti di aggiornamento tra il cluster, ad esempio programmi di installazione software personalizzati, strumenti di aggiornamento del BIOS e di rete host o della scheda bus adapter \(HBA\) gli strumenti di aggiornamento.  
  
Aggiornamento compatibile con cluster consente di coordinare il cluster completato l'operazione in due modalità di aggiornamento:  
  
-   **Modalità di aggiornamento e** per questa modalità, il ruolo del cluster aggiornamento compatibile con cluster viene configurato come carico di lavoro nel cluster di failover che deve essere aggiornato e viene definita una pianificazione di aggiornamento associata. Il cluster si aggiorna automaticamente a intervalli pianificati tramite un profilo predefinito o personalizzato operazione di aggiornamento. Durante l'operazione di aggiornamento, avvia il processo del coordinatore aggiornamento di aggiornamento compatibile con cluster nel nodo attualmente proprietario del ruolo del cluster aggiornamento compatibile con cluster e il processo in modo sequenziale esegue gli aggiornamenti in ogni nodo del cluster. Per aggiornare il nodo del cluster corrente, il ruolo del cluster aggiornamento compatibile con cluster esegue il failover a un altro nodo del cluster e un nuovo processo di coordinatore dell'aggiornamento su tale nodo assume il controllo dell'operazione di aggiornamento. In modalità di aggiornamento e, aggiornamento compatibile con cluster è possibile aggiornare il cluster di failover mediante un processo di aggiornamento end-to-end e totalmente automatizzato. Un amministratore può anche attivare gli aggiornamenti on\ richiesta in questa modalità, o semplicemente utilizzare l'aggiornamento remote\ approccio se desiderate. Nella modalità di aggiornamento e, un amministratore può ottenere informazioni di riepilogo su un'operazione di aggiornamento in corso connettendosi al cluster ed eseguendo il **Get-CauRun** cmdlet di Windows PowerShell.  
  
-   **Modalità di aggiornamento Remote\** per questa modalità, un computer remoto, denominato coordinatore dell'aggiornamento, viene configurato con gli strumenti di aggiornamento compatibile con cluster. Il coordinatore dell'aggiornamento non è un membro del cluster aggiornato nel corso dell'operazione di aggiornamento. Dal computer remoto, l'amministratore può attivare un'operazione di aggiornamento della richiesta on\ utilizzando un profilo predefinito o personalizzato operazione di aggiornamento. Modalità di aggiornamento Remote\ è utile per il monitoraggio dello stato real\ tempo durante l'operazione di aggiornamento e per i cluster di cui sono in esecuzione in installazioni Server Core.  
  
## <a name="hardware-and-software-requirements"></a>Requisiti hardware e software  

Aggiornamento compatibile con cluster può essere usato in tutte le edizioni di Windows Server, incluse le installazioni Server Core. Per informazioni dettagliate sui requisiti, vedere [aggiornamento compatibile con Cluster requisiti e procedure consigliate](cluster-aware-updating-requirements.md).

### <a name="installing-cluster-aware-updating"></a>L'installazione di aggiornamento compatibile con Cluster
Per usare aggiornamento compatibile con cluster, installare la funzionalità Clustering di Failover in Windows Server e creare un cluster di failover. I componenti che supportano la funzionalità aggiornamento compatibile con cluster vengono installati automaticamente in ogni nodo del cluster.

Per installare la funzionalità Clustering di Failover, è possibile utilizzare gli strumenti seguenti:
- Aggiunta guidata ruoli e funzionalità in Server Manager
- [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/install-windowsfeature) cmdlet di Windows PowerShell
- Strumento da riga di comando Gestione e manutenzione immagini distribuzione

Per ulteriori informazioni, vedere [installazione o disinstallazione di ruoli, servizi ruolo o funzionalità](https://technet.microsoft.com/library/hh831809(v=ws.11).aspx).

È inoltre necessario installare gli strumenti di Clustering di Failover, che fanno parte di strumenti di amministrazione remota del Server e vengono installati per impostazione predefinita quando si installa la funzionalità Clustering di Failover in Server Manager. Gli strumenti di Clustering di Failover includono l'interfaccia utente di aggiornamento compatibile con Cluster e i cmdlet di PowerShell. 

È necessario installare gli strumenti per Clustering di Failover come indicato di seguito per supportare le diverse modalità di aggiornamento di aggiornamento compatibile con cluster:

- Per usare aggiornamento compatibile con cluster in modalità di aggiornamento e, installare gli strumenti per Clustering di Failover in ogni nodo del cluster.   
  
- Per abilitare la modalità di aggiornamento remote\, installare gli strumenti per Clustering di Failover in un computer che dispone di connettività di rete al cluster di failover.  
  
> [!NOTE]  
> -   È possibile utilizzare gli strumenti per Clustering di Failover in Windows Server 2012 per gestire l'aggiornamento compatibile con Cluster in una versione più recente di Windows Server. 
> -   Per usare aggiornamento compatibile con cluster solo in modalità di aggiornamento remote\, installazione di strumenti per Clustering di Failover nei nodi del cluster non è necessario. Tuttavia, alcune funzionalità di aggiornamento compatibile con cluster non sarà disponibile. Per ulteriori informazioni, vedere [requisiti e procedure consigliate per aggiornamento compatibile con Cluster\](cluster-aware-updating-requirements.md).  
> -   A meno che non si utilizza aggiornamento compatibile con cluster solo in modalità di aggiornamento e, il computer in cui sono installati gli strumenti di aggiornamento compatibile con cluster e che coordina gli aggiornamenti non può essere un membro del cluster di failover.  
  
### <a name="enabling-self-updating-mode"></a>Abilitazione della modalità di aggiornamento automatico
Per abilitare la modalità di aggiornamento automatico, è necessario aggiungere il ruolo del cluster aggiornamento compatibile con Cluster al cluster di failover. A tale scopo, utilizzare uno dei metodi seguenti:
- In Server Manager, selezionare **strumenti** > **aggiornamento compatibile con Cluster**, quindi nella finestra di aggiornamento compatibile con Cluster, selezionare **cluster configurare opzioni di aggiornamento automatico**. 
- In una sessione di PowerShell, eseguire il [Add-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole) cmdlet.  
  
Per disinstallare aggiornamento compatibile con cluster, disinstallare la funzionalità Clustering di Failover o strumenti per Clustering di Failover tramite Server Manager, il [Uninstall-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/uninstall-windowsfeature) cmdlet o gli strumenti della riga di comando DISM.  
  
### <a name="additional-requirements-and-best-practices"></a>Altri requisiti e procedure consigliate  

Per assicurarsi che sia possibile aggiornare correttamente i nodi del cluster e per indicazioni aggiuntive per la configurazione dell'ambiente cluster di failover per l'utilizzo di aggiornamento compatibile con cluster, è possibile eseguire Best Practices Analyzer di aggiornamento compatibile con cluster.  
  
Per informazioni dettagliate sui requisiti e procedure consigliate per l'utilizzo di aggiornamento compatibile con cluster e informazioni sull'esecuzione di Best Practices Analyzer, vedere [requisiti e procedure consigliate per aggiornamento compatibile con Cluster\](cluster-aware-updating-requirements.md).  
  
### <a name="starting-cluster-aware-updating"></a>Avvio di aggiornamento compatibile con Cluster  
  
##### <a name="to-start-cluster-aware-updating-from-server-manager"></a>Per avviare l'aggiornamento compatibile con Cluster da Server Manager  
  
1.  Avviare Server Manager.  
  
2.  Eseguire una delle operazioni seguenti:  
  
    -   Nel **strumenti** menu, fare clic su **aggiornamento compatibile con Cluster\**.  
  
    -   Se uno o più nodi del cluster o il cluster, viene aggiunto a Server Manager, nel **tutti i server** pagina, fare clic con il nome di un nodo \ (o il nome del cluster\), quindi fare clic su **Cluster di aggiornamento**.  
  
## <a name="see-also"></a>Vedere anche  
I seguenti collegamenti forniscono ulteriori informazioni sull'utilizzo di aggiornamento compatibile con Cluster.  
  
-   [Requisiti e procedure consigliate per aggiornamento compatibile con Cluster\](cluster-aware-updating.md)  
  
-   [Aggiornamento compatibile con Cluster\: Domande frequenti](cluster-aware-updating-faq.md)  
  
-   [Opzioni avanzate e profili delle operazioni di aggiornamento per aggiornamento compatibile con cluster](cluster-aware-updating-options.md)  
  
-   [Funzionamento installazione di plug-in aggiornamento compatibile con cluster](cluster-aware-updating-plug-ins.md)  
  
-   [Cmdlet per aggiornamento compatibile con Cluster\ in Windows PowerShell](https://go.microsoft.com/fwlink/p/?LinkId=237675)  
  
-   [Cluster\ aggiornamento compatibile con installazione di plug-in di riferimento](https://msdn.microsoft.com/library/hh418084.aspx)  
  

