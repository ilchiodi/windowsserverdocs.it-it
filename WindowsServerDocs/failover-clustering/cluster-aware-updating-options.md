---
ms.assetid: 2f4b6641-0ec2-4b1c-85fb-a1f1d16685c8
title: Opzioni avanzate di aggiornamento compatibile con cluster e profili di esecuzione aggiornamento
description: Come configurare le opzioni avanzate e aggiornare i profili di esecuzione per l'aggiornamento compatibile con cluster
ms.topic: article
ms.prod: windows-server
manager: lizross
ms.author: jgerend
author: JasonGerend
ms.date: 08/06/2018
ms.openlocfilehash: 6eb577ce0f0bd05d91a49a8196eb668260b067fe
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828114"
---
# <a name="cluster-aware-updating-advanced-options-and-updating-run-profiles"></a>Opzioni avanzate di aggiornamento compatibile con cluster e profili di esecuzione aggiornamento

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012.

Questo argomento descrive le opzioni di aggiornamento che possono essere configurate per un'operazione di aggiornamento di aggiornamento [compatibile con cluster](cluster-aware-updating.md) . Queste opzioni avanzate possono essere configurate quando si usa l'interfaccia utente di aggiornamento compatibile con cluster o i cmdlet di Windows PowerShell di aggiornamento compatibile con cluster per applicare gli aggiornamenti o per configurare le opzioni di aggiornamento automatico.

La maggior parte delle impostazioni di configurazione può essere salvata come file XML noto come profilo dell'operazione di aggiornamento e riutilizzabile per operazioni di aggiornamento successive. I valori predefiniti per le opzioni delle operazioni di aggiornamento forniti da Aggiornamento compatibile con cluster possono inoltre essere utilizzati in numerosi ambienti cluster.

Per informazioni sulle ulteriori opzioni che è possibile specificare per ogni operazione di aggiornamento e sui profili delle operazioni di aggiornamento, vedere le sezioni seguenti più avanti in questo argomento:

Opzioni specificate quando si richiede un'operazione di aggiornamento usare le opzioni dei profili di aggiornamento che è possibile impostare in un profilo dell'operazione di aggiornamento

Nella tabella seguente sono elencate le opzioni che è possibile impostare in un profilo dell'operazione di aggiornamento di Aggiornamento compatibile con cluster. 

> [!NOTE] 
> Per impostare l'opzione PreUpdateScript o PostUpdateScript, assicurarsi che siano installati Windows PowerShell e .NET Framework 4,6 o 4,5 e che la comunicazione remota di PowerShell sia abilitata in ogni nodo del cluster. Per ulteriori informazioni, vedere Configurare i nodi per la gestione remota in [requisiti e procedure consigliate per l'aggiornamento compatibile con cluster](cluster-aware-updating-requirements.md).


|Opzione|Valore predefinito|Dettagli|  
|------------|-------------------|-------------|  
|**StopAfter**|Tempo illimitato|Tempo espresso in minuti prima che l'esecuzione dell'aggiornamento venga interrotta, se non è stata completata. **Nota:**  Se si specifica uno script di PowerShell di pre-aggiornamento o post-aggiornamento, l'intero processo di esecuzione degli script ed esecuzione degli aggiornamenti deve essere completato entro il limite di tempo **StopAfter** .|  
|**WarnAfter**|Per impostazione predefinita non viene visualizzato alcun avviso|Tempo in muniti dopo il quale viene visualizzato un avviso se l'operazione di aggiornamento, inclusi uno script di pre-aggiornamento e post-aggiornamento, se configurati, non è stata completata.|  
|**MaxRetriesPerNode**|3|Numero massimo di volte in cui verrà ritentato il processo di aggiornamento, inclusi uno script di pre-aggiornamento e post-aggiornamento, se configurati, per nodo. Il numero massimo è 64.|  
|**MaxFailedNodes**|Per la maggior parte dei cluster, numero intero corrispondente a circa un terzo del numero di nodi del cluster|Numero massimo di nodi nei quali l'aggiornamento può avere esito negativo a causa di un errore dei nodi o dell'interruzione del servizio Cluster. Se non riesce su uno o più nodi, l'operazione di aggiornamento viene interrotta.<p> L'intervallo di valori valido è compreso tra 0 e 1, meno il numero di nodi del cluster.|  
|**RequireAllNodesOnline**|None|Specifica che tutti i nodi cluster devono essere online e raggiungibili prima dell'inizio dell'aggiornamento.|  
|**RebootTimeoutMinutes**|15|Tempo espresso in minuti consentito da Aggiornamento compatibile con cluster per il riavvio di un nodo (se il riavvio è necessario) e l'avvio di tutti i servizi ad avvio automatico. Se il processo di riavvio non viene completato entro questo intervallo di tempo, l'operazione di aggiornamento su tale nodo viene contrassegnata come non riuscita.|  
|**PreUpdateScript**|None|Il percorso e il nome file di uno script di PowerShell da eseguire in ogni nodo prima dell'inizio dell'aggiornamento e prima che il nodo venga messo in modalità manutenzione. L'estensione del nome file deve essere **. ps1**e la lunghezza totale del percorso più il nome file non deve superare i 260 caratteri. La procedura consigliata prevede che lo script venga posizionato in un disco nello spazio di archiviazione del cluster oppure in una condivisione file di rete a elevata disponibilità, per assicurarsi che sia sempre accessibile per tutti i nodi del cluster. Se lo script si trova in una condivisione file di rete, assicurarsi che la condivisione file sia configurata con l'autorizzazione di lettura per il gruppo Everyone e limitare l'accesso in scrittura per evitare manomissioni dei file da parte di utenti non autorizzati.<p> Se si specifica uno script di pre-aggiornamento, assicurarsi che le impostazioni quali i limiti di tempo (ad esempio, **StopAfter**) siano configurate per consentire la perfetta esecuzione dello script. Questi limiti riguardano l'intero processo di esecuzione degli script e installazione degli aggiornamenti, non solo il processo di installazione degli aggiornamenti.|  
|**PostUpdateScript**|None|Il percorso e il nome file di uno script di PowerShell da eseguire dopo il completamento dell'aggiornamento (dopo che il nodo ha lasciato la modalità di manutenzione). L'estensione del nome file deve essere **. ps1** e la lunghezza totale del percorso più il nome file non deve superare i 260 caratteri. La procedura consigliata prevede che lo script venga posizionato in un disco nello spazio di archiviazione del cluster oppure in una condivisione file di rete a elevata disponibilità, per assicurarsi che sia sempre accessibile per tutti i nodi del cluster. Se lo script si trova in una condivisione file di rete, assicurarsi che la condivisione file sia configurata con l'autorizzazione di lettura per il gruppo Everyone e limitare l'accesso in scrittura per evitare manomissioni dei file da parte di utenti non autorizzati.<p> Se si specifica uno script di post-aggiornamento, assicurarsi che le impostazioni quali i limiti di tempo (ad esempio, **StopAfter**) siano configurate per consentire la perfetta esecuzione dello script. Questi limiti riguardano l'intero processo di esecuzione degli script e installazione degli aggiornamenti, non solo il processo di installazione degli aggiornamenti.|  
|**ConfigurationName**|Questa impostazione influisce solo se si eseguono script.<p> Se si specifica uno script di pre-aggiornamento o uno script di post-aggiornamento, ma non si specifica un **ConfigurationName**, viene usata la configurazione di sessione predefinita per PowerShell (Microsoft. PowerShell).|Specifica la configurazione di sessione di PowerShell che definisce la sessione in cui gli script (specificati da **PreUpdateScript** e **PostUpdateScript**) vengono eseguiti e possono limitare i comandi che possono essere eseguiti.|  
|**CauPluginName**|**Microsoft. WindowsUpdatePlugin**|Plug-in con il quale si configura l'Aggiornamento compatibile con cluster da usare per l'anteprima degli aggiornamenti o per eseguire un'operazione di aggiornamento. Per ulteriori informazioni, vedere funzionamento dei [plug-in di aggiornamento compatibile con cluster](cluster-aware-updating-plug-ins.md).|  
|**CauPluginArguments**|None|Fornisce al plug-in di aggiornamento un insieme di coppie *nome=valore* (argomenti), ad esempio:<p> **Dominio = Domain. local**<p> Queste coppie *nome=valore* devono essere significative per il plug-in specificato in **CauPluginName**.<p> Per specificare un argomento tramite l'interfaccia utente di Aggiornamento compatibile con cluster, digitare il *nome*, premere TAB e quindi digitare il *valore* corrispondente. Premere di nuovo il tasto Tab per fornire l'argomento successivo. Ciascun *nome* e *valore* sono automaticamente separati dal segno (=). Coppie multiple sono automaticamente separate da punti e virgola.<p> Per il plug-in **Microsoft. WindowsUpdatePlugin** predefinito, non sono necessari argomenti. È comunque possibile specificare un argomento facoltativo, ad esempio per specificare una stringa di query dell'agente di Windows Update standard per filtrare il set di aggiornamenti applicati dal plug-in. Per un *nome*, usare **QueryString**e, per un *valore*, racchiudere l'intera query tra virgolette.<p> Per ulteriori informazioni, vedere funzionamento dei [plug-in di aggiornamento compatibile con cluster](cluster-aware-updating-plug-ins.md).|  
  
##  <a name="options-that-you-specify-when-you-request-an-updating-run"></a><a name="BKMK_runtime"></a>Opzioni specificate quando si richiede un'operazione di aggiornamento  
 Nella tabella che segue sono elencate le opzioni (diverse da quelle del profilo di operazione di aggiornamento) da specificare quando si richiede Operazione di aggiornamento. Per informazioni sulle opzioni che è possibile impostare nel profilo dell'operazione di aggiornamento, vedere la tabella precedente.  
  
|Opzione|Valore predefinito|Dettagli|  
|------------|-------------------|-------------|  
|**NomeCluster**|None <br>**Nota:**  Questa opzione deve essere impostata solo quando l'interfaccia utente di aggiornamento compatibile con cluster non viene eseguita in un nodo del cluster di failover o si vuole fare riferimento a un cluster di failover diverso da quello in cui viene eseguita l'interfaccia utente di aggiornamento compatibile|Nome NetBIOS del cluster in cui eseguire l'operazione di aggiornamento.|  
|**Credenziali**|Credenziali dell'account corrente|Credenziali amministrative per il cluster di destinazione sul quale viene eseguita l'operazione di aggiornamento. Se si avvia l'interfaccia utente di aggiornamento compatibile con cluster (o si apre una sessione di PowerShell, se si usano i cmdlet PowerShell di aggiornamento compatibile con cluster) da un account che dispone di diritti e autorizzazioni di amministratore per il cluster, è possibile che si disponga già delle credenziali necessarie.|  
|**NodeOrder**|Per impostazione predefinita Aggiornamento compatibile con cluster inizia dal nodo che possiede meno ruoli cluster, quindi elabora il secondo con meno ruoli e così via.|Nomi dei nodi cluster nell'ordine in cui devono essere aggiornati (se possibile).|  
  
##  <a name="use-updating-run-profiles"></a><a name="BKMK_profile"></a>Usare i profili delle operazioni di aggiornamento  
 Ogni operazione di aggiornamento può essere associata a un profilo dell'operazione di aggiornamento specifico. Il profilo dell'operazione di aggiornamento predefinito viene archiviato nella cartella *%windir%\Cluster* Se si usa l'interfaccia utente di aggiornamento compatibile con cluster in modalità di aggiornamento remoto, è possibile specificare un profilo dell'operazione di aggiornamento nel momento in cui si applicano gli aggiornamenti oppure è possibile usare il profilo dell'operazione di aggiornamento predefinito. Se si usa aggiornamento compatibile con cluster in modalità di aggiornamento automatico, è possibile importare le impostazioni da un profilo dell'operazione di aggiornamento specificato quando si configurano le opzioni di aggiornamento automatico. In entrambi i casi è possibile sostituire i valori visualizzati per le opzioni dell'operazione di aggiornamento in base alle specifiche esigenze. Se necessario, è possibile salvare le opzioni dell'operazione di aggiornamento come profilo con lo stesso nome di file o uno diverso. In occasione della successiva applicazione degli aggiornamenti o durante la configurazione delle opzioni di aggiornamento automatico, viene selezionato automaticamente il profilo dell'operazione di aggiornamento selezionato in precedenza.  
  
 È possibile modificare un profilo dell'operazione di aggiornamento esistente o crearne uno nuovo selezionando **Crea o modifica profilo** operazione di aggiornamento nell'interfaccia utente di aggiornamento compatibile con cluster.

Di seguito sono riportate alcune note importanti sull'utilizzo dei profili di esecuzione aggiornamento:

* Un profilo dell'operazione di aggiornamento non archivia informazioni specifiche del cluster, ad esempio le credenziali amministrative. Se si usa aggiornamento compatibile con cluster in modalità di aggiornamento automatico, il profilo dell'operazione di aggiornamento non archivia anche le informazioni sulla pianificazione dell'aggiornamento automatico. In questo modo è possibile condividere un profilo in tutti i cluster di failover di una classe specifica.
* Se si configurano le opzioni di aggiornamento automatico usando un profilo dell'operazione di aggiornamento e successivamente si modifica il profilo con valori diversi per le opzioni dell'operazione di aggiornamento, la configurazione con aggiornamento automatico non cambia automaticamente. Per applicare le nuove impostazioni dell'operazione di aggiornamento, è necessario configurare di nuovo le opzioni di aggiornamento automatico.
* L'editor del profilo di esecuzione Sfortunatamente non supporta i percorsi di file che includono spazi, ad esempio *c:\Programmi*. Come soluzione alternativa, archiviare gli script di pre e post-aggiornamento in un percorso che non includa spazi oppure usare PowerShell esclusivamente per gestire i profili di esecuzione, inserendo le virgolette intorno al percorso quando si esegue **Invoke-CauRun**.

### <a name="windows-powershell-equivalent-commands"></a>Comandi equivalenti di Windows PowerShell
  
 È possibile importare le impostazioni da un profilo dell'operazione di aggiornamento quando si esegue il cmdlet **Invoke-CauRun**, **Add-CauClusterRole**o **set-CauClusterRole** .  
  
 L'esempio seguente consente di eseguire un'analisi e un'operazione di aggiornamento completa nel cluster denominato *CONTOSO-FC1* con le opzioni specificate in *C:\Windows\Cluster\DefaultParameters.xml*. Per gli altri parametri del cmdlet vengono utilizzati i valori predefiniti.  
  
```powershell  
$MyRunProfile = Import-Clixml C:\Windows\Cluster\DefaultParameters.xml  
Invoke-CauRun –ClusterName CONTOSO-FC1 @MyRunProfile   
```  
  
 È possibile utilizzare un profilo dell'operazione di aggiornamento per aggiornamenti ripetibili di un cluster di failover con impostazioni coerenti per la gestione delle eccezioni, i limiti di tempo e altri parametri operativi. Poiché queste impostazioni sono in genere specifiche di una classe di cluster di failover, ad esempio "tutti i cluster Microsoft SQL Server" o "i cluster cruciali per l'azienda", potrebbe essere necessario assegnare un nome a ogni profilo dell'operazione di aggiornamento in base alla classe di cluster di failover con cui verrà usato. È inoltre consigliabile gestire il profilo dell'operazione di aggiornamento in una condivisione file accessibile per tutti i cluster di failover di una classe specifica nell'organizzazione IT.  
  
  
  
## <a name="see-also"></a>Vedere anche

-   [Aggiornamento compatibile con cluster](cluster-aware-updating.md)
  
-   [Cmdlet di aggiornamento compatibile con cluster in Windows PowerShell](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps)