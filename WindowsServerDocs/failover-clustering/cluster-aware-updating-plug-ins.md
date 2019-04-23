---
ms.assetid: d44d4430-41e2-483a-9685-92610cdef32a
title: Funzionamento dei plug-in di aggiornamento compatibile con Cluster
ms.topic: article
ms.prod: windows-server-threshold
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 04/28/2017
ms.technology: storage-failover-clustering
description: Come usare plug-in per coordinare gli aggiornamenti quando si usa aggiornamento compatibile con Cluster in Windows Server per installare gli aggiornamenti in un cluster.
ms.openlocfilehash: d09addb5e6787a8386d50570c0d27640646aa587
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854562"
---
# <a name="how-cluster-aware-updating-plug-ins-work"></a>Funzionamento dei plug-in di aggiornamento compatibile con Cluster

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

[Aggiornamento compatibile con cluster](cluster-aware-updating.md) (aggiornamento compatibile con cluster) Usa plug-in per coordinare l'installazione degli aggiornamenti tra nodi in un cluster di failover. In questo argomento fornisce informazioni sull'utilizzo predefiniti\-nel plug di aggiornamento compatibile con cluster\-aggiuntivi o altri plug\-in installabili per aggiornamento compatibile con cluster.

## <a name="BKMK_INSTALL"></a>Installare un plug\-in  
Un plug\-in diverso da quello predefinito collegare\-aggiuntivi che vengono installati con aggiornamento compatibile con cluster \( **Microsoft. windowsupdateplugin** e **Microsoft. hotfixplugin** \)deve essere installato separatamente. Se aggiornamento compatibile con cluster viene usato in self\-modalità, la spina di aggiornamento\-in deve essere installato in tutti i nodi del cluster. Se aggiornamento compatibile con cluster viene usato in remoto\-modalità, la spina di aggiornamento\-in deve essere installato nel computer coordinatore dell'aggiornamento remoto. Un plug\-in quanto possono avere requisiti di installazione aggiuntivi in ogni nodo.  
  
Per installare un plug\-, seguire le istruzioni della spina\-nel server di pubblicazione. Per registrare manualmente un plug\-con aggiornamento compatibile con cluster, eseguire il [Register-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) cmdlet in ogni computer in cui la spina\-in è installato.  
  
## <a name="specify-a-plug-in-and-plug-in-arguments"></a>Specificare un plug\-in e collegare\-negli argomenti  
  
### <a name="specify-a-cau-plug-in"></a>Specificare un plug di aggiornamento compatibile con cluster\-in

Nella UI CAU, si seleziona un plug\-in un elenco\-all'elenco di plug disponibili\-aggiuntivi quando si usa aggiornamento compatibile con cluster per eseguire le azioni seguenti:  
  
-   Applicare aggiornamenti al cluster  
  
-   Vedere un'anteprima degli aggiornamenti per il cluster  
  
-   Configurare cluster self\-opzioni di aggiornamento  
  
Per impostazione predefinita, aggiornamento compatibile con cluster seleziona il plug\-nelle **Microsoft. windowsupdateplugin**. Tuttavia, è possibile specificare qualsiasi plug\-in quanto è installato e registrato con aggiornamento compatibile con cluster.

> [!TIP]  
> Nella UI CAU, è possibile specificare solo un singolo plug\-in per aggiornamento compatibile con cluster da usare per visualizzare in anteprima o per applicare gli aggiornamenti durante un'operazione di aggiornamento. Usando i cmdlet di PowerShell di aggiornamento compatibile con cluster, è possibile specificare uno o più plug\-aggiuntivi. Se è necessario installare più tipi di aggiornamenti nel cluster, è in genere più efficiente specificare più plug\-aggiuntivi in un'unica operazione di aggiornamento, anziché utilizzare un separato operazione di aggiornamento per ogni plug\-in. Si verificheranno ad esempio meno riavvii di nodo.

Usando i cmdlet di PowerShell di aggiornamento compatibile con cluster elencati nella tabella seguente, è possibile specificare uno o più plug\-ins per un'operazione di aggiornamento o di analisi passando il **– CauPluginName** parametro. È possibile specificare più plug\-nei nomi, separandole con virgole. Se si specificano più plug\-i componenti aggiuntivi, è inoltre possibile controllare come la spina\-aggiuntivi influenzare reciproca durante un'operazione di aggiornamento specificando la  **\-RunPluginsSerially**,  **\- StopOnPluginFailure**, e **– SeparateReboots** parametri. Per altre informazioni sull'uso di più plug\-aggiuntivi, usare i collegamenti forniti alla documentazione del cmdlet nella tabella seguente.  
  
|Cmdlet|Descrizione|  
|----------|---------------|  
|[Add-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole)|Aggiunge il ruolo del cluster aggiornamento compatibile con cluster che fornisce il self\-l'aggiornamento di funzionalità al cluster specificato.|  
|[Invoke-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun)|Esegue un'analisi dei nodi del cluster per verificare l'applicabilità di aggiornamenti e li installa tramite un'operazione di aggiornamento sul cluster specificato.|  
|[Invoke-CauScan](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-causcan)|Esegue un'analisi dei nodi del cluster per verificare l'applicabilità di aggiornamenti e restituisce un elenco del set di aggiornamenti iniziale che verrebbe applicato a ogni nodo nel cluster specificato.|  
|[Set-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/set-cauclusterrole)|Imposta le proprietà di configurazione per il ruolo cluster di Aggiornamento compatibile con cluster sul cluster specificato.|  
  
Se non si specifica un plug di aggiornamento compatibile con cluster\-nel parametro usando questi cmdlet, il valore predefinito è la spina\-nelle **Microsoft. windowsupdateplugin**.  
  
### <a name="specify-cau-plug-in-arguments"></a>Specifica aggiornamento compatibile con cluster collegare\-negli argomenti  
Quando si configurano le opzioni, è possibile specificare uno o più *name\=valore* coppie \(argomenti\) per la spina selezionata\-in da utilizzare. Nell'interfaccia utente di Aggiornamento compatibile con cluster, ad esempio, è possibile specificare più argomenti come indicato di seguito:  
  
**Name1\=Value1;Name2\=Value2;Name3\=Value3**  
  
Questi *name\=valore* coppie devono essere significative per la spina\-in quanto si specifica. Per alcuni plug\-aggiuntivi gli argomenti sono facoltativi.  
  
La sintassi del plug-di aggiornamento compatibile con cluster\-negli argomenti segue queste regole generali:  
  
-   Più *name\=valore* coppie sono separate da punti e virgola.  
  
-   I valori che contengono spazi vengono racchiusi tra virgolette, ad esempio: **Nome1\="Valore contenente spazi"**.  
  
-   La sintassi esatta della *valore* dipende la spina\-in.  
  
Per specificare plug\-negli argomenti usando i cmdlet di PowerShell di aggiornamento compatibile con cluster che supportano il **– CauPluginParameters** parametro, passare un parametro nel formato:  
  
**\-CauPluginArguments @{Name1\=Value1; Nome2\=Value2; Name3\=Value3}**  
  
È anche possibile usare una tabella hash di PowerShell predefinita. Per specificare plug\-negli argomenti di più di un plug\-, passare più tabelle hash di argomenti, separati da virgole. Passare la spina\-negli argomenti nella spina\-in modo che sia specificato **CauPluginName**.  
  
### <a name="specify-optional-plug-in-arguments"></a>Specificare plug facoltativo\-negli argomenti  
La spina\-aggiuntivi che aggiornamento compatibile con cluster consente di installare \( **Microsoft. windowsupdateplugin** e **Microsoft. hotfixplugin** \) forniscono opzioni aggiuntive che è possibile selezionare. Nella UI CAU, questi elementi vengono visualizzati in un **opzioni aggiuntive** pagina dopo aver configurato le opzioni di aggiornamento per il plug\-in. Se si usa il cmdlet di PowerShell di aggiornamento compatibile con cluster, queste opzioni sono configurate come facoltativi plug\-negli argomenti. Per altre informazioni, vedere [Usare Microsoft.WindowsUpdatePlugin](#BKMK_WUP) e [Usare Microsoft.HotfixPlugin](#BKMK_HFP) , più avanti in questo argomento.  
  
## <a name="manage-plug-ins-using-windows-powershell-cmdlets"></a>Gestire plug\-tramite i cmdlet di Windows PowerShell  
  
|Cmdlet|Descrizione|  
|----------|---------------|  
|[Get-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/get-cauplugin)|Recupera le informazioni sull'aggiornamento plug uno o più software\-aggiuntivi che vengono registrati nel computer locale.|  
|[Register-CauPlugin]((https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin))|Registra un plug di aggiornamento software di aggiornamento compatibile con cluster\-nel computer locale.|  
|[Unregister-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/unregister-cauplugin)|Rimuove un plug di aggiornamento software\-dall'elenco dei plug\-aggiuntivi che possono essere utilizzato da aggiornamento compatibile con cluster. **Nota:** La spina\-aggiuntivi che vengono installati con aggiornamento compatibile con cluster \( **Microsoft. windowsupdateplugin** e il **Microsoft. hotfixplugin** \) non può essere annullata.|  
  
## <a name="BKMK_WUP"></a>Uso di Microsoft. windowsupdateplugin  

La spina predefinito\-in per aggiornamento compatibile con cluster, **Microsoft. windowsupdateplugin**, esegue le azioni seguenti:
- Comunica con l'agente di Windows Update su ciascun nodo del cluster di failover per applicare gli aggiornamenti necessari per i prodotti Microsoft in esecuzione su ciascun nodo.
- Le installazioni del cluster direttamente gli aggiornamenti da Windows Update o Microsoft Update o da sul\-locale Windows Server Update Services \(WSUS\) server.
- Installa solo selezionati, versione generali di distribuzione \(GDR\) gli aggiornamenti. Per impostazione predefinita, la spina\-in applica solo agli aggiornamenti software importanti. Nessuna configurazione richiesta. La configurazione predefinita consente di scaricare e installare gli aggiornamenti GDR importanti. 

> [!NOTE]
> Per applicare gli aggiornamenti diverso dagli aggiornamenti software importanti selezionati per impostazione predefinita \(ad esempio, Aggiorna driver\), è possibile configurare un plug facoltativo\-nel parametro. Per altre informazioni, vedere [Configurare la stringa di query per l'agente di Windows Update](#BKMK_QUERY).

### <a name="requirements"></a>Requisiti

- Il cluster di failover e il computer coordinatore dell'aggiornamento \(se utilizzata\) deve soddisfare i requisiti per aggiornamento compatibile con cluster e la configurazione necessaria per la gestione remota, elencata in [requisiti e procedure consigliate per aggiornamento compatibile con cluster ](cluster-aware-updating-requirements.md).
- Consultare [Raccomandazioni per l'applicazione di aggiornamenti Microsoft](cluster-aware-updating-requirements.md#BKMK_BP_WUA), quindi apportare le eventuali modifiche necessarie alla configurazione di Microsoft Update per i nodi del cluster di failover.
- Per ottenere risultati ottimali, è consigliabile eseguire l'aggiornamento compatibile con cluster Best Practices Analyzer \(Best Practices Analyzer\) per assicurarsi che l'ambiente cluster e di aggiornamento siano configurati correttamente per applicare gli aggiornamenti tramite aggiornamento compatibile con cluster. Per altre informazioni, vedere [Test di disponibilità per l'aggiornamento con Aggiornamento compatibile con cluster](cluster-aware-updating-requirements.md#BKMK_BPA).

> [!NOTE]
> Gli aggiornamenti che richiedono l'accettazione delle condizioni di licenza Microsoft o l'interazione dell'utente sono esclusi e devono essere installati manualmente.

### <a name="additional-options"></a>Opzioni aggiuntive

Facoltativamente, è possibile specificare i seguenti plug\-negli argomenti per espandere o limitare il set di aggiornamenti applicati dalla spina\-in:
- Per configurare il plug\-per applicare gli aggiornamenti consigliati oltre a quelli importanti in ogni nodo, la UI CAU, il **opzioni aggiuntive** pagina, selezionare il **assegnare me consigliato Aggiorna lo stesso modo in cui Aggiornamenti importanti** casella di controllo.
<br>In alternativa, configurare il **'IncludeRecommendedUpdates'\='True'** collegare\-nell'argomento.
- Per configurare il plug\-in per filtrare i tipi di aggiornamenti GDR che vengono applicati a ogni nodo del cluster, specificare una stringa di query dell'agente di Windows Update usando un **QueryString** collegare\-nell'argomento. Per altre informazioni, vedere [Configurare la stringa di query per l'agente di Windows Update](#BKMK_QUERY).

### <a name="BKMK_QUERY"></a>Configurare la stringa di query dell'agente di Windows Update  
È possibile configurare un plug\-nell'argomento per il valore predefinito collegare\-, in **Microsoft. windowsupdateplugin**, che è costituito da un agente di Windows Update \(WUA\) stringa di query. Questa istruzione usa l'API agente di Windows Update (WUA) per identificare uno o più gruppi di aggiornamenti Microsoft da applicare a ogni nodo, sulla base di specifici criteri di selezione. È possibile combinare più criteri usando operatori AND od OR logici. La stringa di query WUA viene specificata in un plug\-nell'argomento come indicato di seguito:  
  
**QueryString\="Criterion1\=Value1 e\/o Criterion2\=valore2 e\/or..."**  
  
**Microsoft.WindowsUpdatePlugin**, ad esempio, seleziona automaticamente gli aggiornamenti importanti usando un argomento **QueryString** predefinito costruito usando i criteri **IsInstalled**, **Type**, **IsHidden** e **IsAssigned**:  
  
**QueryString\="IsInstalled\=0 e il tipo\="Software"e IsHidden\=0 e IsAssigned\=1"**  
  
Se si specifica un **QueryString** argomento, viene usato al posto del valore predefinito **QueryString** configurato per il plug\-in.  
  
#### <a name="example-1"></a>Esempio 1
  
Per configurare un **QueryString** argomento che installa un aggiornamento specifico identificato dall'ID *f6ce46c1\-971c\-43f9\-a2aa\-783df125f003*:  
  
**QueryString\="UpdateID\='f6ce46c1\-971c\-43f9\-a2aa\-783df125f003' and IsInstalled\=0"**  
  
> [!NOTE]  
> L'esempio precedente è valido per applicare gli aggiornamenti usando il Cluster\-aggiornamento guidato compatibile con. Se si desidera installare un aggiornamento specifico configurando self\-aggiornamento delle opzioni con l'UI CAU o usando il **Add\-CauClusterRole** o **impostare\-CauClusterRole**Cmdlet di PowerShell, è necessario formattare il valore UpdateID con solo due\-virgolette:  
>   
> **QueryString\="UpdateID\=''f6ce46c1\-971c\-43f9\-a2aa\-783df125f003'' and IsInstalled\=0"**  
  
#### <a name="example-2"></a>Esempio 2
  
Per configurare un argomento **QueryString** che installa solo driver:  
  
**QueryString\="IsInstalled\=0 e il tipo\='Driver' e IsHidden\=0"**  
  
Per altre informazioni sulle stringhe di query per il valore predefinito collegare\-, in **Microsoft. windowsupdateplugin**, i criteri di ricerca \(, ad esempio **IsInstalled**\), e la sintassi che è possibile includere nelle stringhe di query, vedere la sezione relativa ai criteri di ricerca nel [riferimento all'API di Windows Update Agent (WUA)](https://go.microsoft.com/fwlink/p/?LinkId=223304).  
  
## <a name="BKMK_HFP"></a>Usare Microsoft. hotfixplugin  
La spina\-in **Microsoft. hotfixplugin** può essere usato per applicare Microsoft a distribuzione release \(LDR\) aggiornamenti \(chiamati anche aggiornamenti rapidi e in precedenza noti come QFE\) che vanno scaricati indipendentemente per risolvere problemi specifici del software Microsoft. Il plug-in installa gli aggiornamenti da una cartella radice in una condivisione file SMB e può essere personalizzato anche per applicare non\-Microsoft driver, firmware e aggiornamenti del BIOS.

> [!NOTE]
> Gli aggiornamenti rapidi sono talvolta resi disponibili per il download di Microsoft negli articoli della Knowledge Base, ma vengono anche forniti ai clienti in un oggetto come\-necessità.

### <a name="requirements"></a>Requisiti

- Il cluster di failover e il computer coordinatore dell'aggiornamento \(se utilizzata\) deve soddisfare i requisiti per aggiornamento compatibile con cluster e la configurazione necessaria per la gestione remota, elencata in [requisiti e procedure consigliate per aggiornamento compatibile con cluster ](cluster-aware-updating-requirements.md).
- Consultare [Consigli per l'uso di Microsoft.HotfixPlugin](cluster-aware-updating-requirements.md#BKMK_BP_HF).
- Per ottenere risultati ottimali, è consigliabile eseguire l'aggiornamento compatibile con cluster Best Practices Analyzer \(Best Practices Analyzer\) modello per assicurare che l'ambiente cluster e di aggiornamento siano configurati correttamente per applicare gli aggiornamenti tramite aggiornamento compatibile con cluster. Per altre informazioni, vedere [Test di disponibilità per l'aggiornamento con Aggiornamento compatibile con cluster](cluster-aware-updating-requirements.md#BKMK_BPA).
- Ottenere gli aggiornamenti dal server di pubblicazione e copiarli o estrarli in un Server Message Block \(SMB\) condivisione file \(cartella radice degli aggiornamenti rapidi\) che supporti almeno SMB 2.0 e che sia accessibile da tutti i cluster i nodi e il computer coordinatore dell'aggiornamento \(se aggiornamento compatibile con cluster viene usato in remoto\-modalità di aggiornamento\). Per altre informazioni, vedere [Configurare una struttura di cartella radice dell'aggiornamento rapido](#BKMK_HF_ROOT) più avanti in questo argomento. 

    > [!NOTE]
    > Per impostazione predefinita, questo plug-\-in installa solo aggiornamenti rapidi con le estensioni di file seguenti: msu,. msi e msp.

- Copiare il file defaulthotfixconfig. XML \(in cui viene fornito il **% systemroot %\\System32\\WindowsPowerShell\\v1.0\\moduli\\ ClusterAwareUpdating** cartella su un computer in cui sono installati gli strumenti di aggiornamento compatibile con cluster\) nella cartella radice degli aggiornamenti rapidi che è stato creato e in cui sono stati estratti gli aggiornamenti rapidi. Ad esempio, copiare il file di configurazione  *\\ \\MyFileServer\\hotfix\\radice\\*. 

    > [!NOTE]
    > Per installare la maggior parte degli aggiornamenti rapidi forniti da Microsoft e altri aggiornamenti, è possibile usare il file di configurazione per gli aggiornamenti rapidi predefinito senza modifiche. Se richiesto per lo scenario in uso, è possibile personalizzare il file di configurazione come attività avanzata. Il file di configurazione può includere, ad esempio, ruoli personalizzati per la gestione di file di aggiornamento rapido con estensioni specifiche oppure per definire i comportamenti per determinate condizioni di uscita. Per altre informazioni, vedere [Personalizzare il file di configurazione degli aggiornamenti rapidi](#BKMK_CONFIG_FILE) più avanti in questo argomento.

### <a name="configuration"></a>Configurazione

Configurare le opzioni descritte di seguito. Per altre informazioni, vedere i collegamenti alle sezioni più avanti in questo argomento.
- Il percorso alla cartella radice degli aggiornamenti rapidi condivisa contenente gli aggiornamenti da applicare nonché il file di configurazione degli aggiornamenti rapidi. È possibile digitare questo percorso nell'UI CAU o configurare il **HotfixRootFolderPath\=\<Path >** PowerShell plug\-nell'argomento. 

   > [!NOTE]
   > È possibile specificare la cartella radice degli aggiornamenti rapidi come percorso di cartella locale o come percorso UNC nel formato  *\\ \\ServerName\\condivisione\\RootFolderName*. Un dominio\-su o percorso Namespace DFS autonomo può essere usato. Tuttavia, la spina\-nella funzionalità che consentono di controllare l'accesso le autorizzazioni nel file di configurazione degli aggiornamenti rapidi sono compatibili con percorsi DFS Namespace, pertanto se si configura uno, è necessario disabilitare il controllo delle autorizzazioni di accesso utilizzando la UI CAU o configurando il **DisableAclChecks\='True'** collegare\-nell'argomento.
- Le impostazioni nel server che ospita la cartella radice degli aggiornamenti rapidi per verificare la presenza delle autorizzazioni appropriate per accedere alla cartella e garantire l'integrità dei dati accessibili da SMB una cartella condivisa \(firma SMB o della crittografia SMB\). Per altre informazioni, vedere [Limitare l'accesso alla cartella radice degli aggiornamenti rapidi](#BKMK_ACL).

### <a name="additional-options"></a>Opzioni aggiuntive

- Facoltativamente, configurare la spina\-in modo che la crittografia SMB viene applicata quando l'accesso ai dati dalla condivisione file degli aggiornamenti rapidi. Nella UI CAU, sul **opzioni aggiuntive** pagina, selezionare la **Richiedi crittografia SMB per l'accesso alla cartella radice degli aggiornamenti rapidi** oppure configurare il **RequireSMBEncryption\= 'True'** PowerShell plug\-nell'argomento. 
  > [!IMPORTANT]
  > È necessario eseguire passaggi di configurazione aggiuntivi nel server SMB per abilitare l'integrità dei dati SMB con firma o crittografia SMB. Per altre informazioni, vedere il passaggio 4 in [Limitare l'accesso alla cartella radice degli aggiornamenti rapidi](#BKMK_ACL). Se si seleziona l'opzione per imporre l'uso della crittografia SMB e la cartella radice degli aggiornamenti rapidi non è configurata per l'accesso mediante la crittografia SMB, l'operazione di aggiornamento non riuscirà.
- Facoltativamente, disabilitare i controlli predefiniti sulla presenza delle autorizzazioni necessarie per la cartella radice e per il file di configurazione degli aggiornamenti rapidi. Nella UI CAU, selezionare **Disabilita controllo accesso amministratore per il file di configurazione e di cartella radice degli aggiornamenti rapidi**, oppure configurare il **DisableAclChecks\='True'** collegare\-in discussione.
- Facoltativamente, configurare il **HotfixInstallerTimeoutMinutes\= <Integer>**  argomento per specificare la durata dell'aggiornamento rapido collegare\-in attesa per il processo di installazione dell'hotfix da restituire. \(Il valore predefinito è 30 minuti.\) Ad esempio, per specificare un periodo di timeout di due ore, impostare **HotfixInstallerTimeoutMinutes\=120**.
- Facoltativamente, configurare il **HotfixConfigFileName \= <name>**  collegare\-nell'argomento per specificare un nome per il file di configurazione che si trova nella cartella radice degli aggiornamenti rapidi. Se non ne viene specificato uno, verrà usato il nome predefinito DefaultHotfixConfig.xml.
  
### <a name="BKMK_HF_ROOT"></a>Configurare una struttura di cartella radice degli aggiornamenti rapidi

Per l'aggiornamento rapido collegare\-in funzioni, gli aggiornamenti rapidi devono essere archiviati in corretto\-definito struttura in una condivisione file SMB \(cartella radice degli aggiornamenti rapidi\), ed è necessario configurare la spina di hotfix\-con il percorso il cartella radice degli aggiornamenti rapidi tramite l'UI CAU o i cmdlet di PowerShell di aggiornamento compatibile con cluster. Questo percorso viene passato per il plug\-in come i **HotfixRootFolderPath** argomento. In base alle esigenze di aggiornamento, è possibile scegliere tra diverse strutture per la cartella radice degli aggiornamenti rapidi, come illustrato negli esempi seguenti. File o cartelle non conformi a questa struttura verranno ignorati.  
  
#### <a name="example-1---folder-structure-used-to-apply-hotfixes-to-all-cluster-nodes"></a>Esempio 1: struttura di cartelle usata per applicare gli aggiornamenti rapidi a tutti i nodi del cluster
  
Per specificare che gli aggiornamenti rapidi si applicano a tutti i nodi del cluster, copiarli in una cartella denominata **CAUHotfix\_tutto** sotto la cartella radice degli aggiornamenti rapidi. In questo esempio, il **HotfixRootFolderPath** collegare\-nell'argomento è impostato su *\\ \\MyFileServer\\hotfix\\radice\\*. Il **CAUHotfix\_tutto** cartella contiene tre aggiornamenti con le estensioni msu,. msi e msp che verranno applicate a tutti i nodi del cluster. I nomi file degli aggiornamenti qui riportati hanno solo scopo illustrativo.  
  
> [!NOTE]  
> In questo esempio e in quelli seguenti il file di configurazione degli aggiornamenti rapidi con il nome predefinito DefaultHotfixConfig.xml è visualizzato nella posizione richiesta nella cartella radice degli aggiornamenti rapidi.  
  
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
  
Per specificare aggiornamenti rapidi applicabili solo a un nodo specifico, usare una sottocartella della cartella radice degli aggiornamenti rapidi con il nome del nodo. Usare il nome NetBIOS del nodo del cluster, ad esempio *ContosoNode1*. Spostare quindi gli aggiornamenti applicabili solo al nodo in questione nella sottocartella. Nell'esempio seguente, il **HotfixRootFolderPath** collegare\-nell'argomento è impostato su  *\\ \\MyFileServer\\hotfix\\radice\\*. Gli aggiornamenti nella **CAUHotfix\_tutte** cartella verrà applicata a tutti i nodi del cluster, e *Node1\_specifici\_Update.msu* verranno applicate solo a *ContosoNode1*.  
  
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
  
#### <a name="example-3---folder-structure-used-to-apply-updates-other-than-msu-msi-and-msp-files"></a>Esempio 3: struttura di cartelle usata per applicare gli aggiornamenti ad eccezione dei file. msu,. msi e msp
  
Per impostazione predefinita, **Microsoft.HotfixPlugin** applica unicamente aggiornamenti con le estensioni .msu, .msi o .msp. Alcuni aggiornamenti potrebbero tuttavia avere estensioni diverse e potrebbero richiedere comandi di installazione diversi. Ad esempio, potrebbe essere necessario applicare un aggiornamento firmware con estensione .exe a un nodo in un cluster. È possibile configurare la cartella radice degli aggiornamenti rapidi con una sottocartella che indichi un oggetto specifico non\-tipo di aggiornamento predefinito deve essere installato. È inoltre necessario configurare una regola di installazione della cartella corrispondente che specifica il comando di installazione nell'elemento `<FolderRules>` nel file XML di configurazione degli aggiornamenti rapidi.  
  
Nell'esempio seguente, il **HotfixRootFolderPath** collegare\-nell'argomento è impostato su  *\\ \\MyFileServer\\hotfix\\radice\\*. Molti aggiornamenti verranno applicati a tutti i nodi del cluster, mentre l'aggiornamento firmware *SpecialHotfix1.exe* verrà applicato a *ContosoNode1* usando *FolderRule1*. Per informazioni sulla configurazione di *FolderRule1* nel file di configurazione degli aggiornamenti rapidi, vedere [Personalizzare il file di configurazione degli aggiornamenti rapidi](#BKMK_CONFIG_FILE) più avanti in questo argomento.  
  
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
Il file di configurazione degli aggiornamenti rapidi controlla il modo in cui **Microsoft.HotfixPlugin** installa tipi di file degli aggiornamenti rapidi specifici in un cluster di failover. Lo schema XML per il file di configurazione è definito in HotfixConfigSchema.xsd, che si trova nella cartella seguente in un computer in cui sono installati gli strumenti di Aggiornamento compatibile con cluster:  
  
**%systemroot%\\System32\\WindowsPowerShell\\v1.0\\Modules\\ClusterAwareUpdating folder**  
  
Per personalizzare il file di configurazione degli aggiornamenti rapidi, copiare il file di configurazione di esempio DefaultHotfixConfig.xml da questo percorso alla cartella radice degli aggiornamenti rapidi e apportare le modifiche appropriate per il proprio scenario.  
  
> [!IMPORTANT]  
> Per applicare la maggior parte degli aggiornamenti rapidi forniti da Microsoft e altri aggiornamenti, è possibile usare il file di configurazione per gli aggiornamenti rapidi predefinito senza modifiche. La personalizzazione del file di configurazione degli aggiornamenti rapidi è un'attività destinata solo a scenari di utilizzo avanzato.  
  
Per impostazione predefinita, il file XML di configurazione degli aggiornamenti rapidi definisce regole di installazione e condizioni di uscita per le due categorie di aggiornamenti rapidi seguenti:  
  
-   I file dell'hotfix con estensioni che la spina\-in installabili per impostazione predefinita \(file. msu,. msi e msp\).  
  
    Questi vengono definiti elementi `<ExtensionRules>` nell'elemento `<DefaultRules>`. È presente un elemento `<Extension>` per ognuno dei tipi di file predefiniti supportati. La struttura generale XML è la seguente:  
  
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
  
    Se è necessario applicare determinati tipi di aggiornamenti a tutti i nodi del cluster nell'ambiente, è possibile definire elementi `<Extension>` aggiuntivi.  
  
-   Aggiornamento rapido o un altro aggiornamento file che non sono file con estensione msi, msu o file con estensione msp, ad esempio, non\-Microsoft driver, firmware e aggiornamenti del BIOS.  
  
    Ogni non\-tipo di file predefinito è configurato come un `<Folder>` elemento il `<FolderRules>` elemento. L'attributo del nome dell'elemento `<Folder>` deve essere identico al nome di una cartella all'interno della cartella degli aggiornamenti rapidi che conterrà gli aggiornamenti del tipo corrispondente. La cartella può trovarsi nel **CAUHotfix\_tutto** cartella o in un nodo\-cartella specifica. Se *FolderRule1* , ad esempio, è configurato nella cartella radice degli aggiornamenti rapidi, configurare l'elemento seguente nel file XML per definire un modello di installazione e delle condizioni di uscita per gli aggiornamenti in quella cartella:  
  
    ```xml  
    <FolderRules>  
          <Folder name="FolderRule1">  
            <!-- Template and ExitConditions elements for installation of updates in FolderRule1 follow -->  
             ...  
          </Folder>  
          ...  
    </FolderRules>  
    ```  
  
Nelle tabelle seguenti sono descritti gli attributi `<Template>` e i possibili sottoelementi `<ExitConditions>` .  
  
|Attributo `<Template>`|Descrizione|  
|--------------------------|---------------|  
|`path`|Percorso completo del programma di installazione per il tipo di file definito nell'attributo `<Extension name>` .<br /><br />Per specificare il percorso a un file di aggiornamento nella struttura della cartella radice degli aggiornamenti rapidi, usare `$update$`.|  
|`parameters`|Stringa di parametri obbligatori o facoltativi per il programma specificato in `path`.<br /><br />Per specificare un parametro corrispondente al percorso a un file di aggiornamento nella struttura della cartella radice degli aggiornamenti rapidi, usare `$update$`.|  
  
|Sottoelemento `<ExitConditions>`|Descrizione|  
|---------------------------------|---------------|  
|`<Success>`|Definisce uno o più codici di uscita che indicano che l'aggiornamento specificato è riuscito. Si tratta di un sottoelemento obbligatorio.|  
|`<Success_RebootRequired>`|Definisce facoltativamente uno o più codici di uscita che indicano che l'aggiornamento specificato è riuscito e che il nodo deve essere riavviato. <br>**Nota:** Facoltativamente, l'elemento `<Folder>` può contenere l'attributo `alwaysReboot`. Se l'attributo è impostato, indica che se un aggiornamento rapido installato restituisce uno dei codici di uscita definiti in `<Success>`, ciò verrà interpretato come condizione di uscita `<Success_RebootRequired>` .|  
|`<Fail_RebootRequired>`|Definisce facoltativamente uno o più codici di uscita che indicano che l'aggiornamento specificato non è riuscito e che il nodo deve essere riavviato.|  
|`<AlreadyInstalled>`|Definisce facoltativamente uno o più codici di uscita che indicano che l'aggiornamento specificato non è stato applicato perché già installato.|  
|`<NotApplicable>`|Definisce facoltativamente uno o più codici di uscita che indicano che l'aggiornamento specificato non è stato applicato perché non applicabile al nodo del cluster.|  
  
> [!IMPORTANT]  
> Qualsiasi codice di uscita non esplicitamente definito in `<ExitConditions>` viene interpretato come aggiornamento non riuscito e il nodo non viene riavviato.  
  
### <a name="BKMK_ACL"></a>Limitare l'accesso alla cartella radice degli aggiornamenti rapidi  
Per configurare il file server e la condivisione file SMB per garantire accesso alla cartella radice e al file di configurazione degli aggiornamenti rapidi esclusivamente nel contesto di **Microsoft.HotfixPlugin**, è necessario eseguire diversi passaggi. Tali passaggi consentono di abilitare varie funzionalità che aiutano a prevenire possibili manomissioni dei file di aggiornamento rapido che potrebbero compromettere il cluster di failover.  
  
In generale, i passaggi sono i seguenti:  
  
1.  Identificare l'account utente utilizzato per operazioni di aggiornamento utilizzando la spina\-in  
  
2.  Configurare l'account utente individuato nei gruppi necessari in un file server SMB  
  
3.  Configurare le autorizzazioni per accedere alla cartella radice degli aggiornamenti rapidi  
  
4.  Configurare le impostazioni per l'integrità dei dati SMB  
  
5.  Abilitare una regola di Windows Firewall nel server SMB  
  
#### <a name="step-1-identify-the-user-account-that-is-used-for-updating-runs-by-using-the-hotfix-plug-in"></a>Passaggio 1. Identificare l'account utente utilizzato per operazioni di aggiornamento utilizzando la spina di hotfix\-in
  
L'account che viene usato in aggiornamento compatibile con cluster per verificare le impostazioni di sicurezza durante l'esecuzione di un'operazione di aggiornamento usando **Microsoft. hotfixplugin** dipende dal fatto che aggiornamento compatibile con cluster viene usato in remoto\-aggiornamento modalità o self\-, modalità di aggiornamento come di seguito:  
  
-   **Remote\-modalità di aggiornamento** l'account con privilegi di amministratore sul cluster per visualizzare in anteprima e applica gli aggiornamenti.  
  
-   **Self\-modalità di aggiornamento** ruoli in cluster il nome dell'oggetto computer virtuale configurato in Active Directory per l'aggiornamento compatibile con cluster. Si tratta del nome di un oggetto computer virtuale preinstallato in Active Directory per il ruolo del cluster di Aggiornamento compatibile con cluster o del nome generato da quest'ultimo per il ruolo del cluster. Per ottenere il nome se viene generato da aggiornamento compatibile con cluster, eseguire la **ottenere\-CauClusterRole** cmdlet di PowerShell di aggiornamento compatibile con cluster. Nell'output **ResourceGroupName** è il nome dell'account dell'oggetto computer virtuale generato.  
  
#### <a name="step-2-configure-this-user-account-in-the-necessary-groups-on-an-smb-file-server"></a>Passaggio 2. Configurare l'account utente individuato nei gruppi necessari in un file server SMB
  
> [!IMPORTANT]  
> È necessario aggiungere l'account usato per le operazioni di aggiornamento come account Administrator locale nel server SMB. Se ciò non viene consentito a causa dei criteri di sicurezza dell'organizzazione, configurare questo account con le autorizzazioni necessarie sul server SMB usando la procedura riportata di seguito.  
  
##### <a name="to-configure-a-user-account-on-the-smb-server"></a>Per configurare un account utente sul server SMB  
  
1.  Aggiungere l'account usato per le operazioni di aggiornamento al gruppo Distributed COM Users e a uno dei gruppi seguenti: Power User, Server operator o Print operator.  
  
2.  Per abilitare le autorizzazioni WMI necessarie per l'account, avviare la console di gestione WMI nel server SMB. Avviare PowerShell e quindi digitare il comando seguente:  
  
    ```  
    wmimgmt.msc  
    ```  
  
3.  Nell'albero della console, pulsante destro del mouse\-fare clic su **controllo WMI \(locale\)**, quindi fare clic su **proprietà**.  
  
4.  Fare clic su **Sicurezza**, quindi espandere **Radice**.  
  
5.  Fare clic su **CIMV2**, quindi su **Sicurezza**.  
  
6.  Aggiungere l'account usato per le operazioni di aggiornamento all'elenco **Utenti e gruppi**.  
  
7.  Concedere le autorizzazioni **Esegui metodi** e **Abilita remoto** all'account usato per le operazioni di aggiornamento.  
  
#### <a name="step-3-configure-permissions-to-access-the-hotfix-root-folder"></a>Passaggio 3. Configurare le autorizzazioni per accedere alla cartella radice degli aggiornamenti rapidi
  
Per impostazione predefinita, quando si tenta di applicare gli aggiornamenti, l'aggiornamento rapido collegare\-in controlla la configurazione di autorizzazioni di file system NTFS per l'accesso alla cartella radice degli aggiornamenti rapidi. Se le autorizzazioni di accesso della cartella non sono configurate correttamente, un'operazione di aggiornamento utilizzando la spina di hotfix\-in potrebbe avere esito negativo.  
  
Se si usa la configurazione predefinita del plug-di hotfix\-, assicurarsi che le autorizzazioni di accesso cartella soddisfino i requisiti seguenti.  
  
-   Il gruppo Users deve disporre delle autorizzazioni di lettura.  
  
-   Se la spina\-in applicherà gli aggiornamenti con estensione .exe, il gruppo di utenti dispone dell'autorizzazione Execute.  
  
-   Sono consentite solo determinate entità di sicurezza \(ma non sono necessarie\) di scrittura e di modificare le autorizzazioni. Le entità di sicurezza ammesse sono il gruppo Administrators locale, SYSTEM, CREATOR OWNER e TrustedInstaller. Nessun altro account o gruppo può disporre di autorizzazioni di scrittura o modifica per la cartella radice degli aggiornamenti rapidi.  
  
Facoltativamente, è possibile disabilitare i controlli preventivi che la spina\-in esegue per impostazione predefinita. Puoi eseguire questa operazione in due modi:  
  
-   Se si usa il cmdlet di PowerShell di aggiornamento compatibile con cluster, configurare il **DisableAclChecks\='True'** argomento nel **CauPluginArguments** parametro per la spina di hotfix\-in.  
  
-   Se si sta usando l'interfaccia utente di Aggiornamento compatibile con cluster, selezionare l'opzione **Disabilita controllo accesso amministratore alla cartella radice degli aggiornamenti rapidi e al file di configurazione** nella pagina **Opzioni di aggiornamento aggiuntive** della procedura guidata per la configurazione delle opzioni relative alle operazioni di aggiornamento.  
  
Come procedura consigliata per molti ambienti si raccomanda tuttavia l'impiego della configurazione predefinita che prevede l'imposizione di tali controlli.  
  
#### <a name="step-4-configure-settings-for-smb-data-integrity"></a>Passaggio 4. Configurare le impostazioni per l'integrità dei dati SMB
  
Per verificare l'integrità dei dati nelle connessioni tra i nodi del cluster e la condivisione file SMB, collegare l'hotfix\-in è necessario abilitare le impostazioni della condivisione file SMB per la firma o crittografia SMB. Crittografia SMB, che fornisce sicurezza avanzata e prestazioni migliorate in molti ambienti, è supportata a partire da Windows Server 2012. È possibile abilitare una di queste impostazioni, o entrambe, come indicato di seguito:  
  
-   Per abilitare la firma SMB, vedere la procedura nell' [articolo 887429](https://support.microsoft.com/kb/887429) della Microsoft Knowledge Base.  
  
-   Per abilitare la crittografia SMB per la cartella SMB condivisa, eseguire il cmdlet di PowerShell seguente nel server SMB:  
  
    ```PowerShell  
    Set-SmbShare <ShareName> -EncryptData $true  
    ```  
  
    Dove <*ShareName*> è il nome della cartella SMB condivisa.  
  
Facoltativamente, per imporre l'uso della crittografia SMB nelle connessioni al server SMB, selezionare la **Richiedi crittografia SMB per l'accesso alla cartella radice degli aggiornamenti rapidi** nell'UI CAU oppure configurare il **RequireSMBEncryption \='True'** collegare\-nell'argomento usando i cmdlet di PowerShell di aggiornamento compatibile con cluster.  
  
> [!IMPORTANT]  
> Se si seleziona l'opzione per imporre l'uso della crittografia SMB e la cartella radice degli aggiornamenti rapidi non è configurata per connessioni che usano la crittografia SMB, l'operazione di aggiornamento non riuscirà.  
  
#### <a name="step-5-enable-a-windows-firewall-rule-on-the-smb-server"></a>Passaggio 5. Abilitare una regola di Windows Firewall nel server SMB
  
È necessario abilitare la **gestione remota File Server \(SMB\-nelle\)**  regola Windows Firewall sul file server SMB. Questa è abilitata per impostazione predefinita in Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Panoramica di aggiornamento compatibile con cluster](cluster-aware-updating.md)
  
-   [Cmdlet di PowerShell Windows ad aggiornamento compatibile con cluster](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating)  
  
-   [Informazioni di riferimento del plug-in aggiornamento compatibile con il cluster](https://msdn.microsoft.com/library/hh418084.aspx)  
  
