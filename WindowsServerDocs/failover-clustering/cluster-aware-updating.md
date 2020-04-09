---
title: Panoramica di Aggiornamento compatibile con cluster
description: Aggiornamento compatibile con cluster automatizza l'installazione degli aggiornamenti software nei cluster che eseguono Windows Server.
ms.topic: article
ms.prod: windows-server
manager: lizross
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 08/06/2018
ms.assetid: 3c2993b4-aa81-452b-a5c3-3724ad95d892
ms.openlocfilehash: 6a2b6ad06b8a003f9cbf020956994b08cb8cf194
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80827994"
---
# <a name="cluster-aware-updating-overview"></a>Panoramica di Aggiornamento compatibile con cluster

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento fornisce una panoramica dell'aggiornamento compatibile con cluster\-\(aggiornamento compatibile con cluster\), una funzionalità che consente di automatizzare il processo di aggiornamento del software nei server del cluster mantenendo la disponibilità.

> [!NOTE]
> Quando si aggiornano i cluster di [spazi di archiviazione diretta](../storage/storage-spaces/storage-spaces-direct-overview.md) , è consigliabile usare l'aggiornamento compatibile con cluster.
  
## <a name="feature-description"></a><a name="BKMK_OVER"></a>Descrizione delle funzionalità  
Aggiornamento compatibile con cluster è una funzionalità automatizzata che consente di aggiornare i server in un [cluster di failover](failover-clustering-overview.md) con una perdita di disponibilità minima o nulla durante il processo di aggiornamento. Durante un'operazione di aggiornamento, l'aggiornamento compatibile con cluster esegue in modo trasparente le attività seguenti:  

1. Inserisce ogni nodo del cluster in modalità di manutenzione del nodo.
2. Sposta i ruoli del cluster fuori dal nodo.
3. Installa gli aggiornamenti e gli eventuali aggiornamenti dipendenti.
4. Se necessario, esegue un riavvio.
5. Porta fuori dalla modalità di manutenzione del nodo.
6. Ripristina i ruoli del cluster nel nodo.
7. Consente di spostare l'aggiornamento del nodo successivo.

Per molti ruoli del cluster nel cluster, il processo di aggiornamento automatico attiva un failover pianificato. Ciò può causare un'interruzione del servizio temporanea per i client connessi. Tuttavia, nel caso di carichi di lavoro continuamente disponibili, ad esempio Hyper\-V con migrazione in tempo reale o file server con failover trasparente SMB, aggiornamento compatibile con cluster consente di coordinare gli aggiornamenti del cluster senza alcun effetto sulla disponibilità del servizio.    
  
## <a name="practical-applications"></a>Applicazioni pratiche  
  
-   Aggiornamento compatibile con cluster riduce le interruzioni del servizio nei servizi del cluster, riduce la necessità di soluzioni alternative per l'aggiornamento manuale e rende il\-finale\-fine del processo di aggiornamento del cluster più affidabile per l'amministratore. Quando la funzionalità aggiornamento compatibile con cluster viene utilizzata insieme ai carichi di lavoro del cluster continuamente disponibili, ad esempio file server continuamente disponibili \(file server carico di lavoro con failover trasparente SMB\) o Hyper\-V, gli aggiornamenti del cluster possono essere eseguiti senza alcun effetto sulla disponibilità dei servizi per i client.  
  
-   Con Aggiornamento compatibile con cluster risulta facilitata l'adozione di processi IT coerenti nell'intera organizzazione. È possibile creare profili delle operazioni di aggiornamento per classi diverse di cluster di failover e quindi gestirli a livello centrale in una condivisione file per garantire che le distribuzioni di aggiornamento compatibile con cluster nell'intera organizzazione IT applichino gli aggiornamenti in modo coerente, anche se i cluster sono gestiti da righe diverse\-di\-business o amministratori.  
  
-   Aggiornamento compatibile con cluster supporta la pianificazione delle operazioni di aggiornamento a intervalli regolari giornalieri, settimanali o mensili per agevolare il coordinamento degli aggiornamenti del cluster con altri processi di gestione IT.  
  
-   Aggiornamento compatibile con cluster fornisce un'architettura estendibile per aggiornare l'inventario software del cluster in un cluster\-la modalità di compatibilità. Questa operazione può essere utilizzata dagli autori per coordinare l'installazione degli aggiornamenti software non pubblicati in Windows Update o Microsoft Update o che non sono disponibili da Microsoft, ad esempio aggiornamenti per i driver di dispositivo non\-Microsoft.  
  
-   La modalità di aggiornamento di aggiornamento compatibile con cluster\-consente a un dispositivo "cluster in a box" \(un set di computer fisici in cluster, in genere assemblati in un unico chassis\) di aggiornarsi autonomamente. Questi sistemi vengono in genere distribuiti in succursali con un supporto IT locale minimo per la gestione dei cluster. La modalità di aggiornamento automatico di\-offre un notevole valore in questi scenari di distribuzione.  
  
## <a name="important-functionality"></a>Funzionalità importanti  
Di seguito è riportata una descrizione di importanti funzionalità di aggiornamento compatibile con cluster:  
  
-   Interfaccia utente \(\) dell'interfaccia utente-la finestra di aggiornamento compatibile con cluster e un set di cmdlet che è possibile usare per visualizzare in anteprima, applicare, monitorare e creare report sugli aggiornamenti  
  
-   Un\-end per\-l'automazione finale del cluster\-operazione di aggiornamento \(un'operazione di aggiornamento\), orchestrata da uno o più computer coordinatore dell'aggiornamento  
  
-   Un\-di plug-in che si integra con l'agente di Windows Update esistente \(WUA\) e Windows Server Update Services \(WSUS\) infrastruttura di Windows Server per l'applicazione di importanti aggiornamenti Microsoft  
  
-   Un secondo plug\-in che può essere usato per applicare gli hotfix Microsoft e che può essere personalizzato per applicare aggiornamenti non\-Microsoft  
  
-   Profili delle operazioni di aggiornamento configurabili con impostazioni per le opzioni delle operazioni di aggiornamento, come il numero massimo di tentativi di applicazione dell'aggiornamento per nodo. I profili delle operazioni di aggiornamento consentono di riutilizzare rapidamente le stesse impostazioni per più operazioni di aggiornamento e di condividere facilmente le impostazioni di aggiornamento con altri cluster di failover.  
  
-   Un'architettura estendibile che supporta nuovi plug\-in fase di sviluppo per coordinare altri nodi\-l'aggiornamento degli strumenti nel cluster, ad esempio programmi di installazione di software personalizzati, strumenti di aggiornamento del BIOS e scheda di rete o scheda bus host \(HBA\) gli strumenti di aggiornamento.  
  
Aggiornamento compatibile con cluster consente di coordinare l'operazione di aggiornamento del cluster completa in due modalità:  
  
-   **Modalità di aggiornamento automatico\-** Per questa modalità, il ruolo del cluster di aggiornamento compatibile con cluster viene configurato come carico di lavoro nel cluster di failover da aggiornare e viene definita una pianificazione di aggiornamento associata. Il cluster si aggiorna automaticamente a intervalli pianificati tramite un profilo dell'operazione di aggiornamento predefinito o personalizzato. Nel corso dell'operazione di aggiornamento, il processo del coordinatore dell'aggiornamento di Aggiornamento compatibile con cluster viene avviato sul nodo proprietario del ruolo cluster di Aggiornamento compatibile con cluster e il processo esegue gli aggiornamenti in sequenza su ogni nodo del cluster. Per aggiornare il nodo del cluster corrente, il ruolo del cluster di Aggiornamento compatibile con cluster esegue il failover su un altro nodo del cluster e un nuovo processo del coordinatore dell'aggiornamento su tale nodo assume il controllo dell'operazione di aggiornamento. In modalità di aggiornamento\-automatico, aggiornamento compatibile con cluster è in grado di aggiornare il cluster di failover usando un\-finale completamente automatizzato per\-terminare il processo di aggiornamento. Un amministratore può anche attivare gli aggiornamenti su richiesta\-in questa modalità oppure utilizzare semplicemente l'approccio di aggiornamento remoto\-se lo si desidera. In modalità di aggiornamento automatico\-un amministratore può ottenere informazioni di riepilogo su un'operazione di aggiornamento in corso connettendosi al cluster ed eseguendo il cmdlet **get\-CauRun** di Windows PowerShell.  
  
-   **Modalità di aggiornamento\-remoto** Per questa modalità, un computer remoto, denominato Coordinatore dell'aggiornamento, viene configurato con gli strumenti di aggiornamento compatibile con cluster. Il coordinatore dell'aggiornamento non è un membro del cluster aggiornato nel corso dell'operazione di aggiornamento. Dal computer remoto, l'amministratore attiva un'operazione di aggiornamento su richiesta\-utilizzando un profilo dell'operazione di aggiornamento predefinito o personalizzato. La modalità di aggiornamento\-remoto è utile per il monitoraggio dello stato di avanzamento del\-reale durante l'operazione di aggiornamento e per i cluster in esecuzione in installazioni dei componenti di base del server.  
  
## <a name="hardware-and-software-requirements"></a>Requisiti hardware e software  

Aggiornamento compatibile con cluster può essere usato in tutte le edizioni di Windows Server, incluse le installazioni Server Core. Per informazioni dettagliate sui requisiti, vedere [requisiti e procedure consigliate per l'aggiornamento compatibile con cluster](cluster-aware-updating-requirements.md).

### <a name="installing-cluster-aware-updating"></a>Installazione di aggiornamento compatibile con cluster
Per usare aggiornamento compatibile con cluster, installare la funzionalità clustering di failover in Windows Server e creare un cluster di failover. I componenti che supportano la funzionalità di Aggiornamento compatibile con cluster vengono automaticamente installati in ogni nodo del cluster.

Per installare la funzionalità Clustering di failover, è possibile usare gli strumenti seguenti:
- Aggiunta guidata ruoli e funzionalità in Server Manager
- [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/Install-WindowsFeature?view=winserver2012r2-ps&viewFallbackFrom=win10-ps) cmdlet di Windows PowerShell
- Strumento da riga di comando di Gestione e manutenzione immagini distribuzione

Per ulteriori informazioni, vedere [installare la funzionalità clustering di failover](create-failover-cluster.md#install-the-failover-clustering-feature).

È inoltre necessario installare gli strumenti di clustering di failover, che fanno parte del Strumenti di amministrazione remota del server e vengono installati per impostazione predefinita quando si installa la funzionalità clustering di failover in Server Manager. Gli strumenti di clustering di failover includono l'interfaccia utente di aggiornamento compatibile con cluster e i cmdlet di PowerShell. 

È necessario installare gli Strumenti per Clustering di failover come indicato di seguito per poter supportare le diverse modalità di aggiornamento di Aggiornamento compatibile con cluster:

- Per usare aggiornamento compatibile con cluster in modalità di aggiornamento\-automatico, installare gli strumenti di clustering di failover in ogni nodo del cluster.   
  
- Per abilitare la modalità di aggiornamento\-remoto, installare gli strumenti di clustering di failover in un computer con connettività di rete al cluster di failover.  
  
> [!NOTE]  
> -   Non è possibile usare gli strumenti di clustering di failover in Windows Server 2012 per gestire l'aggiornamento compatibile con cluster in una versione più recente di Windows Server. 
> -   Per usare aggiornamento compatibile con cluster solo nella modalità di aggiornamento\-remoto, non è necessario installare gli strumenti di clustering di failover nei nodi del cluster. Certe funzionalità di Aggiornamento compatibile con cluster, tuttavia, non saranno disponibili. Per ulteriori informazioni, vedere [requisiti e procedure consigliate per l'aggiornamento compatibile con\-cluster](cluster-aware-updating-requirements.md).  
> -   A meno che non si usi aggiornamento compatibile con cluster solo nella modalità di aggiornamento\-automatico, il computer in cui sono installati gli strumenti di aggiornamento compatibile con cluster e che coordina gli aggiornamenti non può essere un membro del cluster di failover.  
  
### <a name="enabling-self-updating-mode"></a>Abilitazione della modalità di aggiornamento automatico
Per abilitare la modalità di aggiornamento automatico, è necessario aggiungere il ruolo del cluster di aggiornamento compatibile con cluster al cluster di failover. A tale scopo, usare uno dei metodi seguenti:
- In Server Manager selezionare **strumenti** > **aggiornamento compatibile con cluster**, quindi nella finestra aggiornamento compatibile con cluster selezionare **Configura opzioni di aggiornamento automatico del cluster**. 
- In una sessione di PowerShell eseguire il cmdlet [Add-CauClusterRole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole?view=win10-ps) .  
  
Per disinstallare aggiornamento compatibile con cluster, disinstallare la funzionalità clustering di failover o gli strumenti di clustering di failover usando Server Manager, il cmdlet [Uninstall-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/Uninstall-WindowsFeature?view=win10-ps) o il comando DISM\-Line Tools.  
  
### <a name="additional-requirements-and-best-practices"></a>Requisiti aggiuntivi e procedure consigliate  

Per assicurarsi che sia possibile aggiornare correttamente i nodi del cluster con Aggiornamento compatibile con cluster e per indicazioni aggiuntive per la configurazione dell'ambiente cluster di failover per l'utilizzo di Aggiornamento compatibile con cluster, è possibile eseguire Best Practices Analyzer.  
  
Per informazioni dettagliate sui requisiti e Best Practices Analyzer le procedure consigliate per l'uso di aggiornamento compatibile con cluster e informazioni sull'esecuzione di [aggiornamento compatibile con cluster, vedere requisiti e procedure consigliate per l'aggiornamento compatibile con\-](cluster-aware-updating-requirements.md).  
  
### <a name="starting-cluster-aware-updating"></a>Avvio di aggiornamento compatibile con cluster  
  
##### <a name="to-start-cluster-aware-updating-from-server-manager"></a>Per avviare l'aggiornamento compatibile con cluster da Server Manager  
  
1.  Avviare Server Manager.  
  
2.  Esegui una delle operazioni seguenti:  
  
    -   Nel menu **strumenti** fare clic su **cluster\-aggiornamento compatibile**.  
  
    -   Se uno o più nodi del cluster, o il cluster, vengono aggiunti a Server Manager, nella pagina **tutti i server** fare clic con il pulsante destro del mouse\-fare clic sul nome di un nodo \(o sul nome del cluster\)e quindi fare clic su **Aggiorna cluster**.  
  
## <a name="see-also"></a>Vedere anche  
I collegamenti seguenti forniscono ulteriori informazioni sull'utilizzo di aggiornamento compatibile con cluster.  
  
-   [Requisiti e procedure consigliate per Aggiornamento compatibile con cluster\-  
  
-   [Aggiornamento compatibile con\-cluster: domande frequenti](cluster-aware-updating-faq.md)  
  
-   [Opzioni avanzate e profili di esecuzione aggiornamento per aggiornamento compatibile con cluster](cluster-aware-updating-options.md)  
  
-   [Funzionamento del plug-in di aggiornamento compatibile con cluster\-](cluster-aware-updating-plug-ins.md)  
  
-   [Cmdlet di aggiornamento compatibile con\-cluster in Windows PowerShell](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps&viewFallbackFrom=winserverr2-ps)  
  
-   [\-del plug-in di aggiornamento compatibile con\-cluster in riferimento](https://docs.microsoft.com/previous-versions/windows/desktop/mscs/cluster-aware-update-plug-in-interfaces-and-classes)  
  

