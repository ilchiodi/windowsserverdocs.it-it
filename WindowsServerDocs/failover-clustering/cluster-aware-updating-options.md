---
ms.assetid: 2f4b6641-0ec2-4b1c-85fb-a1f1d16685c8
title: Aggiornamento compatibile con cluster advanced opzioni e aggiornare i profili di esecuzione
ms.topic: article
ms.prod: windows-server-threshold
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 08/06/2018
description: Come configurare le opzioni avanzate e l'aggiornamento di profili di esecuzione per l'aggiornamento (compatibile con Cluster)
ms.openlocfilehash: 5fac31ad35422e623b98caaabdd9eae183e2e5d8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830552"
---
# <a name="cluster-aware-updating-advanced-options-and-updating-run-profiles"></a>Aggiornamento compatibile con cluster advanced opzioni e aggiornare i profili di esecuzione

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento descrive le opzioni di aggiornamento che possono essere configurate per un [aggiornamento compatibile con Cluster](cluster-aware-updating.md) operazione di aggiornamento (aggiornamento compatibile con cluster). Queste opzioni avanzate possono essere configurate quando si usa la UI CAU o i cmdlet di aggiornamento compatibile con Windows PowerShell per applicare gli aggiornamenti o per configurare opzioni di aggiornamento automatico.

La maggior parte delle impostazioni di configurazione può essere salvata come file XML noto come profilo dell'operazione di aggiornamento e riutilizzabile per operazioni di aggiornamento successive. I valori predefiniti per le opzioni delle operazioni di aggiornamento forniti da Aggiornamento compatibile con cluster possono inoltre essere utilizzati in numerosi ambienti cluster.

Per informazioni sulle ulteriori opzioni che è possibile specificare per ogni operazione di aggiornamento e sui profili delle operazioni di aggiornamento, vedere le sezioni seguenti più avanti in questo argomento:

Opzioni specificate quando si richiede un aggiornamento Usa l'aggiornamento eseguire i profili di opzioni di esecuzione che può essere impostata in un profilo

Nella tabella seguente sono elencate le opzioni che è possibile impostare in un profilo dell'operazione di aggiornamento di Aggiornamento compatibile con cluster. 

> [!NOTE] 
> Per impostare l'opzione PreUpdateScript o PostUpdateScript, assicurarsi che Windows PowerShell e .NET Framework 4.6 o 4.5 siano installati e che la comunicazione remota PowerShell sia abilitata in ogni nodo del cluster. Per altre informazioni, vedere Configurare i nodi per la gestione remota nel [i requisiti e procedure consigliate per aggiornamento compatibile con Cluster](cluster-aware-updating-requirements.md).


|Opzione|Valore predefinito|Dettagli|  
|------------|-------------------|-------------|  
|**StopAfter**|Tempo illimitato|Tempo espresso in minuti prima che l'esecuzione dell'aggiornamento venga interrotta, se non è stata completata. **Nota:**  Se si specifica un pre-aggiornamento o uno script di PowerShell di post-aggiornamento, l'intero processo di esecuzione di script e degli aggiornamenti dovrà essere completato entro il **StopAfter** limite di tempo.|  
|**WarnAfter**|Per impostazione predefinita non viene visualizzato alcun avviso|Tempo in muniti dopo il quale viene visualizzato un avviso se l'operazione di aggiornamento, inclusi uno script di pre-aggiornamento e post-aggiornamento, se configurati, non è stata completata.|  
|**MaxRetriesPerNode**|3|Numero massimo di volte in cui verrà ritentato il processo di aggiornamento, inclusi uno script di pre-aggiornamento e post-aggiornamento, se configurati, per nodo. Il numero massimo è 64.|  
|**MaxFailedNodes**|Per la maggior parte dei cluster, numero intero corrispondente a circa un terzo del numero di nodi del cluster|Numero massimo di nodi nei quali l'aggiornamento può avere esito negativo a causa di un errore dei nodi o dell'interruzione del servizio Cluster. Se non riesce su uno o più nodi, l'operazione di aggiornamento viene interrotta.<br /><br /> L'intervallo di valori valido è compreso tra 0 e 1, meno il numero di nodi del cluster.|  
|**RequireAllNodesOnline**|Nessuno|Specifica che tutti i nodi cluster devono essere online e raggiungibili prima dell'inizio dell'aggiornamento.|  
|**RebootTimeoutMinutes**|15|Tempo espresso in minuti consentito da Aggiornamento compatibile con cluster per il riavvio di un nodo (se il riavvio è necessario) e l'avvio di tutti i servizi ad avvio automatico. Se il processo di riavvio non viene completato entro questo intervallo, l'operazione di aggiornamento su tale nodo è contrassegnato come non riuscito.|  
|**PreUpdateScript**|Nessuno|Il percorso e il nome di uno script di PowerShell da eseguire in ogni nodo prima dell'inizio dell'aggiornamento e prima che il nodo venga posizionato in modalità manutenzione. L'estensione del nome file deve essere **ps1**, e la lunghezza totale del nome del percorso, più file non deve eccedere i 260 caratteri. La procedura consigliata prevede che lo script venga posizionato in un disco nello spazio di archiviazione del cluster oppure in una condivisione file di rete a elevata disponibilità, per assicurarsi che sia sempre accessibile per tutti i nodi del cluster. Se lo script si trova in una condivisione file di rete, assicurarsi che la condivisione file sia configurata con l'autorizzazione di lettura per il gruppo Everyone e limitare l'accesso in scrittura per evitare manomissioni dei file da parte di utenti non autorizzati.<br /><br /> Se si specifica uno script di pre-aggiornamento, assicurarsi che le impostazioni quali i limiti di tempo (ad esempio, **StopAfter**) siano configurate per consentire la perfetta esecuzione dello script. Questi limiti riguardano l'intero processo di esecuzione degli script e installazione degli aggiornamenti, non solo il processo di installazione degli aggiornamenti.|  
|**PostUpdateScript**|Nessuno|Il percorso e il nome di uno script di PowerShell da eseguire dopo il completamento dell'aggiornamento (dopo il nodo ha lasciato la modalità di manutenzione). L'estensione del nome file deve essere **ps1** e la lunghezza totale del nome del percorso, più file non deve eccedere i 260 caratteri. La procedura consigliata prevede che lo script venga posizionato in un disco nello spazio di archiviazione del cluster oppure in una condivisione file di rete a elevata disponibilità, per assicurarsi che sia sempre accessibile per tutti i nodi del cluster. Se lo script si trova in una condivisione file di rete, assicurarsi che la condivisione file sia configurata con l'autorizzazione di lettura per il gruppo Everyone e limitare l'accesso in scrittura per evitare manomissioni dei file da parte di utenti non autorizzati.<br /><br /> Se si specifica uno script di post-aggiornamento, assicurarsi che le impostazioni quali i limiti di tempo (ad esempio, **StopAfter**) siano configurate per consentire la perfetta esecuzione dello script. Questi limiti riguardano l'intero processo di esecuzione degli script e installazione degli aggiornamenti, non solo il processo di installazione degli aggiornamenti.|  
|**ConfigurationName**|Questa impostazione influisce solo se si eseguono script.<br /><br /> Se si specifica uno script di pre-aggiornamento o post-aggiornamento, ma non si specifica un **ConfigurationName**, la sessione predefinita viene utilizzata la configurazione di PowerShell (PowerShell).|Specifica la configurazione di sessione di PowerShell che definisce la sessione in cui gli script (specificati da **PreUpdateScript** e **PostUpdateScript**) vengono eseguiti e possono limitare i comandi che possono essere eseguiti.|  
|**CauPluginName**|**Microsoft.WindowsUpdatePlugin**|Plug-in con il quale si configura l'Aggiornamento compatibile con cluster da usare per l'anteprima degli aggiornamenti o per eseguire un'operazione di aggiornamento. Per altre informazioni, vedere [plug-nella funzionalità di aggiornamento compatibile con Cluster come funzionano](cluster-aware-updating-plug-ins.md).|  
|**CauPluginArguments**|Nessuno|Fornisce al plug-in di aggiornamento un insieme di coppie *nome=valore* (argomenti), ad esempio:<br /><br /> **Domain=Domain.local**<br /><br /> Queste coppie *nome=valore* devono essere significative per il plug-in specificato in **CauPluginName**.<br /><br /> Per specificare un argomento tramite l'interfaccia utente di Aggiornamento compatibile con cluster, digitare il *nome*, premere TAB e quindi digitare il *valore* corrispondente. Premere di nuovo il tasto Tab per fornire l'argomento successivo. Ciascun *nome* e *valore* sono automaticamente separati dal segno (=). Coppie multiple sono automaticamente separate da punti e virgola.<br /><br /> Per impostazione predefinita **Microsoft. windowsupdateplugin** plug-in, non sono necessari argomenti. È comunque possibile specificare un argomento facoltativo, ad esempio per specificare una stringa di query dell'agente di Windows Update standard per filtrare il set di aggiornamenti applicati dal plug-in. Per un *name*, usare **QueryString**e un *valore*, racchiudere l'intera query tra virgolette.<br /><br /> Per altre informazioni, vedere [plug-nella funzionalità di aggiornamento compatibile con Cluster come funzionano](cluster-aware-updating-plug-ins.md).|  
  
##  <a name="BKMK_runtime"></a> Opzioni da specificare quando richiesta un'operazione di aggiornamento  
 Nella tabella che segue sono elencate le opzioni (diverse da quelle del profilo di operazione di aggiornamento) da specificare quando si richiede Operazione di aggiornamento. Per informazioni sulle opzioni che è possibile impostare nel profilo dell'operazione di aggiornamento, vedere la tabella precedente.  
  
|Opzione|Valore predefinito|Dettagli|  
|------------|-------------------|-------------|  
|**ClusterName**|Nessuno <br>**Nota:**  Questa opzione deve essere impostata solo quando l'interfaccia utente di Aggiornamento compatibile con cluster non è in esecuzione su un nodo del cluster di failover oppure quando si desidera fare riferimento a un cluster di failover diverso da quello in cui si esegue l'interfaccia utente di Aggiornamento compatibile con cluster.|Nome NetBIOS del cluster in cui eseguire l'operazione di aggiornamento.|  
|**Credenziali**|Credenziali dell'account corrente|Credenziali amministrative per il cluster di destinazione sul quale viene eseguita l'operazione di aggiornamento. Si disponga già delle credenziali necessarie se si avvia la UI CAU (o aprire una sessione di PowerShell, se si usano i cmdlet di PowerShell di aggiornamento compatibile con cluster) da un account che dispone di autorizzazioni e diritti di amministratore sul cluster.|  
|**NodeOrder**|Per impostazione predefinita Aggiornamento compatibile con cluster inizia dal nodo che possiede meno ruoli cluster, quindi elabora il secondo con meno ruoli e così via.|Nomi dei nodi cluster nell'ordine in cui devono essere aggiornati (se possibile).|  
  
##  <a name="BKMK_profile"></a> Usare i profili di esecuzione l'aggiornamento  
 Ogni operazione di aggiornamento può essere associata a un profilo dell'operazione di aggiornamento specifico. Il profilo viene archiviato in valore predefinito di *%windir%\Cluster.* cartella. Se si usa la UI CAU nell'aggiornamento remoto modalità, è possibile specificare un profilo al momento di applicare gli aggiornamenti oppure è possibile usare il profilo operazione di aggiornamento predefinito. Se si usa aggiornamento compatibile con cluster in modalità di aggiornamento automatico, è possibile importare le impostazioni da un specificato profilo quando si configurano le opzioni di aggiornamento automatico. In entrambi i casi è possibile sostituire i valori visualizzati per le opzioni dell'operazione di aggiornamento in base alle specifiche esigenze. Se necessario, è possibile salvare le opzioni dell'operazione di aggiornamento come profilo con lo stesso nome di file o uno diverso. In occasione della successiva applicazione degli aggiornamenti o durante la configurazione delle opzioni di aggiornamento automatico, viene selezionato automaticamente il profilo dell'operazione di aggiornamento selezionato in precedenza.  
  
 È possibile modificare un aggiornamento eseguito profilo esistente o crearne uno nuovo selezionando **crea o modifica profilo** nell'UI CAU.

Ecco alcune note importanti sull'uso di profili di eseguire l'aggiornamento:

* Un profilo non archivia informazioni specifiche per i cluster, ad esempio le credenziali amministrative. Se si usa aggiornamento compatibile con cluster in modalità di aggiornamento automatico, il profilo anche non archivia le informazioni sulla pianificazione di aggiornamento automatico. In questo modo è possibile condividere un profilo in tutti i cluster di failover di una classe specifica.
* Se si configurano le opzioni di aggiornamento automatico usando un profilo e modificarla successivamente il profilo con valori diversi per le opzioni, la configurazione di aggiornamento automatico non cambia automaticamente. Per applicare le nuove impostazioni dell'operazione di aggiornamento, è necessario configurare di nuovo le opzioni di aggiornamento automatico.
* L'Editor profili di esecuzione Sfortunatamente non supporta i percorsi di file che includono spazi, ad esempio *C:\Program Files*. In alternativa, archiviare la pre e post gli script di aggiornamento in un percorso che non includa spazi o usare PowerShell in modo esclusivo per gestire i profili di esecuzione, l'inserimento di virgolette che racchiudono il percorso quando si esegue **Invoke-CauRun**.

### <a name="windows-powershell-equivalent-commands"></a>Comandi equivalenti di Windows PowerShell
  
 È possibile importare le impostazioni da un profilo quando si esegue la **Invoke-CauRun**, **Add-CauClusterRole**, o **Set-CauClusterRole** cmdlet.  
  
 L'esempio seguente consente di eseguire un'analisi e un'operazione di aggiornamento completa nel cluster denominato *CONTOSO-FC1* con le opzioni specificate in *C:\Windows\Cluster\DefaultParameters.xml*. Per gli altri parametri del cmdlet vengono utilizzati i valori predefiniti.  
  
```powershell  
$MyRunProfile = Import-Clixml C:\Windows\Cluster\DefaultParameters.xml  
Invoke-CauRun –ClusterName CONTOSO-FC1 @MyRunProfile   
```  
  
 È possibile utilizzare un profilo dell'operazione di aggiornamento per aggiornamenti ripetibili di un cluster di failover con impostazioni coerenti per la gestione delle eccezioni, i limiti di tempo e altri parametri operativi. Dato che queste impostazioni sono generalmente specifiche di una classe di cluster di failover, ad esempio "Tutti i cluster di Microsoft SQL Server" o "Cluster cruciali per l'azienda", può essere utile denominare ogni profilo in base alla classe di cluster di failover con la quale verrà utilizzato. È inoltre consigliabile gestire il profilo dell'operazione di aggiornamento in una condivisione file accessibile per tutti i cluster di failover di una classe specifica nell'organizzazione IT.  
  
  
  
## <a name="see-also"></a>Vedere anche

-   [Aggiornamento compatibile con cluster](cluster-aware-updating.md)
  
-   [Cmdlet per aggiornamento compatibile con cluster in Windows PowerShell](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps)