---
ms.assetid: 6416d125-bcaf-433d-971a-2f0283bca2c2
title: 'Aggiornamento compatibile con cluster: domande frequenti'
ms.topic: article
ms.prod: windows-server-threshold
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 04/28/2017
description: Risposte alle domande frequenti sull'aggiornamento compatibile con Cluster in Windows Server.
ms.openlocfilehash: f9009811093823554f16295cc1205f1b99ead93f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882522"
---
# <a name="cluster-aware-updating-frequently-asked-questions"></a>Aggiornamento compatibile con cluster: Domande frequenti

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

[Aggiornamento compatibile con cluster](cluster-aware-updating.md) \(aggiornamento compatibile con cluster\) è una funzionalità che coordina software aggiornato in tutti i server in un cluster di failover in modo da non influire sulla disponibilità del servizio qualsiasi più di un failover pianificato di un nodo del cluster. Per alcune applicazioni con funzionalità di disponibilità continui \(, ad esempio Hyper\-V con migrazione in tempo reale o un file server SMB 3.x con Failover trasparente SMB\), aggiornamento compatibile con cluster può coordinare l'aggiornamento del cluster automatico senza impatto sulla disponibilità del servizio.

## <a name="does-cau-support-updating-storage-spaces-direct-clusters"></a>Aggiornamento compatibile con cluster supporta l'aggiornamento dei cluster di spazi di archiviazione diretta?  
Sì. Aggiornamento compatibile con cluster supporta l'aggiornamento [spazi di archiviazione diretta](../storage/storage-spaces/storage-spaces-direct-overview.md) cluster indipendentemente dal tipo di distribuzione: hyper convergente o iperconvergente. In particolare, orchestrazione di aggiornamento compatibile con cluster garantisce che la sospensione di ogni nodo del cluster è in attesa per lo spazio di archiviazione in cluster sottostante sia integro.

## <a name="does-cau-work-with-windowsserver-2008r2-or-windows7"></a>Aggiornamento compatibile con cluster funziona con Windows Server 2008 R2 o con Windows 7?  
No. Aggiornamento compatibile con cluster coordina l'operazione solo da computer che eseguono Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10, Windows 8.1 o Windows 8 di aggiornamento. Il cluster di failover in corso l'aggiornamento deve eseguire Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.
  
## <a name="is-cau-limited-to-specific-clustered-applications"></a>Aggiornamento compatibile con cluster è limitato ad applicazioni in cluster specifiche?  
No. La funzionalità Aggiornamento compatibile con cluster è indipendente dal tipo di applicazione in cluster. Aggiornamento compatibile con cluster è un cluster esterno\-aggiornamento della soluzione che si basa sulla funzionalità clustering di API e cmdlet di PowerShell. Di conseguenza, aggiornamento compatibile con cluster può coordinare l'aggiornamento per qualsiasi applicazione in cluster configurata in un cluster di failover di Windows Server.  
  
> [!NOTE]  
> Attualmente, i carichi di lavoro in cluster seguenti sono testati e certificati per aggiornamento compatibile con cluster: SMB, Hyper\-V, replica DFS, spazi dei nomi DFS, iSCSI e NFS.  
  
## <a name="does-cau-support-updates-from-microsoft-update-and-windows-update"></a>Aggiornamento compatibile con cluster supporta gli aggiornamenti da Microsoft Update e Windows Update?  
Sì. Per impostazione predefinita, aggiornamento compatibile con cluster è configurato con un plug\-che usa l'agente di Windows Update \(WUA\) API dell'utilità nei nodi del cluster. L'infrastruttura di agente può essere configurato per puntare a Microsoft Update e Windows Update o a Windows Server Update Services \(WSUS\) come origine degli aggiornamenti.  
  
## <a name="does-cau-support-wsus-updates"></a>Aggiornamento compatibile con cluster supporta gli aggiornamenti da Windows Server Update Services (WSUS)?  
Sì. Per impostazione predefinita, aggiornamento compatibile con cluster è configurato con un plug\-che usa l'agente di Windows Update \(WUA\) API dell'utilità nei nodi del cluster. L'infrastruttura di agente può essere configurato per puntare a Microsoft Update e Windows Update o a una locale di Windows Server Update Services \(WSUS\) server come origine degli aggiornamenti.  
  
## <a name="can-cau-apply-limited-distribution-release-updates"></a>La funzionalità Aggiornamento compatibile con cluster può applicare aggiornamenti a distribuzione ridotta?  
Sì. Limitato del GDR \(LDR\) aggiornamenti, chiamati anche aggiornamenti rapidi, non sono pubblicati tramite Microsoft Update o Windows Update, in modo che non possono essere scaricati dall'agente Windows Update \(WUA\) collegare\-aggiornamento compatibile con cluster Usa per impostazione predefinita.  
  
Tuttavia, aggiornamento compatibile con cluster include un secondo plug\-in quanto è possibile selezionare per applicare gli aggiornamenti rapidi. Questo plug-hotfix\-in può essere personalizzato anche per applicare non\-Microsoft driver, firmware e aggiornamenti del BIOS.  
  
## <a name="can-i-use-cau-to-apply-cumulative-updates"></a>È possibile usare Aggiornamento compatibile con cluster per applicare aggiornamenti cumulativi?  
Sì. Se gli aggiornamenti cumulativi sono aggiornamenti pubblici o aggiornamenti a distribuzione ridotta, possono essere applicati tramite Aggiornamento compatibile con cluster.  
  
## <a name="can-i-schedule-updates"></a>È possibile pianificare gli aggiornamenti?  
Sì. Aggiornamento compatibile con cluster supporta le modalità di aggiornamento seguenti, entrambe le quali consentono la pianificazione degli aggiornamenti:  
  
**Self\-l'aggiornamento** consente al cluster di aggiornarsi in base a un profilo definito e una pianificazione regolare, ad esempio durante una finestra di manutenzione mensile. È anche possibile avviare un Self\-operazione di aggiornamento su richiesta in qualsiasi momento. Per abilitare self\-modalità di aggiornamento, è necessario aggiungere il ruolo del cluster aggiornamento compatibile con cluster al cluster. L'aggiornamento compatibile con cluster self\-aggiornamento funzionalità opera come qualsiasi altro carico di lavoro del cluster e funziona perfettamente con i failover pianificati e di un computer coordinatore dell'aggiornamento.  
  
**Remote\-l'aggiornamento** consente di avviare un'operazione di aggiornamento in qualsiasi momento da un computer che eseguono Windows o Windows Server. È possibile avviare un'operazione di aggiornamento tramite la finestra di aggiornamento compatibile con Cluster o usando il **Invoke\-CauRun** cmdlet di PowerShell. Remote\-l'aggiornamento è la modalità di aggiornamento per aggiornamento compatibile con cluster predefinita. È possibile usare l'Utilità di pianificazione per eseguire il cmdlet [Invoke-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun) in base alla pianificazione desiderata da un computer remoto diverso dai nodi del cluster.  
  
## <a name="can-i-schedule-updates-to-apply-during-a-backup"></a>È possibile pianificare gli aggiornamenti da applicare durante un backup?  
Sì. Aggiornamento compatibile con cluster non impone alcun vincolo in questo senso. Tuttavia, eseguire gli aggiornamenti software in un server \(con il rischio associato viene riavviato\) mentre un backup del server è in corso non è una procedura IT consigliata. Tenere presente che Aggiornamento compatibile con cluster si basa solo sulle API di clustering per determinare i failover e i failback delle risorse, di conseguenza Aggiornamento compatibile con cluster non è a conoscenza dello stato di backup del server.  
  
## <a name="can-cau-work-with-system-center-configuration-manager"></a>Aggiornamento compatibile con cluster è possibile usare con System Center Configuration Manager?  
Aggiornamento compatibile con cluster è uno strumento che coordina gli aggiornamenti software in un nodo del cluster e Configuration Manager esegue anche gli aggiornamenti software di server. È importante configurare questi strumenti in modo che non hanno sovrapposizione copertura degli stessi server in qualsiasi distribuzione Data Center, incluso l'uso di diversi server di Windows Server Update Services. Ciò garantisce che l'obiettivo alla base dell'utilizzo di aggiornamento compatibile con cluster non venga accidentalmente annullato, poiché Configuration Manager\-basato su aggiornamento non incorporare compatibilità con cluster.  
  
## <a name="do-i-need-administrative-credentials-to-run-cau"></a>Sono necessarie credenziali amministrative per eseguire Aggiornamento compatibile con cluster?  
Sì. Per l'esecuzione degli strumenti di Aggiornamento compatibile con cluster sono necessarie credenziali amministrative nel server locale oppure il diritto utente **Rappresenta un client dopo l'autenticazione** nel server locale o nel computer client in cui è in esecuzione. Tuttavia, per coordinare gli aggiornamenti software nei nodi del cluster, Aggiornamento compatibile con cluster richiede credenziali amministrative per il cluster in ogni nodo. Sebbene la UI CAU può avviata senza credenziali, viene richiesto per le credenziali amministrative del cluster quando si connette a un'istanza del cluster per visualizzare in anteprima o applicare gli aggiornamenti.  
  
## <a name="can-i-script-cau"></a>È possibile creare uno script per aggiornamento compatibile con cluster?  
Sì. Aggiornamento compatibile con cluster include cmdlet di PowerShell che offrono un'ampia gamma di opzioni di scripting. Sono gli stessi cmdlet che l'interfaccia utente di Aggiornamento compatibile con cluster chiama per eseguire le azioni di aggiornamento.  

## <a name="what-happens-to-active-clustered-roles"></a>Cosa accade ai ruoli del cluster attivi?

Ruoli del cluster \(in precedenza denominati servizi e applicazioni\) che sono attive in un nodo, eseguire il failover in altri nodi prima che inizi l'aggiornamento del software. Aggiornamento compatibile con cluster orchestra i failover tramite la modalità manutenzione, che sospende e svuota il nodo da tutti i ruoli del cluster attivi. Una volta completati gli aggiornamenti software, Aggiornamento compatibile con cluster riprende il nodo ed esegue il failback dei ruoli del cluster nel nodo aggiornato. Questo garantisce che la distribuzione dei nodi del cluster relativi ai nodi rimanga invariata durante le operazioni di aggiornamento di un cluster.  
  
## <a name="how-does-cau-select-target-nodes-for-clustered-roles"></a>In che modo aggiornamento compatibile con cluster seleziona i nodi di destinazione per i ruoli del cluster?

Aggiornamento compatibile con cluster usa le API di clustering per coordinare i failover. L'implementazione di API di clustering seleziona i nodi di destinazione base a metriche interne ed euristica di posizionamento intelligente \(ad esempio i livelli di carico di lavoro\) tra i nodi di destinazione.  
  
## <a name="does-cau-load-balance-the-clustered-roles"></a>Aggiornamento compatibile con il bilanciamento del carico i ruoli del cluster?

Aggiornamento compatibile con cluster non bilanciare i nodi del cluster, ma si tenta di mantenere la distribuzione dei ruoli del cluster. Al termine dell'aggiornamento di un nodo del cluster, tenta di eseguire il failback dei ruoli del cluster nei nodi che li ospitavano in precedenza. Aggiornamento compatibile con cluster usa le API di clustering per eseguire il failback delle risorse all'inizio del processo di sospensione. Di conseguenza, in assenza di failover pianificati e di impostazioni del proprietario preferito, la distribuzione dei nodi del cluster deve rimanere invariata.  
  
## <a name="how-does-cau-select-the-order-of-nodes-to-update"></a>In che modo Aggiornamento compatibile con cluster seleziona l'ordine dei nodi da aggiornare?  
Per impostazione predefinita, Aggiornamento compatibile con cluster seleziona l'ordine dei nodi da aggiornare in base al livello di attività. I nodi che ospitano il minor numero di ruoli del cluster vengono aggiornati per primi. Tuttavia, un amministratore può specificare un ordine particolare per l'aggiornamento dei nodi, specificando un parametro per l'operazione di aggiornamento nell'UI CAU o usando i cmdlet di PowerShell.  
  
## <a name="what-happens-if-a-cluster-node-is-offline"></a>Cosa accade se un nodo del cluster è offline?

L'amministratore che avvia un'operazione di aggiornamento può specificare la soglia accettabile per il numero di nodi che possono essere offline. Di conseguenza, è possibile eseguire un'operazione di aggiornamento in un cluster anche se non tutti i nodi del cluster sono online.  
  
## <a name="can-i-use-cau-to-update-only-a-single-node"></a>È possibile usare aggiornamento compatibile con cluster per aggiornare solo un singolo nodo?  
No. Aggiornamento compatibile con cluster è un cluster\-con ambito, strumento di aggiornamento in modo che solo consente di selezionare cluster da aggiornare. Per aggiornare un singolo nodo, è possibile usare gli strumenti di aggiornamento del server esistenti, indipendentemente da Aggiornamento compatibile con cluster.  
  
## <a name="can-cau-report-updates-that-are-initiated-from-outside-cau"></a>Aggiornamento compatibile con cluster può segnalare gli aggiornamenti che vengono avviati da aggiornamento compatibile con cluster esterno?  
No. Aggiornamento compatibile con cluster può segnalare solo operazioni di aggiornamento avviate dall'interno di Aggiornamento compatibile con cluster. Tuttavia, quando una successiva operazione di aggiornamento viene avviata, gli aggiornamenti installati tramite non\-metodi di aggiornamento compatibile con cluster sono considerati in modo appropriato per determinare quali aggiornamenti aggiuntivi che possono essere applicati a ogni nodo del cluster.  
  
## <a name="can-cau-support-my-unique-it-process-needs"></a>Possibile il supporto di aggiornamento compatibile con cluster deve mio processo IT univoco?  
Sì. Aggiornamento compatibile con cluster offre le dimensioni di flessibilità seguenti per soddisfare le esigenze specifiche dei clienti aziendali in termini di processi IT:  
  
**Gli script** un'operazione di aggiornamento può specificare una versione preliminare\-aggiornare script di PowerShell e un post\-aggiornare script di PowerShell. Pre\-script di aggiornamento viene eseguito in ogni nodo del cluster prima che il nodo venga sospeso. Il post\-script di aggiornamento viene eseguito in ogni nodo del cluster dopo aver installati gli aggiornamenti di nodo.  
  
> [!NOTE]  
> .NET framework 4.6 o 4.5 e PowerShell devono essere installati in ogni nodo del cluster in cui si desidera eseguire il pre\-aggiornamento e post\-aggiornare gli script. È anche necessario abilitare la comunicazione remota di PowerShell nei nodi del cluster. Per informazioni sui requisiti di sistema dettagliati, vedere [i requisiti e procedure consigliate per aggiornamento compatibile con Cluster](cluster-aware-updating-requirements.md).  
  
**Aggiornamento delle opzioni avanzate** l'amministratore può inoltre specificare da un ampio set di opzioni di operazione di aggiornamento avanzate, ad esempio il numero massimo di volte in cui il processo di aggiornamento viene ripetuto in ogni nodo. Queste opzioni possono essere specificate utilizzando la UI CAU o i cmdlet di PowerShell di aggiornamento compatibile con cluster. Queste impostazioni personalizzate possono essere salvate in un profilo dell'operazione di aggiornamento e riutilizzate per operazioni di aggiornamento successive.  
  
**Pubblica plug\-nell'architettura** aggiornamento compatibile con cluster include le funzionalità per Register, Unregister, e selezionare collegare\-aggiuntivi. Aggiornamento compatibile con cluster viene fornito con due predefiniti plug\-aggiuntivi: uno coordina l'agente di Windows Update \(WUA\) API in ogni nodo del cluster; il secondo applica gli hotfix che vengono manualmente copiati in una condivisione file accessibile ai nodi del cluster. Se un'azienda ha esigenze specifiche che non possono essere soddisfatte con questi due plug\-i componenti aggiuntivi, può creare un nuovo plug di aggiornamento compatibile con cluster\-in base alla specifica API pubblica. Per altre informazioni, vedere [Cluster\-Plug di aggiornamento compatibile con\-nel riferimento](https://msdn.microsoft.com/library/hh418084(VS.85).aspx).  
  
Per informazioni sulla configurazione e personalizzazione di aggiornamento compatibile con cluster collegare\-l'aggiornamento di scenari, vedere i componenti aggiuntivi per supportare diversi [come collegare\-aggiuntivi funzionano](assetId:///847b571b-12b3-473c-953f-75a5a1f51333).  
  
## <a name="how-can-i-export-the-cau-preview-and-update-results"></a>Come è possibile esportare i risultati dell'aggiornamento e dell'anteprima di Aggiornamento compatibile con cluster?  
Aggiornamento compatibile con cluster offre opzioni tramite il comando di esportazione\-interfaccia della riga e l'interfaccia utente.  
  
**Comando\-opzioni dell'interfaccia della riga:**  
  
-   Visualizzare in anteprima i risultati usando il cmdlet di PowerShell **Invoke\-CauScan | ConvertTo\-Xml**. Output: XML  
  
-   Risultati del report usando il cmdlet di PowerShell **Invoke\-CauRun | ConvertTo\-Xml**. Output: XML  
  
-   Risultati del report usando il cmdlet di PowerShell **ottenere\-CauReport | Esportare\-CauReport**. Output: HTML, CSV  
  
**Opzioni dell'interfaccia utente:**  
  
-   Copia dei risultati del report dalla schermata **Anteprima aggiornamenti**. Output: CSV  
  
-   Copia dei risultati del report dalla schermata **Genera rapporto** . Output: CSV  
  
-   Esportazione dei risultati del report dalla schermata **Genera rapporto** . Output: HTML  
  
## <a name="how-do-i-install-cau"></a>Come si installa Aggiornamento compatibile con cluster?  
Un'installazione di Aggiornamento compatibile con cluster è integrata nella funzionalità Clustering di failover. La funzionalità di Aggiornamento compatibile con cluster viene installata come segue:  
  
-   Quando il Clustering di Failover è installato in un nodo del cluster di aggiornamento compatibile con Windows Management Instrumentation \(WMI\) provider viene installato automaticamente.  
  
-   Quando la funzionalità strumenti per Clustering di Failover viene installata in un server o computer client, l'interfaccia utente di aggiornamento compatibile con Cluster e cmdlet di PowerShell vengono installati automaticamente.
  
## <a name="does-cau-need-components-running-on-the-cluster-nodes-that-are-being-updated"></a>Aggiornamento compatibile con cluster è necessario che i componenti in esecuzione nei nodi del cluster che vengono aggiornati?  
Aggiornamento compatibile con cluster non è necessario un servizio in esecuzione nei nodi del cluster. Tuttavia, aggiornamento compatibile con cluster deve essere un componente software \(il provider WMI\) installata nei nodi del cluster. Questo componente viene installato con la funzionalità Clustering di failover.  
  
Per abilitare self\-modalità di aggiornamento, il ruolo del cluster aggiornamento compatibile con cluster deve anche essere aggiunto al cluster.  
  
## <a name="what-is-the-difference-between-using-cau-and-vmm"></a>Che cos'è la differenza tra l'uso di aggiornamento compatibile con cluster e VMM?  
  
-   System Center Virtual Machine Manager \(VMM\) riguarda l'aggiornamento sola Hyper\-V cluster, mentre aggiornamento compatibile con cluster è possibile aggiornare qualsiasi tipo di cluster di failover supportate, incluso Hyper\-cluster V.  
  
-   VMM richiede licenze aggiuntive, mentre aggiornamento compatibile con cluster è concesso in licenza per tutte le piattaforme Windows Server. Le funzionalità, gli strumenti e l'interfaccia utente di Aggiornamento compatibile con cluster sono installati con i componenti di Clustering di failover.  
  
-   Se si dispone già di una licenza di System Center, è possibile continuare a usare VMM per aggiornare Hyper\-V cluster in quanto offre una gestione integrata e l'esperienza di aggiornamento software.  
  
-   Aggiornamento compatibile con cluster è supportata solo nei cluster che eseguono Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012. VMM supporta anche Hyper\-V cluster nei computer che eseguono Windows Server 2008 R2 e Windows Server 2008.  
  
## <a name="can-i-use-remote-updating-on-a-cluster-that-is-configured-for-self-updating"></a>È possibile usare remote\-ad aggiornamento in un cluster configurato per l'oggetto corrente\-l'aggiornamento?  
Sì. Cluster di failover in un self\-l'aggiornamento della configurazione può essere aggiornato tramite remote\-ad aggiornamento in\-facoltative, come è possibile forzare una scansione di Windows Update in qualsiasi momento nel computer, anche se Windows Update è configurato per installare gli aggiornamenti automaticamente. È tuttavia necessario assicurarsi che un'operazione di aggiornamento non sia già in corso.  
  
## <a name="can-i-reuse-my-cluster-update-settings-across-clusters"></a>È possibile riutilizzare le impostazioni di aggiornamento del cluster per altri cluster?  
Sì. Aggiornamento compatibile con cluster supporta numerose opzioni dell'operazione di aggiornamento che determinano il comportamento dell'operazione di aggiornamento del cluster. È possibile salvare queste opzioni come un profilo dell'operazione di aggiornamento e riutilizzarle per qualsiasi cluster. È consigliabile salvare e riutilizzare le impostazioni per i cluster di failover con esigenze di aggiornamento simili. Ad esempio, è possibile creare un "Business\-critici SQL Server Cluster Updating Run Profile" per tutti i cluster di Microsoft SQL Server che supportano business\-servizi critici.  
  
## <a name="where-is-the-cau-plug-in-specification"></a>Dove è la spina di aggiornamento compatibile con cluster\-nella specifica?  
  
-   [Cluster\-Plug per aggiornamento compatibile con\-nel riferimento](https://msdn.microsoft.com/library/hh418084(VS.85).aspx)  
  
-   [Plug di aggiornamento compatibile con cluster\-nell'esempio](https://code.msdn.microsoft.com/windowsdesktop/Cluster-Aware-Updating-6a8854c9)  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Cluster\-Panoramica di aggiornamento compatibile con](cluster-aware-updating.md)  
  
