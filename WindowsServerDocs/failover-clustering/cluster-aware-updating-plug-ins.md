---
ms.assetid: d44d4430-41e2-483a-9685-92610cdef32a
title: Funzionamento dei plug-in aggiornamento compatibile con Cluster
ms.topic: article
ms.prod: windows-server-threshold
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 4/28/2017
ms.technology: storage-failover-clustering
description: Come utilizzare i plug-in per coordinare gli aggiornamenti quando si utilizza aggiornamento compatibile con Cluster in Windows Server per installare gli aggiornamenti in un cluster.
ms.openlocfilehash: eb606dfbe6596ecabd35e6ac36624fab2b4436b9
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="how-cluster-aware-updating-plug-ins-work"></a>Funzionamento dei plug-in aggiornamento compatibile con Cluster

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

[Aggiornamento compatibile con cluster](cluster-aware-updating.md) (aggiornamento compatibile con cluster) utilizza plug-in per coordinare l'installazione degli aggiornamenti tra nodi in un cluster di failover. In questo argomento fornisce informazioni sull'utilizzo di predefinita aggiornamento compatibile con installazione di plug-in o altri installazione di plug-in installabili per aggiornamento compatibile con cluster.

## <a name="BKMK_INSTALL"></a>Installare un plug-in  
Un plug-in diverso da installazione di plug-aggiuntivi predefiniti che vengono installati con aggiornamento compatibile con cluster \ (**Microsoft. windowsupdateplugin** e **Microsoft. hotfixplugin**\) deve essere installato separatamente. Se aggiornamento compatibile con cluster viene usato in modalità di aggiornamento e, l'installazione di plug-in deve essere installato in tutti i nodi del cluster. Se aggiornamento compatibile con cluster viene usato in modalità di aggiornamento remote\, l'installazione di plug-in deve essere installato nel computer coordinatore dell'aggiornamento remoto. Un plug-in per l'installazione può avere requisiti di installazione aggiuntivi su ciascun nodo.  
  
Per installare un plug-in, seguire le istruzioni dell'autore del plug-in. Per registrare manualmente un plug-in con aggiornamento compatibile con cluster, eseguire il [Register-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) cmdlet in ogni computer in cui l'installazione di plug-in è installato.  
  
## <a name="specify-a-plug-in-and-plug-in-arguments"></a>Specificare un plug-in e plug-in  
  
### <a name="specify-a-cau-plug-in"></a>Specificare un aggiornamento compatibile con cluster plug-in

Nella UI CAU, si seleziona un plug-in da un elenco di elenchi a discesa di installazione di plug-in disponibili quando si utilizza aggiornamento compatibile con cluster per eseguire le azioni seguenti:  
  
-   Applicare gli aggiornamenti al cluster  
  
-   Aggiornamenti di anteprima per il cluster  
  
-   Configurare le opzioni di aggiornamento e di cluster  
  
Per impostazione predefinita, aggiornamento compatibile con cluster seleziona il plug-in **Microsoft. windowsupdateplugin**. Tuttavia, è possibile specificare qualsiasi plug-in che viene installato e registrato con aggiornamento compatibile con cluster.

> [!TIP]  
> Nella UI CAU, è possibile specificare solo un singolo plug-in per aggiornamento compatibile con cluster da utilizzare per visualizzare in anteprima o applicare gli aggiornamenti durante un'operazione di aggiornamento. Utilizzando i cmdlet PowerShell di aggiornamento compatibile con cluster, è possibile specificare uno o più di installazione di plug-in. Se è necessario installare più tipi di aggiornamenti nel cluster, risulta in genere più efficiente specificare più di installazione di plug-in un'unica operazione di aggiornamento, anziché usare separato operazione di aggiornamento per ogni installazione di plug-in. Ad esempio meno riavvii di nodo in genere si verificherà.

Utilizzando i cmdlet PowerShell di aggiornamento compatibile con cluster elencati nella tabella seguente, è possibile specificare uno o più di installazione di plug-in per un'operazione di aggiornamento o di analisi passando il **– CauPluginName** parametro. È possibile specificare più nomi di plug-in, separandoli con virgole. Se si specifica più di installazione di plug-in, è inoltre possibile controllare l'installazione di plug-aggiuntivi influenzano reciproca durante un'operazione di aggiornamento specificando i **\-RunPluginsSerially**, **\-StopOnPluginFailure**, e **– SeparateReboots** parametri. Per ulteriori informazioni sull'utilizzo di più di installazione di plug-in, utilizzare i collegamenti disponibili alla documentazione dei cmdlet nella tabella seguente.  
  
|Cmdlet|Descrizione|  
|----------|---------------|  
|[Add-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole)|Aggiunge il ruolo del cluster aggiornamento compatibile con cluster che fornisce la funzionalità di aggiornamento e per il cluster specificato.|  
|[Invoke-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun)|Esegue un'analisi dei nodi del cluster per gli aggiornamenti applicabili e li installa tramite un'operazione di aggiornamento sul cluster specificato.|  
|[Invoke-CauScan](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-causcan)|Esegue un'analisi dei nodi del cluster per gli aggiornamenti applicabili e restituisce un elenco del set iniziale di aggiornamenti che verrebbe applicato a ogni nodo del cluster specificato.|  
|[Set-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/set-cauclusterrole)|Imposta le proprietà di configurazione per il ruolo del cluster aggiornamento compatibile con cluster sul cluster specificato.|  
  
Se non si specifica un parametro di plug-in aggiornamento compatibile con cluster usando questi cmdlet, il valore predefinito è il plug-in **Microsoft. windowsupdateplugin**.  
  
### <a name="specify-cau-plug-in-arguments"></a>Specificare gli argomenti plug-in aggiornamento compatibile con cluster  
Quando si configurano le opzioni di operazione di aggiornamento, è possibile specificare uno o più *name\ = valore* coppie \(arguments\) per il plug-in da utilizzare. Ad esempio, nella UI CAU, è possibile specificare più argomenti come indicato di seguito:  
  
**Name1\ = Value1; Name2\ = Value2; Name3\ = valore3**  
  
Questi *name\ = valore* coppie devono essere significative per il plug-in specificato. Per alcuni installazione di plug-negli argomenti sono facoltativi.  
  
La sintassi degli argomenti dell'installazione di plug-in di aggiornamento compatibile con cluster segue queste regole generali:  
  
-   Più *name\ = valore* coppie sono separate da punto e virgola.  
  
-   Un valore che contengono spazi verrà racchiusi tra virgolette, ad esempio: **Name1\ = "Value with Spaces"**.  
  
-   La sintassi esatta di *valore* dipende dal plug-in.  
  
Per specificare gli argomenti del plug-in usando i cmdlet PowerShell di aggiornamento compatibile con cluster che supportano il **– CauPluginParameters** parametro, passare un parametro nel formato:  
  
**\-CauPluginArguments @{Name1\ = Value1; Name2\ = Value2; Name3\ = valore3}**  
  
È inoltre possibile utilizzare una tabella hash PowerShell predefinita. Per specificare gli argomenti del plug-in per più di un plug-in, passare più tabelle hash di argomenti, separati da virgole. Passare gli argomenti del plug-nell'ordine dei plug-in specificato in **CauPluginName**.  
  
### <a name="specify-optional-plug-in-arguments"></a>Specificare argomenti facoltativi del plug-in  
L'installazione di plug-in installati aggiornamento compatibile con cluster da \ (**Microsoft. windowsupdateplugin** e **Microsoft. hotfixplugin**\) forniscono opzioni aggiuntive che è possibile selezionare. Nella UI CAU, vengono visualizzati in un **opzioni aggiuntive** pagina dopo aver configurato le opzioni di operazione di aggiornamento per l'installazione di plug-in. Se si utilizza il cmdlet PowerShell di aggiornamento compatibile con cluster, tali opzioni vengono configurate come argomenti facoltativi del plug-in. Per ulteriori informazioni, vedere [usare Microsoft. windowsupdateplugin](#BKMK_WUP) e [usare Microsoft. hotfixplugin](#BKMK_HFP) più avanti in questo argomento.  
  
## <a name="manage-plug-ins-using-windows-powershell-cmdlets"></a>Gestire l'installazione di plug-aggiuntivi utilizzando i cmdlet di Windows PowerShell  
  
|Cmdlet|Descrizione|  
|----------|---------------|  
|[Get-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/get-cauplugin)|Recupera le informazioni sull'installazione di plug-in registrati nel computer locale di aggiornamento software di uno o più.|  
|[Register-CauPlugin]((https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin))|Registra un plug-nel computer locale di aggiornamento del software di aggiornamento compatibile con cluster.|  
|[Unregister-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/unregister-cauplugin)|Rimuove un plug-in dall'elenco di installazione di plug-in che possono essere utilizzati da aggiornamento compatibile con cluster di aggiornamento software. **Nota:** l'installazione di plug-aggiuntivi che vengono installati con aggiornamento compatibile con cluster \ (**Microsoft. windowsupdateplugin** e **Microsoft. hotfixplugin**\) non è possibile annullare la registrazione.|  
  
## <a name="BKMK_WUP"></a>Con Microsoft. windowsupdateplugin  

Il valore predefinito plug-in per aggiornamento compatibile con cluster, **Microsoft. windowsupdateplugin**, esegue le operazioni seguenti:
- Comunica con agente di Windows Update su ciascun nodo del cluster di failover per applicare gli aggiornamenti necessari per i prodotti Microsoft in esecuzione in ogni nodo.
- Installa gli aggiornamenti del cluster direttamente da Windows Update o Microsoft Update o da un server di Windows Server Update Services \(WSUS\) on\ locale.
- Installa solo selezionata, la distribuzione generale rilascia aggiornamenti \(GDR\). Per impostazione predefinita, il plug-in applica solo gli aggiornamenti software importanti. È richiesta alcuna configurazione. La configurazione predefinita Scarica e installa gli aggiornamenti GDR importanti su ciascun nodo. 

> [!NOTE]
> Per applicare gli aggiornamenti diversi aggiornamenti software importanti selezionati per impostazione predefinita \ (ad esempio, driver updates\), è possibile configurare un parametro facoltativo plug-in. Per ulteriori informazioni, vedere [configurare la stringa di query di agente di Windows Update](#BKMK_QUERY).

### <a name="requirements"></a>Requisiti

- Cluster di failover e remoto coordinatore dell'aggiornamento computer \(if used\) deve soddisfare i requisiti per aggiornamento compatibile con cluster e la configurazione richiesta per la gestione remota che è elencato nella [requisiti e procedure consigliate per aggiornamento compatibile con cluster](cluster-aware-updating-requirements.md).
- Revisione [consigli per l'applicazione di aggiornamenti Microsoft](cluster-aware-updating-requirements.md#BKMK_BP_WUA), quindi apportare le modifiche necessarie alla configurazione di Microsoft Update per i nodi di cluster di failover.
- Per ottenere risultati ottimali, è consigliabile eseguire l'aggiornamento compatibile con cluster Best Practices Analyzer \(BPA\) per garantire che l'ambiente cluster e di aggiornamento siano configurati correttamente per applicare gli aggiornamenti tramite aggiornamento compatibile con cluster. Per ulteriori informazioni, vedere [aggiornamento compatibile con cluster Test disponibilità all'aggiornamento](cluster-aware-updating-requirements.md#BKMK_BPA).

> [!NOTE]
> Gli aggiornamenti che richiedono l'accettazione delle condizioni di licenza Microsoft o richiedono l'interazione dell'utente sono esclusi e devono essere installati manualmente.

### <a name="additional-options"></a>Opzioni aggiuntive

Facoltativamente, è possibile specificare i seguenti argomenti plug-in per espandere o limitare il set di aggiornamenti applicati dal plug-in:
- Per configurare il plug-in per applicare gli aggiornamenti consigliati oltre agli aggiornamenti importanti in ogni nodo, la UI CAU, il **opzioni aggiuntive** pagina, selezionare il **Scarica gli aggiornamenti consigliati allo stesso modo che si ricevono gli aggiornamenti importanti** casella di controllo.
<br>In alternativa, configurare il **'IncludeRecommendedUpdates' \ = 'True'** argomento plug-in.
- Per configurare il plug-in per filtrare i tipi di aggiornamenti GDR che vengono applicati a ogni nodo del cluster, specificare una stringa di query agente di Windows Update usando un **QueryString** argomento plug-in. Per ulteriori informazioni, vedere [configurare la stringa di query di agente di Windows Update](#BKMK_QUERY).

### <a name="BKMK_QUERY"></a>Configurare la stringa di query di agente di Windows Update  
È possibile configurare un argomento del plug-in per il valore predefinito plug-in, **Microsoft. windowsupdateplugin**, che consiste in una stringa di query \(WUA\) agente di Windows Update. Questa istruzione Usa l'API agente di Windows Update per identificare uno o più gruppi di aggiornamenti Microsoft da applicare a ogni nodo, in base a specifici criteri di selezione. È possibile combinare più criteri usando operatori AND od OR logici. La stringa di query WUA è specificata in un argomento del plug-in come indicato di seguito:  
  
**QueryString\ = "Criterion1\ = Value1 and\ / Criterion2\ o = Value2 and\ / o..."**  
  
Ad esempio, **Microsoft. windowsupdateplugin** seleziona automaticamente gli aggiornamenti importanti usando un valore predefinito **QueryString** argomento costruito usando i **IsInstalled**, **tipo**, **IsHidden**, e **IsAssigned** criteri:  
  
**QueryString\ = "IsInstalled\ = 0 e Type\ ="Software"e IsHidden\ = 0 e IsAssigned\ = 1"**  
  
Se si specifica un **QueryString** argomento, viene utilizzato il valore predefinito al posto di **QueryString** che è configurato per l'installazione di plug-in.  
  
#### <a name="example-1"></a>Esempio 1
  
Per configurare un **QueryString** argomento che installa un aggiornamento specifico identificato dall'ID *f6ce46c1\-971c\-43f9\-a2aa\-783df125f003*:  
  
**QueryString\ = "UpdateID\ = 'f6ce46c1\-971c\-43f9\-a2aa\-783df125f003' e IsInstalled\ = 0"**  
  
> [!NOTE]  
> L'esempio precedente è valido per l'applicazione degli aggiornamenti tramite la procedura guidata di aggiornamento compatibile con Cluster\. Se si desidera installare un aggiornamento specifico configurando opzioni di aggiornamento e con la UI CAU o tramite il **Add-CauClusterRole** o **set-CauClusterRole**cmdlet di PowerShell, è necessario formattare il valore UpdateID racchiudendolo tra due caratteri apartment offerta:  
>   
> **QueryString\ = "UpdateID\ = ' f6ce46c1\-971c\-43f9\-a2aa\-783df125f003 ' e IsInstalled\ = 0"**  
  
#### <a name="example-2"></a>Esempio 2
  
Per configurare un **QueryString** argomento che installa solo driver:  
  
**QueryString\ = "IsInstalled\ = 0 e Type\ ='' e driver IsHidden\ = 0"**  
  
Per ulteriori informazioni sulle stringhe di query per il valore predefinito plug-in, **Microsoft. windowsupdateplugin**, i criteri di ricerca \ (ad esempio **IsInstalled**\), e la sintassi che è possibile includere nelle stringhe di query, vedere la sezione relativa ai criteri di ricerca nel [agente Windows Update (WUA) API Reference](http://go.microsoft.com/fwlink/p/?LinkId=223304).  
  
## <a name="BKMK_HFP"></a>Usare Microsoft. hotfixplugin  
Installazione di plug-in **Microsoft. hotfixplugin** può essere usato per applicare aggiornamenti Microsoft a distribuzione ridotta \(LDR\) \ (detto anche aggiornamenti rapidi e in precedenza denominati QFEs\) che vanno scaricati indipendentemente per risolvere problemi specifici del software Microsoft. Il plug-in installa gli aggiornamenti da una cartella radice in una condivisione file SMB e può essere personalizzato anche per applicare gli aggiornamenti del BIOS, firmware e driver fornitori non Microsoft.

> [!NOTE]
> Gli aggiornamenti rapidi sono talvolta resi disponibili per il download da Microsoft in articoli della Knowledge Base, ma vengono anche forniti ai clienti necessario as\.

### <a name="requirements"></a>Requisiti

- Cluster di failover e remoto coordinatore dell'aggiornamento computer \(if used\) deve soddisfare i requisiti per aggiornamento compatibile con cluster e la configurazione richiesta per la gestione remota che è elencato nella [requisiti e procedure consigliate per aggiornamento compatibile con cluster](cluster-aware-updating-requirements.md).
- Revisione [consigli per l'uso di Microsoft. hotfixplugin](cluster-aware-updating-requirements.md#BKMK_BP_HF).
- Per ottenere risultati ottimali, è consigliabile eseguire il modello di aggiornamento compatibile con cluster Best Practices Analyzer \(BPA\) per garantire che l'ambiente cluster e di aggiornamento siano configurati correttamente per applicare gli aggiornamenti tramite aggiornamento compatibile con cluster. Per ulteriori informazioni, vedere [aggiornamento compatibile con cluster Test disponibilità all'aggiornamento](cluster-aware-updating-requirements.md#BKMK_BPA).
- Ottenere gli aggiornamenti dell'autore e copiarli o estrarli in una condivisione di file Server Message Block \(SMB\) \ (folder\ radice degli aggiornamenti rapidi) che supporti almeno SMB 2.0 e che sia accessibile da tutti i nodi del cluster e il computer coordinatore dell'aggiornamento remoto \ (se aggiornamento compatibile con cluster viene usato in aggiornamento remote\ mode\). Per ulteriori informazioni, vedere [configurare una struttura di cartella radice degli aggiornamenti rapidi](#BKMK_HF_ROOT) più avanti in questo argomento. 

    > [!NOTE]
    > Per impostazione predefinita, questo plug-in installa solo aggiornamenti rapidi con le estensioni di file seguenti: msu,. msi e msp.

- Copiare il file DefaultHotfixConfig.xml \ (in cui viene fornita la **%systemroot%\\System32\\WindowsPowerShell\\v1.0\\Modules\\ClusterAwareUpdating** cartella su un computer in cui gli strumenti di aggiornamento compatibile con cluster sono installed\) nella cartella radice degli aggiornamenti rapidi creato e in cui sono stati estratti gli aggiornamenti rapidi. Ad esempio, copiare il file di configurazione *\\\MyFileServer\\Hotfixes\\Root\\*. 

    > [!NOTE]
    > Per installare la maggior parte degli aggiornamenti rapidi forniti da Microsoft e altri aggiornamenti, è possibile utilizzare il file di configurazione degli aggiornamenti rapidi predefinito senza modifiche. Se lo scenario lo richiede, è possibile personalizzare il file di configurazione come un'attività avanzata. Il file di configurazione può includere, ad esempio, ruoli personalizzati per la gestione di file hotfix con estensioni specifiche oppure per definire i comportamenti per determinate condizioni di uscita. Per ulteriori informazioni, vedere [personalizzare il file di configurazione](#BKMK_CONFIG_FILE) più avanti in questo argomento.

### <a name="configuration"></a>Configurazione

Configurare le impostazioni seguenti. Per ulteriori informazioni, vedere i collegamenti alle sezioni più avanti in questo argomento.
- Il percorso alla cartella radice degli aggiornamenti rapidi condivisa contenente gli aggiornamenti da applicare e che contiene il file di configurazione. È possibile digitare questo percorso nell'UI CAU o configurare il **HotfixRootFolderPath\ = \ < percorso >** argomento plug-in PowerShell. 

   > [!NOTE]
   > È possibile specificare la cartella radice degli aggiornamenti rapidi come percorso di una cartella locale o un percorso UNC nel formato *\\\ServerName\\Share\\RootFolderName *. È possibile utilizzare un percorso Namespace DFS autonomo o basata su domain \. Tuttavia, funzionalità di plug-in che verificano le autorizzazioni di accesso nel file di configurazione degli aggiornamenti rapidi non sono compatibili con un percorso DFS Namespace, in modo che se ne viene configurato uno, sarà necessario disabilitare il controllo per le autorizzazioni di accesso usando l'UI CAU oppure configurando il **DisableAclChecks\ = 'True'** argomento plug-in.
- Impostazioni del server che ospita la cartella radice degli aggiornamenti rapidi per controllare le autorizzazioni appropriate per accedere alla cartella e garantire l'integrità dei dati accessibili da SMB cartella condivisa \ (la firma SMB o Encryption\ SMB). Per ulteriori informazioni, vedere [limitare l'accesso alla cartella radice degli aggiornamenti rapidi ](#BKMK_ACL).

### <a name="additional-options"></a>Opzioni aggiuntive

- Facoltativamente, configurare il plug-in modo che la crittografia SMB venga imposta quando accedono ai dati dalla condivisione file degli aggiornamenti rapidi. Nella UI CAU, nel **opzioni aggiuntive** pagina, selezionare il **Richiedi crittografia SMB per l'accesso alla cartella radice degli aggiornamenti rapidi** oppure configurare il **RequireSMBEncryption\ = 'True'** argomento plug-in PowerShell. 
  > [!IMPORTANT]
  > È necessario eseguire ulteriori passaggi di configurazione nel server SMB per abilitare l'integrità dei dati SMB con firma o crittografia SMB. Per ulteriori informazioni, vedere il passaggio 4 in [limitare l'accesso alla cartella radice degli aggiornamenti rapidi ](#BKMK_ACL). Se si seleziona l'opzione per imporre l'uso della crittografia SMB e la cartella radice degli aggiornamenti rapidi non è configurato per l'accesso mediante la crittografia SMB, l'operazione di aggiornamento avrà esito negativo.
- Facoltativamente, disabilitare i controlli predefiniti disponibili autorizzazioni sufficienti per la cartella radice degli aggiornamenti rapidi e il file di configurazione. Nella UI CAU, selezionare **Disabilita controllo accesso amministratore nel file di configurazione e cartella radice degli aggiornamenti rapidi**, o configurare il **DisableAclChecks\ = 'True'** argomento plug-in.
- Facoltativamente, configurare il **HotfixInstallerTimeoutMinutes\ =<Integer>** argomento per specificare il tempo di attesa per il processo di installazione degli aggiornamenti rapidi restituire l'hotfix plug-in. \ (Il valore predefinito è 30 minuti. \) per specificare un periodo di timeout pari a due ore, ad esempio, è consigliabile impostare **HotfixInstallerTimeoutMinutes\ = 120**.
- Facoltativamente, configurare il **HotfixConfigFileName \ = <name>**argomento plug-in per specificare un nome per il file di configurazione che si trova nella cartella radice degli aggiornamenti rapidi. Se non specificato, viene utilizzato il nome predefinito DefaultHotfixConfig.xml.
  
### <a name="BKMK_HF_ROOT"></a>Configurare una struttura di cartella radice degli aggiornamenti rapidi

Per l'aggiornamento rapido plug-in per l'uso, gli aggiornamenti rapidi devono essere archiviati in una struttura ben definito in una condivisione file SMB \ (hotfix radice folder\), ed è necessario configurare l'aggiornamento rapido plug-in con il percorso alla cartella radice degli aggiornamenti rapidi tramite l'UI CAU o i cmdlet PowerShell di aggiornamento compatibile con cluster. Questo percorso viene passato al plug-in come il **HotfixRootFolderPath** argomento. È possibile scegliere tra diverse strutture per la cartella radice degli aggiornamenti rapidi, in base alle esigenze di aggiornamento, come illustrato negli esempi seguenti. File o cartelle non conformi alla struttura verranno ignorate.  
  
#### <a name="example-1---folder-structure-used-to-apply-hotfixes-to-all-cluster-nodes"></a>Esempio 1: struttura di cartelle usata per applicare gli aggiornamenti rapidi a tutti i nodi del cluster
  
Per specificare che gli aggiornamenti rapidi si applicano a tutti i nodi del cluster, copiarli in una cartella denominata **CAUHotfix\_All** sotto la cartella radice degli aggiornamenti rapidi. In questo esempio, il **HotfixRootFolderPath** argomento plug-in è impostato su *\\\MyFileServer\\Hotfixes\\Root\\*. Il **CAUHotfix\_All** cartella contiene tre aggiornamenti con le estensioni msu,. msi e msp che verranno applicate a tutti i nodi del cluster. I nomi dei file di aggiornamento sono solo a scopo illustrativo.  
  
> [!NOTE]  
> In questo esempio e gli esempi seguenti, il file di configurazione degli aggiornamenti rapidi con il nome predefinito DefaultHotfixConfig.xml è visualizzato nella relativa posizione richiesta nella cartella radice degli aggiornamenti rapidi.  
  
```
\\MyFileServer\Hotfixes\Root\   
   DefaultHotfixConfig.xml  
   CAUHotfix_All\   
      Update1.msu   
      Update2.msi   
      Update3.msp  
      ...  
```  
  
#### <a name="example-2---folder-structure-used-to-apply-certain-updates-only-to-a-specific-node"></a>Esempio 2: struttura di cartelle usata per applicare determinati aggiornamenti solo a un nodo specifico
  
Per specificare aggiornamenti rapidi applicabili solo a un nodo specifico, utilizzare una sottocartella nella cartella radice degli aggiornamenti rapidi con il nome del nodo. Utilizzare il nome NetBIOS del nodo del cluster, ad esempio, *ContosoNode1*. Spostare quindi gli aggiornamenti che si applicano solo a questo nodo in questione nella sottocartella. Nell'esempio seguente, il **HotfixRootFolderPath** argomento plug-in è impostato su *\\\MyFileServer\\Hotfixes\\Root\\*. Aggiorna il **CAUHotfix\_All** cartella verrà applicata a tutti i nodi del cluster, e *Node1\_Specific\_Update.msu* verrà applicato solo a *ContosoNode1*.  
  
```
\\MyFileServer\Hotfixes\Root\   
   DefaultHotfixConfig.xml  
   CAUHotfix_All\   
      Update1.msu   
      Update2.msi   
      Update3.msp  
      ...  
   ContosoNode1\   
      Node1_Specific_Update.msu   
      ...  
```  
  
#### <a name="example-3---folder-structure-used-to-apply-updates-other-than-msu-msi-and-msp-files"></a>Esempio 3: struttura di cartelle usata per applicare gli aggiornamenti ad eccezione dei file con estensione msu,. msi e msp
  
Per impostazione predefinita, **Microsoft. hotfixplugin** applica unicamente aggiornamenti con estensione msu,. msi o msp. Tuttavia, alcuni aggiornamenti potrebbero avere estensioni diverse e richiedono i comandi di installazione diversa. Ad esempio, si potrebbe essere necessario applicare un aggiornamento del firmware con estensione .exe a un nodo in un cluster. È possibile configurare la cartella radice degli aggiornamenti rapidi con una sottocartella che indichi un oggetto specifico, il tipo di aggiornamento non predefinita deve essere installato. È inoltre necessario configurare una regola di installazione della cartella corrispondente che specifica il comando di installazione nel `<FolderRules>`elemento nel file XML di configurazione degli aggiornamenti rapidi.  
  
Nell'esempio seguente, il **HotfixRootFolderPath** argomento plug-in è impostato su *\\\MyFileServer\\Hotfixes\\Root\\*. Alcuni aggiornamenti verranno applicati a tutti i nodi del cluster e un aggiornamento del firmware *SpecialHotfix1.exe* verranno applicate a *ContosoNode1* utilizzando *FolderRule1*. Per informazioni sulla configurazione *FolderRule1* nel file di configurazione degli aggiornamenti rapidi, vedere [personalizzare il file di configurazione](#BKMK_CONFIG_FILE) più avanti in questo argomento.  
  
```
\\MyFileServer\Hotfixes\Root\   
   DefaultHotfixConfig.xml  
   CAUHotfix_All\   
      Update1.msu   
      Update2.msi   
      Update3.msp  
      ...  
  
   ContosoNode1\   
      FolderRule1\  
          SpecialHotfix1.exe  
      ...  
```

### <a name="BKMK_CONFIG_FILE"></a>Personalizzare il file di configurazione  
La configurazione degli aggiornamenti rapidi di file come controlli **Microsoft. hotfixplugin** installa tipi di file degli aggiornamenti rapidi specifici in un cluster di failover. Lo schema XML per il file di configurazione è definito in HotfixConfigSchema.xsd, che si trova nella cartella seguente in un computer in cui sono installati gli strumenti di aggiornamento compatibile con cluster:  
  
**Cartella %SystemRoot%\\System32\\WindowsPowerShell\\v1.0\\Modules\\ClusterAwareUpdating**  
  
Per personalizzare il file di configurazione degli aggiornamenti rapidi, copiare il file di configurazione di esempio DefaultHotfixConfig.xml da questo percorso alla cartella radice degli aggiornamenti rapidi e apportare le modifiche appropriate per lo scenario.  
  
> [!IMPORTANT]  
> Per applicare la maggior parte degli aggiornamenti rapidi forniti da Microsoft e altri aggiornamenti, è possibile utilizzare il file di configurazione degli aggiornamenti rapidi predefinito senza modifiche. Personalizzazione del file di configurazione degli aggiornamenti rapidi è un'attività solo in scenari di utilizzo avanzato.  
  
Per impostazione predefinita, il file XML di configurazione degli aggiornamenti rapidi definisce regole di installazione e condizioni di uscita per le seguenti due categorie di hotfix:  
  
-   File di aggiornamento rapido con estensioni che è possibile installare il plug-in per impostazione predefinita \ (con estensione msu,. msi e msp Files \).  
  
    Questi vengono definiti `<ExtensionRules>`gli elementi di `<DefaultRules>`elemento. Esiste una `<Extension>`elemento per ognuno dei tipi di file predefiniti supportati. La struttura generale XML è come segue:  
  
    ```xml  
    <DefaultRules>  
        <ExtensionRules>  
          <Extension name="MSI">  
            <!-- Template and ExitConditions elements for installation of .msi files follow -->  
             ...  
          </Extension>  
          <Extension name="MSU">  
            <!-- Template and ExitConditions elements for installation of .msu files follow -->  
             ...  
          </Extension>  
          <Extension name="MSP">  
            <!-- Template and ExitConditions elements for installation of .msp files follow -->  
             ...  
          </Extension>  
             ...  
       </ExtensionRules>  
    </DefaultRules>  
    ```  
  
    Se è necessario applicare determinati tipi di aggiornamenti per tutti i nodi del cluster nell'ambiente in uso, è possibile definire ulteriori `<Extension>`elementi.  
  
-   Aggiornamenti rapidi o altri file di aggiornamento che non sono file con estensione msi,. msu o. msp, ad esempio, BIOS, firmware e driver fornitori non Microsoft degli aggiornamenti.  
  
    Ogni tipo di file non predefinita è configurato come un `<Folder>`elemento il `<FolderRules>`elemento. L'attributo del nome di `<Folder>`elemento deve essere identico al nome di una cartella nella cartella radice degli aggiornamenti rapidi che conterrà gli aggiornamenti del tipo corrispondente. La cartella può trovarsi nel **CAUHotfix\_All** cartella o in una cartella specifica tra. Ad esempio, se *FolderRule1* viene configurato nella cartella radice degli aggiornamenti rapidi, configurare l'elemento seguente nel file XML per definire un modello di installazione e uscire le condizioni per gli aggiornamenti in tale cartella:  
  
    ```xml  
    <FolderRules>  
          <Folder name="FolderRule1">  
            <!-- Template and ExitConditions elements for installation of updates in FolderRule1 follow -->  
             ...  
          </Folder>  
          ...  
    </FolderRules>  
    ```  
  
Le tabelle seguenti descrivono il `<Template>`attributi e i possibili `<ExitConditions>`sottoelementi.  
  
|`<Template>` attributo|Descrizione|  
|--------------------------|---------------|  
|`path`|Il percorso completo del programma di installazione per il tipo di file definito nel `<Extension name>`attributo.<br /><br />Per specificare il percorso di un file di aggiornamento nella struttura della cartella radice degli aggiornamenti rapidi, usare `$update$`.|  
|`parameters`|Una stringa di parametri obbligatori e facoltativi per il programma specificato in `path`.<br /><br />Per specificare un parametro che è il percorso di un file di aggiornamento nella struttura della cartella radice degli aggiornamenti rapidi, usare `$update$`.|  
  
|`<ExitConditions>` sottoelemento|Descrizione|  
|---------------------------------|---------------|  
|`<Success>`|Definisce uno o più codici di uscita che indicano l'aggiornamento specificato è riuscito. Si tratta di un sottoelemento obbligatorio.|  
|`<Success_RebootRequired>`|Definisce facoltativamente uno o più codici di uscita che indicano che l'aggiornamento specificato è riuscito e il nodo deve essere riavviato. <br>**Nota:** facoltativamente, il `<Folder>` elemento può contenere la `alwaysReboot` attributo. Se questo attributo è impostato, indica che se un aggiornamento rapido installato restituisce uno dei codici di uscita è definito in `<Success>`, ciò verrà interpretato come un `<Success_RebootRequired>` condizione di uscita.|  
|`<Fail_RebootRequired>`|Definisce facoltativamente uno o più codici di uscita che indicano che l'aggiornamento specificato non è riuscito e il nodo deve essere riavviato.|  
|`<AlreadyInstalled>`|Definisce facoltativamente uno o più codici di uscita che indicano che l'aggiornamento specificato non è stato applicato perché è già installato.|  
|`<NotApplicable>`|Definisce facoltativamente uno o più codici di uscita che indicano che l'aggiornamento specificato non è stato applicato perché non si applica al nodo del cluster.|  
  
> [!IMPORTANT]  
> Qualsiasi che non è definito in modo esplicito nel codice di uscita `<ExitConditions>` viene interpretato come aggiornamento non riuscito, e il nodo non viene riavviato.  
  
### <a name="BKMK_ACL"></a>Limitare l'accesso alla cartella radice degli aggiornamenti rapidi  
È necessario eseguire diversi passaggi per configurare la condivisione file SMB server e i file per proteggere il file di cartella radice degli aggiornamenti rapidi e al file di configurazione per l'accesso solo nel contesto di **Microsoft. hotfixplugin**. Questi passaggi abilitano varie funzionalità che aiutano a prevenire possibili manomissioni i file degli aggiornamenti rapidi in modo che potrebbero compromettere il cluster di failover.  
  
I passaggi generali sono i seguenti:  
  
1.  Identificare l'account utente utilizzato per operazioni di aggiornamento utilizzando l'installazione di plug-in  
  
2.  Configurare l'account utente in tutti i gruppi necessari in un file server SMB  
  
3.  Configurare le autorizzazioni per accedere alla cartella radice degli aggiornamenti rapidi  
  
4.  Configurare le impostazioni per l'integrità dei dati SMB  
  
5.  Abilitare una regola di Windows Firewall nel server SMB  
  
#### <a name="step-1-identify-the-user-account-that-is-used-for-updating-runs-by-using-the-hotfix-plug-in"></a>Passaggio 1. Identificare l'account utente utilizzato per operazioni di aggiornamento tramite l'aggiornamento rapido plug-in
  
L'account usato in aggiornamento compatibile con cluster per controllare le impostazioni di sicurezza durante l'esecuzione di un'operazione di aggiornamento utilizzando **Microsoft. hotfixplugin** dipende dal fatto che aggiornamento compatibile con cluster viene usato in modalità di aggiornamento remote\ o la modalità di aggiornamento e, come indicato di seguito:  
  
-   **Modalità di aggiornamento Remote\** l'account con privilegi amministrativi nel cluster in anteprima e applicare gli aggiornamenti.  
  
-   **Modalità di aggiornamento e** ruolo del cluster il nome dell'oggetto computer virtuale configurato in Active Directory per l'aggiornamento compatibile con cluster. Questo è il nome di un oggetto computer virtuale preinstallato in Active Directory per il ruolo del cluster aggiornamento compatibile con cluster o il nome che viene generato da aggiornamento compatibile con cluster per il ruolo del cluster. Per ottenere il nome se viene generato da aggiornamento compatibile con cluster, eseguire il **Get-CauClusterRole** cmdlet PowerShell di aggiornamento compatibile con cluster. Nell'output, **ResourceGroupName** è il nome dell'account dell'oggetto computer virtuale generato.  
  
#### <a name="step-2-configure-this-user-account-in-the-necessary-groups-on-an-smb-file-server"></a>Passaggio 2. Configurare l'account utente in tutti i gruppi necessari in un file server SMB
  
> [!IMPORTANT]  
> È necessario aggiungere l'account utilizzato per operazioni di aggiornamento di un account amministratore locale nel server SMB. Se non è autorizzato a causa di criteri di sicurezza dell'organizzazione, configurare l'account con le autorizzazioni necessarie nel server SMB usando la procedura seguente.  
  
##### <a name="to-configure-a-user-account-on-the-smb-server"></a>Per configurare un account utente nel server SMB  
  
1.  Aggiungere l'account utilizzato per operazioni di aggiornamento al gruppo Distributed COM Users e a uno dei gruppi seguenti: Power User, Server Operation o Print Operator.  
  
2.  Per abilitare le autorizzazioni WMI necessarie per l'account, avviare la Console di gestione WMI nel server SMB. Avviare PowerShell e quindi digitare il comando seguente:  
  
    ```  
    wmimgmt.msc  
    ```  
  
3.  Nell'albero della console, fare clic con **controllo WMI \(Local\)**, quindi fare clic su **proprietà**.  
  
4.  Fare clic su **sicurezza**, quindi espandere **radice**.  
  
5.  Fare clic su **CIMV2**, quindi fare clic su **sicurezza**.  
  
6.  Aggiungere l'account utilizzato per operazioni di aggiornamento per il **utenti e gruppi** elenco.  
  
7.  Concedere il **Esegui metodi** e **Abilita remoto** le autorizzazioni per l'account utilizzato per operazioni di aggiornamento.  
  
#### <a name="step-3-configure-permissions-to-access-the-hotfix-root-folder"></a>Passaggio 3. Configurare le autorizzazioni per accedere alla cartella radice degli aggiornamenti rapidi
  
Per impostazione predefinita, quando si tenta di applicare gli aggiornamenti, l'aggiornamento rapido plug-in controlla la configurazione delle autorizzazioni di file system NTFS per l'accesso alla cartella radice degli aggiornamenti rapidi. Se le autorizzazioni di accesso cartella non sono configurate correttamente, un'operazione di aggiornamento usando l'aggiornamento rapido plug-in potrebbe avere esito negativo.  
  
Se si utilizza la configurazione predefinita dell'aggiornamento rapido plug-in, assicurarsi che le autorizzazioni di accesso cartella soddisfino i requisiti seguenti.  
  
-   Il gruppo Users disponga dell'autorizzazione di lettura.  
  
-   Se il plug-in applicherà gli aggiornamenti con estensione .exe, il gruppo utenti dispone dell'autorizzazione di esecuzione.  
  
-   Sono consentite solo determinate entità di sicurezza \ (ma non sono required\) alla scrittura o modificare l'autorizzazione. L'entità di sicurezza ammesse sono il locale Administrators gruppo, SYSTEM, CREATOR OWNER e TrustedInstaller. Altri gruppi o account non sono consentiti per scrittura o modificare le autorizzazioni per la cartella radice degli aggiornamenti rapidi.  
  
Facoltativamente, è possibile disabilitare i controlli preventivi eseguiti il plug-in per impostazione predefinita. È possibile farlo in due modi:  
  
-   Se si utilizza il cmdlet PowerShell di aggiornamento compatibile con cluster, configurare il **DisableAclChecks\ = 'True'** argomento il **CauPluginArguments** parametro per l'aggiornamento rapido plug-in.  
  
-   Se si utilizza l'UI CAU, selezionare il **Disabilita controllo accesso amministratore nel file di configurazione e cartella radice degli aggiornamenti rapidi** opzione il **opzioni di aggiornamento aggiuntive** pagina della procedura guidata che viene utilizzata per configurare le opzioni di operazione di aggiornamento.  
  
Tuttavia, come procedura consigliata in molti ambienti, si consiglia di utilizzare la configurazione predefinita di applicare questi controlli.  
  
#### <a name="step-4-configure-settings-for-smb-data-integrity"></a>Passaggio 4. Configurare le impostazioni per l'integrità dei dati SMB
  
Per verificare l'integrità dei dati nelle connessioni tra i nodi del cluster e la condivisione file SMB, l'aggiornamento rapido plug-in richiede l'abilitazione delle impostazioni della condivisione file SMB per la firma SMB o la crittografia SMB. La crittografia SMB, che fornisce sicurezza avanzata e prestazioni migliorate in molti ambienti, è supportata a partire da Windows Server 2012. È possibile abilitare una o entrambe queste impostazioni, come indicato di seguito:  
  
-   Per abilitare la firma SMB, vedere la procedura nel [articolo 887429](http://support.microsoft.com/kb/887429) della Microsoft Knowledge Base.  
  
-   Per abilitare la crittografia SMB per la cartella SMB condivisa, eseguire il seguente cmdlet PowerShell nel server SMB:  
  
    ```PowerShell  
    Set-SmbShare <ShareName> -EncryptData $true  
    ```  
  
    Dove <*nomecondivisione*> è il nome della cartella SMB condivisa.  
  
Facoltativamente, per imporre l'uso della crittografia SMB nelle connessioni al server SMB, selezionare il **Richiedi crittografia SMB per l'accesso alla cartella radice degli aggiornamenti rapidi** nell'UI CAU oppure configurare il **RequireSMBEncryption\ = 'True'** argomento plug-in usando i cmdlet PowerShell di aggiornamento compatibile con cluster.  
  
> [!IMPORTANT]  
> Se si seleziona l'opzione per imporre l'uso della crittografia SMB e la cartella radice degli aggiornamenti rapidi non è configurato per le connessioni che usano la crittografia SMB, l'operazione di aggiornamento avrà esito negativo.  
  
#### <a name="step-5-enable-a-windows-firewall-rule-on-the-smb-server"></a>Passaggio 5. Abilitare una regola di Windows Firewall nel server SMB
  
È necessario abilitare il **gestione remota File Server \(SMB\-in\)** regola in Windows Firewall nel file server SMB. Questo è abilitato per impostazione predefinita in Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Panoramica di aggiornamento compatibile con cluster](cluster-aware-updating.md)
  
-   [Cmdlet di PowerShell Windows aggiornamento compatibile con cluster](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating)  
  
-   [Cluster di aggiornamento compatibile con riferimento plug-in](http://msdn.microsoft.com/library/hh418084.aspx)  
  
