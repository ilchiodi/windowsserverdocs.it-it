---
ms.assetid: d44d4430-41e2-483a-9685-92610cdef32a
title: Funzionamento del plug-in di aggiornamento compatibile con cluster
ms.topic: article
ms.prod: windows-server
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 04/28/2017
ms.technology: storage-failover-clustering
description: Come usare i plug-in per coordinare gli aggiornamenti quando si usa l'aggiornamento compatibile con cluster in Windows Server per installare gli aggiornamenti in un cluster.
ms.openlocfilehash: f6c572a397530704dd91d9c67c5c1758ccc085c4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361294"
---
# <a name="how-cluster-aware-updating-plug-ins-work"></a>Funzionamento del plug-in di aggiornamento compatibile con cluster

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

[Aggiornamento compatibile con cluster](cluster-aware-updating.md) usa i plug-in per coordinare l'installazione degli aggiornamenti nei nodi di un cluster di failover. Questo argomento fornisce informazioni sull'uso del plug-in no__t-0cm di aggiornamento compatibile con cluster @ no__t-1ins o di un altro plug @ no__t-2Ins installato per aggiornamento compatibile con cluster.

## <a name="BKMK_INSTALL"></a>Installare un plug @ no__t-1in  
È necessario installare separatamente un plug @ no__t-0cm diverso da quello predefinito plug @ no__t-1ins installato con aggiornamento compatibile con cluster \(**Microsoft. WindowsUpdatePlugin** e **microsoft. HotfixPlugin**\). Se aggiornamento compatibile con cluster viene usato in modalità self @ no__t-0updating, è necessario installare il plug @ no__t-1in in tutti i nodi del cluster. Se aggiornamento compatibile con cluster viene usato in modalità remota @ no__t-0updating, è necessario installare il plug @ no__t-1in nel computer coordinatore dell'aggiornamento remoto. Un plug @ no__t-0cm installato potrebbe avere requisiti di installazione aggiuntivi in ogni nodo.  
  
Per installare un plug @ no__t-0cm, seguire le istruzioni riportate nel server di pubblicazione plug @ no__t-1in. Per registrare manualmente un plug-in no__t-0cm con aggiornamento compatibile con cluster, eseguire il cmdlet [Register-CauPlugin](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/register-cauplugin) in ogni computer in cui è installato il plug @ no__t-2in.  
  
## <a name="specify-a-plug-in-and-plug-in-arguments"></a>Specificare gli argomenti plug @ no__t-0cm e plug @ no__t-1in  
  
### <a name="specify-a-cau-plug-in"></a>Specificare un plug-in di aggiornamento compatibile con cluster @ no__t-0cm

Nell'interfaccia utente di aggiornamento compatibile con cluster si seleziona un plug @ no__t-0cm da un elenco a discesa @ no__t-1down di plug-in disponibili per il plug-in no__t-2Ins quando si usa aggiornamento compatibile con cluster per eseguire le azioni seguenti:  
  
-   Applicare aggiornamenti al cluster  
  
-   Vedere un'anteprima degli aggiornamenti per il cluster  
  
-   Configurare le opzioni del cluster self @ no__t-0updating  
  
Per impostazione predefinita, aggiornamento compatibile con cluster seleziona il plug @ no__t-0cm **Microsoft. WindowsUpdatePlugin**. Tuttavia, è possibile specificare qualsiasi plug-in no__t-0cm installato e registrato con aggiornamento compatibile con cluster.

> [!TIP]  
> Nell'interfaccia utente di aggiornamento compatibile con cluster è possibile specificare un singolo plug @ no__t-0cm per aggiornamento compatibile con cluster da usare per visualizzare in anteprima o applicare aggiornamenti durante un'operazione di aggiornamento. Usando i cmdlet di PowerShell per aggiornamento compatibile con cluster, è possibile specificare uno o più plug @ no__t-0ins. Se è necessario installare più tipi di aggiornamenti nel cluster, in genere è più efficiente specificare più plug @ no__t-0ins in un'unica operazione di aggiornamento, anziché usare un'operazione di aggiornamento separata per ogni plug-in @ no__t-1in. Si verificheranno ad esempio meno riavvii di nodo.

Usando i cmdlet di PowerShell per aggiornamento compatibile con cluster elencati nella tabella seguente, è possibile specificare uno o più plug @ no__t-0ins per un'operazione di aggiornamento o un'analisi passando il parametro **– CauPluginName** . È possibile specificare più nomi plug @ no__t-0cm separandoli con virgole. Se si specifica più plug @ no__t-0ins, è anche possibile controllare il modo in cui il plug @ no__t-1ins influisce tra loro durante un'operazione di aggiornamento specificando i **\-RunPluginsSerially**, **\-StopOnPluginFailure**e **– SeparateReboots** parametri. Per ulteriori informazioni sull'utilizzo di più plug @ no__t-0ins, utilizzare i collegamenti forniti alla documentazione del cmdlet nella tabella seguente.  
  
|Cmdlet|Descrizione|  
|----------|---------------|  
|[Add-CauClusterRole](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/add-cauclusterrole)|Aggiunge il ruolo del cluster di aggiornamento compatibile con cluster che fornisce la funzionalità self @ no__t-0updating al cluster specificato.|  
|[Invoke-CauRun](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/invoke-caurun)|Esegue un'analisi dei nodi del cluster per verificare l'applicabilità di aggiornamenti e li installa tramite un'operazione di aggiornamento sul cluster specificato.|  
|[Invoke-CauScan](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/invoke-causcan)|Esegue un'analisi dei nodi del cluster per verificare l'applicabilità di aggiornamenti e restituisce un elenco del set di aggiornamenti iniziale che verrebbe applicato a ogni nodo nel cluster specificato.|  
|[Set-CauClusterRole](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/set-cauclusterrole)|Imposta le proprietà di configurazione per il ruolo cluster di Aggiornamento compatibile con cluster sul cluster specificato.|  
  
Se non si specifica un parametro plug @ no__t-0cm di aggiornamento compatibile con cluster usando questi cmdlet, il valore predefinito è il plug @ no__t-1in **Microsoft. WindowsUpdatePlugin**.  
  
### <a name="specify-cau-plug-in-arguments"></a>Specificare gli argomenti del plug-in no__t-0cm  
Quando si configurano le opzioni dell'operazione di aggiornamento, è possibile specificare una o più coppie *nome @ no__t-1value* \(arguments @ no__t-3 per il plug @ no__t-4in selezionato da usare. Nell'interfaccia utente di Aggiornamento compatibile con cluster, ad esempio, è possibile specificare più argomenti come indicato di seguito:  
  
**Name1 @ no__t-1Value1; Name2 @ no__t-2Value2; Name3 @ no__t-3Value3**  
  
Queste coppie *nome @ no__t-1value* devono essere significative per il plug @ no__t-2in specificato. Per alcuni plug @ no__t-0ins gli argomenti sono facoltativi.  
  
La sintassi degli argomenti plug @ no__t-0cm di aggiornamento compatibile con cluster segue le regole generali seguenti:  
  
-   Più coppie *nome @ no__t-1value* sono separate da punti e virgola.  
  
-   I valori che contengono spazi vengono racchiusi tra virgolette, ad esempio: **Name1 @ no__t-1 "valore con spazi"** .  
  
-   La sintassi esatta del *valore* dipende dal plug @ no__t-1in.  
  
Per specificare gli argomenti plug @ no__t-0cm usando i cmdlet di PowerShell per aggiornamento compatibile con cluster che supportano il parametro **– CauPluginParameters** , passare un parametro nel formato seguente:  
  
**\-CauPluginArguments @ {name1 @ no__t-2Value1; Name2 @ no__t-3Value2; Name3 @ no__t-4Value3}**  
  
È anche possibile usare una tabella hash di PowerShell predefinita. Per specificare gli argomenti plug @ no__t-0cm per più di un plug @ no__t-1in, passare più tabelle hash di argomenti, separate da virgole. Passare gli argomenti plug @ no__t-0cm nell'ordine plug @ no__t-1in specificato in **CauPluginName**.  
  
### <a name="specify-optional-plug-in-arguments"></a>Specifica gli argomenti facoltativi plug @ no__t-0cm  
Il plug-in no__t-0ins installato da aggiornamento compatibile con cluster \(**Microsoft. WindowsUpdatePlugin** e **microsoft. HotfixPlugin**\) forniscono opzioni aggiuntive che è possibile selezionare. Nell'interfaccia utente di aggiornamento compatibile con cluster, questi vengono visualizzati in una pagina di **Opzioni aggiuntive** dopo aver configurato le opzioni di aggiornamento per il plug-in no__t-1in. Se si usano i cmdlet di PowerShell per aggiornamento compatibile con cluster, queste opzioni vengono configurate come argomenti facoltativi plug @ no__t-0cm. Per altre informazioni, vedere [Usare Microsoft.WindowsUpdatePlugin](#BKMK_WUP) e [Usare Microsoft.HotfixPlugin](#BKMK_HFP) , più avanti in questo argomento.  
  
## <a name="manage-plug-ins-using-windows-powershell-cmdlets"></a>Gestire il plug-in no__t-0ins usando i cmdlet di Windows PowerShell  
  
|Cmdlet|Descrizione|  
|----------|---------------|  
|[Get-CauPlugin](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/get-cauplugin)|Recupera informazioni su uno o più plug-in di aggiornamento software @ no__t-0ins registrati nel computer locale.|  
|[Register-CauPlugin]((https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/register-cauplugin))|Registra un plug-in di aggiornamento software di aggiornamento compatibile con cluster @ no__t-0cm nel computer locale.|  
|[Unregister-CauPlugin](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating/unregister-cauplugin)|Rimuove un plug-in di aggiornamento software @ no__t-0cm dall'elenco plug @ no__t-1ins che può essere usato da aggiornamento compatibile con cluster. **Nota:** Non è possibile annullare la registrazione del plug @ no__t-0ins installato con aggiornamento compatibile con cluster \(**Microsoft. WindowsUpdatePlugin** e **microsoft. HotfixPlugin**\).|  
  
## <a name="BKMK_WUP"></a>Utilizzo di Microsoft. WindowsUpdatePlugin  

Il plug-in predefinito no__t-0cm per aggiornamento compatibile con cluster, **Microsoft. WindowsUpdatePlugin**, esegue le azioni seguenti:
- Comunica con l'agente di Windows Update su ciascun nodo del cluster di failover per applicare gli aggiornamenti necessari per i prodotti Microsoft in esecuzione su ciascun nodo.
- Installa gli aggiornamenti del cluster direttamente da Windows Update o Microsoft Update o da un in @ no__t-0premises Windows Server Update Services \(WSUS @ no__t-2 server.
- Installa solo gli aggiornamenti selezionati, General Distribution Release \(GDR @ no__t-1. Per impostazione predefinita, il plug-in no__t-0cm applica solo aggiornamenti software importanti. Nessuna configurazione richiesta. La configurazione predefinita consente di scaricare e installare gli aggiornamenti GDR importanti. 

> [!NOTE]
> Per applicare aggiornamenti diversi dagli importanti aggiornamenti software selezionati per impostazione predefinita \(per, aggiornamenti del driver @ no__t-1, è possibile configurare un parametro opzionale plug @ no__t-2in. Per altre informazioni, vedere [Configurare la stringa di query per l'agente di Windows Update](#BKMK_QUERY).

### <a name="requirements"></a>Requisiti

- Il computer del cluster di failover e del coordinatore dell'aggiornamento remoto \(If usato @ no__t-1 deve soddisfare i requisiti per aggiornamento compatibile con cluster e la configurazione necessaria per la gestione remota elencata in [requisiti e procedure consigliate per](cluster-aware-updating-requirements.md)aggiornamento compatibile con cluster.
- Consultare [Raccomandazioni per l'applicazione di aggiornamenti Microsoft](cluster-aware-updating-requirements.md#BKMK_BP_WUA), quindi apportare le eventuali modifiche necessarie alla configurazione di Microsoft Update per i nodi del cluster di failover.
- Per ottenere risultati ottimali, è consigliabile eseguire il Best Practices Analyzer di aggiornamento compatibile con cluster \(BPA @ no__t-1 per assicurarsi che l'ambiente cluster e di aggiornamento siano configurati correttamente per l'applicazione di aggiornamenti tramite aggiornamento compatibile con cluster. Per altre informazioni, vedere [Test di disponibilità per l'aggiornamento con Aggiornamento compatibile con cluster](cluster-aware-updating-requirements.md#BKMK_BPA).

> [!NOTE]
> Gli aggiornamenti che richiedono l'accettazione delle condizioni di licenza Microsoft o l'interazione dell'utente sono esclusi e devono essere installati manualmente.

### <a name="additional-options"></a>Opzioni aggiuntive

Facoltativamente, è possibile specificare gli argomenti plug @ no__t-0cm seguenti per aumentare o limitare il set di aggiornamenti applicati dal plug @ no__t-1in:
- Per configurare il plug-in no__t-0cm in modo da applicare gli aggiornamenti consigliati oltre agli aggiornamenti importanti in ogni nodo, nell'interfaccia utente di aggiornamento compatibile con cluster, nella pagina **Opzioni aggiuntive** , selezionare l'opzione **Give me recommended Updates nello stesso modo** in cui ricevo il controllo degli aggiornamenti importanti dialogo.
<br>In alternativa, configurare l'argomento **' IncludeRecommendedUpdates ' \=' true '** plug @ no__t-2in.
- Per configurare il plug-in no__t-0cm in modo da filtrare i tipi di aggiornamenti GDR applicati a ogni nodo del cluster, specificare una stringa di query di Windows Update Agent usando un argomento **QueryString** plug @ no__t-2in. Per altre informazioni, vedere [Configurare la stringa di query per l'agente di Windows Update](#BKMK_QUERY).

### <a name="BKMK_QUERY"></a>Configurare la stringa di query dell'agente di Windows Update  
È possibile configurare un argomento plug @ no__t-0cm per il plug-in predefinito @ no__t-1in, **Microsoft. WindowsUpdatePlugin**, che è costituito da una stringa di query Windows Update Agent \(WUA @ no__t-4. Questa istruzione usa l'API agente di Windows Update (WUA) per identificare uno o più gruppi di aggiornamenti Microsoft da applicare a ogni nodo, sulla base di specifici criteri di selezione. È possibile combinare più criteri usando operatori AND od OR logici. La stringa di query WUA viene specificata in un argomento plug @ no__t-0cm, come indicato di seguito:  
  
**QueryString @ no__t-1 "Criterion1 @ no__t-2Value1 e @ no__t-3o Criterion2 @ no__t-4Value2 e @ no__t-5o..."**  
  
**Microsoft.WindowsUpdatePlugin**, ad esempio, seleziona automaticamente gli aggiornamenti importanti usando un argomento **QueryString** predefinito costruito usando i criteri **IsInstalled**, **Type**, **IsHidden** e **IsAssigned**:  
  
**QueryString @ no__t-1 "disinstallata @ no__t-20 e tipo @ no__t-3'Software ' and Hidden @ no__t-40 and unsigned @ no__t-51"**  
  
Se si specifica un argomento **QueryString** , questo viene usato al posto della **QueryString** predefinita configurata per il plug-in no__t-2in.  
  
#### <a name="example-1"></a>Esempio 1
  
Per configurare un argomento **QueryString** che installa un aggiornamento specifico come identificato da ID *f6ce46c1 @ no__t-2971c @ no__t-343f9 @ no__t-4a2aa @ no__t-5783df125f003*:  
  
**QueryString @ no__t-1 "codice UpdateID @ no__t-2'f6ce46c1 @ no__t-3971c @ no__t-443f9 @ no__t-5a2aa @ no__t-6783df125f003' e disinstallata @ no__t-70"**  
  
> [!NOTE]  
> L'esempio precedente è valido per l'applicazione di aggiornamenti tramite cluster @ no__t-0Aware Updating Wizard. Se si vuole installare un aggiornamento specifico configurando le opzioni self @ no__t-0updating con l'interfaccia utente di aggiornamento compatibile con cluster o usando il cmdlet di PowerShell **Add @ no__t-2CauClusterRole** o **set @ no__t-4CauClusterRole**, è necessario formattare il valore di codice UpdateID con due singoli caratteri @ no__t-5quote:  
>   
> **QueryString @ no__t-1 "codice UpdateID @ no__t-2''f6ce46c1 @ no__t-3971c @ no__t-443f9 @ no__t-5a2aa @ no__t-6783df125f003'' e disinstallare @ no__t-70"**  
  
#### <a name="example-2"></a>Esempio 2
  
Per configurare un argomento **QueryString** che installa solo driver:  
  
**QueryString @ no__t-1 "disinstallato @ no__t-20 e tipo @ no__t-3'Driver ' e nascosto @ no__t-40"**  
  
Per ulteriori informazioni sulle stringhe di query per il plug-in predefinito no__t-0cm, **Microsoft. WindowsUpdatePlugin**, i criteri di ricerca \(Such come **disinstallati**\) e la sintassi che è possibile includere nelle stringhe di query, vedere la sezione informazioni sui criteri di ricerca nel [riferimento all'API agente di Windows Update (WUA)](https://go.microsoft.com/fwlink/p/?LinkId=223304).  
  
## <a name="BKMK_HFP"></a>Usare Microsoft. HotfixPlugin  
È possibile usare il plug @ no__t-0cm **Microsoft. HotfixPlugin** per applicare la versione di distribuzione limitata Microsoft \(LDR @ no__t-3 updates \(also, detti hotfix, e in precedenza denominati QFE @ no__t-5 scaricabili in modo indipendente per l'indirizzo problemi specifici del software Microsoft. Il plug-in installa gli aggiornamenti da una cartella radice in una condivisione file SMB e può essere personalizzato anche per applicare un driver non @ no__t-0Microsoft, firmware e aggiornamenti del BIOS.

> [!NOTE]
> Gli hotfix sono talvolta disponibili per il download da Microsoft negli articoli della Knowledge base, ma vengono anche forniti ai clienti su un come @ no__t-0needed.

### <a name="requirements"></a>Requisiti

- Il computer del cluster di failover e del coordinatore dell'aggiornamento remoto \(If usato @ no__t-1 deve soddisfare i requisiti per aggiornamento compatibile con cluster e la configurazione necessaria per la gestione remota elencata in [requisiti e procedure consigliate per](cluster-aware-updating-requirements.md)aggiornamento compatibile con cluster.
- Consultare [Consigli per l'uso di Microsoft.HotfixPlugin](cluster-aware-updating-requirements.md#BKMK_BP_HF).
- Per ottenere risultati ottimali, è consigliabile eseguire il modello di aggiornamento compatibile con cluster Best Practices Analyzer \(BPA @ no__t-1 per verificare che il cluster e l'ambiente di aggiornamento siano configurati correttamente per l'applicazione di aggiornamenti tramite aggiornamento compatibile con cluster. Per altre informazioni, vedere [Test di disponibilità per l'aggiornamento con Aggiornamento compatibile con cluster](cluster-aware-updating-requirements.md#BKMK_BPA).
- Ottenere gli aggiornamenti dal server di pubblicazione e copiarli o estrarli in un Server Message Block \(SMB @ no__t-1 condivisione file \(hotfix cartella radice @ no__t-3 che supporta almeno SMB 2,0 e accessibile da tutti i nodi del cluster e l'aggiornamento remoto Coordinator computer \(SE aggiornamento compatibile con cluster viene usato in modalità remota @ no__t-5updating @ no__t-6. Per altre informazioni, vedere [Configurare una struttura di cartella radice dell'aggiornamento rapido](#BKMK_HF_ROOT) più avanti in questo argomento. 

    > [!NOTE]
    > Per impostazione predefinita, questo plug-in no__t-0cm installa solo aggiornamenti rapidi con le estensioni di file seguenti:. msu,. msi e. msp.

- Copiare il file DefaultHotfixConfig. XML \(which è disponibile nella cartella **% systemroot% \\System32 @ no__t-3WindowsPowerShell @ no__t-4V 1.0 @ no__t-5Modules @ no__t-6ClusterAwareUpdating** in un computer in cui si trovano gli strumenti di aggiornamento compatibile con cluster installare @ no__t-7 nella cartella radice degli aggiornamenti rapidi creata e in cui sono stati estratti gli aggiornamenti rapidi. Ad esempio, copiare il file di configurazione in *\\ @ no__t-2MyFileServer @ no__t-3Hotfixes @ no__t-4Root @ no__t-5*. 

    > [!NOTE]
    > Per installare la maggior parte degli aggiornamenti rapidi forniti da Microsoft e altri aggiornamenti, è possibile usare il file di configurazione per gli aggiornamenti rapidi predefinito senza modifiche. Se richiesto per lo scenario in uso, è possibile personalizzare il file di configurazione come attività avanzata. Il file di configurazione può includere, ad esempio, ruoli personalizzati per la gestione di file di aggiornamento rapido con estensioni specifiche oppure per definire i comportamenti per determinate condizioni di uscita. Per altre informazioni, vedere [Personalizzare il file di configurazione degli aggiornamenti rapidi](#BKMK_CONFIG_FILE) più avanti in questo argomento.

### <a name="configuration"></a>Configurazione

Configurare le opzioni descritte di seguito. Per altre informazioni, vedere i collegamenti alle sezioni più avanti in questo argomento.
- Il percorso alla cartella radice degli aggiornamenti rapidi condivisa contenente gli aggiornamenti da applicare nonché il file di configurazione degli aggiornamenti rapidi. È possibile digitare questo percorso nell'interfaccia utente di aggiornamento compatibile con cluster o configurare l'argomento **HotfixRootFolderPath @ no__t-1 @ no__t-2Path >** PowerShell plug @ no__t-3in. 

   > [!NOTE]
   > È possibile specificare la cartella radice degli aggiornamenti rapidi come un percorso di cartella locale o un percorso UNC nel formato *\\ @ no__t-2ServerName @ no__t-3Share @ no__t-4RootFolderName*. È possibile usare un percorso di spazio dei nomi DFS Domain @ no__t-0based o autonomo. Tuttavia, le funzionalità plug @ no__t-0cm che controllano le autorizzazioni di accesso nel file di configurazione degli aggiornamenti rapidi non sono compatibili con un percorso dello spazio dei nomi DFS, quindi se ne viene configurato uno, è necessario disabilitare il controllo delle autorizzazioni di accesso usando l'interfaccia utente di aggiornamento compatibile con cluster o configurando il  **Argomento disableaclchecks @ no__t-2'True '** plug @ no__t-3in argomento.
- Impostazioni nel server in cui è ospitata la cartella radice degli aggiornamenti rapidi per verificare le autorizzazioni appropriate per accedere alla cartella e garantire l'integrità dei dati a cui si accede dalla cartella condivisa SMB \(SMB signing o SMB Encryption @ no__t-1. Per altre informazioni, vedere [Limitare l'accesso alla cartella radice degli aggiornamenti rapidi](#BKMK_ACL).

### <a name="additional-options"></a>Opzioni aggiuntive

- Facoltativamente, configurare il plug-in no__t-0cm in modo che la crittografia SMB venga applicata quando si accede ai dati dalla condivisione file degli aggiornamenti rapidi. Nell'interfaccia utente di aggiornamento compatibile con cluster, nella pagina **Opzioni aggiuntive** , selezionare l'opzione **Richiedi crittografia SMB nell'accesso alla cartella radice dell'hotfix** oppure configurare l'argomento **RequireSMBEncryption @ no__t-3'True '** PowerShell plug @ no__t-4in. 
  > [!IMPORTANT]
  > È necessario eseguire passaggi di configurazione aggiuntivi nel server SMB per abilitare l'integrità dei dati SMB con firma o crittografia SMB. Per altre informazioni, vedere il passaggio 4 in [Limitare l'accesso alla cartella radice degli aggiornamenti rapidi](#BKMK_ACL). Se si seleziona l'opzione per imporre l'uso della crittografia SMB e la cartella radice degli aggiornamenti rapidi non è configurata per l'accesso mediante la crittografia SMB, l'operazione di aggiornamento non riuscirà.
- Facoltativamente, disabilitare i controlli predefiniti sulla presenza delle autorizzazioni necessarie per la cartella radice e per il file di configurazione degli aggiornamenti rapidi. Nell'interfaccia utente di aggiornamento compatibile con cluster selezionare **Disabilita verifica l'accesso amministratore alla cartella radice degli aggiornamenti rapidi e al file di configurazione**oppure configurare l'argomento **argomento disableaclchecks @ no__t-2'True '** plug @ no__t-3in.
- Facoltativamente, configurare l'argomento **argomento hotfixinstallertimeoutminutes @ no__t-1 @ no__t-2** per specificare per quanto tempo il plug-in hotfix @ no__t-3in attende che il processo del programma di installazione dell'hotfix venga restituito. il valore predefinito \(The è 30 minuti. \) Ad esempio, per specificare un periodo di timeout di due ore, impostare **argomento hotfixinstallertimeoutminutes @ no__t-1120**.
- Facoltativamente, configurare l'argomento **argomento hotfixconfigfilename \= <name>** plug @ no__t-3in per specificare un nome per il file di configurazione dell'hotfix che si trova nella cartella radice degli aggiornamenti rapidi. Se non ne viene specificato uno, verrà usato il nome predefinito DefaultHotfixConfig.xml.
  
### <a name="BKMK_HF_ROOT"></a>Configurare una struttura di cartelle radice degli aggiornamenti rapidi

Per il corretto funzionamento del plug-in hotfix @ no__t-0cm, gli hotfix devono essere archiviati in una struttura ben @ no__t-1defined in una condivisione file SMB \(hotfix cartella radice @ no__t-3 ed è necessario configurare il plug-in hotfix @ no__t-4in con il percorso della cartella radice degli aggiornamenti rapidi tramite l'interfaccia utente di aggiornamento compatibile con cluster o cmdlet di PowerShell per aggiornamento compatibile con cluster. Questo percorso viene passato al plug @ no__t-0cm come argomento **HotfixRootFolderPath** . In base alle esigenze di aggiornamento, è possibile scegliere tra diverse strutture per la cartella radice degli aggiornamenti rapidi, come illustrato negli esempi seguenti. File o cartelle non conformi a questa struttura verranno ignorati.  
  
#### <a name="example-1---folder-structure-used-to-apply-hotfixes-to-all-cluster-nodes"></a>Esempio 1: struttura di cartelle usata per applicare aggiornamenti rapidi a tutti i nodi del cluster
  
Per specificare che gli aggiornamenti rapidi si applicano a tutti i nodi del cluster, copiarli in una cartella denominata **CAUHotfix @ no__t-1all** nella cartella radice degli aggiornamenti rapidi. In questo esempio, l'argomento **HotfixRootFolderPath** plug @ no__t-1in è impostato su *\\ @ no__t-4MyFileServer @ no__t-5Hotfixes @ no__t-6Root @ no__t-7*. La cartella **CAUHotfix @ no__t-1all** contiene tre aggiornamenti con Extensions. msu,. msi e. msp che verranno applicati a tutti i nodi del cluster. I nomi file degli aggiornamenti qui riportati hanno solo scopo illustrativo.  
  
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
  
Per specificare aggiornamenti rapidi applicabili solo a un nodo specifico, usare una sottocartella della cartella radice degli aggiornamenti rapidi con il nome del nodo. Usare il nome NetBIOS del nodo del cluster, ad esempio *ContosoNode1*. Spostare quindi gli aggiornamenti applicabili solo al nodo in questione nella sottocartella. Nell'esempio seguente, l'argomento **HotfixRootFolderPath** plug @ no__t-1in è impostato su *\\ @ no__t-4MyFileServer @ no__t-5Hotfixes @ no__t-6Root @ no__t-7*. Gli aggiornamenti nella cartella **CAUHotfix @ no__t-1all** verranno applicati a tutti i nodi del cluster e *node1 @ no__t-3Specific\_Update.msu* verrà applicato solo a *ContosoNode1*.  
  
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
  
Per impostazione predefinita, **Microsoft.HotfixPlugin** applica unicamente aggiornamenti con le estensioni .msu, .msi o .msp. Alcuni aggiornamenti potrebbero tuttavia avere estensioni diverse e potrebbero richiedere comandi di installazione diversi. Ad esempio, potrebbe essere necessario applicare un aggiornamento firmware con estensione .exe a un nodo in un cluster. È possibile configurare la cartella radice degli aggiornamenti rapidi con una sottocartella che indica che deve essere installato un tipo di aggiornamento specifico, non @ no__t-0Valore predefinito. È inoltre necessario configurare una regola di installazione della cartella corrispondente che specifica il comando di installazione nell'elemento `<FolderRules>` nel file XML di configurazione degli aggiornamenti rapidi.  
  
Nell'esempio seguente, l'argomento **HotfixRootFolderPath** plug @ no__t-1in è impostato su *\\ @ no__t-4MyFileServer @ no__t-5Hotfixes @ no__t-6Root @ no__t-7*. Molti aggiornamenti verranno applicati a tutti i nodi del cluster, mentre l'aggiornamento firmware *SpecialHotfix1.exe* verrà applicato a *ContosoNode1* usando *FolderRule1*. Per informazioni sulla configurazione di *FolderRule1* nel file di configurazione degli aggiornamenti rapidi, vedere [Personalizzare il file di configurazione degli aggiornamenti rapidi](#BKMK_CONFIG_FILE) più avanti in questo argomento.  
  
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
  
**% SystemRoot% \\System32 @ no__t-2WindowsPowerShell @ no__t-3V 1.0 @ no__t-4Modules @ no__t-cartella 5ClusterAwareUpdating**  
  
Per personalizzare il file di configurazione degli aggiornamenti rapidi, copiare il file di configurazione di esempio DefaultHotfixConfig.xml da questo percorso alla cartella radice degli aggiornamenti rapidi e apportare le modifiche appropriate per il proprio scenario.  
  
> [!IMPORTANT]  
> Per applicare la maggior parte degli aggiornamenti rapidi forniti da Microsoft e altri aggiornamenti, è possibile usare il file di configurazione per gli aggiornamenti rapidi predefinito senza modifiche. La personalizzazione del file di configurazione degli aggiornamenti rapidi è un'attività destinata solo a scenari di utilizzo avanzato.  
  
Per impostazione predefinita, il file XML di configurazione degli aggiornamenti rapidi definisce regole di installazione e condizioni di uscita per le due categorie di aggiornamenti rapidi seguenti:  
  
-   File hotfix con estensioni che il plug-in no__t-0cm può installare per impostazione predefinita \(. msu,. msi e file. msp @ no__t-2.  
  
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
  
-   Aggiornamenti rapidi o altri file di aggiornamento che non sono file. msi,. msu o. msp, ad esempio, driver non @ no__t-0Microsoft, firmware e aggiornamenti del BIOS.  
  
    Ogni tipo di file non @ no__t-0Valore predefinito è configurato come elemento `<Folder>` nell'elemento `<FolderRules>`. L'attributo del nome dell'elemento `<Folder>` deve essere identico al nome di una cartella all'interno della cartella degli aggiornamenti rapidi che conterrà gli aggiornamenti del tipo corrispondente. La cartella può trovarsi nella cartella **CAUHotfix @ no__t-1all** o in una cartella node @ no__t-2specific. Se *FolderRule1* , ad esempio, è configurato nella cartella radice degli aggiornamenti rapidi, configurare l'elemento seguente nel file XML per definire un modello di installazione e delle condizioni di uscita per gli aggiornamenti in quella cartella:  
  
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
  
1.  Identificare l'account utente usato per le esecuzioni di aggiornamento usando il plug-in no__t-0cm  
  
2.  Configurare l'account utente individuato nei gruppi necessari in un file server SMB  
  
3.  Configurare le autorizzazioni per accedere alla cartella radice degli aggiornamenti rapidi  
  
4.  Configurare le impostazioni per l'integrità dei dati SMB  
  
5.  Abilitare una regola di Windows Firewall nel server SMB  
  
#### <a name="step-1-identify-the-user-account-that-is-used-for-updating-runs-by-using-the-hotfix-plug-in"></a>Passaggio 1. Identificare l'account utente usato per le esecuzioni di aggiornamento usando il plug-in hotfix @ no__t-0cm
  
L'account usato in aggiornamento compatibile con cluster per verificare le impostazioni di sicurezza durante l'esecuzione di un'operazione di aggiornamento tramite **Microsoft. HotfixPlugin** varia a seconda che aggiornamento compatibile con cluster venga usato in modalità remota @ no__t-1updating o in modalità self @ no__t-2updating, come indicato di seguito:  
  
-   **Remote @ no__t-modalità 1updating** Account con privilegi amministrativi sul cluster per visualizzare in anteprima e applicare gli aggiornamenti.  
  
-   **Modalità self @ no__t-1updating** Nome dell'oggetto computer virtuale configurato in Active Directory per il ruolo del cluster di aggiornamento compatibile con cluster. Si tratta del nome di un oggetto computer virtuale preinstallato in Active Directory per il ruolo del cluster di Aggiornamento compatibile con cluster o del nome generato da quest'ultimo per il ruolo del cluster. Per ottenere il nome se viene generato da aggiornamento compatibile con cluster, eseguire il cmdlet PowerShell **Get @ no__t-1CauClusterRole** di aggiornamento compatibile con cluster. Nell'output **ResourceGroupName** è il nome dell'account dell'oggetto computer virtuale generato.  
  
#### <a name="step-2-configure-this-user-account-in-the-necessary-groups-on-an-smb-file-server"></a>Passaggio 2. Configurare l'account utente individuato nei gruppi necessari in un file server SMB
  
> [!IMPORTANT]  
> È necessario aggiungere l'account usato per le operazioni di aggiornamento come account Administrator locale nel server SMB. Se ciò non viene consentito a causa dei criteri di sicurezza dell'organizzazione, configurare questo account con le autorizzazioni necessarie sul server SMB usando la procedura riportata di seguito.  
  
##### <a name="to-configure-a-user-account-on-the-smb-server"></a>Per configurare un account utente sul server SMB  
  
1.  Aggiungere l'account usato per le operazioni di aggiornamento al gruppo Distributed COM Users e a uno dei gruppi seguenti: Power User, Server operator o Print operator.  
  
2.  Per abilitare le autorizzazioni WMI necessarie per l'account, avviare la console di gestione WMI nel server SMB. Avviare PowerShell e digitare il comando seguente:  
  
    ```  
    wmimgmt.msc  
    ```  
  
3.  Nell'albero della console fare clic con il pulsante destro del mouse su controllo WMI no__t-0click **\(Local @ no__t-3**, quindi fare clic su **Proprietà**.  
  
4.  Fare clic su **Sicurezza**, quindi espandere **Radice**.  
  
5.  Fare clic su **CIMV2**, quindi su **Sicurezza**.  
  
6.  Aggiungere l'account usato per le operazioni di aggiornamento all'elenco **Utenti e gruppi**.  
  
7.  Concedere le autorizzazioni **Esegui metodi** e **Abilita remoto** all'account usato per le operazioni di aggiornamento.  
  
#### <a name="step-3-configure-permissions-to-access-the-hotfix-root-folder"></a>Passaggio 3. Configurare le autorizzazioni per accedere alla cartella radice degli aggiornamenti rapidi
  
Per impostazione predefinita, quando si tenta di applicare gli aggiornamenti, il plug-in hotfix @ no__t-0cm verifica la configurazione delle autorizzazioni di file system NTFS per l'accesso alla cartella radice degli aggiornamenti rapidi. Se le autorizzazioni di accesso alla cartella non sono configurate correttamente, un'operazione di aggiornamento tramite il plug-in hotfix @ no__t-0cm potrebbe non riuscire.  
  
Se si usa la configurazione predefinita del plug-in hotfix @ no__t-0cm, verificare che le autorizzazioni di accesso alla cartella soddisfino i requisiti seguenti.  
  
-   Il gruppo Users deve disporre delle autorizzazioni di lettura.  
  
-   Se il plug-in no__t-0cm applicherà gli aggiornamenti con estensione exe, il gruppo Users avrà l'autorizzazione Execute.  
  
-   Sono consentite solo determinate entità di sicurezza \(but non sono necessarie @ no__t-1 per avere l'autorizzazione di scrittura o modifica. Le entità di sicurezza ammesse sono il gruppo Administrators locale, SYSTEM, CREATOR OWNER e TrustedInstaller. Nessun altro account o gruppo può disporre di autorizzazioni di scrittura o modifica per la cartella radice degli aggiornamenti rapidi.  
  
Facoltativamente, è possibile disabilitare i controlli precedenti eseguiti dal plug-in no__t-0cm per impostazione predefinita. Puoi eseguire questa operazione in due modi:  
  
-   Se si usano i cmdlet di PowerShell per aggiornamento compatibile con cluster, configurare l'argomento **argomento disableaclchecks @ no__t-1'True '** nel parametro **CauPluginArguments** per il plug-in hotfix @ no__t-3in.  
  
-   Se si sta usando l'interfaccia utente di Aggiornamento compatibile con cluster, selezionare l'opzione **Disabilita controllo accesso amministratore alla cartella radice degli aggiornamenti rapidi e al file di configurazione** nella pagina **Opzioni di aggiornamento aggiuntive** della procedura guidata per la configurazione delle opzioni relative alle operazioni di aggiornamento.  
  
Come procedura consigliata per molti ambienti si raccomanda tuttavia l'impiego della configurazione predefinita che prevede l'imposizione di tali controlli.  
  
#### <a name="step-4-configure-settings-for-smb-data-integrity"></a>Passaggio 4. Configurare le impostazioni per l'integrità dei dati SMB
  
Per verificare l'integrità dei dati nelle connessioni tra i nodi del cluster e la condivisione file SMB, il plug-in hotfix @ no__t-0cm richiede l'abilitazione delle impostazioni nella condivisione file SMB per la firma SMB o la crittografia SMB. La crittografia SMB, che fornisce sicurezza avanzata e prestazioni migliori in molti ambienti, è supportata a partire da Windows Server 2012. È possibile abilitare una di queste impostazioni, o entrambe, come indicato di seguito:  
  
-   Per abilitare la firma SMB, vedere la procedura nell' [articolo 887429](https://support.microsoft.com/kb/887429) della Microsoft Knowledge Base.  
  
-   Per abilitare la crittografia SMB per la cartella SMB condivisa, eseguire il seguente cmdlet di PowerShell nel server SMB:  
  
    ```PowerShell  
    Set-SmbShare <ShareName> -EncryptData $true  
    ```  
  
    Dove <*ShareName*> è il nome della cartella SMB condivisa.  
  
Facoltativamente, per imporre l'uso della crittografia SMB nelle connessioni al server SMB, selezionare l'opzione **Richiedi crittografia SMB nell'accesso alla cartella radice degli aggiornamenti rapidi** nell'interfaccia utente di aggiornamento compatibile con cluster o configurare il **RequireSMBEncryption @ no__t-2'True '** plug @ no__ argomento t-3in usando i cmdlet di PowerShell per aggiornamento compatibile con cluster.  
  
> [!IMPORTANT]  
> Se si seleziona l'opzione per imporre l'uso della crittografia SMB e la cartella radice degli aggiornamenti rapidi non è configurata per connessioni che usano la crittografia SMB, l'operazione di aggiornamento non riuscirà.  
  
#### <a name="step-5-enable-a-windows-firewall-rule-on-the-smb-server"></a>Passaggio 5. Abilitare una regola di Windows Firewall nel server SMB
  
È necessario abilitare la regola **gestione remota file Server \(SMB @ no__t-2in @ no__t-3** in Windows Firewall nel file server SMB. Questa funzionalità è abilitata per impostazione predefinita in Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Panoramica di aggiornamento compatibile con cluster](cluster-aware-updating.md)
  
-   [Cmdlet di Windows PowerShell per aggiornamento compatibile con cluster](https://docs.microsoft.com/en-us/powershell/module/clusterawareupdating)  
  
-   [Riferimento al plug-in di aggiornamento compatibile con cluster](https://msdn.microsoft.com/library/hh418084.aspx)  
  
