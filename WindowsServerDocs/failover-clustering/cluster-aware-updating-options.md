---
ms.assetid: 2f4b6641-0ec2-4b1c-85fb-a1f1d16685c8
title: Opzioni avanzate di aggiornamento compatibile con specifico e aggiornamento profili di esecuzione
ms.topic: article
ms.prod: storage-failover-clustering
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 6/7/2017
description: Come configurare le opzioni avanzate e profili di esecuzione di aggiornamento per l'aggiornamento (compatibile con Cluster)
ms.openlocfilehash: 5b6f035791a946ff96ff6a95a1f753ef505d54b4
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-aware-updating-advanced-options-and-updating-run-profiles"></a>Aggiornamento compatibile con cluster opzioni avanzate e profili di esecuzione di aggiornamento

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento descrive opzioni dell'operazione di aggiornamento che possono essere configurate per una [aggiornamento compatibile con Cluster](cluster-aware-updating.md) operazione di aggiornamento (aggiornamento compatibile con cluster). Queste opzioni avanzate possono essere configurate quando si utilizza l'UI CAU o i cmdlet di aggiornamento compatibile con Windows PowerShell per applicare gli aggiornamenti o per configurare le opzioni di aggiornamento automatico.

La maggior parte delle impostazioni di configurazione possono essere salvate come file XML denominato un profilo operazione di aggiornamento e riutilizzate per operazioni di aggiornamento successive. I valori predefiniti per le opzioni dell'operazione di aggiornamento forniti da aggiornamento compatibile con cluster possono essere utilizzati anche in numerosi ambienti cluster.

Per informazioni sulle opzioni aggiuntive che è possibile specificare per ogni operazione di aggiornamento e sui profili di eseguire l'aggiornamento, vedere le sezioni seguenti più avanti in questo argomento:

Opzioni specificate quando si richiede un aggiornamento eseguire utilizza l'aggiornamento eseguire profili le opzioni che possono essere impostate in un profilo operazione di aggiornamento

La tabella seguente elenca le opzioni che è possibile impostare in un aggiornamento compatibile con cluster profilo operazione di aggiornamento. 

> [!NOTE] 
> Per impostare l'opzione PreUpdateScript o PostUpdateScript, assicurarsi che Windows PowerShell e .NET Framework 4.6 o 4.5 siano installati e che comunicazione remota di PowerShell è abilitata in ogni nodo del cluster. Per ulteriori informazioni, vedere Configurare i nodi per la gestione remota in [requisiti e procedure consigliate per aggiornamento compatibile con Cluster](cluster-aware-updating-requirements.md).


|Opzione|Valore predefinito|Dettagli|  
|------------|-------------------|-------------|  
|**StopAfter**|Periodo di tempo illimitato|Tempo in minuti dopo il quale l'operazione di aggiornamento venga interrotta, se non è stata completata. **Nota:** se si specifica un pre-aggiornamento o uno script di PowerShell di post-aggiornamento, l'intero processo di esecuzione degli script e degli aggiornamenti dovrà essere completato entro il **StopAfter** limite di tempo.|  
|**WarnAfter**|Per impostazione predefinita, viene visualizzato alcun avviso|Tempo in minuti dopo il quale verrà visualizzato un avviso se l'operazione di aggiornamento (inclusi uno script di pre-aggiornamento e uno script di post-aggiornamento, se configurate) non è stata completata.|  
|**MaxRetriesPerNode**|3|Numero massimo di volte in cui verrà ritentato il processo di aggiornamento (inclusi uno script di pre-aggiornamento e uno script di post-aggiornamento, se configurate) per ogni nodo. Il valore massimo è 64.|  
|**MaxFailedNodes**|Per la maggior parte dei cluster, numero intero corrispondente a circa un terzo del numero di nodi del cluster|Numero massimo di nodi in cui l'aggiornamento può avere esito negativo, sia perché dei nodi o l'interruzione del servizio Cluster. Se uno o più nodi non riesce, l'operazione di aggiornamento è stato arrestato.<br /><br /> L'intervallo di valori valido è 0 e 1, meno il numero di nodi del cluster.|  
|**RequireAllNodesOnline**|Nessuno|Specifica che tutti i nodi devono essere online e raggiungibili prima dell'aggiornamento viene avviata.|  
|**RebootTimeoutMinutes**|15|Tempo in minuti che consentirà aggiornamento compatibile con cluster per il riavvio di un nodo (se è necessario un riavvio) e l'avvio di tutti i servizi ad avvio automatico. Se il processo di riavvio non viene completato entro questo intervallo, l'operazione di aggiornamento su quel nodo verrà contrassegnata come non riuscita.|  
|**PreUpdateScript**|Nessuno|Il percorso e il nome di uno script di PowerShell da eseguire su ciascun nodo prima dell'inizio dell'aggiornamento e prima che il nodo venga posizionato in modalità manutenzione. L'estensione del nome file deve essere **ps1**, e la lunghezza totale del percorso di più file il nome non deve eccedere i 260 caratteri. Come procedura consigliata, lo script deve trovarsi su un disco nell'archiviazione cluster o in una condivisione file di rete a disponibilità elevata, per assicurarsi che sia sempre accessibile a tutti i nodi del cluster. Se lo script si trova in una condivisione di file di rete, assicurarsi di configurare la condivisione file per l'autorizzazione di lettura per il gruppo Everyone e limitare l'accesso in scrittura per prevenire la manomissione di file da parte di utenti non autorizzati.<br /><br /> Se si specifica uno script di pre-aggiornamento, assicurarsi che le impostazioni, ad esempio il tempo limiti (ad esempio, **StopAfter**) sono configurati per consentire il corretta esecuzione dello script. Questi limiti riguardano l'intero processo di esecuzione degli script e installazione degli aggiornamenti, non solo il processo di installazione degli aggiornamenti.|  
|**PostUpdateScript**|Nessuno|Il percorso e il nome di uno script di PowerShell da eseguire al termine dell'aggiornamento (dopo il nodo ha lasciato la modalità di manutenzione). L'estensione del nome file deve essere **ps1** e la lunghezza totale del percorso di più file il nome non deve eccedere i 260 caratteri. Come procedura consigliata, lo script deve trovarsi su un disco nell'archiviazione cluster o in una condivisione file di rete a disponibilità elevata, per assicurarsi che sia sempre accessibile a tutti i nodi del cluster. Se lo script si trova in una condivisione di file di rete, assicurarsi di configurare la condivisione file per l'autorizzazione di lettura per il gruppo Everyone e limitare l'accesso in scrittura per prevenire la manomissione di file da parte di utenti non autorizzati.<br /><br /> Se si specifica uno script di post-aggiornamento, assicurarsi che le impostazioni, ad esempio il tempo limiti (ad esempio, **StopAfter**) sono configurati per consentire il corretta esecuzione dello script. Questi limiti riguardano l'intero processo di esecuzione degli script e installazione degli aggiornamenti, non solo il processo di installazione degli aggiornamenti.|  
|**ConfigurationName**|Questa impostazione ha effetto solo se l'esecuzione di script.<br /><br /> Se si specifica uno script di pre-aggiornamento o uno script di post-aggiornamento, ma non si specifica un **ConfigurationName**, la sessione predefinita viene utilizzata la configurazione per PowerShell (Microsoft. PowerShell).|Specifica la configurazione di sessione di PowerShell che definisce la sessione in cui gli script (specificati da **PreUpdateScript** e **PostUpdateScript**) vengono eseguiti e possono limitare i comandi che possono essere eseguiti.|  
|**CauPluginName**|**Microsoft. windowsupdateplugin**|Del plug-in che configurare aggiornamento compatibile con Cluster da usare per visualizzare in anteprima gli aggiornamenti o eseguire un'operazione di aggiornamento. Per ulteriori informazioni, vedere [plug-in come aggiornamento compatibile con Cluster funziona](cluster-aware-updating-plug-ins.md).|  
|**CauPluginArguments**|Nessuno|Un set di *nome = valore* coppie (argomenti) per il plug-in di utilizzare, ad esempio l'aggiornamento:<br /><br /> **Domain=domain.Local**<br /><br /> Questi *nome = valore* coppie devono essere significative per il plug-in specificato in **CauPluginName**.<br /><br /> Per specificare un argomento tramite l'UI CAU, digitare il *nome*, premere il tasto Tab e quindi digitare corrispondente *valore*. Premere il tasto Tab per fornire l'argomento successivo. Ogni *nome* e *valore* sono automaticamente separate da un segno di uguale (=). Coppie multiple sono automaticamente separate da punti e virgola.<br /><br /> Per impostazione predefinita **Microsoft. windowsupdateplugin** plug-in, non sono necessari argomenti. Tuttavia, è possibile specificare un argomento facoltativo, ad esempio per specificare una stringa di query di agente di Windows Update standard per filtrare il set di aggiornamenti applicati dal plug-in. Per un *nome*, utilizzare **QueryString**e per un *valore*, racchiudere l'intera query tra virgolette.<br /><br /> Per ulteriori informazioni, vedere [plug-in come aggiornamento compatibile con Cluster funziona](cluster-aware-updating-plug-ins.md).|  
  
##  <a name="BKMK_runtime"></a>Opzioni da specificare quando si richiede un'operazione di aggiornamento  
 La tabella seguente elenca le opzioni (diverse da quelle in un profilo) che è possibile specificare quando si richiede un'operazione di aggiornamento. Per informazioni sulle opzioni che è possibile impostare in un profilo operazione di aggiornamento, vedere la tabella precedente.  
  
|Opzione|Valore predefinito|Dettagli|  
|------------|-------------------|-------------|  
|**NomeCluster**|Nessuno <br>**Nota:** questa opzione deve essere impostata solo quando la UI CAU non viene eseguita in un nodo di cluster di failover o si desidera fare riferimento a un cluster di failover diverso da cui viene eseguita il UI CAU.|Nome NetBIOS del cluster su cui eseguire l'operazione di aggiornamento.|  
|**Credenziali**|Credenziali dell'account corrente|Credenziali amministrative per la destinazione del cluster in cui l'operazione di aggiornamento verrà eseguita. Si potrebbero già le credenziali necessarie se si avvia la UI CAU (o aprire una sessione di PowerShell, se stai usando i cmdlet PowerShell di aggiornamento compatibile con cluster) da un account che dispone di autorizzazioni e diritti di amministratore nel cluster.|  
|**NodeOrder**|Per impostazione predefinita, aggiornamento compatibile con cluster inizia con il nodo che possiede meno ruoli cluster, quindi elabora il nodo con il secondo con meno numero e così via.|Nomi dei nodi del cluster nell'ordine che devono essere aggiornati (se possibile).|  
  
##  <a name="BKMK_profile"></a>Utilizzare i profili di esecuzione di aggiornamento  
 Ogni operazione di aggiornamento può essere associato a un determinato profilo. Il profilo operazione di aggiornamento viene archiviato nel valore predefinito di *%windir%\Cluster.* cartella. Se si utilizza l'UI CAU aggiornamento remoto modalità, è possibile specificare un profilo al momento che si applicano gli aggiornamenti oppure è possibile utilizzare il profilo operazione di aggiornamento predefinito. Se si utilizza aggiornamento compatibile con cluster in modalità di aggiornamento automatico, è possibile importare le impostazioni da un profilo specificato quando si configurano le opzioni di aggiornamento automatico. In entrambi i casi, è possibile sostituire i valori visualizzati per le opzioni dell'operazione di aggiornamento in base alle esigenze. Se si desidera, è possibile salvare le opzioni come un profilo con lo stesso nome di file o un nome diverso. Al successivo che applicare gli aggiornamenti o configurare automaticamente le opzioni di aggiornamento automatico, aggiornamento compatibile con cluster seleziona il profilo selezionato in precedenza.  
  
 È possibile modificare un aggiornamento eseguire profilo esistente o crearne uno nuovo selezionando **crea o modifica profilo operazione di aggiornamento** nell'UI CAU.

Ecco alcune note importanti sull'utilizzo di profili di esecuzione di aggiornamento:

* Un profilo operazione di aggiornamento non memorizza informazioni specifiche del cluster, ad esempio le credenziali amministrative. Se si utilizza aggiornamento compatibile con cluster in modalità di aggiornamento automatico, il profilo anche non archiviare le informazioni sulla pianificazione di aggiornamento automatico. Questo rende possibile condividere un profilo in tutti i cluster di failover di una classe specifica.
* Se si configurano le opzioni di aggiornamento automatico utilizzando un profilo e in seguito si modifica il profilo con valori diversi per le opzioni dell'operazione di aggiornamento, la configurazione di aggiornamento automatico non cambia automaticamente. Per applicare le nuove impostazioni di operazione di aggiornamento, è necessario configurare nuovamente le opzioni di aggiornamento automatico.
* Editor di profili di esecuzione non supporta Purtroppo percorsi di file che contengono spazi, ad esempio *C:\Program Files*. In alternativa, archiviare il pre e post script di aggiornamento in un percorso che non includere spazi o usare PowerShell esclusivamente per gestire i profili di esecuzione, inserire le virgolette per racchiudere il percorso quando si esegue **Invoke-CauRun**.

### <a name="windows-powershell-equivalent-commands"></a>Comandi equivalenti di Windows PowerShell
  
 È possibile importare le impostazioni da un profilo quando si esegue il **Invoke-CauRun**, **Add-CauClusterRole**, o **Set-CauClusterRole** cmdlet.  
  
 L'esempio seguente esegue una scansione e un'operazione di aggiornamento completa nel cluster denominato *CONTOSO-FC1*, utilizzando le opzioni specificate in *C:\Windows\Cluster\DefaultParameters.xml*. Vengono utilizzati i valori predefiniti per i parametri del cmdlet rimanenti.  
  
```powershell  
$MyRunProfile = Import-Clixml C:\Windows\Cluster\DefaultParameters.xml  
Invoke-CauRun –ClusterName CONTOSO-FC1 @MyRunProfile   
```  
  
 Utilizzando un profilo operazione di aggiornamento, è possibile aggiornare ripetibili di un cluster di failover con impostazioni coerenti per la gestione delle eccezioni, i limiti di tempo e altri parametri operativi. Poiché queste impostazioni sono generalmente specifiche di una classe di cluster di failover, ad esempio "Tutti i cluster di Microsoft SQL Server" o "Miei cluster business-critical", si consiglia di denominare ogni profilo in base alla classe di cluster di Failover da utilizzare con. Inoltre, si desidera gestire il profilo in una condivisione file accessibile a tutti i cluster di failover di una classe specifica nell'organizzazione IT.  
  
  
  
## <a name="see-also"></a>Vedere anche

-   [Aggiornamento compatibile con cluster](cluster-aware-updating.md)
  
-   [Cmdlet per aggiornamento compatibile con cluster in Windows PowerShell](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating)