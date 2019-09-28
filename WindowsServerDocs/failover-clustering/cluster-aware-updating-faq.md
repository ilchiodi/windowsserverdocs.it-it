---
ms.assetid: 6416d125-bcaf-433d-971a-2f0283bca2c2
title: Aggiornamento compatibile con cluster-domande frequenti
ms.topic: article
ms.prod: windows-server
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 04/28/2017
description: Risposte alle domande frequenti sull'aggiornamento compatibile con cluster in Windows Server.
ms.openlocfilehash: a08366c7e64d9612d63e348d4cecdb4b2389737a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361354"
---
# <a name="cluster-aware-updating-frequently-asked-questions"></a>Aggiornamento compatibile con cluster: Domande frequenti

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

[Aggiornamento compatibile con Cluster](cluster-aware-updating.md) \(CAU @ no__t-2 è una funzionalità che coordina gli aggiornamenti software in tutti i server in un cluster di failover in modo che non influisca sulla disponibilità del servizio più di un failover pianificato di un nodo del cluster. Per alcune applicazioni con funzionalità di disponibilità continua \(such come Hyper @ no__t-1V con migrazione in tempo reale o SMB 3. x file server con failover trasparente SMB @ no__t-2, aggiornamento compatibile con cluster può coordinare l'aggiornamento automatico del cluster senza alcun effetto sul servizio disponibilità.

## <a name="does-cau-support-updating-storage-spaces-direct-clusters"></a>Aggiornamento compatibile con cluster supporta l'aggiornamento di cluster Spazi di archiviazione diretta?  
Sì. Aggiornamento compatibile con cluster supporta l'aggiornamento di cluster di [spazi di archiviazione diretta](../storage/storage-spaces/storage-spaces-direct-overview.md) indipendentemente dal tipo di distribuzione: iperconvergente o convergente. In particolare, l'orchestrazione di aggiornamento compatibile con cluster garantisce che la sospensione di ogni nodo del cluster sia in attesa che lo spazio di archiviazione del cluster sottostante sia integro.

## <a name="does-cau-work-with-windowsserver-2008r2-or-windows7"></a>Aggiornamento compatibile con cluster funziona con Windows Server 2008 R2 o con Windows 7?  
No. Aggiornamento compatibile con cluster coordina l'operazione di aggiornamento del cluster solo da computer che eseguono Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10, Windows 8.1 o Windows 8. Il cluster di failover da aggiornare deve eseguire Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.
  
## <a name="is-cau-limited-to-specific-clustered-applications"></a>Aggiornamento compatibile con cluster è limitato a specifiche applicazioni cluster?  
No. La funzionalità Aggiornamento compatibile con cluster è indipendente dal tipo di applicazione in cluster. Aggiornamento compatibile con cluster è una soluzione di cluster esterno @ no__t-0updating che è sovrapposta alle API di clustering e ai cmdlet di PowerShell. Di conseguenza, aggiornamento compatibile con cluster può coordinare l'aggiornamento per qualsiasi applicazione in cluster configurata in un cluster di failover di Windows Server.  
  
> [!NOTE]  
> Attualmente, i carichi di lavoro cluster seguenti sono testati e certificati per aggiornamento compatibile con cluster: SMB, Hyper @ no__t-0V, Replica DFS, spazi dei nomi DFS, iSCSI e NFS.  
  
## <a name="does-cau-support-updates-from-microsoft-update-and-windows-update"></a>Aggiornamento compatibile con cluster supporta gli aggiornamenti da Microsoft Update e Windows Update?  
Sì. Per impostazione predefinita, aggiornamento compatibile con cluster è configurato con un plug-in no__t-0cm che usa le API di utilità Windows Update Agent \(WUA @ no__t-2 nei nodi del cluster. L'infrastruttura WUA può essere configurata in modo da puntare a Microsoft Update e Windows Update o a Windows Server Update Services \(WSUS @ no__t-1 come origine degli aggiornamenti.  
  
## <a name="does-cau-support-wsus-updates"></a>Aggiornamento compatibile con cluster supporta gli aggiornamenti da Windows Server Update Services (WSUS)?  
Sì. Per impostazione predefinita, aggiornamento compatibile con cluster è configurato con un plug-in no__t-0cm che usa le API di utilità Windows Update Agent \(WUA @ no__t-2 nei nodi del cluster. L'infrastruttura WUA può essere configurata in modo che punti a Microsoft Update e Windows Update o a un Windows Server Update Services locale \(WSUS @ no__t-1 server come origine degli aggiornamenti.  
  
## <a name="can-cau-apply-limited-distribution-release-updates"></a>La funzionalità Aggiornamento compatibile con cluster può applicare aggiornamenti a distribuzione ridotta?  
Sì. Gli aggiornamenti della versione di distribuzione limitata \(LDR @ no__t-1, detti anche hotfix, non vengono pubblicati tramite Microsoft Update o Windows Update, quindi non possono essere scaricati dall'agente Windows Update \(WUA @ no__t-3 plug @ no__t-4in usato da aggiornamento compatibile con cluster per impostazione predefinita .  
  
Tuttavia, aggiornamento compatibile con cluster include un secondo plug @ no__t-0cm che è possibile selezionare per applicare gli aggiornamenti rapidi. Questo hotfix plug @ no__t-0cm può essere personalizzato anche per applicare un driver non @ no__t-1Microsoft, firmware e aggiornamenti del BIOS.  
  
## <a name="can-i-use-cau-to-apply-cumulative-updates"></a>È possibile usare Aggiornamento compatibile con cluster per applicare aggiornamenti cumulativi?  
Sì. Se gli aggiornamenti cumulativi sono aggiornamenti pubblici o aggiornamenti a distribuzione ridotta, possono essere applicati tramite Aggiornamento compatibile con cluster.  
  
## <a name="can-i-schedule-updates"></a>È possibile pianificare gli aggiornamenti?  
Sì. Aggiornamento compatibile con cluster supporta le modalità di aggiornamento seguenti, entrambe le quali consentono la pianificazione degli aggiornamenti:  
  
**Self @ no__t-1updating** Consente al cluster di aggiornarsi in base a un profilo definito e a una pianificazione regolare, ad esempio durante una finestra di manutenzione mensile. È anche possibile avviare un self @ no__t-0Updating eseguito su richiesta in qualsiasi momento. Per abilitare la modalità self-no__t-0updating, è necessario aggiungere il ruolo del cluster di aggiornamento compatibile con cluster al cluster. La funzionalità aggiornamento compatibile con cluster self @ no__t-0updating funziona come qualsiasi altro carico di lavoro in cluster e può funzionare senza problemi con i failover pianificati e non pianificati di un computer coordinatore dell'aggiornamento.  
  
**Remote @ no__t-1updating** Consente di avviare un'operazione di aggiornamento in qualsiasi momento da un computer in cui viene eseguito Windows o Windows Server. È possibile avviare un'operazione di aggiornamento tramite la finestra di aggiornamento compatibile con cluster o usando il cmdlet di PowerShell **Invoke @ no__t-1CauRun** . Remote @ no__t-0updating è la modalità di aggiornamento predefinita per aggiornamento compatibile con cluster. È possibile usare l'Utilità di pianificazione per eseguire il cmdlet [Invoke-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun) in base alla pianificazione desiderata da un computer remoto diverso dai nodi del cluster.  
  
## <a name="can-i-schedule-updates-to-apply-during-a-backup"></a>È possibile pianificare gli aggiornamenti da applicare durante un backup?  
Sì. Aggiornamento compatibile con cluster non impone alcun vincolo in questo senso. Tuttavia, l'esecuzione di aggiornamenti software in un server @no__t 0with i potenziali riavvii associati @ no__t-1 mentre è in corso un backup del server non è una procedura consigliata. Tenere presente che Aggiornamento compatibile con cluster si basa solo sulle API di clustering per determinare i failover e i failback delle risorse, di conseguenza Aggiornamento compatibile con cluster non è a conoscenza dello stato di backup del server.  
  
## <a name="can-cau-work-with-system-center-configuration-manager"></a>Aggiornamento compatibile con cluster può essere usato con System Center Configuration Manager?  
Aggiornamento compatibile con cluster è uno strumento che coordina gli aggiornamenti software in un nodo del cluster e Configuration Manager inoltre esegue gli aggiornamenti del software del server. È importante configurare questi strumenti in modo che non abbiano una copertura sovrapposta degli stessi server in qualsiasi distribuzione di Data Center, incluso l'uso di server Windows Server Update Services diversi. In questo modo si garantisce che l'obiettivo alla base dell'utilizzo di aggiornamento compatibile con cluster non venga inavvertitamente sconfitto, perché l'aggiornamento di Configuration Manager @ no__t-0driven non incorpora la  
  
## <a name="do-i-need-administrative-credentials-to-run-cau"></a>Sono necessarie credenziali amministrative per eseguire Aggiornamento compatibile con cluster?  
Sì. Per l'esecuzione degli strumenti di Aggiornamento compatibile con cluster sono necessarie credenziali amministrative nel server locale oppure il diritto utente **Rappresenta un client dopo l'autenticazione** nel server locale o nel computer client in cui è in esecuzione. Tuttavia, per coordinare gli aggiornamenti software nei nodi del cluster, Aggiornamento compatibile con cluster richiede credenziali amministrative per il cluster in ogni nodo. Sebbene l'interfaccia utente di aggiornamento compatibile con cluster possa essere avviata senza le credenziali, richiede le credenziali amministrative del cluster quando si connette a un'istanza del cluster per visualizzare in anteprima o applicare gli aggiornamenti.  
  
## <a name="can-i-script-cau"></a>È possibile creare uno script di aggiornamento compatibile con cluster?  
Sì. Aggiornamento compatibile con cluster include cmdlet di PowerShell che offrono un set completo di opzioni di scripting. Sono gli stessi cmdlet che l'interfaccia utente di Aggiornamento compatibile con cluster chiama per eseguire le azioni di aggiornamento.  

## <a name="what-happens-to-active-clustered-roles"></a>Cosa accade ai ruoli del cluster attivi?

I ruoli del cluster \(formerly chiamati applicazioni e servizi @ no__t-1 che sono attivi in un nodo, eseguono il failover su altri nodi prima che l'aggiornamento del software possa iniziare. Aggiornamento compatibile con cluster orchestra i failover tramite la modalità manutenzione, che sospende e svuota il nodo da tutti i ruoli del cluster attivi. Una volta completati gli aggiornamenti software, Aggiornamento compatibile con cluster riprende il nodo ed esegue il failback dei ruoli del cluster nel nodo aggiornato. Questo garantisce che la distribuzione dei nodi del cluster relativi ai nodi rimanga invariata durante le operazioni di aggiornamento di un cluster.  
  
## <a name="how-does-cau-select-target-nodes-for-clustered-roles"></a>In che modo aggiornamento compatibile con cluster seleziona i nodi di destinazione per i ruoli del cluster?

Aggiornamento compatibile con cluster usa le API di clustering per coordinare i failover. L'implementazione dell'API di clustering seleziona i nodi di destinazione basandosi su metriche interne e euristica di posizionamento intelligente \(such come livelli del carico di lavoro @ no__t-1 tra i nodi di destinazione.  
  
## <a name="does-cau-load-balance-the-clustered-roles"></a>Aggiornamento compatibile con cluster esegue il bilanciamento del carico dei ruoli del cluster?

Aggiornamento compatibile con cluster non esegue il bilanciamento del carico dei nodi del cluster, ma tenta di preservare la distribuzione dei ruoli del cluster. Al termine dell'aggiornamento di un nodo del cluster, tenta di eseguire il failback dei ruoli del cluster nei nodi che li ospitavano in precedenza. Aggiornamento compatibile con cluster usa le API di clustering per eseguire il failback delle risorse all'inizio del processo di sospensione. Di conseguenza, in assenza di failover pianificati e di impostazioni del proprietario preferito, la distribuzione dei nodi del cluster deve rimanere invariata.  
  
## <a name="how-does-cau-select-the-order-of-nodes-to-update"></a>In che modo Aggiornamento compatibile con cluster seleziona l'ordine dei nodi da aggiornare?  
Per impostazione predefinita, Aggiornamento compatibile con cluster seleziona l'ordine dei nodi da aggiornare in base al livello di attività. I nodi che ospitano il minor numero di ruoli del cluster vengono aggiornati per primi. Un amministratore può tuttavia specificare un ordine specifico per l'aggiornamento dei nodi specificando un parametro per l'operazione di aggiornamento nell'interfaccia utente di aggiornamento compatibile con cluster o tramite i cmdlet di PowerShell.  
  
## <a name="what-happens-if-a-cluster-node-is-offline"></a>Cosa accade se un nodo del cluster è offline?

L'amministratore che avvia un'operazione di aggiornamento può specificare la soglia accettabile per il numero di nodi che possono essere offline. Di conseguenza, è possibile eseguire un'operazione di aggiornamento in un cluster anche se non tutti i nodi del cluster sono online.  
  
## <a name="can-i-use-cau-to-update-only-a-single-node"></a>È possibile usare aggiornamento compatibile con cluster per aggiornare solo un singolo nodo?  
No. Aggiornamento compatibile con cluster è uno strumento di aggiornamento cluster @ no__t-0scoped che consente di selezionare i cluster da aggiornare. Per aggiornare un singolo nodo, è possibile usare gli strumenti di aggiornamento del server esistenti, indipendentemente da Aggiornamento compatibile con cluster.  
  
## <a name="can-cau-report-updates-that-are-initiated-from-outside-cau"></a>Aggiornamento compatibile con cluster può segnalare gli aggiornamenti avviati dall'esterno di aggiornamento compatibile con cluster?  
No. Aggiornamento compatibile con cluster può segnalare solo operazioni di aggiornamento avviate dall'interno di Aggiornamento compatibile con cluster. Tuttavia, quando viene avviata un'operazione di aggiornamento di aggiornamento compatibile con cluster successiva, gli aggiornamenti installati tramite metodi non @ no__t-0CAU sono considerati in modo appropriato per determinare gli aggiornamenti aggiuntivi che potrebbero essere applicabili a ogni nodo del cluster.  
  
## <a name="can-cau-support-my-unique-it-process-needs"></a>Aggiornamento compatibile con cluster supporta le esigenze di un processo IT univoco?  
Sì. Aggiornamento compatibile con cluster offre le dimensioni di flessibilità seguenti per soddisfare le esigenze specifiche dei clienti aziendali in termini di processi IT:  
  
**Script** Un'operazione di aggiornamento può specificare uno script di PowerShell pre @ no__t-1Update e uno script di PowerShell post @ no__t-2Update. Lo script pre @ no__t-0update viene eseguito in ogni nodo del cluster prima che il nodo venga sospeso. Lo script post @ no__t-0update viene eseguito in ogni nodo del cluster dopo l'installazione degli aggiornamenti del nodo.  
  
> [!NOTE]  
> .NET Framework 4,6 o 4,5 e PowerShell devono essere installati in ogni nodo del cluster in cui si desidera eseguire gli script pre @ no__t-0update e post @ no__t-1Update. È anche necessario abilitare la comunicazione remota di PowerShell nei nodi del cluster. Per informazioni dettagliate sui requisiti di sistema, vedere [requisiti e procedure consigliate per aggiornamento compatibile con cluster](cluster-aware-updating-requirements.md).  
  
**Opzioni di esecuzione aggiornamento avanzato** L'amministratore può inoltre specificare da un ampio set di opzioni di esecuzione aggiornamento avanzate, ad esempio il numero massimo di tentativi di esecuzione del processo di aggiornamento in ogni nodo. Queste opzioni possono essere specificate usando l'interfaccia utente di aggiornamento compatibile con cluster o i cmdlet di PowerShell per aggiornamento compatibile con cluster. Queste impostazioni personalizzate possono essere salvate in un profilo dell'operazione di aggiornamento e riutilizzate per operazioni di aggiornamento successive.  
  
**Plug-in pubblico @ no__t-architettura 1in** Aggiornamento compatibile con cluster include funzionalità per la registrazione, l'annullamento della registrazione e la selezione di plug @ no__t-2Ins. Aggiornamento compatibile con cluster viene fornito con due plug-in predefinito no__t-0ins: uno coordina le API di Windows Update Agent \(WUA @ no__t-2 in ogni nodo del cluster. il secondo applica gli hotfix che vengono copiati manualmente in una condivisione file accessibile ai nodi del cluster. Se un'azienda ha esigenze specifiche che non possono essere soddisfatte con questi due plug-in no__t-0ins, l'azienda può creare un nuovo plug-in di aggiornamento compatibile con cluster @ no__t-1in in base alla specifica API pubblica. Per altre informazioni, vedere il [riferimento a cluster @ no__t-1Aware per l'aggiornamento del plug @ no__t-2in](https://msdn.microsoft.com/library/hh418084(VS.85).aspx).  
  
Per informazioni sulla configurazione e la personalizzazione di aggiornamento compatibile con cluster, vedere la pagina relativa alla modalità di [funzionamento del plug](assetId:///847b571b-12b3-473c-953f-75a5a1f51333)-in no__t-0ins per supportare diversi scenari di aggiornamento.  
  
## <a name="how-can-i-export-the-cau-preview-and-update-results"></a>Come è possibile esportare i risultati dell'aggiornamento e dell'anteprima di Aggiornamento compatibile con cluster?  
Aggiornamento compatibile con cluster offre opzioni di esportazione tramite l'interfaccia Command @ no__t-0line e tramite l'interfaccia utente.  
  
**Command @ no__t-opzioni interfaccia 1line:**  
  
-   Visualizzare in anteprima i risultati tramite il cmdlet di PowerShell **Invoke @ no__t-1CauScan | ConvertTo @ no__t-2Xml**. Output: XML  
  
-   Risultati del report tramite il cmdlet di PowerShell **Invoke @ no__t-1CauRun | ConvertTo @ no__t-2Xml**. Output: XML  
  
-   Risultati del report tramite il cmdlet di PowerShell **Get @ no__t-1CauReport | Export @ no__t-2CauReport**. Output: HTML, CSV  
  
**Opzioni interfaccia utente:**  
  
-   Copia dei risultati del report dalla schermata **Anteprima aggiornamenti**. Output: CSV  
  
-   Copia dei risultati del report dalla schermata **Genera rapporto** . Output: CSV  
  
-   Esportazione dei risultati del report dalla schermata **Genera rapporto** . Output: HTML  
  
## <a name="how-do-i-install-cau"></a>Come si installa Aggiornamento compatibile con cluster?  
Un'installazione di Aggiornamento compatibile con cluster è integrata nella funzionalità Clustering di failover. La funzionalità di Aggiornamento compatibile con cluster viene installata come segue:  
  
-   Quando il clustering di failover viene installato in un nodo del cluster, il provider di aggiornamento compatibile con cluster Strumentazione gestione Windows \(WMI @ no__t-1 viene installato automaticamente.  
  
-   Quando la funzionalità Strumenti Clustering di failover è installata in un server o in un computer client, vengono installati automaticamente i cmdlet di PowerShell e l'interfaccia utente di aggiornamento compatibile con cluster.
  
## <a name="does-cau-need-components-running-on-the-cluster-nodes-that-are-being-updated"></a>Aggiornamento compatibile con cluster necessita di componenti in esecuzione nei nodi del cluster che vengono aggiornati?  
Aggiornamento compatibile con cluster non richiede l'esecuzione di un servizio nei nodi del cluster. Tuttavia, aggiornamento compatibile con cluster necessita di un componente software @no__t 0The provider WMI @ no__t-1 installato nei nodi del cluster. Questo componente viene installato con la funzionalità Clustering di failover.  
  
Per abilitare la modalità self-no__t-0updating, è necessario aggiungere anche il ruolo del cluster di aggiornamento compatibile con cluster al cluster.  
  
## <a name="what-is-the-difference-between-using-cau-and-vmm"></a>Qual è la differenza tra l'utilizzo di aggiornamento compatibile con cluster e VMM?  
  
-   System Center Virtual Machine Manager \(VMM @ no__t-1 si concentra sull'aggiornamento solo dei cluster Hyper @ no__t-2V, mentre aggiornamento compatibile con cluster può aggiornare qualsiasi tipo di cluster di failover supportato, inclusi i cluster Hyper @ no__t-3V.  
  
-   VMM richiede licenze aggiuntive, mentre aggiornamento compatibile con cluster è concesso in licenza per tutti i server Windows. Le funzionalità, gli strumenti e l'interfaccia utente di Aggiornamento compatibile con cluster sono installati con i componenti di Clustering di failover.  
  
-   Se si dispone già di una licenza di System Center, è possibile continuare a usare VMM per aggiornare i cluster Hyper @ no__t-0V, perché offre un'esperienza di gestione integrata e di aggiornamento del software.  
  
-   Aggiornamento compatibile con cluster è supportato solo nei cluster che eseguono Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012. VMM supporta anche i cluster Hyper @ no__t-0V in computer che eseguono Windows Server 2008 R2 e Windows Server 2008.  
  
## <a name="can-i-use-remote-updating-on-a-cluster-that-is-configured-for-self-updating"></a>È possibile usare Remote @ no__t-0updating in un cluster configurato per self @ no__t-1updating?  
Sì. Un cluster di failover in una configurazione self-no__t-0updating può essere aggiornato tramite Remote @ no__t-1updating in @ no__t-2demand, proprio come è possibile forzare un'analisi di Windows Update in qualsiasi momento nel computer, anche se Windows Update è configurato per l'installazione degli aggiornamenti automaticamente. È tuttavia necessario assicurarsi che un'operazione di aggiornamento non sia già in corso.  
  
## <a name="can-i-reuse-my-cluster-update-settings-across-clusters"></a>È possibile riutilizzare le impostazioni di aggiornamento del cluster per altri cluster?  
Sì. Aggiornamento compatibile con cluster supporta numerose opzioni dell'operazione di aggiornamento che determinano il comportamento dell'operazione di aggiornamento del cluster. È possibile salvare queste opzioni come un profilo dell'operazione di aggiornamento e riutilizzarle per qualsiasi cluster. È consigliabile salvare e riutilizzare le impostazioni per i cluster di failover con esigenze di aggiornamento simili. Ad esempio, è possibile creare un "business @ no__t-0Critical SQL Server profilo di esecuzione aggiornamento cluster" per tutti i cluster Microsoft SQL Server che supportano i servizi business @ no__t-1critical.  
  
## <a name="where-is-the-cau-plug-in-specification"></a>Dove è la specifica plug-in di aggiornamento compatibile con cluster @ no__t-0cm?  
  
-   [Cluster @ no__t-1Aware aggiornamento plug @ no__t-2in riferimento](https://msdn.microsoft.com/library/hh418084(VS.85).aspx)  
  
-   [Esempio di plug-in di aggiornamento compatibile con cluster @ no__t-1in](https://code.msdn.microsoft.com/windowsdesktop/Cluster-Aware-Updating-6a8854c9)  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Panoramica sull'aggiornamento di cluster @ no__t-1Aware](cluster-aware-updating.md)  
  
