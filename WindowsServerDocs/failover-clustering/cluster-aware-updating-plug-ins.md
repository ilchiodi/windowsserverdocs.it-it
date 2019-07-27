---
ms.assetid: d44d4430-41e2-483a-9685-92610cdef32a
title: Funzionamento del plug-in di aggiornamento compatibile con cluster
ms.topic: article
ms.prod: windows-server-threshold
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 04/28/2017
ms.technology: storage-failover-clustering
description: Come usare i plug-in per coordinare gli aggiornamenti quando si usa l'aggiornamento compatibile con cluster in Windows Server per installare gli aggiornamenti in un cluster.
ms.openlocfilehash: bd31a6056376b04fcb5a4a941b81a363548a2209
ms.sourcegitcommit: 6f968368c12b9dd699c197afb3a3d13c2211f85b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/26/2019
ms.locfileid: "68544506"
---
# <a name="how-cluster-aware-updating-plug-ins-work"></a>Funzionamento del plug-in di aggiornamento compatibile con cluster

>Si applica a Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

[Aggiornamento compatibile con cluster](cluster-aware-updating.md) (Aggiornamento compatibile con cluster) USA i plug-in per coordinare l'installazione degli aggiornamenti nei nodi di un cluster di failover. Questo argomento fornisce informazioni sull'uso dei plug\-\--in di aggiornamento compatibile con cluster\-predefiniti o su altri plug-in installati per aggiornamento compatibile con cluster.

## <a name="BKMK_INSTALL"></a>Installare un plug\--in  
Un plug\--in diverso da quello\-predefinito installato con aggiornamento compatibile \(con cluster **Microsoft. WindowsUpdatePlugin** e **Microsoft. HotfixPlugin** \) deve essere installato separatamente. Se aggiornamento compatibile con cluster viene\-usato in modalità di aggiornamento\-automatico, il plug-in deve essere installato in tutti i nodi del cluster. Se aggiornamento compatibile con cluster viene\-usato in modalità di aggiornamento\-remoto, il plug-in deve essere installato nel computer coordinatore dell'aggiornamento remoto. Un plug\--in installato potrebbe avere requisiti di installazione aggiuntivi per ogni nodo.  
  
Per installare un plug\--in, seguire le istruzioni riportate nell'autore del plug\--in. Per registrare manualmente un plug\--in con aggiornamento compatibile con cluster, eseguire il cmdlet [Register-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) in ogni\-computer in cui è installato il plug-in.  
  
## <a name="specify-a-plug-in-and-plug-in-arguments"></a>Specificare un plug\--in e\-gli argomenti del plug-in  
  
### <a name="specify-a-cau-plug-in"></a>Specificare un plug\--in di aggiornamento compatibile con cluster

Nell'interfaccia utente di aggiornamento compatibile con cluster selezionare\-un plug-in\-da un elenco a discesa\-di plug-in disponibili quando si usa aggiornamento compatibile con cluster per eseguire le azioni seguenti:  
  
-   Applicare aggiornamenti al cluster  
  
-   Vedere un'anteprima degli aggiornamenti per il cluster  
  
-   Configurare le opzioni\-di aggiornamento automatico del cluster  
  
Per impostazione predefinita, aggiornamento compatibile con\-cluster seleziona il plug in **Microsoft. WindowsUpdatePlugin**. Tuttavia, è possibile specificare qualsiasi plug\--in installato e registrato con aggiornamento compatibile con cluster.

> [!TIP]  
> Nell'interfaccia utente di aggiornamento compatibile con cluster è possibile specificare un\-solo plug-in per aggiornamento compatibile con cluster da usare per visualizzare in anteprima o applicare aggiornamenti durante un'operazione di aggiornamento. Usando i cmdlet di PowerShell per aggiornamento compatibile con cluster è possibile specificare uno o\-più plug-in. Se è necessario installare più tipi di aggiornamenti nel cluster, in genere è più efficiente specificare più plug\--in in un'unica operazione di aggiornamento, anziché usare un'operazione di aggiornamento separata per ogni plug\--in. Si verificheranno ad esempio meno riavvii di nodo.

Usando i cmdlet di PowerShell per aggiornamento compatibile con cluster elencati nella tabella seguente, è possibile specificare uno o più plug\--in per un'operazione di aggiornamento o un'analisi passando il parametro **– CauPluginName** . È possibile specificare più nomi\-di plug-in separandoli con virgole. Se si specificano più\-plug-in, è anche possibile controllare il\-modo in cui i plug-in incidono tra loro durante  **\-** un'operazione di aggiornamento specificando RunPluginsSerially,  **\-StopOnPluginFailure** parametri e **-SeparateReboots** . Per ulteriori informazioni sull'utilizzo di più\-plug-in, utilizzare i collegamenti forniti alla documentazione del cmdlet nella tabella seguente.  
  
|Cmdlet|Descrizione|  
|----------|---------------|  
|[Add-CauClusterRole](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/add-cauclusterrole)|Aggiunge il ruolo del cluster di aggiornamento compatibile con cluster\-che fornisce la funzionalità di aggiornamento automatico al cluster specificato.|  
|[Invoke-CauRun](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/invoke-caurun)|Esegue un'analisi dei nodi del cluster per verificare l'applicabilità di aggiornamenti e li installa tramite un'operazione di aggiornamento sul cluster specificato.|  
|[Invoke-CauScan](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/invoke-causcan)|Esegue un'analisi dei nodi del cluster per verificare l'applicabilità di aggiornamenti e restituisce un elenco del set di aggiornamenti iniziale che verrebbe applicato a ogni nodo nel cluster specificato.|  
|[Set-CauClusterRole](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/set-cauclusterrole)|Imposta le proprietà di configurazione per il ruolo cluster di Aggiornamento compatibile con cluster sul cluster specificato.|  
  
Se non si specifica un parametro del plug\--in di aggiornamento compatibile con cluster usando questi cmdlet, il valore\-predefinito è il plug in **Microsoft. WindowsUpdatePlugin**.  
  
### <a name="specify-cau-plug-in-arguments"></a>Specifica argomenti del\-plug-in di aggiornamento compatibile con cluster  
Quando si configurano le opzioni dell'operazione di aggiornamento, è possibile specificare uno o \(più\) argomenti di coppie *nome\=-valore* per il plug\--in selezionato da usare. Nell'interfaccia utente di Aggiornamento compatibile con cluster, ad esempio, è possibile specificare più argomenti come indicato di seguito:  
  
**Name1\=value1; Name2\=value2; Valore3\=name3**  
  
Queste *coppie\=nome/valore* devono essere significative per\-il plug-in specificato. Per alcuni plug\--in gli argomenti sono facoltativi.  
  
La sintassi degli argomenti del plug\--in di aggiornamento compatibile con cluster rispetta le regole generali seguenti:  
  
-   Le *coppie\=nome/valore* multiple sono separate da punti e virgola.  
  
-   I valori che contengono spazi vengono racchiusi tra virgolette, ad esempio: **Name1\="valore con spazi"** .  
  
-   La sintassi esatta del *valore* dipende dal plug\--in.  
  
Per specificare gli\-argomenti del plug-in usando i cmdlet di PowerShell per aggiornamento compatibile con cluster che supportano il parametro **– CauPluginParameters** , passare un parametro nel formato seguente:  
  
**\-CauPluginArguments @ {name1\=value1; Name2\=value2; Name3\=valore3}**  
  
È anche possibile usare una tabella hash di PowerShell predefinita. Per specificare gli\-argomenti del plug-in per più\-di un plug-in, passare più tabelle hash di argomenti, separate da virgole. Passare gli argomenti\-\-del plug-in nell'ordine specificato in **CauPluginName**.  
  
### <a name="specify-optional-plug-in-arguments"></a>Specificare gli argomenti\-del plug-in facoltativo  
I plug\--in che aggiornamento \(compatibile con cluster installa **Microsoft. WindowsUpdatePlugin** e **Microsoft. HotfixPlugin** \) forniscono opzioni aggiuntive che è possibile selezionare. Nell'interfaccia utente di aggiornamento compatibile con cluster questi vengono visualizzati in una pagina di **Opzioni aggiuntive** dopo aver configurato le\-opzioni di aggiornamento per il plug-in. Se si usano i cmdlet di PowerShell per aggiornamento compatibile con cluster, queste opzioni vengono configurate come argomenti plug\--in facoltativi. Per altre informazioni, vedere [Usare Microsoft.WindowsUpdatePlugin](#BKMK_WUP) e [Usare Microsoft.HotfixPlugin](#BKMK_HFP) , più avanti in questo argomento.  
  
## <a name="manage-plug-ins-using-windows-powershell-cmdlets"></a>Gestire i\-plug-in usando i cmdlet di Windows PowerShell  
  
|Cmdlet|Descrizione|  
|----------|---------------|  
|[Get-CauPlugin](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/get-cauplugin)|Recupera informazioni su uno o più plug\--in di aggiornamento software registrati nel computer locale.|  
|[Register-CauPlugin]((https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/register-cauplugin))|Registra un plug\--in di aggiornamento software di aggiornamento compatibile con cluster nel computer locale.|  
|[Unregister-CauPlugin](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/unregister-cauplugin)|Rimuove un plug\--in di aggiornamento software dall'elenco di\-plug-in che possono essere usati da aggiornamento compatibile con cluster. **Nota:** Non è\-possibile annullare la registrazione dei plug \(-in installati con aggiornamento compatibile con cluster **Microsoft. WindowsUpdatePlugin** e **Microsoft. HotfixPlugin** \) .|  
  
## <a name="BKMK_WUP"></a>Utilizzo di Microsoft. WindowsUpdatePlugin  

Il plug\--in predefinito per aggiornamento compatibile con cluster, **Microsoft. WindowsUpdatePlugin**, esegue le azioni seguenti:
- Comunica con l'agente di Windows Update su ciascun nodo del cluster di failover per applicare gli aggiornamenti necessari per i prodotti Microsoft in esecuzione su ciascun nodo.
- Installa gli aggiornamenti del cluster direttamente da Windows Update o Microsoft Update o da un\-server locale \(Windows Server Update Services\) WSUS.
- Installa solo gli aggiornamenti GDR \(\) selezionati, General Distribution Release. Per impostazione predefinita, il\-plug-in applica solo aggiornamenti software importanti. Nessuna configurazione richiesta. La configurazione predefinita consente di scaricare e installare gli aggiornamenti GDR importanti. 

> [!NOTE]
> Per applicare aggiornamenti diversi dagli importanti aggiornamenti software selezionati per impostazione predefinita \(, ad esempio gli aggiornamenti\)dei driver, è possibile configurare un parametro di\-plug-in facoltativo. Per altre informazioni, vedere [Configurare la stringa di query per l'agente di Windows Update](#BKMK_QUERY).

### <a name="requirements"></a>Requisiti

- Se usato \(\) , il cluster di failover e il computer coordinatore dell'aggiornamento remoto devono soddisfare i requisiti per aggiornamento compatibile con cluster e la configurazione necessaria per la gestione remota elencata in [requisiti e procedure consigliate per](cluster-aware-updating-requirements.md)aggiornamento compatibile con cluster.
- Consultare [Raccomandazioni per l'applicazione di aggiornamenti Microsoft](cluster-aware-updating-requirements.md#BKMK_BP_WUA), quindi apportare le eventuali modifiche necessarie alla configurazione di Microsoft Update per i nodi del cluster di failover.
- Per ottenere risultati ottimali, è consigliabile eseguire aggiornamento compatibile con \(cluster\) Best Practices Analyzer BPA per assicurarsi che l'ambiente cluster e di aggiornamento siano configurati correttamente per l'applicazione di aggiornamenti tramite aggiornamento compatibile con cluster. Per altre informazioni, vedere [Test di disponibilità per l'aggiornamento con Aggiornamento compatibile con cluster](cluster-aware-updating-requirements.md#BKMK_BPA).

> [!NOTE]
> Gli aggiornamenti che richiedono l'accettazione delle condizioni di licenza Microsoft o l'interazione dell'utente sono esclusi e devono essere installati manualmente.

### <a name="additional-options"></a>Opzioni aggiuntive

Facoltativamente, è possibile specificare gli argomenti del\-plug-in seguenti per aumentare o limitare il set di aggiornamenti applicati dal plug\--in:
- Per configurare il plug\--in in modo da applicare gli aggiornamenti consigliati, oltre agli aggiornamenti importanti in ogni nodo, nell'interfaccia utente di aggiornamento compatibile con cluster, nella pagina **Opzioni aggiuntive** , selezionare l'opzione **Give me recommended Updates nello stesso modo** in cui vengono ricevuti gli aggiornamenti importanti casella di controllo.
<br>In alternativa, configurare l'argomento del plug\--in **' IncludeRecommendedUpdates '\=true '** .
- Per configurare il plug\--in in modo da filtrare i tipi di aggiornamenti GDR applicati a ogni nodo del cluster, specificare una stringa di query di Windows Update Agent\-utilizzando un argomento del plug-in **QueryString** . Per altre informazioni, vedere [Configurare la stringa di query per l'agente di Windows Update](#BKMK_QUERY).

### <a name="BKMK_QUERY"></a>Configurare la stringa di query dell'agente di Windows Update  
È possibile configurare un argomento\-del plug-in per il\-plug-in predefinito, **Microsoft. WindowsUpdatePlugin**, costituito da \(una\) stringa di query di Windows Update Agent WUA. Questa istruzione usa l'API agente di Windows Update (WUA) per identificare uno o più gruppi di aggiornamenti Microsoft da applicare a ogni nodo, sulla base di specifici criteri di selezione. È possibile combinare più criteri usando operatori AND od OR logici. La stringa di query WUA viene specificata in un\-argomento del plug-in come indicato di seguito:  
  
**QueryString\="Criterion1\=value1 and\/o Criterion2\=value2 and\/or..."**  
  
**Microsoft.WindowsUpdatePlugin**, ad esempio, seleziona automaticamente gli aggiornamenti importanti usando un argomento **QueryString** predefinito costruito usando i criteri **IsInstalled**, **Type**, **IsHidden** e **IsAssigned**:  
  
**QueryString\=\="\=disinstalled\=0 e Type" software "e unhidden 0 e Assigned\=1"**  
  
Se si specifica un argomento **QueryString** , questo viene usato al posto della **QueryString** predefinita configurata per il plug\--in.  
  
#### <a name="example-1"></a>Esempio 1
  
Per configurare un argomento **QueryString** che installa un aggiornamento specifico come identificato da ID *f6ce46c1\-971C\-43f9\-a2aa\-783df125f003*:  
  
**QueryString\="codice UpdateID\=' f6ce46c1\-971C\-\=43f9\-a2aa783df125f003'and\-disinstalled 0"**  
  
> [!NOTE]  
> L'esempio precedente è valido per l'applicazione di aggiornamenti tramite la\-procedura guidata di aggiornamento compatibile con cluster. Se si vuole installare un aggiornamento specifico configurando le\-opzioni di aggiornamento automatico con l'interfaccia utente di aggiornamento compatibile con cluster o usando il cmdlet **Add\-CauClusterRole** o **set\-CauClusterRole**di PowerShell, è necessario formattare Valore codice UpdateID con due virgolette singole\-:  
>   
> **QueryString\="codice UpdateID\='' f6ce46c1\-971C\-43f9\-\=a2aa783df125f003''anddisinstalled0\-"**  
  
#### <a name="example-2"></a>Esempio 2
  
Per configurare un argomento **QueryString** che installa solo driver:  
  
**QueryString\="\=\=disinstalled 0 e Type ' driver ' and Hidden 0"\=**  
  
Per ulteriori informazioni sulle stringhe di query per il plug\--in predefinito, **Microsoft. WindowsUpdatePlugin**, i \(criteri di ricerca, ad esempio l' **installazione**\)di e la sintassi che è possibile includere nella query per le stringhe, vedere la sezione relativa ai criteri di ricerca nel [riferimento all'API agente di Windows Update (WUA)](https://go.microsoft.com/fwlink/p/?LinkId=223304).  
  
## <a name="BKMK_HFP"></a>Usare Microsoft. HotfixPlugin  
Il plug\-- **in Microsoft. HotfixPlugin** può essere usato per applicare gli \(aggiornamenti \(Microsoft Limited\) Distribution Release LDR, detti anche hotfix, e in\) precedenza denominati QFE scaricare in modo indipendente per risolvere problemi specifici del software Microsoft. Il plug-in installa gli aggiornamenti da una cartella radice in una condivisione file SMB e può essere personalizzato anche per applicare\-aggiornamenti di driver, firmware e BIOS non Microsoft.

> [!NOTE]
> Gli hotfix sono talvolta disponibili per il download da Microsoft negli articoli della Knowledge base, ma vengono anche forniti ai clienti in\-base alle esigenze.

### <a name="requirements"></a>Requisiti

- Se usato \(\) , il cluster di failover e il computer coordinatore dell'aggiornamento remoto devono soddisfare i requisiti per aggiornamento compatibile con cluster e la configurazione necessaria per la gestione remota elencata in [requisiti e procedure consigliate per](cluster-aware-updating-requirements.md)aggiornamento compatibile con cluster.
- Consultare [Consigli per l'uso di Microsoft.HotfixPlugin](cluster-aware-updating-requirements.md#BKMK_BP_HF).
- Per ottenere risultati ottimali, è consigliabile eseguire il modello BPA \(Best Practices Analyzer\) BPA per verificare che il cluster e l'ambiente di aggiornamento siano configurati correttamente per l'applicazione di aggiornamenti tramite aggiornamento compatibile con cluster. Per altre informazioni, vedere [Test di disponibilità per l'aggiornamento con Aggiornamento compatibile con cluster](cluster-aware-updating-requirements.md#BKMK_BPA).
- Ottenere gli aggiornamenti dal server di pubblicazione e copiarli o estrarli \(in una cartella\) radice dell'hotfix\) della condivisione \(file SMB del Server Message Block che supporta almeno SMB 2,0 e accessibile da parte di tutti i cluster i nodi e il computer \(coordinatore dell'aggiornamento remoto se aggiornamento compatibile con cluster viene usato in modalità\)di aggiornamento remoto\-. Per altre informazioni, vedere [Configurare una struttura di cartella radice dell'aggiornamento rapido](#BKMK_HF_ROOT) più avanti in questo argomento. 

    > [!NOTE]
    > Per impostazione predefinita, questo\-plug-in installa solo aggiornamenti rapidi con le estensioni di file seguenti:. msu,. msi e. msp.

- Copiare il \(file DefaultHotfixConfig. XML disponibile nella cartella **% systemroot%\\system32\\WindowsPowerShell\\v\\1.0 modules\\ClusterAwareUpdating** in un computer in cui sono installati\) gli strumenti di aggiornamento compatibile con cluster nella cartella radice degli aggiornamenti rapidi creata e in cui sono stati estratti gli aggiornamenti rapidi. Ad esempio, copiare il file di configurazione nella  *\\\\\\radice degli\\ \\aggiornamenti rapidi di FS*. 

    > [!NOTE]
    > Per installare la maggior parte degli aggiornamenti rapidi forniti da Microsoft e altri aggiornamenti, è possibile usare il file di configurazione per gli aggiornamenti rapidi predefinito senza modifiche. Se richiesto per lo scenario in uso, è possibile personalizzare il file di configurazione come attività avanzata. Il file di configurazione può includere, ad esempio, ruoli personalizzati per la gestione di file di aggiornamento rapido con estensioni specifiche oppure per definire i comportamenti per determinate condizioni di uscita. Per altre informazioni, vedere [Personalizzare il file di configurazione degli aggiornamenti rapidi](#BKMK_CONFIG_FILE) più avanti in questo argomento.

### <a name="configuration"></a>Configurazione

Configurare le opzioni descritte di seguito. Per altre informazioni, vedere i collegamenti alle sezioni più avanti in questo argomento.
- Il percorso alla cartella radice degli aggiornamenti rapidi condivisa contenente gli aggiornamenti da applicare nonché il file di configurazione degli aggiornamenti rapidi. È possibile digitare questo percorso nell'interfaccia utente di aggiornamento compatibile con cluster o configurare il **percorso HotfixRootFolderPath\=\<>** argomento del plug\--in di PowerShell. 

   > [!NOTE]
   > È possibile specificare la cartella radice degli aggiornamenti rapidi come un percorso di cartella locale o un percorso UNC nel formato  *\\ \\\\servername\\Share RootFolderName*. È possibile\-utilizzare un percorso di spazio dei nomi DFS basato su dominio o autonomo. Tuttavia, le funzionalità\-di plug-in che controllano le autorizzazioni di accesso nel file di configurazione degli aggiornamenti rapidi non sono compatibili con un percorso dello spazio dei nomi DFS, quindi se ne viene configurato uno, è necessario disabilitare il controllo delle autorizzazioni di accesso usando l'interfaccia utente di aggiornamento compatibile con cluster o configurando Argomento delplug\--in **argomento disableaclchecks\=' true '** .
- Impostazioni nel server che ospita la cartella radice degli aggiornamenti rapidi per verificare le autorizzazioni appropriate per accedere alla cartella e garantire l'integrità dei dati a cui si accede dalla \(firma SMB della cartella condivisa\)SMB o dalla crittografia SMB. Per altre informazioni, vedere [Limitare l'accesso alla cartella radice degli aggiornamenti rapidi](#BKMK_ACL).

### <a name="additional-options"></a>Opzioni aggiuntive

- Facoltativamente, configurare il plug\--in in modo che la crittografia SMB venga applicata quando si accede ai dati dalla condivisione file degli aggiornamenti rapidi. Nell'interfaccia utente di aggiornamento compatibile con cluster, nella pagina **Opzioni aggiuntive** , selezionare l'opzione **Richiedi crittografia SMB per l'accesso alla cartella radice degli hotfix** oppure configurare l'argomento del plug\--in di PowerShell **RequireSMBEncryption\=' true '** . 
  > [!IMPORTANT]
  > È necessario eseguire passaggi di configurazione aggiuntivi nel server SMB per abilitare l'integrità dei dati SMB con firma o crittografia SMB. Per altre informazioni, vedere il passaggio 4 in [Limitare l'accesso alla cartella radice degli aggiornamenti rapidi](#BKMK_ACL). Se si seleziona l'opzione per imporre l'uso della crittografia SMB e la cartella radice degli aggiornamenti rapidi non è configurata per l'accesso mediante la crittografia SMB, l'operazione di aggiornamento non riuscirà.
- Facoltativamente, disabilitare i controlli predefiniti sulla presenza delle autorizzazioni necessarie per la cartella radice e per il file di configurazione degli aggiornamenti rapidi. Nell'interfaccia utente di aggiornamento compatibile con cluster selezionare **Disabilita verifica l'accesso amministratore alla cartella radice degli aggiornamenti rapidi e al file di configurazione**oppure configurare\-l'argomento del plug-in **argomento disableaclchecks\=' true '** .
- Facoltativamente, configurare l' **argomento\= argomento hotfixinstallertimeoutminutes<Integer>**  per specificare per quanto tempo il plug\--in hotfix attende la restituzione del processo di installazione dell'hotfix. \(Il valore predefinito è 30 minuti.\) Ad esempio, per specificare un periodo di timeout di due ore, **impostare\=argomento hotfixinstallertimeoutminutes 120**.
- Facoltativamente, configurare l'argomento del\-plug-in **argomento hotfixconfigfilename \= <name>**  per specificare un nome per il file di configurazione dell'hotfix che si trova nella cartella radice degli aggiornamenti rapidi. Se non ne viene specificato uno, verrà usato il nome predefinito DefaultHotfixConfig.xml.
  
### <a name="BKMK_HF_ROOT"></a>Configurare una struttura di cartelle radice degli aggiornamenti rapidi

Per il corretto funzionamento\-del plug-in degli aggiornamenti rapidi, gli hotfix devono\-essere archiviati in una struttura ben definita \(in una cartella\)radice degli hotfix della condivisione file SMB ed\-è necessario configurare il plug-in con il percorso del cartella radice degli aggiornamenti rapidi tramite l'interfaccia utente di aggiornamento compatibile con cluster o i cmdlet di PowerShell per. Questo percorso viene passato al plug\--in come argomento **HotfixRootFolderPath** . In base alle esigenze di aggiornamento, è possibile scegliere tra diverse strutture per la cartella radice degli aggiornamenti rapidi, come illustrato negli esempi seguenti. File o cartelle non conformi a questa struttura verranno ignorati.  
  
#### <a name="example-1---folder-structure-used-to-apply-hotfixes-to-all-cluster-nodes"></a>Esempio 1: struttura di cartelle usata per applicare aggiornamenti rapidi a tutti i nodi del cluster
  
Per specificare che gli aggiornamenti rapidi si applicano a tutti i nodi del cluster, copiarli in una cartella denominata **CAUHotfix\_all** sotto la cartella radice degli aggiornamenti rapidi. In questo esempio, l'  argomento del\-plug  *\\\\ \\\\-in HotfixRootFolderPath è impostato sulla radice degliaggiornamentirapididiFS.\\* La **cartella\_CAUHotfix all** contiene tre aggiornamenti con Extensions. msu,. msi e. msp che verranno applicati a tutti i nodi del cluster. I nomi file degli aggiornamenti qui riportati hanno solo scopo illustrativo.  
  
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
  
Per specificare aggiornamenti rapidi applicabili solo a un nodo specifico, usare una sottocartella della cartella radice degli aggiornamenti rapidi con il nome del nodo. Usare il nome NetBIOS del nodo del cluster, ad esempio *ContosoNode1*. Spostare quindi gli aggiornamenti applicabili solo al nodo in questione nella sottocartella. Nell'esempio seguente, l'argomento  del plug\-  *\\\\ \\\\-in HotfixRootFolderPath è impostato sulla radice degliaggiornamentirapididiFS.\\* Gli aggiornamenti nella **cartella\_CAUHotfix all** verranno applicati a tutti i nodi del cluster e *node1\_Update\_. msu specifico* verrà applicato solo a *ContosoNode1*.  
  
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
  
#### <a name="example-3---folder-structure-used-to-apply-updates-other-than-msu-msi-and-msp-files"></a>Esempio 3: struttura di cartelle usata per applicare aggiornamenti diversi dai file. msu,. msi e. msp
  
Per impostazione predefinita, **Microsoft.HotfixPlugin** applica unicamente aggiornamenti con le estensioni .msu, .msi o .msp. Alcuni aggiornamenti potrebbero tuttavia avere estensioni diverse e potrebbero richiedere comandi di installazione diversi. Ad esempio, potrebbe essere necessario applicare un aggiornamento firmware con estensione .exe a un nodo in un cluster. È possibile configurare la cartella radice degli aggiornamenti rapidi con una sottocartella che indica che\-deve essere installato un tipo di aggiornamento non predefinito specifico. È inoltre necessario configurare una regola di installazione della cartella corrispondente che specifica il comando di installazione nell'elemento `<FolderRules>` nel file XML di configurazione degli aggiornamenti rapidi.  
  
Nell'esempio seguente, l'argomento  del plug\-  *\\\\ \\\\-in HotfixRootFolderPath è impostato sulla radice degliaggiornamentirapididiFS.\\* Molti aggiornamenti verranno applicati a tutti i nodi del cluster, mentre l'aggiornamento firmware *SpecialHotfix1.exe* verrà applicato a *ContosoNode1* usando *FolderRule1*. Per informazioni sulla configurazione di *FolderRule1* nel file di configurazione degli aggiornamenti rapidi, vedere [Personalizzare il file di configurazione degli aggiornamenti rapidi](#BKMK_CONFIG_FILE) più avanti in questo argomento.  
  
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

### <a name="BKMK_CONFIG_FILE"></a>Personalizzare il file di configurazione degli aggiornamenti rapidi  
Il file di configurazione degli aggiornamenti rapidi controlla il modo in cui **Microsoft.HotfixPlugin** installa tipi di file degli aggiornamenti rapidi specifici in un cluster di failover. Lo schema XML per il file di configurazione è definito in HotfixConfigSchema.xsd, che si trova nella cartella seguente in un computer in cui sono installati gli strumenti di Aggiornamento compatibile con cluster:  
  
**% SystemRoot%\\system32\\WindowsPowerShell\\v 1.0\\modules\\cartella ClusterAwareUpdating**  
  
Per personalizzare il file di configurazione degli aggiornamenti rapidi, copiare il file di configurazione di esempio DefaultHotfixConfig.xml da questo percorso alla cartella radice degli aggiornamenti rapidi e apportare le modifiche appropriate per il proprio scenario.  
  
> [!IMPORTANT]  
> Per applicare la maggior parte degli aggiornamenti rapidi forniti da Microsoft e altri aggiornamenti, è possibile usare il file di configurazione per gli aggiornamenti rapidi predefinito senza modifiche. La personalizzazione del file di configurazione degli aggiornamenti rapidi è un'attività destinata solo a scenari di utilizzo avanzato.  
  
Per impostazione predefinita, il file XML di configurazione degli aggiornamenti rapidi definisce regole di installazione e condizioni di uscita per le due categorie di aggiornamenti rapidi seguenti:  
  
-   File hotfix con estensioni che il plug\--in è in grado \(di installare per impostazione predefinita. msu,. msi\)e file con estensione msp.  
  
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
  
-   Aggiornamenti rapidi o altri file di aggiornamento che non sono file. msi,. msu o. msp, ad esempio\-, driver non Microsoft, firmware e aggiornamenti del BIOS.  
  
    Ogni tipo\-di file non predefinito viene configurato `<Folder>` come elemento nell' `<FolderRules>` elemento. L'attributo del nome dell'elemento `<Folder>` deve essere identico al nome di una cartella all'interno della cartella degli aggiornamenti rapidi che conterrà gli aggiornamenti del tipo corrispondente. La cartella può trovarsi nella **cartella\_CAUHotfix all** o in una cartella\-specifica del nodo. Se *FolderRule1* , ad esempio, è configurato nella cartella radice degli aggiornamenti rapidi, configurare l'elemento seguente nel file XML per definire un modello di installazione e delle condizioni di uscita per gli aggiornamenti in quella cartella:  
  
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
  
### <a name="BKMK_ACL"></a>Limita l'accesso alla cartella radice degli aggiornamenti rapidi  
Per configurare il file server e la condivisione file SMB per garantire accesso alla cartella radice e al file di configurazione degli aggiornamenti rapidi esclusivamente nel contesto di **Microsoft.HotfixPlugin**, è necessario eseguire diversi passaggi. Tali passaggi consentono di abilitare varie funzionalità che aiutano a prevenire possibili manomissioni dei file di aggiornamento rapido che potrebbero compromettere il cluster di failover.  
  
In generale, i passaggi sono i seguenti:  
  
1.  Identificare l'account utente usato per le operazioni di aggiornamento usando il plug\--in  
  
2.  Configurare l'account utente individuato nei gruppi necessari in un file server SMB  
  
3.  Configurare le autorizzazioni per accedere alla cartella radice degli aggiornamenti rapidi  
  
4.  Configurare le impostazioni per l'integrità dei dati SMB  
  
5.  Abilitare una regola di Windows Firewall nel server SMB  
  
#### <a name="step-1-identify-the-user-account-that-is-used-for-updating-runs-by-using-the-hotfix-plug-in"></a>Passaggio 1. Identificare l'account utente usato per le operazioni di aggiornamento usando il plug\--in per gli aggiornamenti rapidi
  
L'account usato in aggiornamento compatibile con cluster per verificare le impostazioni di sicurezza durante l'esecuzione di un'operazione di aggiornamento tramite **Microsoft. HotfixPlugin** varia a seconda\-che aggiornamento compatibile con\-cluster venga usato in modalità di aggiornamento remoto o in modalità di aggiornamento automatico, come indicato di seguito:  
  
-   **Modalità\-di aggiornamento remoto** l'account con privilegi amministrativi nel cluster per visualizzare in anteprima e applicare gli aggiornamenti.  
  
-   **Modalità\-di aggiornamento automatico** il nome dell'oggetto computer virtuale configurato in Active Directory per il ruolo del cluster di aggiornamento compatibile con cluster. Si tratta del nome di un oggetto computer virtuale preinstallato in Active Directory per il ruolo del cluster di Aggiornamento compatibile con cluster o del nome generato da quest'ultimo per il ruolo del cluster. Per ottenere il nome se viene generato da aggiornamento compatibile con cluster, eseguire il cmdlet di PowerShell **get\-CauClusterRole** aggiornamento compatibile con cluster. Nell'output **ResourceGroupName** è il nome dell'account dell'oggetto computer virtuale generato.  
  
#### <a name="step-2-configure-this-user-account-in-the-necessary-groups-on-an-smb-file-server"></a>Passaggio 2. Configurare l'account utente individuato nei gruppi necessari in un file server SMB
  
> [!IMPORTANT]  
> È necessario aggiungere l'account usato per le operazioni di aggiornamento come account Administrator locale nel server SMB. Se ciò non viene consentito a causa dei criteri di sicurezza dell'organizzazione, configurare questo account con le autorizzazioni necessarie sul server SMB usando la procedura riportata di seguito.  
  
##### <a name="to-configure-a-user-account-on-the-smb-server"></a>Per configurare un account utente sul server SMB  
  
1.  Aggiungere l'account usato per le operazioni di aggiornamento al gruppo Distributed COM Users e a uno dei gruppi seguenti: Power User, Server operator o Print operator.  
  
2.  Per abilitare le autorizzazioni WMI necessarie per l'account, avviare la console di gestione WMI nel server SMB. Avviare PowerShell e digitare il comando seguente:  
  
    ```  
    wmimgmt.msc  
    ```  
  
3.  Nell'albero della console fare clic\-con il pulsante destro del mouse su **controllo \(WMI locale\)** e quindi scegliere **Proprietà**.  
  
4.  Fare clic su **Sicurezza**, quindi espandere **Radice**.  
  
5.  Fare clic su **CIMV2**, quindi su **Sicurezza**.  
  
6.  Aggiungere l'account usato per le operazioni di aggiornamento all'elenco **Utenti e gruppi**.  
  
7.  Concedere le autorizzazioni **Esegui metodi** e **Abilita remoto** all'account usato per le operazioni di aggiornamento.  
  
#### <a name="step-3-configure-permissions-to-access-the-hotfix-root-folder"></a>Passaggio 3. Configurare le autorizzazioni per accedere alla cartella radice degli aggiornamenti rapidi
  
Per impostazione predefinita, quando si tenta di applicare gli aggiornamenti, il\-plug-in hotfix verifica la configurazione delle autorizzazioni di file system NTFS per l'accesso alla cartella radice degli aggiornamenti rapidi. Se le autorizzazioni di accesso alla cartella non sono configurate correttamente, un'operazione di\-aggiornamento tramite il plug-in hotfix potrebbe non riuscire.  
  
Se si usa la configurazione predefinita del plug\--in hotfix, verificare che le autorizzazioni di accesso alla cartella soddisfino i requisiti seguenti.  
  
-   Il gruppo Users deve disporre delle autorizzazioni di lettura.  
  
-   Se il plug\--in applicherà gli aggiornamenti con estensione exe, il gruppo Users avrà l'autorizzazione Execute.  
  
-   Sono consentite \(solo determinate entità di sicurezza, ma\) non è necessario disporre dell'autorizzazione di scrittura o modifica. Le entità di sicurezza ammesse sono il gruppo Administrators locale, SYSTEM, CREATOR OWNER e TrustedInstaller. Nessun altro account o gruppo può disporre di autorizzazioni di scrittura o modifica per la cartella radice degli aggiornamenti rapidi.  
  
Facoltativamente, è possibile disabilitare i controlli precedenti eseguiti dal plug\--in per impostazione predefinita. Questa operazione può essere eseguita in due modi:  
  
-   Se si usano i cmdlet di PowerShell per aggiornamento compatibile con cluster, configurare l'argomento **argomento disableaclchecks\=' true '** nel parametro **CauPluginArguments** per il\-plug-in hotfix.  
  
-   Se si sta usando l'interfaccia utente di Aggiornamento compatibile con cluster, selezionare l'opzione **Disabilita controllo accesso amministratore alla cartella radice degli aggiornamenti rapidi e al file di configurazione** nella pagina **Opzioni di aggiornamento aggiuntive** della procedura guidata per la configurazione delle opzioni relative alle operazioni di aggiornamento.  
  
Come procedura consigliata per molti ambienti si raccomanda tuttavia l'impiego della configurazione predefinita che prevede l'imposizione di tali controlli.  
  
#### <a name="step-4-configure-settings-for-smb-data-integrity"></a>Passaggio 4. Configurare le impostazioni per l'integrità dei dati SMB
  
Per verificare l'integrità dei dati nelle connessioni tra i nodi del cluster e la condivisione file SMB, il plug\--in per gli aggiornamenti rapidi richiede l'abilitazione delle impostazioni nella condivisione file SMB per la firma SMB o la crittografia SMB. La crittografia SMB, che fornisce sicurezza avanzata e prestazioni migliori in molti ambienti, è supportata a partire da Windows Server 2012. È possibile abilitare una di queste impostazioni, o entrambe, come indicato di seguito:  
  
-   Per abilitare la firma SMB, vedere la procedura nell' [articolo 887429](https://support.microsoft.com/kb/887429) della Microsoft Knowledge Base.  
  
-   Per abilitare la crittografia SMB per la cartella SMB condivisa, eseguire il seguente cmdlet di PowerShell nel server SMB:  
  
    ```PowerShell  
    Set-SmbShare <ShareName> -EncryptData $true  
    ```  
  
    Dove <*ShareName*> è il nome della cartella SMB condivisa.  
  
Facoltativamente, per imporre l'uso della crittografia SMB nelle connessioni al server SMB, selezionare l'opzione **Richiedi crittografia SMB nell'accesso alla cartella radice degli aggiornamenti rapidi** nell'interfaccia utente di aggiornamento compatibile con cluster o configurare il plug-in **RequireSMBEncryption\=' true '** \-in argomento usando i cmdlet di PowerShell per aggiornamento compatibile con cluster.  
  
> [!IMPORTANT]  
> Se si seleziona l'opzione per imporre l'uso della crittografia SMB e la cartella radice degli aggiornamenti rapidi non è configurata per connessioni che usano la crittografia SMB, l'operazione di aggiornamento non riuscirà.  
  
#### <a name="step-5-enable-a-windows-firewall-rule-on-the-smb-server"></a>Passaggio 5. Abilitare una regola di Windows Firewall nel server SMB
  
È necessario abilitare la **gestione \(remota SMB\-di file server\) nella** regola Windows Firewall sul file server SMB. Questa funzionalità è abilitata per impostazione predefinita in Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Panoramica di aggiornamento compatibile con cluster](cluster-aware-updating.md)
  
-   [Cmdlet di Windows PowerShell per aggiornamento compatibile con cluster](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating)  
  
-   [Riferimento al plug-in di aggiornamento compatibile con cluster](https://msdn.microsoft.com/library/hh418084.aspx)  
  
