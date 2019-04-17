---
ms.assetid: 6416d125-bcaf-433d-971a-2f0283bca2c2
title: Aggiornamento compatibile con cluster - domande frequenti
ms.topic: article
ms.prod: storage-failover-clustering
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 4/28/2017
description: Risposte alle domande frequenti sull'aggiornamento compatibile con Cluster in Windows Server.
ms.openlocfilehash: 8417ea8b6b76e16c3f4db3bac5062d90a8da3ff2
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-aware-updating-frequently-asked-questions"></a>Aggiornamento compatibile con cluster: Domande frequenti

> Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

[Aggiornamento compatibile con cluster](cluster-aware-updating.md) \(CAU\) è una funzionalità che coordina gli aggiornamenti software in tutti i server in un cluster di failover in modo che l'operazione non influisce sulla disponibilità del servizio qualsiasi più di un failover pianificato di un nodo del cluster. Per alcune applicazioni con funzionalità di disponibilità continua \ ad esempio (Hyper\-V con migrazione in tempo reale) o un file server SMB 3. x con Failover\ trasparente SMB, aggiornamento compatibile con cluster consente di coordinare l'aggiornamento del cluster automatico senza impatto sulla disponibilità del servizio.

## <a name="does-cau-support-updating-storage-spaces-direct-clusters"></a>Aggiornamento compatibile con cluster supporta i cluster di spazi di archiviazione diretta aggiornamento?  
Sì. Aggiornamento compatibile con cluster supporta l'aggiornamento [spazi di archiviazione diretta](../storage/storage-spaces/storage-spaces-direct-overview.md) cluster indipendentemente dal tipo di distribuzione: iperconvergente o convergente. In particolare, dell'orchestrazione di aggiornamento compatibile con cluster garantisce che ogni nodo del cluster di sospensione attende per lo spazio di archiviazione in cluster sottostante integrità.

## <a name="does-cau-work-with-windows-server-2008-r2-or-windows-7"></a>Aggiornamento compatibile con cluster funziona con Windows Server 2008 R2 o Windows 7?  
No. Aggiornamento compatibile con cluster coordina l'operazione solo dai computer che eseguono Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10, Windows 8.1 o Windows 8 di aggiornamento. Il cluster di failover da aggiornare è necessario eseguire Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.
  
## <a name="is-cau-limited-to-specific-clustered-applications"></a>Aggiornamento compatibile con cluster è limitata alle applicazioni in cluster specifiche?  
No. Aggiornamento compatibile con cluster è indipendente dal tipo di applicazione in cluster. Aggiornamento compatibile con cluster è una soluzione di aggiornamento cluster\ esterna che si basa sulla funzionalità clustering di API e cmdlet di PowerShell. Di conseguenza, aggiornamento compatibile con cluster può coordinare l'aggiornamento per qualsiasi applicazione in cluster configurata in un cluster di failover Windows Server.  
  
> [!NOTE]  
> Attualmente, i carichi di lavoro in cluster seguenti sono testati e certificati per aggiornamento compatibile con cluster: SMB, Hyper\-V, replica DFS, spazi dei nomi DFS, iSCSI e NFS.  
  
## <a name="does-cau-support-updates-from-microsoft-update-and-windows-update"></a>Aggiornamento compatibile con cluster supporta gli aggiornamenti da Microsoft Update e Windows Update?  
Sì. Per impostazione predefinita, aggiornamento compatibile con cluster è configurata con un'installazione di plug-in che usa l'API di utilità \(WUA\) agente di Windows Update sui nodi del cluster. L'infrastruttura di agente può essere configurato per puntare a Microsoft Update e Windows Update o a Windows Server Update Services \(WSUS\) come origine degli aggiornamenti.  
  
## <a name="does-cau-support-wsus-updates"></a>Aggiornamento compatibile con cluster supporta gli aggiornamenti di WSUS?  
Sì. Per impostazione predefinita, aggiornamento compatibile con cluster è configurata con un'installazione di plug-in che usa l'API di utilità \(WUA\) agente di Windows Update sui nodi del cluster. L'infrastruttura di agente può essere configurato per puntare a Microsoft Update e Windows Update o a un server Windows Server Update Services \(WSUS\) locale come origine degli aggiornamenti.  
  
## <a name="can-cau-apply-limited-distribution-release-updates"></a>Aggiornamento compatibile con cluster può applicare aggiornamenti a distribuzione ridotta?  
Sì. \(LDR\) aggiornamenti a distribuzione ridotta, chiamati anche aggiornamenti rapidi, non sono pubblicati tramite Microsoft Update o Windows Update, in modo che non possono essere scaricati da \(WUA\) plug\ l'agente di Windows Update-in aggiornamento compatibile con cluster Usa per impostazione predefinita.  
  
Tuttavia, aggiornamento compatibile con cluster include un secondo plug-in che è possibile selezionare per applicare aggiornamenti rapidi. Questo aggiornamento rapido plug-in può essere personalizzato anche per applicare gli aggiornamenti del BIOS, firmware e driver fornitori non Microsoft.  
  
## <a name="can-i-use-cau-to-apply-cumulative-updates"></a>È possibile utilizzare aggiornamento compatibile con cluster per applicare aggiornamenti cumulativi?  
Sì. Se gli aggiornamenti cumulativi sono aggiornamenti Chiamati o rilascio generale di distribuzione, aggiornamento compatibile con cluster possibile applicarli.  
  
## <a name="can-i-schedule-updates"></a>È possibile pianificare gli aggiornamenti?  
Sì. Aggiornamento compatibile con cluster supporta la modalità di aggiornamento seguenti, che consentono entrambe pianificazione degli aggiornamenti:  
  
**L'aggiornamento e** consente al cluster di aggiornarsi in base a un profilo definito e una pianificazione regolare, ad esempio durante una finestra di manutenzione mensile. È inoltre possibile avviare un'operazione di aggiornamento e su richiesta in qualsiasi momento. Per abilitare la modalità di aggiornamento e, è necessario aggiungere il ruolo del cluster aggiornamento compatibile con cluster al cluster. La funzionalità di aggiornamento e di aggiornamento compatibile con cluster opera come qualsiasi altro carico di lavoro in cluster e funziona perfettamente con i failover pianificati e non pianificati di un computer coordinatore dell'aggiornamento.  
  
**L'aggiornamento Remote\** consente di avviare un'operazione di aggiornamento in qualsiasi momento da un computer che esegue Windows o Windows Server. È possibile avviare un'operazione di aggiornamento tramite la finestra di aggiornamento compatibile con Cluster o usando il **Invoke-CauRun** cmdlet di PowerShell. L'aggiornamento Remote\ è il valore predefinito di modalità di aggiornamento per aggiornamento compatibile con cluster. È possibile utilizzare l'utilità di pianificazione per eseguire il [Invoke-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun) cmdlet in una pianificazione desiderata da un computer remoto che non è uno dei nodi del cluster.  
  
## <a name="can-i-schedule-updates-to-apply-during-a-backup"></a>È possibile pianificare gli aggiornamenti da applicare durante un backup?  
Sì. Aggiornamento compatibile con cluster non impone alcun vincolo in questo senso. Tuttavia, l'esecuzione di aggiornamenti software in un server \ (con il restarts\ potenziali associati) mentre un backup del server è in corso non è una procedura IT consigliata. Tenere presente che aggiornamento compatibile con cluster si basa solo sulle API di clustering per determinare i failover delle risorse e i failback; di conseguenza, aggiornamento compatibile con cluster è a conoscenza dello stato di backup del server.  
  
## <a name="can-cau-work-with-system-center-configuration-manager"></a>Aggiornamento compatibile con cluster può lavorare con System Center Configuration Manager?  
Aggiornamento compatibile con cluster è uno strumento che coordina gli aggiornamenti software in un nodo del cluster e anche Configuration Manager esegue aggiornamenti del prodotto server. È importante configurare questi strumenti in modo che non hanno sovrapposizioni degli stessi server in qualsiasi distribuzione Data Center, incluso l'utilizzo di diversi server di Windows Server Update Services. Ciò garantisce che l'obiettivo alla base dell'utilizzo di aggiornamento compatibile con cluster non venga accidentalmente annullato, perché l'aggiornamento basato su Configuration Manager non incorpora compatibilità con cluster.  
  
## <a name="do-i-need-administrative-credentials-to-run-cau"></a>Sono necessarie credenziali amministrative per eseguire l'aggiornamento compatibile con cluster?  
Sì. Per l'esecuzione di strumenti di aggiornamento compatibile con cluster, aggiornamento compatibile con cluster sono necessarie credenziali amministrative nel server locale o, è necessario il **rappresenta un Client dopo l'autenticazione** utente direttamente nel server locale o nel computer client in cui è in esecuzione. Tuttavia, per coordinare gli aggiornamenti software nei nodi del cluster, aggiornamento compatibile con cluster richiede credenziali amministrative su ogni nodo. Sebbene sia possibile avviare l'UI CAU senza le credenziali, viene richiesto per le credenziali amministrative del cluster quando si connette a un'istanza del cluster per applicare aggiornamenti od ottenerne un'anteprima.  
  
## <a name="can-i-script-cau"></a>È possibile creare uno script aggiornamento compatibile con cluster?  
Sì. Aggiornamento compatibile con cluster include cmdlet di PowerShell che offrono un set avanzato di opzioni di scripting. Questi sono gli stessi cmdlet che l'UI CAU chiama per eseguire azioni di aggiornamento compatibile con cluster.  

## <a name="what-happens-to-active-clustered-roles"></a>Cosa accade ai ruoli del cluster attivi?

Ruoli del cluster \ (denominato in precedenza applicazioni e sistema \) che sono attivi in un nodo, il failover su altri nodi prima che inizi l'aggiornamento del software. Aggiornamento compatibile con cluster Orchestra i failover di tramite la modalità di manutenzione, che sospende e svuota il nodo da tutti i ruoli del cluster attivi. Una volta completati gli aggiornamenti software, aggiornamento compatibile con cluster riprende il nodo e i ruoli del cluster eseguano il failback nel nodo aggiornato. Ciò garantisce che la distribuzione dei ruoli del cluster relativi ai nodi rimanga invariata durante le operazioni di aggiornamento di un cluster.  
  
## <a name="how-does-cau-select-target-nodes-for-clustered-roles"></a>In che modo aggiornamento compatibile con cluster seleziona i nodi di destinazione per i ruoli del cluster?

Aggiornamento compatibile con cluster si basa su API di clustering per coordinare i failover. L'implementazione delle API di clustering seleziona i nodi di destinazione base a metriche interne ed euristica di posizionamento intelligente \ (ad esempio, il carico di lavoro levels\) tra i nodi di destinazione.  
  
## <a name="does-cau-load-balance-the-clustered-roles"></a>Aggiornamento compatibile con cluster Bilanciamento del carico i ruoli del cluster?

Aggiornamento compatibile con cluster non bilanciare i nodi del cluster, ma tenta di mantenerne la distribuzione di ruoli del cluster. Al termine dell'aggiornamento di un nodo del cluster, tenta di esito negativo back ospitavano in precedenza ruoli del cluster per tale nodo. Aggiornamento compatibile con cluster si basa su API di clustering per eseguire il failback delle risorse necessarie per l'inizio del processo di sospensione. Pertanto in assenza di failover non pianificato e le impostazioni proprietario preferito, la distribuzione dei ruoli del cluster deve rimanere invariata.  
  
## <a name="how-does-cau-select-the-order-of-nodes-to-update"></a>In che modo aggiornamento compatibile con cluster seleziona l'ordine dei nodi da aggiornare?  
Per impostazione predefinita, aggiornamento compatibile con cluster seleziona l'ordine dei nodi da aggiornare in base al livello di attività. I nodi che ospitano i minor numero ruoli del cluster vengono aggiornati per primi. Tuttavia, un amministratore può specificare un ordine specifico per l'aggiornamento dei nodi, specificando un parametro per l'operazione di aggiornamento nell'UI CAU o utilizzando i cmdlet di PowerShell.  
  
## <a name="what-happens-if-a-cluster-node-is-offline"></a>Cosa accade se un nodo del cluster non è in linea.

L'amministratore che avvia un'operazione di aggiornamento può specificare la soglia accettabile per il numero di nodi che possono essere offline. Di conseguenza, un'operazione di aggiornamento possibile procedere in un cluster anche se tutti i nodi del cluster non sono in linea.  
  
## <a name="can-i-use-cau-to-update-only-a-single-node"></a>È possibile utilizzare aggiornamento compatibile con cluster per aggiornare solo un singolo nodo?  
No. Aggiornamento compatibile con cluster è uno strumento di aggiornamento con ambito cluster\, quindi consente di selezionare i cluster per aggiornare solo. Se si desidera aggiornare un singolo nodo, è possibile utilizzare gli strumenti indipendentemente da aggiornamento compatibile con cluster di aggiornamento del server esistente.  
  
## <a name="can-cau-report-updates-that-are-initiated-from-outside-cau"></a>Aggiornamento compatibile con cluster può segnalare gli aggiornamenti che vengono avviati da aggiornamento compatibile con cluster esterno?  
No. Aggiornamento compatibile con cluster può solo report operazioni di aggiornamento avviate dall'interno di aggiornamento compatibile con cluster. Tuttavia, quando viene avviata una successiva operazione di aggiornamento, gli aggiornamenti installati tramite metodi-aggiornamento compatibile con cluster sono considerati in modo appropriato per determinare gli aggiornamenti aggiuntivi che possono essere applicati a ogni nodo del cluster.  
  
## <a name="can-cau-support-my-unique-it-process-needs"></a>Supporto di aggiornamento compatibile con cluster deve miei processi IT univoco può?  
Sì. Aggiornamento compatibile con cluster offre le dimensioni di flessibilità per soddisfare aziendale necessita di processi IT univoco dei clienti seguenti:  
  
**Script** un'operazione di aggiornamento può specificare uno script di PowerShell pre-aggiornamento e uno script di PowerShell post\-aggiornamento. Lo script di pre-aggiornamento viene eseguito in ogni nodo del cluster prima che il nodo venga sospeso. Lo script post\-aggiornamento viene eseguito in ogni nodo del cluster dopo aver installati gli aggiornamenti del nodo.  
  
> [!NOTE]  
> .NET framework 4.6 o 4.5 e PowerShell devono essere installati in ogni nodo del cluster in cui si desidera eseguire gli script di pre-aggiornamento e post\-aggiornamento. È anche necessario abilitare comunicazione remota di PowerShell nei nodi del cluster. Per i requisiti di sistema dettagliate, vedere [requisiti e procedure consigliate per aggiornamento compatibile con Cluster](cluster-aware-updating-requirements.md).  
  
**Opzioni dell'operazione di aggiornamento avanzate** l'amministratore può inoltre specificare da un ampio set di opzioni di operazione di aggiornamento avanzate, ad esempio il numero massimo di tentativi che il processo di aggiornamento è eseguito in ogni nodo. Possono specificare queste opzioni utilizzando la UI CAU o i cmdlet PowerShell di aggiornamento compatibile con cluster. Queste impostazioni personalizzate possono essere salvate in un profilo operazione di aggiornamento e riutilizzate per operazioni di aggiornamento successive.  
  
**Architettura plug-in pubblica** aggiornamento compatibile con cluster include le funzionalità per registrare, Unregister, e l'installazione di plug-aggiuntivi selezionare aggiornamento compatibile con cluster viene fornito con due predefinito di installazione di plug-in: uno coordina l'agente di Windows Update \(WUA\) API in ogni nodo del cluster il secondo applica gli hotfix che vengono manualmente copiati in una condivisione file accessibile ai nodi del cluster. Se un'azienda ha esigenze specifiche che non possono essere soddisfatte con questi due installazione di plug-in, può creare un nuovo aggiornamento compatibile con cluster plug-in base alla specifica API pubblica. Per ulteriori informazioni, vedere [riferimento plug-in aggiornamento compatibile con Cluster\](http://msdn.microsoft.com/library/hh418084(VS.85).aspx).  
  
Per informazioni sulla configurazione e personalizzazione di aggiornamento compatibile con installazione di plug-in per supportare diversi scenari di aggiornamento vedere [funzionamento installazione di plug-in](assetId:///847b571b-12b3-473c-953f-75a5a1f51333).  
  
## <a name="how-can-i-export-the-cau-preview-and-update-results"></a>Come è possibile esportare l'anteprima di aggiornamento compatibile con cluster e risultati dell'aggiornamento?  
Aggiornamento compatibile con cluster offre opzioni di esportazione e l'interfaccia utente tramite l'interfaccia della riga di comando.  
  
**Opzioni dell'interfaccia della riga di comando:**  
  
-   Anteprima dei risultati con il cmdlet PowerShell **Invoke-CauScan | ConvertTo-Xml**. Output: XML  
  
-   Risultati del report tramite il cmdlet PowerShell **Invoke-CauRun | ConvertTo-Xml**. Output: XML  
  
-   Risultati del report tramite il cmdlet PowerShell **Get-CauReport | Export-CauReport**. Output: HTML, CSV  
  
**Opzioni dell'interfaccia utente:**  
  
-   Copiare i risultati del report dal **anteprima degli aggiornamenti** dello schermo. Output: CSV  
  
-   Copiare i risultati del report dal **genera report** dello schermo. Output: CSV  
  
-   Esportare i risultati del report dal **genera report** dello schermo. Output: HTML  
  
## <a name="how-do-i-install-cau"></a>Come si installa aggiornamento compatibile con cluster?  
Un'installazione di aggiornamento compatibile con cluster è integrata nella funzionalità Clustering di Failover. Aggiornamento compatibile con cluster viene installata come segue:  
  
-   Quando il Clustering di Failover viene installato in un nodo del cluster, il provider di Strumentazione gestione Windows di aggiornamento compatibile con cluster \(WMI\) viene installato automaticamente.  
  
-   Quando la funzionalità strumenti per Clustering di Failover viene installata in un server o computer client, vengono installati automaticamente i cmdlet di PowerShell e l'interfaccia utente di aggiornamento compatibile con Cluster.
  
## <a name="does-cau-need-components-running-on-the-cluster-nodes-that-are-being-updated"></a>Aggiornamento compatibile con cluster necessita di componenti eseguiti sui nodi del cluster che vengono aggiornati?  
Aggiornamento compatibile con cluster non è necessario un servizio in esecuzione nei nodi del cluster. Tuttavia, aggiornamento compatibile con cluster necessita di un componente software \ (il provider WMI) installato sui nodi del cluster. Questo componente viene installato con la funzionalità Clustering di Failover.  
  
Per abilitare la modalità di aggiornamento e, il ruolo del cluster aggiornamento compatibile con cluster deve inoltre essere aggiunti al cluster.  
  
## <a name="what-is-the-difference-between-using-cau-and-vmm"></a>Che cos'è la differenza tra l'utilizzo di aggiornamento compatibile con cluster e VMM?  
  
-   System Center Virtual Machine Manager \(VMM\) è incentrata sull'aggiornamento solo i cluster Hyper\-V, mentre aggiornamento compatibile con cluster è possibile aggiornare qualsiasi tipo di cluster di failover supportato, inclusi i cluster Hyper\-V.  
  
-   VMM richiede licenze aggiuntive, mentre aggiornamento compatibile con cluster è concesso in licenza per tutti i Server di Windows. La funzionalità aggiornamento compatibile con cluster, strumenti e dell'interfaccia utente vengono installati con i componenti di Clustering di Failover.  
  
-   Se si possiede già una licenza di System Center, è possibile continuare a utilizzare VMM per aggiornare i cluster Hyper\-V, in quanto offre una gestione integrata e l'esperienza di aggiornamento software.  
  
-   Aggiornamento compatibile con cluster è supportata solo nei cluster che eseguono Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012. VMM supporta inoltre i cluster Hyper\-V nei computer che eseguono Windows Server 2008 R2 e Windows Server 2008.  
  
## <a name="can-i-use-remote-updating-on-a-cluster-that-is-configured-for-self-updating"></a>È possibile utilizzare l'aggiornamento remote\ in un cluster che è configurato per l'aggiornamento e?  
Sì. Un cluster di failover in una configurazione di aggiornamento e può essere aggiornato tramite l'aggiornamento remote\ on\ richiesta, esattamente come è possibile forzare un'analisi di Windows Update in qualsiasi momento nel computer, anche se Windows Update è configurato per installare automaticamente gli aggiornamenti. Tuttavia, è necessario assicurarsi che un'operazione di aggiornamento non è già in corso.  
  
## <a name="can-i-reuse-my-cluster-update-settings-across-clusters"></a>È possibile riutilizzare le impostazioni di aggiornamento del cluster tra cluster?  
Sì. Aggiornamento compatibile con cluster supporta una serie di opzioni dell'operazione di aggiornamento che determinano l'operazione di aggiornamento comportamento quando l'aggiornamento del cluster. Queste opzioni possono essere salvate come un profilo operazione di aggiornamento e riutilizzarle per qualsiasi cluster. È consigliabile salvare e riutilizzare le impostazioni per i cluster di failover con esigenze di aggiornamento simili. Ad esempio, è possibile creare un "Business\ critici SQL Server Cluster profilo operazione di aggiornamento" per tutti i cluster di Microsoft SQL Server che supportano servizi critici business\.  
  
## <a name="where-is-the-cau-plug-in-specification"></a>Dov'è la specifica installazione di plug-in aggiornamento compatibile con cluster?  
  
-   [Cluster\ aggiornamento compatibile con installazione di plug-in di riferimento](http://msdn.microsoft.com/library/hh418084(VS.85).aspx)  
  
-   [Esempio di aggiornamento compatibile con installazione di plug-in del cluster](http://code.msdn.microsoft.com/windowsdesktop/Cluster-Aware-Updating-6a8854c9)  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Panoramica di aggiornamento compatibile con Cluster\](cluster-aware-updating.md)  
  
