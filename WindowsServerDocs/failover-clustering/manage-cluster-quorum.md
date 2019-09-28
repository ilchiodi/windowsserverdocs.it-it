---
title: Configurare e gestire il quorum in un cluster di failover
description: Informazioni dettagliate su come gestire il quorum del cluster in un cluster di failover di Windows Server.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.openlocfilehash: 03e155cb9d30bc32da407f0d9ae915308f31494a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361020"
---
# <a name="configure-and-manage-quorum"></a>Configurare e gestire il quorum

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento vengono fornite informazioni generali e procedure per la configurazione e la gestione del quorum in un cluster di failover di Windows Server.

## <a name="understanding-quorum"></a>Informazioni sul quorum

Il quorum per un cluster è determinato dal numero di elementi di voto che devono essere inclusi tra i membri del cluster attivo in modo che tale cluster possa essere avviato correttamente o che sia possibile continuarne l'esecuzione. Per una spiegazione più dettagliata, vedere il [documento informazioni sul cluster e sul quorum del pool](../storage/storage-spaces/understand-quorum.md).

## <a name="quorum-configuration-options"></a>Opzioni di configurazione quorum

Il modello di quorum in Windows Server è flessibile. Se è necessario modificare la configurazione quorum per il cluster, è possibile usare la configurazione guidata quorum del cluster o i cmdlet di Windows PowerShell per i cluster di failover. Per procedure e considerazioni sulla configurazione quorum, vedere [Configurare il quorum del cluster](#configure-the-cluster-quorum) più avanti in questo argomento.

La tabella seguente elenca le tre opzioni di configurazione quorum disponibili nella Configurazione guidata quorum del cluster.

| Opzione  |Descrizione  |
| --------- | ---------|
| Usa impostazioni tipiche     |  Il cluster assegna automaticamente un voto a ogni nodo e gestisce in modo dinamico i voti del nodo. Se appropriato per il cluster in uso e se è disponibile l'archiviazione condivisa cluster, il cluster selezionerà un disco di controllo. Questa opzione è consigliata nella maggior parte dei casi, perché il software del cluster sceglie automaticamente una configurazione di quorum e controllo che fornisce la massima disponibilità per il cluster.       |
| Aggiungi o modifica il quorum di controllo     |   È possibile aggiungere, modificare o rimuovere una risorsa di controllo. È possibile configurare una condivisione file o un disco di controllo. Il cluster assegna automaticamente un voto a ogni nodo e gestisce in modo dinamico i voti del nodo.      |
| Configurazione quorum avanzata e selezione controllo     | Selezionare questa opzione solo se esistono requisiti specifici dell'applicazione o del sito per la configurazione quorum. È possibile modificare il quorum di controllo, aggiungere o rimuovere i voti dei nodi e scegliere se il cluster potrà gestire in modo dinamico i voti dei nodi. Per impostazione predefinita, i voti sono assegnati a tutti i nodi e i voti dei nodi sono gestiti in modo dinamico.        |

In base all'opzione di configurazione quorum selezionata e dalle impostazioni specifiche, il cluster sarà configurato in una delle modalità quorum seguenti:

| Modalità  | Descrizione  |
| --------- | ---------|
| Maggioranza dei nodi (senza controllo)     |   Solo i nodi dispongono di voti. Non è configurato alcun quorum di controllo. Il quorum del cluster corrisponde alla maggioranza dei nodi votanti tra i membri del cluster attivo.      |
| Maggioranza dei nodi con controllo (disco o condivisione file)     |   I nodi dispongono di voti. Il quorum di controllo, inoltre, dispone di un voto. Il quorum del cluster corrisponde alla maggioranza dei nodi votanti tra i membri del cluster attivo più un voto del disco di controllo. Un quorum di controllo può essere un disco di controllo designato o una condivisione file di controllo designata. 
| Nessuna maggioranza (solo disco di controllo)     | Nessun nodo dispone di voti. Solo un disco di controllo dispone di un voto. <br>Il quorum del cluster è determinato dallo stato del disco di controllo. In genere, questa modalità non è consigliata ed è opportuno non selezionarla, poiché crea un singolo punto di errore per il cluster.       |

Nelle sottosezioni riportate di seguito vengono fornite ulteriori informazioni sulle impostazioni di configurazione quorum avanzate.

### <a name="witness-configuration"></a>Configurazione del controllo

In genere, quando si configura un quorum è consigliabile che il numero di elementi votanti nel cluster sia dispari. Se quindi il cluster include un numero pari di nodi votanti, è consigliabile configurare un disco di controllo o una condivisione file di controllo. Il cluster sarà in grado di sostenere un nodo aggiuntivo inattivo. L'aggiunta di un voto di controllo permette inoltre al cluster di continuare l'esecuzione in caso di inattività o disconnessione della metà dei nodi del cluster.

Un disco di controllo è in genere consigliato se tutti i nodi possono vedere il disco. Una condivisione file di controllo è consigliata quando è necessario prendere in considerazione il ripristino di emergenza multisito con archiviazione replicata. La configurazione di un disco di controllo con la risorsa di archiviazione replicata è possibile solo se il fornitore del sistema di archiviazione supporta l'accesso in lettura/scrittura da tutti i siti all'archiviazione replicata. <strong>*Il disco di controllo non è supportato con spazi di archiviazione diretta*</strong>.

La tabella seguente include informazioni e considerazioni aggiuntive sui tipi di quorum di controllo.

| Tipo di controllo  | Descrizione  | Requisiti e consigli  |
| ---------    |---------        |---------                        |
| Disco di controllo     |  <ul><li> LUN dedicato per l'archiviazione di una copia del database del cluster</li><li> Particolarmente utile per i cluster con archiviazione condivisa (non replicata)</li>       |  <ul><li>Le dimensioni di LUN devo essere pari almeno a 512 MB</li><li> Deve essere dedicato all'uso da parte del cluster e non assegnato ad alcun ruolo del cluster</li><li> Deve essere incluso nell'archiviazione cluster e deve superare i test di convalida dell'archiviazione</li><li> Non può essere un disco di tipo CSV (Cluster Shared Volume)</li><li> Disco di base con un singolo volume</li><li> Non necessita di lettera di unità</li><li> Formattabile con NTFS o ReFS</li><li> Può essere configurato con RAID hardware per tolleranza di errore</li><li> Deve essere escluso da backup e scansione antivirus</li><li> Il disco di controllo non è supportato con Spazi di archiviazione diretta</li>|
| Condivisione file di controllo     | <ul><li>Condivisione file SMB configurata in un file server che esegue Windows Server</li><li> Non archivia alcuna copia del database cluster</li><li> Mantiene le informazioni del cluster solo in un file witness.log</li><li> Particolarmente utile per cluster multisito con archiviazione replicata </li>       |  <ul><li>Deve avere almeno 5 MB di spazio disponibile</li><li> Deve essere dedicata al singolo cluster e non usata per l'archiviazione di dati utente o applicazione</li><li> Deve disporre di autorizzazioni di scrittura abilitate per l'oggetto computer per il nome del cluster</li></ul><br>Di seguito sono riportate alcune considerazioni aggiuntive per un file server che ospita la condivisione file di controllo:<ul><li>Un singolo file server può essere configurato con condivisioni file di controllo per più cluster.</li><li> Il file server deve trovarsi in un sito separato dal carico di lavoro del cluster. Ciò offre a qualsiasi sito di cluster uguali opportunità di sopravvivere in caso di perdita di comunicazioni di rete da sito a sito. Se il file server si trova nello stesso sito, tale sito diventa il sito primario e sarà l'unico sito in grado di raggiungere la condivisione file.</li><li> Il file server può essere eseguito in una macchina virtuale se la VM non è ospitata nello stesso cluster che usa la condivisione file di controllo.</li><li> Per assicurare una disponibilità elevata, è possibile configurare il file in un cluster di failover separato. </li>      |
| Server di controllo cloud     |  <ul><li>Un file witness archiviato nell'archivio BLOB di Azure</li><li> Consigliato quando tutti i server del cluster hanno una connessione Internet affidabile.</li>      |  Vedere [distribuire un cloud di](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness)controllo.       |

### <a name="node-vote-assignment"></a>Assegnazione di voti al nodo

Come opzione avanzata di configurazione quorum, è possibile scegliere di assegnare o rimuovere i voti del quorum in base ai singoli nodi. Per impostazione predefinita, i voti sono assegnati a tutti i nodi. Indipendentemente dall'assegnazione dei voti, tutti i nodi continuano a funzionare nel cluster, ricevono aggiornamenti del database cluster e possono ospitare applicazioni.

È consigliabile rimuovere voti dai nodi in determinate configurazioni di ripristino di emergenza. Ad esempio, in un cluster multisito, è possibile rimuovere voti dai nodi in un sito di backup, in modo che tali nodi non influiscano sui calcoli del quorum. Questa configurazione è consigliata solo per il failover manuale tra siti. Per altre informazioni, vedere [Considerazioni relative al quorum per configurazioni di ripristino di emergenza](#quorum-considerations-for-disaster-recovery-configurations) più avanti in questo argomento.

Il voto configurato di un nodo può essere verificato cercando la proprietà comune **NodeWeight** del nodo del cluster usando il cmdlet di Windows PowerShell [Get-ClusterNode](https://technet.microsoft.com/library/hh847268.aspx). Un valore pari a 0 indica che per il nodo non è stato configurato alcun voto di quorum. Un valore pari a 1 indica che il voto di quorum del nodo è stato assegnato ed è gestito dal cluster. Per altre informazioni sulla gestione dei voti di nodo, vedere [Gestione dinamica del quorum](#dynamic-quorum-management) più avanti in questo argomento.

L'assegnazione di voti per tutti i nodi del cluster può essere verificata tramite il test di convalida **Convalida del quorum del cluster**.

#### <a name="additional-considerations-for-node-vote-assignment"></a>Considerazioni aggiuntive per l'assegnazione di voti ai nodi

  - L'assegnazione di voti ai nodi non è consigliata per imporre un numero dispari di nodi votanti. Configurare invece un disco di controllo o una condivisione file di controllo. Per ulteriori informazioni, vedere Configurazione del server di controllo del [mirroring](#witness-configuration) più avanti in questo argomento.
  - Se la gestione dinamica del quorum è abilitata, sarà possibile assegnare o rimuovere voti in modo dinamico solo per i nodi configurati per l'assegnazione di voti di nodo. Per altre informazioni, vedere [Gestione dinamica del quorum](#dynamic-quorum-management) più avanti in questo argomento.

### <a name="dynamic-quorum-management"></a>Gestione dinamica del quorum

In Windows Server 2012, come opzione avanzata di configurazione quorum, è possibile scegliere di abilitare la gestione dinamica del quorum per cluster. Per informazioni dettagliate sul funzionamento del quorum dinamico, vedere [questa spiegazione](../storage/storage-spaces/understand-quorum.md#dynamic-quorum-behavior).

La gestione dinamica del quorum permette anche l'esecuzione del cluster sull'ultimo nodo del cluster attivo. Grazie alla regolazione dinamica dei requisiti di maggioranza del quorum, il cluster può sostenere arresti sequenziali dei nodi fino a un singolo nodo.

Il voto dinamico assegnato dal cluster di un nodo può essere verificato con la proprietà comune **DynamicWeight** del nodo del cluster usando il cmdlet di Windows PowerShell [Get-ClusterNode](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode?view=win10-ps) . Un valore pari a 0 indica che il nodo non dispone di alcun voto di quorum. Un valore pari a 1 indica che il nodo dispone di un voto di quorum.

L'assegnazione di voti per tutti i nodi del cluster può essere verificata tramite il test di convalida **Convalida del quorum del cluster**.

#### <a name="additional-considerations-for-dynamic-quorum-management"></a>Considerazioni aggiuntive per la gestione dinamica del quorum

- La gestione dinamica del quorum non permette al cluster di sostenere errori simultanei della maggioranza dei membri votanti. Per continuare l'esecuzione, il cluster deve sempre disporre di una maggioranza di quorum al momento dell'arresto o dell'errore di un nodo.

- Se il voto di un nodo è stato rimosso esplicitamente, il cluster non potrà aggiungere o rimuovere in modo dinamico tale voto.
- Quando Spazi di archiviazione diretta è abilitato, il cluster può supportare solo due errori del nodo. Questa operazione è illustrata più nella [sezione quorum del pool](../storage/storage-spaces/understand-quorum.md)

## <a name="general-recommendations-for-quorum-configuration"></a>Indicazioni generali per la configurazione quorum

Il software del cluster configura automaticamente il quorum per un nuovo cluster, in base al numero di nodi configurati e la disponibilità dell'archiviazione condivisa. Questa è in genere la configurazione quorum più appropriata per questo tipo di cluster. È comunque consigliabile verificare la configurazione quorum dopo la creazione del cluster prima di passare il cluster in produzione. Per visualizzare la configurazione dettagliata del quorum del cluster, è possibile usare la convalida guidata configurazione o il cmdlet di Windows PowerShell per il [cluster di test](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps) per eseguire il test **Convalida configurazione quorum** . In Gestione cluster di failover la configurazione quorum di base viene visualizzata nelle informazioni di riepilogo per il cluster selezionato. in alternativa, è possibile esaminare le informazioni sulle risorse del quorum restituite quando si esegue il [Get-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusterquorum?view=win10-ps) di Windows PowerShell cmdlet.

In qualsiasi momento è possibile eseguire il test **Convalida configurazione quorum** per confermare che la configurazione del quorum è ottimale per il cluster. L'output del test indica se è consigliabile una modifica nella configurazione quorum e mostra le impostazioni ottimali. Se è consigliata una modifica, è possibile usare la Configurazione guidata quorum del cluster per applicare le impostazioni consigliate.

Quando il cluster è in produzione, modificare la configurazione quorum solo se è stato stabilito che la modifica è appropriata per il cluster. Prendere in considerazione la modifica della configurazione quorum nelle situazioni seguenti:

- Aggiunta o eliminazione di nodi
- Aggiunta o rimozione di risorse di archiviazione
- Errore a lungo termine di nodi o controlli
- Ripristino di un cluster in uno scenario di ripristino di emergenza multisito

Per altre informazioni sulla convalida del cluster di failover, vedere [Convalidare l'hardware per un cluster di failover](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

## <a name="configure-the-cluster-quorum"></a>Configurare il quorum del cluster

È possibile configurare le impostazioni del quorum del cluster usando Gestione cluster di failover o i cmdlet di Windows PowerShell per i cluster di failover.

> [!IMPORTANT]
> È in genere consigliabile usare la configurazione quorum suggerita dalla Configurazione guidata quorum del cluster. Personalizzare la configurazione quorum solo se la modifica è appropriata per il cluster. Per altre informazioni, vedere [Indicazioni generali per la configurazione del quorum](#general-recommendations-for-quorum-configuration) in questo argomento.

### <a name="configure-the-cluster-quorum-settings"></a>Configurare le impostazioni del quorum del cluster

Per eseguire questa procedura, è necessaria almeno l'appartenenza al gruppo **Administrators** locale o a un gruppo equivalente in ogni server cluster. L'account deve essere inoltre un account utente di dominio.

> [!NOTE]
> È possibile modificare la configurazione quorum del cluster senza arrestare il cluster o portare offline le risorse del cluster.

### <a name="change-the-quorum-configuration-in-a-failover-cluster-by-using-failover-cluster-manager"></a>Modificare la configurazione quorum in un cluster di failover utilizzando Gestione cluster di failover

1. In Gestione cluster di failover selezionare o specificare il cluster da modificare.
2. Con il cluster selezionato, in **azioni**selezionare **altre azioni**, quindi selezionare **Configura impostazioni quorum del cluster**. Sarà visualizzata la Configurazione guidata quorum del cluster. Selezionare **Avanti**.
3. Nella pagina **Selezione opzione configurazione quorum** selezionare una delle tre opzioni di configurazione e completare i passaggi relativi all'opzione selezionata. Prima di configurare le impostazioni del quorum, è possibile verificare le opzioni disponibili. Per ulteriori informazioni sulle opzioni, vedere [informazioni sul quorum](#understanding-quorum), più indietro in questo argomento.

    - Per consentire al cluster di reimpostare automaticamente le impostazioni del quorum ottimali per la configurazione corrente del cluster, selezionare **Usa impostazioni tipiche** e quindi completare la procedura guidata.
    - Per aggiungere o modificare il quorum di controllo, selezionare **Aggiungi o modifica il quorum**di controllo, quindi completare i passaggi seguenti. Per informazioni e considerazioni sulla configurazione di un quorum di controllo, vedere [Configurazione del disco di controllo](#witness-configuration) in precedenza in questo argomento.

      1. Nella pagina **Selezione quorum di controllo** selezionare un'opzione per configurare un disco di controllo o una condivisione file di controllo. Nella procedura guidata sono indicate le opzioni di selezione del controllo consigliate per il cluster.

          > [!NOTE]
          > È anche possibile selezionare **Non configurare quorum di controllo** e quindi completare la procedura guidata. Se il cluster include un numero pari di nodi votanti, è possibile che questa non sia una configurazione consigliata.

      2. Se si seleziona l'opzione per la configurazione di un disco di controllo, nella pagina **Configurazione risorsa di archiviazione di controllo** selezionare il volume di archiviazione da assegnare come disco di controllo e quindi completare la procedura guidata.
      3. Se si seleziona l'opzione per la configurazione di una condivisione file di controllo, nella pagina **Configurazione condivisione file di controllo** digitare o selezionare una condivisione file che sarà usata come risorsa di controllo, quindi completare la procedura guidata.

    - Per configurare le impostazioni di gestione del quorum e per aggiungere o modificare il quorum di controllo, selezionare **configurazione quorum avanzata e selezione**controllo, quindi completare i passaggi seguenti. Per informazioni e considerazioni sulle impostazioni avanzate per la configurazione quorum, vedere [Assegnazione di voti al nodo](#node-vote-assignment) e [Gestione dinamica del quorum](#dynamic-quorum-management) nelle sezioni precedenti di questo argomento.

      1. Nella pagina **Selezione configurazione di voto** selezionare un'opzione per l'assegnazione di voti ai nodi. Per impostazione predefinita, un voto è assegnato a tutti i nodi. Per determinati scenari è tuttavia possibile assegnare voti solo a un subset di nodi.

          > [!NOTE]
          > È anche possibile selezionare **Nessun nodo**. Questa opzione non è in genere consigliata perché non permette ai nodi di prendere parte alla votazione per il quorum e richiede la configurazione di un disco di controllo. Il disco di controllo diventa il singolo punto di errore per il cluster.

      2. Nella pagina **Configurazione gestione quorum** è possibile abilitare o disabilitare l'opzione **Consenti al cluster di gestire dinamicamente l'assegnazione dei voti dei nodi**. La selezione di questa opzione permette in genere di aumentare la disponibilità del cluster. Questa opzione è abilitata per impostazione predefinita ed è consigliabile non disabilitarla. L'opzione permette la continuazione dell'esecuzione del cluster in scenari di errore e ciò non sarebbe possibile con l'opzione deselezionata.
      3. Nella pagina **Selezione quorum di controllo** selezionare un'opzione per configurare un disco di controllo o una condivisione file di controllo. Nella procedura guidata sono indicate le opzioni di selezione del controllo consigliate per il cluster.

          > [!NOTE]
          > È anche possibile selezionare **Non configurare quorum di controllo**e quindi completare la procedura guidata. Se il cluster include un numero pari di nodi votanti, è possibile che questa non sia una configurazione consigliata.

      4. Se si seleziona l'opzione per la configurazione di un disco di controllo, nella pagina **Configurazione risorsa di archiviazione di controllo** selezionare il volume di archiviazione da assegnare come disco di controllo e quindi completare la procedura guidata.
      5. Se si seleziona l'opzione per la configurazione di una condivisione file di controllo, nella pagina **Configurazione condivisione file di controllo** digitare o selezionare una condivisione file che sarà usata come risorsa di controllo, quindi completare la procedura guidata.

4. Selezionare **Avanti**. Confermare le selezioni nella pagina di conferma visualizzata, quindi fare clic su **Avanti**.

Quando viene eseguita la procedura guidata e viene visualizzata la pagina **Riepilogo** , se si desidera visualizzare un report delle attività eseguite dalla procedura guidata, selezionare **Visualizza report**. Il report più recente rimarrà nella cartella <em>systemroot</em> **\\Cluster @ no__t-3Reports** con il nome **QuorumConfiguration. mht**.

> [!NOTE]
> Dopo la configurazione del quorum del cluster, è consigliabile eseguire il test **Convalida configurazione quorum** per verificare le impostazioni aggiornate del quorum.

### <a name="windows-powershell-equivalent-commands"></a>Comandi equivalenti di Windows PowerShell

Gli esempi seguenti illustrano come usare il cmdlet [set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum?view=win10-ps) e altri cmdlet di Windows PowerShell per configurare il quorum del cluster.

Nell'esempio seguente la configurazione del quorum sul cluster *CONTOSO-FC1* viene cambiata in una semplice configurazione a maggioranza di nodi senza quorum di controllo.

```PowerShell
Set-ClusterQuorum –Cluster CONTOSO-FC1 -NodeMajority
```

L'esempio seguente modifica la configurazione quorum nel cluster locale in una configurazione a maggioranza di nodi con configurazione di controllo. La risorsa disco con nome *Cluster Disk 2* è configurata come disco di controllo.

```PowerShell
Set-ClusterQuorum -NodeAndDiskMajority "Cluster Disk 2"
```

L'esempio seguente modifica la configurazione quorum nel cluster locale in una configurazione a maggioranza di nodi con configurazione di controllo. La risorsa di condivisione file denominata *\\ @ no__t-2CONTOSO-FS @ no__t-3fsw* è configurata come condivisione file di controllo.

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

> [!NOTE]
> * Se il Servizio cluster si arresta a causa della perdita del quorum, nel registro di sistema compare l'ID evento 1177.
> * È sempre necessario scoprire le cause della perdita del quorum del cluster.
> * È preferibile ripristinare sempre l'integrità di un nodo o di un quorum di controllo (appartenenza al cluster) piuttosto che avviare il cluster senza quorum.

### <a name="force-start-cluster-nodes"></a>Imporre l'avvio dei nodi del cluster

Dopo avere determinato che non è possibile ripristinare il cluster riportando i nodi o il quorum di controllo a uno stato di integrità, sarà necessario imporre l'avvio del cluster. Se si impone l'avvio del cluster, le impostazioni della configurazione del quorum del cluster saranno ignorate e il cluster sarà avviato in modalità **ForceQuorum**.

L'avvio forzato di un cluster che non dispone di quorum potrebbe essere particolarmente utile in un cluster multisito. Prendere in considerazione uno scenario di ripristino di emergenza con un cluster che contiene siti primari e di backup in posizioni distinte, ovvero, *SiteA* e *SiteB*. In caso di emergenza effettiva in *SiteA*, potrebbe essere necessaria una quantità significativa di tempo per il ritorno online del sito. É quindi probabile che si voglia imporre a *SiteB* di passare online, anche senza quorum.

Quando si avvia un cluster in modalità **ForceQuorum** e dopo che il cluster ottiene di nuovo un numero sufficiente di voti di quorum, il cluster lascia automaticamente lo stato forzato e si comporta normalmente. Non è quindi necessario riavviare di nuovo il cluster normalmente. Se il cluster perde un nodo e perde il quorum, torna offline poiché non si trova più nello stato forzato. Per riportarlo online quando non dispone del quorum, è necessario forzare l'avvio del cluster senza quorum.

> [!IMPORTANT]
> * Dopo l'avvio forzato di un cluster, l'amministratore avrà controllo completo sul cluster.
> * Il cluster usa la configurazione del cluster nel nodo in cui è stato eseguito l'avvio forzato del cluster e ne esegue la replica in tutti gli altri nodi disponibili.
> * Se si impone l'avvio del cluster senza quorum, tutte le impostazioni di configurazione del quorum saranno ignorate finché il cluster rimane in modalità **ForceQuorum**. Ciò include le impostazioni specifiche per le assegnazioni di voti di nodo e per la gestione dinamica del quorum.

### <a name="prevent-quorum-on-remaining-cluster-nodes"></a>Impedire il quorum nei nodi rimanenti del cluster

Dopo l'avvio forzato del cluster in un nodo, è necessario avviare eventuali nodi rimanenti nel cluster con un'impostazione per impedire il quorum. Un nodo avviato con un'impostazione per impedire il quorum indica al Servizio cluster di aggiungere un cluster in esecuzione esistente invece di formare una nuova istanza del cluster. Ciò impedisce ai nodi rimanenti di formare un cluster diviso che contiene due istanze concorrenti.

Ciò risulta necessario quando si deve ripristinare il cluster in alcuni scenari di ripristino di emergenza multisito, dopo l'avvio forzato del cluster nel sito di backup, *SiteB*. Per aggiungere il cluster avviato in modo forzato in *SiteB*, è necessario avviare i nodi nel sito primario, *SiteA*, dopo averne impedito il quorum.

> [!IMPORTANT]
> Dopo l'avvio forzato di un cluster in un nodo, è consigliabile avviare sempre i nodi rimanenti dopo averne impedito il quorum.

Di seguito viene illustrato come ripristinare il cluster con Gestione cluster di failover:

1. In Gestione cluster di failover selezionare o specificare il cluster da ripristinare.
2. Con il cluster selezionato, in **azioni**selezionare **Forza avvio del cluster**.

    Gestione cluster di failover impone l'avvio del cluster in tutti i nodi raggiungibili. Il cluster usa la configurazione corrente del cluster all'avvio.

> [!NOTE]
> * Per forzare l'avvio del cluster in un nodo specifico contenente una configurazione del cluster che si desidera utilizzare, è necessario utilizzare i cmdlet di Windows PowerShell o gli strumenti da riga di comando equivalenti, come illustrato dopo questa procedura. 
> * Se si usa Gestione cluster di failover per la connessione a un cluster avviato in modo forzato e si usa l'azione **Avvia Servizio cluster** per avviare un nodo, il nodo sarà avviato automaticamente con l'impostazione che impedisce il quorum.

#### <a name="windows-powershell-equivalent-commands-start-clusternode"></a>Comandi equivalenti di Windows PowerShell (Start-clusternode)

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

| Elemento  | Descrizione  |
| ---------| ---------|
| Numero di voti di nodo per sito     | Deve essere uguale       |
| Assegnazione di voti al nodo     |  Non rimuovere voti di nodo perché tutti i nodi sono ugualmente importanti       |
| Gestione dinamica del quorum     |   Deve essere abilitata      |
| Configurazione del controllo     |  È consigliabile la condivisione file di controllo configurata in un sito separato dai siti del cluster       |
| Carichi di lavoro     |  I carichi di lavoro possono essere configurati in qualsiasi sito       |

#### <a name="additional-considerations-for-automatic-failover"></a>Considerazioni aggiuntive per il failover automatico

- La configurazione di una condivisione file di controllo in un sito separato è necessaria per offrire a ogni sito la stessa opportunità di sopravvivenza. Per altre informazioni, vedere [Configurazione del disco di controllo](#witness-configuration) in precedenza in questo argomento.

### <a name="manual-failover"></a>Failover manuale

In questa configurazione il cluster è costituito da un sito primario, *SiteA*, e un sito di backup (ripristino), *SiteB*. I ruoli del cluster sono ospitati in *SiteA*. A causa della configurazione del quorum del cluster, in caso di errore in tutti i nodi in *SiteA*, il cluster si arresterà. In questo scenario l'amministratore deve eseguire manualmente il failover dei servizi cluster in *SiteB* e deve eseguire passaggi aggiuntivi per il ripristino del cluster.

La tabella seguente include un riepilogo di considerazioni e suggerimenti per questa configurazione.

| Elemento   |Descrizione  |
| ---------| ---------|
| Numero di voti di nodo per sito     |  <ul><li> I voti di nodo non devono essere rimossi dai nodi nel sito primario, **SiteA**</li><li>I voti di nodo devono essere rimossi dai nodi nel sito di backup, **SiteB**</li><li>In caso di interruzione a lungo termine in **SiteA**, i voti devono essere assegnati ai nodi in **SiteB** per abilitare una maggioranza del quorum in tale sito come parte del ripristino.</li>       |
| Gestione dinamica del quorum     |  Deve essere abilitata       |
| Configurazione del controllo     |  <ul><li>Configurare un controllo se **SiteA include un numero pari di nodi**</li><li>Se è necessario un controllo, configurare una condivisione file di controllo o un disco di controllo accessibile solo ai nodi in **SiteA** (definito a volte disco di controllo asimmetrico)</li>       |
| Carichi di lavoro     |  Usare proprietari preferiti per mantenere i nodi in esecuzione in **SiteA**       |

#### <a name="additional-considerations-for-manual-failover"></a>Considerazioni aggiuntive per il failover manuale

- Solo i nodi in *SiteA* sono configurati inizialmente con voti di quorum. Ciò è necessario per assicurare che lo stato dei nodi in *SiteB* non influisca sul quorum del cluster.
- La procedura di ripristino dipende dalla capacità di *SiteA* di sostenere un errore temporaneo o un errore a lungo termine.

## <a name="more-information"></a>Altre informazioni

* [Clustering di failover](failover-clustering.md)
* [Cmdlet di Windows PowerShell per i cluster di failover](https://docs.microsoft.com/powershell/module/failoverclusters/?view=win10-ps)
* [Informazioni sul quorum del cluster e del pool](../storage/storage-spaces/understand-quorum.md)