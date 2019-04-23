---
title: Configurare e gestire il quorum in un cluster di failover
description: Informazioni dettagliate su come gestire il quorum del cluster in un cluster di failover di Windows Server.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 01/18/2019
ms.localizationpriority: medium
ms.openlocfilehash: 21f99362205e0a6aa90ebd26cef8f3a779bdc1dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836492"
---
# <a name="configure-and-manage-quorum"></a>Configurare e gestire il quorum

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento fornisce informazioni generali e procedure per configurare e gestire il quorum in un cluster di failover di Windows Server.

## <a name="understanding-quorum"></a>Quorum conoscenza

Il quorum per un cluster è determinato dal numero di elementi di voto che devono essere inclusi tra i membri del cluster attivo in modo che tale cluster possa essere avviato correttamente o che sia possibile continuarne l'esecuzione. Per una spiegazione più dettagliata, vedere la [doc di quorum di cluster e pool understanding](../storage/storage-spaces/understand-quorum.md).

## <a name="quorum-configuration-options"></a>Opzioni di configurazione quorum

Il modello di quorum in Windows Server è flessibile. Se è necessario modificare la configurazione del quorum per il cluster, è possibile usare la configurazione guidata quorum del Cluster o i cmdlet di PowerShell di Windows Failover cluster. Per procedure e considerazioni sulla configurazione quorum, vedere [Configurare il quorum del cluster](#configure-the-cluster-quorum) più avanti in questo argomento.

La tabella seguente elenca le tre opzioni di configurazione quorum disponibili nella Configurazione guidata quorum del cluster.

<table>
<thead>
<tr class="header">
<th>Opzione</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Usa impostazioni tipiche</td>
<td>Il cluster assegna automaticamente un voto a ogni nodo e gestisce in modo dinamico i voti del nodo. Se appropriato per il cluster in uso e se è disponibile l'archiviazione condivisa cluster, il cluster selezionerà un disco di controllo. Questa opzione è consigliata nella maggior parte dei casi, perché il software del cluster sceglie automaticamente una configurazione di quorum e controllo che fornisce la massima disponibilità per il cluster.</td>
</tr>
<tr class="even">
<td>Aggiungi o modifica il quorum di controllo</td>
<td>È possibile aggiungere, modificare o rimuovere una risorsa di controllo. È possibile configurare una condivisione file o un disco di controllo. Il cluster assegna automaticamente un voto a ogni nodo e gestisce in modo dinamico i voti del nodo.</td>
</tr>
<tr class="odd">
<td>Configurazione quorum avanzata e selezione controllo</td>
<td>Selezionare questa opzione solo se esistono requisiti specifici dell'applicazione o del sito per la configurazione quorum. È possibile modificare il quorum di controllo, aggiungere o rimuovere i voti dei nodi e scegliere se il cluster potrà gestire in modo dinamico i voti dei nodi. Per impostazione predefinita, i voti sono assegnati a tutti i nodi e i voti dei nodi sono gestiti in modo dinamico.</td>
</tr>
</tbody>
</table>

In base all'opzione di configurazione quorum selezionata e dalle impostazioni specifiche, il cluster sarà configurato in una delle modalità quorum seguenti:

<table>
<thead>
<tr class="header">
<th>Modalità</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Maggioranza dei nodi (senza controllo)</td>
<td>Solo i nodi dispongono di voti. Non è configurato alcun quorum di controllo. Il quorum del cluster corrisponde alla maggioranza dei nodi votanti tra i membri del cluster attivo.</td>
</tr>
<tr class="even">
<td>Maggioranza dei nodi con controllo (disco o condivisione file)</td>
<td>I nodi dispongono di voti. Il quorum di controllo, inoltre, dispone di un voto. Il quorum del cluster corrisponde alla maggioranza dei nodi votanti tra i membri del cluster attivo più un voto del disco di controllo.<br />
<br />
Un quorum di controllo può essere un disco di controllo designato o una condivisione file di controllo designata.</td>
</tr>
<tr class="odd">
<td>Nessuna maggioranza (solo disco di controllo)</td>
<td>Nessun nodo dispone di voti. Solo un disco di controllo dispone di un voto. Il quorum del cluster è determinato dallo stato del disco di controllo.<br />
<br />
Il cluster raggiunge il quorum se un nodo è disponibile e se comunica con un disco specifico nell'archiviazione cluster. In genere, questa modalità non è consigliata ed è opportuno non selezionarla, poiché crea un singolo punto di errore per il cluster.</td>
</tr>
</tbody>
</table>

Le sottosezioni seguenti offrono altre informazioni sulle impostazioni di configurazione quorum avanzata.

### <a name="witness-configuration"></a>Configurazione del controllo

In genere, quando si configura un quorum è consigliabile che il numero di elementi votanti nel cluster sia dispari. Se quindi il cluster include un numero pari di nodi votanti, è consigliabile configurare un disco di controllo o una condivisione file di controllo. Il cluster sarà in grado di sostenere un nodo aggiuntivo inattivo. L'aggiunta di un voto di controllo permette inoltre al cluster di continuare l'esecuzione in caso di inattività o disconnessione della metà dei nodi del cluster.

Un disco di controllo è in genere consigliato se tutti i nodi possono vedere il disco. Una condivisione file di controllo è consigliata quando è necessario prendere in considerazione il ripristino di emergenza multisito con archiviazione replicata. La configurazione di un disco di controllo con la risorsa di archiviazione replicata è possibile solo se il fornitore del sistema di archiviazione supporta l'accesso in lettura/scrittura da tutti i siti all'archiviazione replicata. <strong>*Un disco di controllo non è supportato con spazi di archiviazione diretta*</strong>.

La tabella seguente include informazioni e considerazioni aggiuntive sui tipi di quorum di controllo.

<table>
<thead>
<tr class="header">
<th>Tipo di controllo</th>
<th>Descrizione</th>
<th>Requisiti e consigli</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Disco di controllo</td>
<td>- LUN dedicato che archivia una copia del database del cluster<br />
- Particolarmente utile per i cluster con archiviazione condivisa (non replicata)</td>
<td>- Le dimensioni di LUN devono essere almeno 512 MB<br />
- Deve essere dedicato da usare nel cluster e non assegnati a un ruolo del cluster<br />
- Deve essere incluso nei cluster archiviazione e passaggio di convalida dei test di archiviazione<br />
- Non può essere un disco che è un Volume condiviso Cluster (CSV)<br />
- Disco di base con un singolo volume<br />
- Non è necessario disporre di una lettera di unità<br />
- Può essere formattato con NTFS o ReFS<br />
- Può essere facoltativamente configurato con RAID hardware per tolleranza di errore<br />
- Devono essere esclusi dal backup e scansione antivirus<br />
- Un disco di controllo non è supportato con spazi di archiviazione diretta</td>
</tr>
<tr class="even">
<td>Condivisione file di controllo</td>
<td>- Condivisione file SMB configurata in un file server che esegue Windows Server<br />
- Non archivia una copia del database del cluster<br />
- Mantiene le informazioni del cluster solo in un file witness.<br />
- Particolarmente utile per i cluster multisito con archiviazione replicata</td>
<td>- Deve avere almeno 5 MB di spazio libero<br />
- Deve essere dedicata al singolo cluster e non usati per archiviare i dati utente o un'applicazione<br />
- Devono disporre delle autorizzazioni di scrittura abilitato per l'oggetto computer per il nome del cluster<br />
<br />
Di seguito sono riportate alcune considerazioni aggiuntive per un file server che ospita la condivisione file di controllo:<br />
<br />
- Un singolo file server può essere configurato con condivisioni file di controllo per più cluster.<br />
- Il file server deve essere in un sito separato dal carico di lavoro del cluster. Ciò offre a qualsiasi sito di cluster uguali opportunità di sopravvivere in caso di perdita di comunicazioni di rete da sito a sito. Se il file server si trova nello stesso sito, tale sito diventa il sito primario e sarà l'unico sito in grado di raggiungere la condivisione file.<br />
- Il server di file eseguibili in una macchina virtuale se la macchina virtuale non è ospitata nello stesso cluster che usa il controllo di condivisione file.<br />
- Per la disponibilità elevata, il server di file può essere configurato in un cluster di failover separato.</td>
</tr>

<tr class-"odd">
<td>Cloud di controllo</td>
<td>- Un file di controllo archiviato nell'archivio blob di Azure<br>
-Consigliabile quando tutti i server del cluster dispongano di una connessione Internet affidabile.</td>
<td>Visualizzare <a href="https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness">distribuire un cloud di controllo</a>.</td>
<td>
</td>
</tr>

</tbody>
</table>

### <a name="node-vote-assignment"></a>Assegnazione di voti al nodo

Come opzione di configurazione quorum avanzata, è possibile scegliere di riassegnare o rimuovere voti del quorum in base al nodo. Per impostazione predefinita, i voti sono assegnati a tutti i nodi. Indipendentemente dall'assegnazione dei voti, tutti i nodi continuano a funzionare nel cluster, ricevono aggiornamenti del database cluster e possono ospitare applicazioni.

È consigliabile rimuovere voti dai nodi in determinate configurazioni di ripristino di emergenza. Ad esempio, in un cluster multisito, è possibile rimuovere voti dai nodi in un sito di backup, in modo che tali nodi non influiscano sui calcoli del quorum. Questa configurazione è consigliata solo per il failover manuale tra siti. Per altre informazioni, vedere [Considerazioni relative al quorum per configurazioni di ripristino di emergenza](#quorum-considerations-for-disaster-recovery-configurations) più avanti in questo argomento.

Il voto configurato di un nodo può essere verificato cercando la **NodeWeight** proprietà comuni del nodo del cluster tramite il [Get-ClusterNode](http://technet.microsoft.com/library/hh847268.aspx)cmdlet di Windows PowerShell. Un valore pari a 0 indica che per il nodo non è stato configurato alcun voto di quorum. Un valore pari a 1 indica che il voto di quorum del nodo è stato assegnato ed è gestito dal cluster. Per altre informazioni sulla gestione dei voti di nodo, vedere [Gestione dinamica del quorum](#dynamic-quorum-management) più avanti in questo argomento.

L'assegnazione di voti per tutti i nodi del cluster può essere verificata tramite il test di convalida **Convalida del quorum del cluster**.

#### <a name="additional-considerations-for-node-vote-assignment"></a>Considerazioni aggiuntive per l'assegnazione di voti di nodo

  - L'assegnazione di voti ai nodi non è consigliata per imporre un numero dispari di nodi votanti. Configurare invece un disco di controllo o una condivisione file di controllo. Per altre informazioni, vedere [configurazione del controllo](#witness-configuration) più avanti in questo argomento.
  - Se la gestione dinamica del quorum è abilitata, sarà possibile assegnare o rimuovere voti in modo dinamico solo per i nodi configurati per l'assegnazione di voti di nodo. Per altre informazioni, vedere [Gestione dinamica del quorum](#dynamic-quorum-management) più avanti in questo argomento.

### <a name="dynamic-quorum-management"></a>Gestione dinamica del quorum

In Windows Server 2012, come opzione di configurazione quorum avanzata, è possibile scegliere di abilitare la gestione dinamica del quorum del cluster. Per altre informazioni dettagliate sul funzionamento del quorum dinamico, vedere [questa spiegazione](../storage/storage-spaces/understand-quorum.md#dynamic-quorum-behavior).

La gestione dinamica del quorum permette anche l'esecuzione del cluster sull'ultimo nodo del cluster attivo. Grazie alla regolazione dinamica dei requisiti di maggioranza del quorum, il cluster può sostenere arresti sequenziali dei nodi fino a un singolo nodo.

Il voto dinamico assegnato di un nodo può essere verificato con il **DynamicWeight** proprietà comuni del nodo del cluster tramite il [Get-ClusterNode](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode?view=win10-ps) cmdlet di Windows PowerShell. Un valore pari a 0 indica che il nodo non dispone di alcun voto di quorum. Un valore pari a 1 indica che il nodo dispone di un voto di quorum.

L'assegnazione di voti per tutti i nodi del cluster può essere verificata tramite il test di convalida **Convalida del quorum del cluster**.

#### <a name="additional-considerations-for-dynamic-quorum-management"></a>Considerazioni aggiuntive per la gestione dinamica del quorum

- La gestione dinamica del quorum non permette al cluster di sostenere errori simultanei della maggioranza dei membri votanti. Per continuare l'esecuzione, il cluster deve sempre disporre di una maggioranza di quorum al momento dell'arresto o dell'errore di un nodo.

- Se il voto di un nodo è stato rimosso esplicitamente, il cluster non potrà aggiungere o rimuovere in modo dinamico tale voto.
- Quando spazi di archiviazione diretta è abilitato, il cluster può supportare solo due errori di nodo. Questo aspetto viene spiegato più il [sezione quorum del pool](../storage/storage-spaces/understand-quorum.md#poolQuorum)

## <a name="general-recommendations-for-quorum-configuration"></a>Indicazioni generali per la configurazione quorum

Il software del cluster configura automaticamente il quorum per un nuovo cluster, in base al numero di nodi configurati e la disponibilità dell'archiviazione condivisa. Questa è in genere la configurazione quorum più appropriata per questo tipo di cluster. È comunque consigliabile verificare la configurazione quorum dopo la creazione del cluster prima di passare il cluster in produzione. Per visualizzare la configurazione di quorum del cluster dettagliata, è possibile usare la convalida guidata configurazione, o la [Test-Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps) cmdlet di Windows PowerShell per eseguire il **convalida configurazione Quorum** di test. In Gestione Cluster di Failover, la configurazione di base del quorum è visualizzata nelle informazioni di riepilogo per il cluster selezionato oppure è possibile esaminare le informazioni sulle risorse del quorum restituite quando si esegue la [Get-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusterquorum?view=win10-ps) Cmdlet di Windows PowerShell.

In qualsiasi momento è possibile eseguire il test **Convalida configurazione quorum** per confermare che la configurazione del quorum è ottimale per il cluster. L'output del test indica se è consigliabile una modifica nella configurazione quorum e mostra le impostazioni ottimali. Se è consigliata una modifica, è possibile usare la Configurazione guidata quorum del cluster per applicare le impostazioni consigliate.

Quando il cluster è in produzione, modificare la configurazione quorum solo se è stato stabilito che la modifica è appropriata per il cluster. Prendere in considerazione la modifica della configurazione quorum nelle situazioni seguenti:

- Aggiunta o eliminazione di nodi
- Aggiunta o rimozione di risorse di archiviazione
- Errore a lungo termine di nodi o controlli
- Ripristino di un cluster in uno scenario di ripristino di emergenza multisito

Per altre informazioni sulla convalida del cluster di failover, vedere [Convalidare l'hardware per un cluster di failover](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

## <a name="configure-the-cluster-quorum"></a>Configurare il quorum del cluster

È possibile configurare le impostazioni del quorum del cluster usando Gestione Cluster di Failover o i cmdlet di PowerShell di Windows Failover cluster.

>[!IMPORTANT]
>È in genere consigliabile usare la configurazione quorum suggerita dalla Configurazione guidata quorum del cluster. Personalizzare la configurazione quorum solo se la modifica è appropriata per il cluster. Per altre informazioni, vedere [Indicazioni generali per la configurazione del quorum](#general-recommendations-for-quorum-configuration) in questo argomento.

### <a name="configure-the-cluster-quorum-settings"></a>Configurare le impostazioni del quorum del cluster

Per eseguire questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** locale o a un gruppo equivalente in ogni server cluster. L'account deve essere inoltre un account utente di dominio.

>[!NOTE]
>È possibile modificare la configurazione quorum del cluster senza arrestare il cluster o portare offline le risorse del cluster.

### <a name="change-the-quorum-configuration-in-a-failover-cluster-by-using-failover-cluster-manager"></a>Modificare la configurazione del quorum in un cluster di failover usando Gestione Cluster di Failover

1. In Gestione cluster di failover selezionare o specificare il cluster da modificare.
2. Con il cluster selezionato, in **azioni**, selezionare **altre azioni**, quindi selezionare **Configura impostazioni quorum del Cluster**. Sarà visualizzata la Configurazione guidata quorum del cluster. Selezionare **Avanti**.
3. Nella pagina **Selezione opzione configurazione quorum** selezionare una delle tre opzioni di configurazione e completare i passaggi relativi all'opzione selezionata. Prima di configurare le impostazioni del quorum, è possibile verificare le opzioni disponibili. Per altre informazioni sulle opzioni, vedere [Panoramica del quorum in un cluster di failover](#overview-of-the-quorum-in-a-failover-cluster), più indietro in questo argomento.

    - Per consentire al cluster di reimpostare automaticamente le impostazioni del quorum ottimali per la configurazione del cluster corrente, selezionare **Usa impostazioni tipiche** e quindi completare la procedura guidata.
    - Per aggiungere o modificare il quorum di controllo, selezionare **Aggiungi o modifica quorum di controllo**e quindi completare la procedura seguente. Per informazioni e considerazioni sulla configurazione di un quorum di controllo, vedere [Configurazione del disco di controllo](#witness-configuration) in precedenza in questo argomento.

      1. Nella pagina **Selezione quorum di controllo** selezionare un'opzione per configurare un disco di controllo o una condivisione file di controllo. Nella procedura guidata sono indicate le opzioni di selezione del controllo consigliate per il cluster.

          >[!NOTE]
          >È anche possibile selezionare **Non configurare quorum di controllo** e quindi completare la procedura guidata. Se il cluster include un numero pari di nodi votanti, è possibile che questa non sia una configurazione consigliata.

      2. Se si seleziona l'opzione per la configurazione di un disco di controllo, nella pagina **Configurazione risorsa di archiviazione di controllo** selezionare il volume di archiviazione da assegnare come disco di controllo e quindi completare la procedura guidata.
      3. Se si seleziona l'opzione per la configurazione di una condivisione file di controllo, nella pagina **Configurazione condivisione file di controllo** digitare o selezionare una condivisione file che sarà usata come risorsa di controllo, quindi completare la procedura guidata.

    - Per configurare le impostazioni di gestione del quorum e per aggiungere o modificare il quorum di controllo, selezionare **selezione di controllo del mirroring e configurazione quorum avanzata**e quindi completare la procedura seguente. Per informazioni e considerazioni sulle impostazioni avanzate per la configurazione quorum, vedere [Assegnazione di voti al nodo](#node-vote-assignment) e [Gestione dinamica del quorum](#dynamic-quorum-management) nelle sezioni precedenti di questo argomento.

      1. Nella pagina **Selezione configurazione di voto** selezionare un'opzione per l'assegnazione di voti ai nodi. Per impostazione predefinita, un voto è assegnato a tutti i nodi. Per determinati scenari è tuttavia possibile assegnare voti solo a un subset di nodi.

          >[!NOTE]
          >È anche possibile selezionare **Nessun nodo**. Questa opzione non è in genere consigliata perché non permette ai nodi di prendere parte alla votazione per il quorum e richiede la configurazione di un disco di controllo. Il disco di controllo diventa il singolo punto di errore per il cluster.

      2. Nella pagina **Configurazione gestione quorum** è possibile abilitare o disabilitare l'opzione **Consenti al cluster di gestire dinamicamente l'assegnazione dei voti dei nodi**. La selezione di questa opzione permette in genere di aumentare la disponibilità del cluster. Questa opzione è abilitata per impostazione predefinita ed è consigliabile non disabilitarla. L'opzione permette la continuazione dell'esecuzione del cluster in scenari di errore e ciò non sarebbe possibile con l'opzione deselezionata.
      3. Nella pagina **Selezione quorum di controllo** selezionare un'opzione per configurare un disco di controllo o una condivisione file di controllo. Nella procedura guidata sono indicate le opzioni di selezione del controllo consigliate per il cluster.

          >[!NOTE]
          >È anche possibile selezionare **Non configurare quorum di controllo**e quindi completare la procedura guidata. Se il cluster include un numero pari di nodi votanti, è possibile che questa non sia una configurazione consigliata.

      4. Se si seleziona l'opzione per la configurazione di un disco di controllo, nella pagina **Configurazione risorsa di archiviazione di controllo** selezionare il volume di archiviazione da assegnare come disco di controllo e quindi completare la procedura guidata.
      5. Se si seleziona l'opzione per la configurazione di una condivisione file di controllo, nella pagina **Configurazione condivisione file di controllo** digitare o selezionare una condivisione file che sarà usata come risorsa di controllo, quindi completare la procedura guidata.

4. Selezionare **Avanti**. Confermare le selezioni nella pagina di conferma visualizzata e quindi selezionare **successivo**.

Dopo l'esecuzione della procedura guidata e il **Summary** verrà visualizzata la pagina, se si desidera visualizzare un report delle attività eseguite dalla procedura guidata, selezionare **Visualizza Report**. Il report più recente rimarrà nella *systemroot * * *\\Cluster\\rapporti** cartella con il nome **Quorumconfiguration**.

>[!NOTE]
>Dopo la configurazione del quorum del cluster, è consigliabile eseguire il test **Convalida configurazione quorum** per verificare le impostazioni aggiornate del quorum.

### <a name="windows-powershell-equivalent-commands"></a>Comandi equivalenti di Windows PowerShell

Gli esempi seguenti illustrano come usare il [Set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum?view=win10-ps) cmdlet e altri cmdlet di Windows PowerShell per configurare il quorum del cluster.

Nell'esempio seguente la configurazione del quorum sul cluster *CONTOSO-FC1* viene cambiata in una semplice configurazione a maggioranza di nodi senza quorum di controllo.

```PowerShell
Set-ClusterQuorum –Cluster CONTOSO-FC1 -NodeMajority
```

L'esempio seguente modifica la configurazione quorum nel cluster locale in una configurazione a maggioranza di nodi con configurazione di controllo. La risorsa disco con nome *Cluster Disk 2* è configurata come disco di controllo.

```PowerShell
Set-ClusterQuorum -NodeAndDiskMajority "Cluster Disk 2"
```

L'esempio seguente modifica la configurazione quorum nel cluster locale in una configurazione a maggioranza di nodi con configurazione di controllo. La risorsa di condivisione file denominata  *\\ \\CONTOSO-FS\\fsw* è configurato come controllo di condivisione file.

```PowerShell
Set-ClusterQuorum -NodeAndFileShareMajority "\\fileserver\fsw"
```

Nell'esempio seguente il voto di quorum viene rimosso dal nodo *ContosoFCNode1* nel cluster locale.

```PowerShell
(Get-ClusterNode ContosoFCNode1).NodeWeight=0
```

Nell'esempio seguente il voto di quorum viene aggiunto al nodo *ContosoFCNode1* nel cluster locale.

```PowerShell
(Get-ClusterNode ContosoFCNode1).NodeWeight=1
```

L'esempio seguente abilita la proprietà **DynamicQuorum** del cluster *CONTOSO-FC1* (se era disabilitata):

```PowerShell
(Get-Cluster CONTOSO-FC1).DynamicQuorum=1
```

## <a name="recover-a-cluster-by-starting-without-quorum"></a>Ripristinare un cluster tramite l'avvio senza quorum

Un cluster che non ha abbastanza voti di quorum non sarà avviato. È prima di tutto necessario confermare la configurazione quorum del cluster ed esaminare le ragioni per cui il cluster non dispone più di quorum. Questa situazione può verificarsi se alcuni nodi hanno smesso di rispondere o se il sito primario non è raggiungibile in un cluster multisito. Dopo avere identificato la causa principale dell'errore del cluster, sarà possibile usare i passaggi per il ripristino illustrati in questa sezione.

>[!NOTE]
> * Se il Servizio cluster si arresta a causa della perdita del quorum, nel registro di sistema compare l'ID evento 1177.
> * È sempre necessario scoprire le cause della perdita del quorum del cluster.
> * È preferibile ripristinare sempre l'integrità di un nodo o di un quorum di controllo (appartenenza al cluster) piuttosto che avviare il cluster senza quorum.

### <a name="force-start-cluster-nodes"></a>Imporre l'avvio dei nodi del cluster

Dopo avere determinato che non è possibile ripristinare il cluster riportando i nodi o il quorum di controllo a uno stato di integrità, sarà necessario imporre l'avvio del cluster. Se si impone l'avvio del cluster, le impostazioni della configurazione del quorum del cluster saranno ignorate e il cluster sarà avviato in modalità **ForceQuorum**.

L'avvio forzato di un cluster che non dispone di quorum potrebbe essere particolarmente utile in un cluster multisito. Prendere in considerazione uno scenario di ripristino di emergenza con un cluster che contiene siti primari e di backup in posizioni distinte, ovvero, *SiteA* e *SiteB*. In caso di emergenza effettiva in *SiteA*, potrebbe essere necessaria una quantità significativa di tempo per il ritorno online del sito. É quindi probabile che si voglia imporre a *SiteB* di passare online, anche senza quorum.

Quando si avvia un cluster in modalità **ForceQuorum** e dopo che il cluster ottiene di nuovo un numero sufficiente di voti di quorum, il cluster lascia automaticamente lo stato forzato e si comporta normalmente. Non è quindi necessario riavviare di nuovo il cluster normalmente. Se il cluster perde un nodo e perde il quorum, torna offline poiché non si trova più nello stato forzato. Per attivare la modalità online quando non dispone di quorum è necessario imporre l'avvio senza quorum del cluster.

>[!IMPORTANT]
>* Dopo l'avvio forzato di un cluster, l'amministratore avrà controllo completo sul cluster.
>* Il cluster usa la configurazione del cluster nel nodo in cui è stato eseguito l'avvio forzato del cluster e ne esegue la replica in tutti gli altri nodi disponibili.
>* Se si impone l'avvio del cluster senza quorum, tutte le impostazioni di configurazione del quorum saranno ignorate finché il cluster rimane in modalità **ForceQuorum**. Ciò include le impostazioni specifiche per le assegnazioni di voti di nodo e per la gestione dinamica del quorum.

### <a name="prevent-quorum-on-remaining-cluster-nodes"></a>Impedire il quorum nei nodi rimanenti del cluster

Dopo l'avvio forzato del cluster in un nodo, è necessario avviare eventuali nodi rimanenti nel cluster con un'impostazione per impedire il quorum. Un nodo avviato con un'impostazione per impedire il quorum indica al Servizio cluster di aggiungere un cluster in esecuzione esistente invece di formare una nuova istanza del cluster. Ciò impedisce ai nodi rimanenti di formare un cluster diviso che contiene due istanze concorrenti.

Ciò risulta necessario quando si deve ripristinare il cluster in alcuni scenari di ripristino di emergenza multisito, dopo l'avvio forzato del cluster nel sito di backup, *SiteB*. Per aggiungere il cluster avviato in modo forzato in *SiteB*, è necessario avviare i nodi nel sito primario, *SiteA*, dopo averne impedito il quorum.

>[!IMPORTANT]
>Dopo l'avvio forzato di un cluster in un nodo, è consigliabile avviare sempre i nodi rimanenti dopo averne impedito il quorum.

Di seguito viene illustrato come ripristinare il cluster con gestione Cluster di Failover:

1. In Gestione cluster di failover selezionare o specificare il cluster da ripristinare.
2. Con il cluster selezionato, in **azioni**, selezionare **forza avvio del Cluster**.

    Gestione cluster di failover impone l'avvio del cluster in tutti i nodi raggiungibili. Il cluster usa la configurazione corrente del cluster all'avvio.

>[!NOTE]
>* Per forzare l'avvio in un nodo specifico contenente la configurazione del cluster che si desidera usare il cluster, è necessario usare i cmdlet di Windows PowerShell o strumenti da riga di comando equivalenti, come illustrato dopo questa procedura. 
> * Se si usa Gestione cluster di failover per la connessione a un cluster avviato in modo forzato e si usa l'azione **Avvia Servizio cluster** per avviare un nodo, il nodo sarà avviato automaticamente con l'impostazione che impedisce il quorum.

#### <a name="windows-powershell-equivalent-commands-start-clusternode"></a>Comandi equivalenti di Windows PowerShell (Start-Clusternode)

L'esempio seguente illustra come usare il cmdlet **Start-ClusterNode** per imporre l'avvio del cluster nel nodo *ContosoFCNode1*.

```PowerShell
Start-ClusterNode –Node ContosoFCNode1 –FQ
```

In alternativa, è possibile digitare il comando seguente localmente nel nodo:

```PowerShell
Net Start ClusSvc /FQ
```

L'esempio seguente illustra come usare il cmdlet **Start-ClusterNode** per avviare il Servizio cluster dopo averne impedito il quorum nel nodo *ContosoFCNode1*.

```PowerShell
Start-ClusterNode –Node ContosoFCNode1 –PQ
```

In alternativa, è possibile digitare il comando seguente localmente nel nodo:

```PowerShell
Net Start ClusSvc /PQ
```

## <a name="quorum-considerations-for-disaster-recovery-configurations"></a>Considerazioni relative al quorum per configurazioni di ripristino di emergenza

In questa sezione sono riepilogate le caratteristiche e le configurazioni quorum per due configurazioni di cluster multisito in distribuzioni di ripristino di emergenza. Le indicazioni relative alla configurazione quorum dipendono dalla necessità o meno di failover automatico o failover manuale per carichi di lavoro tra i siti. La configurazione dipende in genere dai contratti di servizio disponibili nell'organizzazione per offrire e supportare carichi di lavoro in cluster in caso di errore o emergenza in un sito.

### <a name="automatic-failover"></a>Failover automatico

In questa configurazione il cluster è costituito da due o più siti che possono ospitare ruoli del cluster. In caso di errore in un sito, deve essere eseguito automaticamente il failover dei ruoli del cluster nei siti rimanenti. Il quorum del cluster deve essere quindi configurato in modo che qualsiasi sito sia in grado di sostenere un errore completo del sito.

La tabella seguente include un riepilogo di considerazioni e suggerimenti per questa configurazione.

<table>
<thead>
<tr class="header">
<th>Item</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Numero di voti di nodo per sito</td>
<td>Deve essere uguale</td>
</tr>
<tr class="even">
<td>Assegnazione di voti al nodo</td>
<td>Non rimuovere voti di nodo perché tutti i nodi sono ugualmente importanti</td>
</tr>
<tr class="odd">
<td>Gestione dinamica del quorum</td>
<td>Deve essere abilitata</td>
</tr>
<tr class="even">
<td>Configurazione del controllo</td>
<td>È consigliabile la condivisione file di controllo configurata in un sito separato dai siti del cluster</td>
</tr>
<tr class="odd">
<td>Carichi di lavoro</td>
<td>I carichi di lavoro possono essere configurati in qualsiasi sito</td>
</tr>
</tbody>
</table>

#### <a name="additional-considerations-for-automatic-failover"></a>Considerazioni aggiuntive per il failover automatico

- La configurazione di una condivisione file di controllo in un sito separato è necessaria per offrire a ogni sito la stessa opportunità di sopravvivenza. Per altre informazioni, vedere [Configurazione del disco di controllo](#witness-configuration) in precedenza in questo argomento.

### <a name="manual-failover"></a>Failover manuale

In questa configurazione il cluster è costituito da un sito primario, *SiteA*, e un sito di backup (ripristino), *SiteB*. I ruoli del cluster sono ospitati in *SiteA*. A causa della configurazione del quorum del cluster, in caso di errore in tutti i nodi in *SiteA*, il cluster si arresterà. In questo scenario l'amministratore deve eseguire manualmente il failover dei servizi cluster in *SiteB* e deve eseguire passaggi aggiuntivi per il ripristino del cluster.

La tabella seguente include un riepilogo di considerazioni e suggerimenti per questa configurazione.

<table>
<thead>
<tr class="header">
<th>Item</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Numero di voti di nodo per sito</td>
<td>Può essere diverso</td>
</tr>
<tr class="even">
<td>Assegnazione di voti al nodo</td>
<td>- I voti di nodo non devono essere rimossi dai nodi nel sito primario, <em>SiteA</em><br />
- I voti di nodo devono essere rimossi dai nodi nel sito di backup, <em>SiteB</em><br />
- Se si verifica un'interruzione a lungo termine <em>SiteA</em>, i voti devono essere assegnati ai nodi in <em>SiteB</em> per abilitare una maggioranza del quorum in tale sito come parte del ripristino</td>
</tr>
<tr class="odd">
<td>Gestione dinamica del quorum</td>
<td>Deve essere abilitata</td>
</tr>
<tr class="even">
<td>Configurazione del controllo</td>
<td>- Configurare un controllo se è presente un numero pari di nodi a <em>SiteA</em><br />
- Se è necessario un controllo, configurare una condivisione file di controllo o un disco di controllo accessibile solo ai nodi in <em>SiteA</em> (talvolta denominato disco di controllo asimmetrico)</td>
</tr>
<tr class="odd">
<td>Carichi di lavoro</td>
<td>Usare proprietari preferiti per mantenere i nodi in esecuzione in <em>SiteA</em></td>
</tr>
</tbody>
</table>

#### <a name="additional-considerations-for-manual-failover"></a>Considerazioni aggiuntive per il failover manuale

- Solo i nodi in *SiteA* sono configurati inizialmente con voti di quorum. Ciò è necessario per assicurare che lo stato dei nodi in *SiteB* non influisca sul quorum del cluster.
- La procedura di ripristino dipende dalla capacità di *SiteA* di sostenere un errore temporaneo o un errore a lungo termine.

## <a name="more-information"></a>Altre informazioni

* [Clustering di failover](failover-clustering.md)
* [Cmdlet di PowerShell di Windows failover cluster](https://docs.microsoft.com/powershell/module/failoverclusters/?view=win10-ps)
* [Informazioni sui Cluster e Pool Quorum](../storage/storage-spaces/understand-quorum.md)