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
ms.sourcegitcommit: 28dc7c7f1e44fee7ab2112228af329a9ce0e02ba
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2019
ms.locfileid: "9023997"
---
# Configurare e gestire il quorum

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento fornisce in background e i passaggi per configurare e gestire il quorum in un cluster di failover di Windows Server.

## Quorum conoscenza

Il quorum per un cluster è determinato dal numero di elementi votanti che deve far parte di adesione di cluster attivo per il cluster per avviare correttamente o continuare l'esecuzione. Per una spiegazione più dettagliata, vedi le [informazioni sui doc di quorum di cluster e pool](../storage/storage-spaces/understand-quorum.md).

## Opzioni di configurazione quorum

Il modello di quorum in Windows Server è flessibile. Se devi modificare la configurazione quorum per il cluster, è possibile utilizzare la configurazione guidata Quorum del Cluster o i cmdlet di Windows PowerShell cluster di Failover. Per passaggi e considerazioni per configurare il quorum, vedere [configurare il quorum del cluster](#configure-the-cluster-quorum) più avanti in questo argomento.

La tabella seguente elenca le opzioni di configurazione tre quorum disponibili nella configurazione guidata Quorum del Cluster.

<table>
<thead>
<tr class="header">
<th>Opzione</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Utilizzare le impostazioni tipiche</td>
<td>Il cluster assegna automaticamente un voto a ogni nodo e gestisce in modo dinamico i voti nodo. Se è adatta per il cluster e archiviazione condivisa del cluster è disponibile, il cluster seleziona un controllo disco. Questa opzione è consigliata nella maggior parte dei casi, perché il software di cluster sceglie automaticamente una configurazione quorum e di controllo che fornisce la disponibilità di più alta per il cluster.</td>
</tr>
<tr class="even">
<td>Aggiungere o modificare il quorum di controllo</td>
<td>Puoi aggiungere, modificare o rimuovere una risorsa di controllo. È possibile configurare un controllo di condivisione o il disco di file. Il cluster assegna automaticamente un voto a ogni nodo e gestisce in modo dinamico i voti nodo.</td>
</tr>
<tr class="odd">
<td>Configurazione del quorum avanzate e selezione di controllo</td>
<td>Selezioni questa opzione solo quando hai specifiche dell'applicazione o conceda requisiti per la configurazione del quorum. Puoi modificare il quorum di controllo, aggiungere o rimuovere voti nodo e scegliere se il cluster gestisce in modo dinamico voti nodo. Per impostazione predefinita, vengono assegnati voti a tutti i nodi e i voti nodo vengono gestiti in modo dinamico.</td>
</tr>
</tbody>
</table>

A seconda che scegli l'opzione di configurazione quorum e le impostazioni specifiche, il cluster verrà configurato in una delle modalità quorum seguenti:

<table>
<thead>
<tr class="header">
<th>Modalità</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Maggioranza dei nodi (nessun controllo)</td>
<td>Solo i nodi presentano voti. Non viene configurata alcuna quorum di controllo. Il quorum del cluster è la maggior parte dei nodi votanti in cluster attivo.</td>
</tr>
<tr class="even">
<td>Maggioranza dei nodi con controllo (condivisione file o disco)</td>
<td>I nodi presentano voti. Inoltre, quorum di controllo ha un voto. Il quorum del cluster è la maggior parte dei nodi votanti in cluster attivo oltre a un voto di controllo.<br />
<br />
Quorum di controllo può essere un controllo disco designato o un controllo di condivisione file designato.</td>
</tr>
<tr class="odd">
<td>Nessuna maggioranza (solo controllo disco)</td>
<td>Nessun nodo hanno voti. Solo un controllo disco ha un voto. Il quorum del cluster è determinato dallo stato del controllo disco.<br />
<br />
Il cluster dispone di quorum se un nodo è disponibile e la comunicazione con un disco specifico nell'archiviazione del cluster. In generale, non è consigliabile questa modalità, e non deve essere selezionata poiché crea un singolo punto di errore per il cluster.</td>
</tr>
</tbody>
</table>

Le sottosezioni seguenti offrono altre informazioni sulle impostazioni di configurazione avanzate quorum.

### Configurazione del controllo

Come regola generale quando si configura il quorum, gli elementi votanti del cluster devono essere un numero dispari. Di conseguenza, se il cluster contiene un numero pari voto nodi, è necessario configurare un controllo disco o un controllo di condivisione file. Il cluster sarà in grado di supportare un altro nodo verso il basso. Aggiunta di un voto di controllo consente inoltre, il cluster di continuare l'esecuzione se metà dei nodi del cluster contemporaneamente Vai verso il basso o viene disconnesso.

Un controllo disco è in genere consigliato se tutti i nodi possono vedere il disco. Un controllo di condivisione file è consigliabile quando è necessario prendere in considerazione il ripristino di emergenza multisito con archiviazione replicata. Configurazione di un controllo disco con archiviazione replicata è possibile solo se il fornitore dell'archiviazione supporta l'accesso in lettura-scrittura da tutti i siti di archiviazione replicata. <strong>*Un controllo disco non è supportato con spazi di archiviazione diretta*</strong>.

La tabella seguente fornisce informazioni aggiuntive e considerazioni sui tipi di quorum di controllo.

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
<td>Controllo disco</td>
<td>- LUN dedicato che archivia una copia del database del cluster<br />
- Più utile per i cluster con archiviazione condivisa (non replicato)</td>
<td>- Dimensioni del LUN devono essere almeno 512 MB<br />
- Deve essere dedicata all'utilizzo del cluster e non assegnati a un ruolo del cluster<br />
- Devono essere inclusi nel cluster archiviazione e calcolo archiviazione test di convalida<br />
- Non può essere un disco che è un Volume condiviso Cluster (CSV)<br />
- Disco di base con un singolo volume<br />
- Non è necessario avere una lettera di unità<br />
- Possono essere formattati con NTFS o ReFS<br />
- Può essere configurato con RAID hardware per la tolleranza di errore<br />
- Devono essere escluse dalla backup e l'analisi antivirus<br />
- Un controllo disco non è supportato con spazi di archiviazione diretta</td>
</tr>
<tr class="even">
<td>Controllo di condivisione file</td>
<td>- Condivisione di file SMB che è configurato in un file server che esegue Windows Server<br />
- Non archiviare una copia del database del cluster<br />
- Gestisce le informazioni di cluster solo in un file witness.log<br />
- Più utile per i cluster multisito con archiviazione replicata</td>
<td>- Deve avere almeno 5 MB di spazio libero<br />
- Deve essere dedicata al singolo cluster e non usati per archiviare i dati utente o applicazione<br />
- Devono avere autorizzazioni di scrittura abilitato per l'oggetto computer per il nome del cluster<br />
<br />
Di seguito sono considerazioni aggiuntive per un file server che ospita il controllo di condivisione file:<br />
<br />
- Un singolo file server può essere configurato con testimoni di condivisione file per più cluster.<br />
- Il file server deve trovarsi in un sito che è separato dal carico di lavoro del cluster. In questo modo pari opportunità per qualsiasi sito di cluster sostenere se la comunicazione di rete da sito a sito viene interrotta. Se il file server è nello stesso sito, tale sito diventa il sito primario ed è l'unico sito in grado di raggiungere la condivisione file.<br />
- Il file server è possibile eseguire in una macchina virtuale se la macchina virtuale non è ospitata nello stesso cluster che usa il controllo di condivisione file.<br />
- Per una disponibilità elevata, file server può essere configurato in un cluster di failover separato.</td>
</tr>

<tr class-"odd">
<td>Cloud di controllo</td>
<td>- Un file di controllo archiviato in archiviazione blob di Azure<br>
-Consigliato quando tutti i server del cluster dispongano di una connessione Internet affidabile.</td>
<td>Vedi <a href="https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness">distribuire un cloud di controllo</a>.</td>
<td>
</td>
</tr>

</tbody>
</table>

### Assegnazione di nodo vota

Come opzione di configurazione avanzate quorum, puoi scegliere di assegnare o rimuovere voti quorum su una base per ogni nodo. Per impostazione predefinita, tutti i nodi vengono assegnati voti. Indipendentemente dal assegnazione vota, tutti i nodi continuano per il funzionamento del cluster, ricevono gli aggiornamenti di database del cluster e possono ospitare applicazioni.

Decidere di rimuovere voti dai nodi in determinate configurazioni di ripristino di emergenza. Ad esempio, in un cluster multisito, è possibile rimuovere voti dai nodi in un sito di backup in modo che i nodi non influiscono sui calcoli di quorum. Questa configurazione è consigliata solo per il failover manuale tra siti. Per altre informazioni, vedi [considerazioni di Quorum per le configurazioni di ripristino di emergenza](#quorum-considerations-for-disaster-recovery-configurations) più avanti in questo argomento.

È possibile verificare il voto configurato di un nodo cercando le proprietà comuni **NodeWeight** del nodo del cluster tramite il cmdlet di Windows PowerShell [Get-ClusterNode](http://technet.microsoft.com/library/hh847268.aspx). Un valore pari a 0 indica che il nodo non dispone di un voto quorum configurato. Un valore pari a 1 indica che è assegnato il voto quorum del nodo e si è gestito da parte del cluster. Per ulteriori informazioni sulla gestione di nodo voti, vedi [Gestione quorum dinamico](#dynamic-quorum-management) più avanti in questo argomento.

L'assegnazione di vota per tutti i nodi del cluster può essere verificato con il test di convalida **Convalidare Quorum del Cluster** .

#### Considerazioni aggiuntive per l'assegnazione di nodo vota

  - L'assegnazione vota nodo non è consigliabile applicare un numero di nodi votanti dispari. Al contrario, è necessario configurare un controllo disco o un controllo di condivisione file. Per altre informazioni, vedi [configurazione del controllo](#witness-configuration) più avanti in questo argomento.
  - Se è abilitata la gestione di quorum dinamico, solo i nodi che sono configurati per avere voti nodo assegnati possono avere loro voti assegnato o rimossa in modo dinamico. Per altre informazioni, vedi [Gestione quorum dinamico](#dynamic-quorum-management) più avanti in questo argomento.

### Gestione di quorum dinamico

In Windows Server 2012, come un'opzione di configurazione avanzate quorum, puoi scegliere di abilitare la gestione di quorum dinamico dal cluster. Per ulteriori informazioni su come dinamico funziona quorum, vedere [questa descrizione](../storage/storage-spaces/understand-quorum.md#dynamic-quorum-behavior).

Con la gestione di quorum dinamico, è anche possibile per un cluster per l'esecuzione sul nodo del cluster sopravvissute ultimo. Regolando il requisito di maggior parte di quorum in modo dinamico, il cluster può sostenere arresti sequenziali nodo a un singolo nodo.

Il voto dinamico di un nodo cluster assegnato possa essere verificato con la proprietà comune **DynamicWeight** del nodo del cluster tramite il cmdlet di Windows PowerShell [Get-ClusterNode](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode?view=win10-ps) . Un valore pari a 0 indica che il nodo non dispone di un voto quorum. Un valore pari a 1 indica che il nodo ha un voto quorum.

L'assegnazione di vota per tutti i nodi del cluster può essere verificato con il test di convalida **Convalidare Quorum del Cluster** .

#### Considerazioni aggiuntive per la gestione di quorum dinamico

- Gestione di quorum dinamico non consente il cluster può sostenere un errore simultaneo della maggior parte dei membri votanti. Per continuare l'esecuzione, il cluster deve avere sempre la maggior parte di quorum al momento di un arresto del nodo o un errore.

- Se sono state rimosse in modo esplicito il voto di un nodo, il cluster in modo dinamico non è possibile aggiungere o rimuovere tale vota.
- Quando spazi di archiviazione diretta è abilitato, il cluster può supportare solo due gli errori dei nodi. Questa operazione è illustrata più nella [sezione quorum pool](../storage/storage-spaces/understand-quorum.md#poolQuorum)

## Consigli generali per la configurazione quorum

Il software di cluster configura automaticamente il quorum per un nuovo cluster, in base al numero di nodi configurati e la disponibilità di archiviazione condivisa. Questo è in genere la configurazione quorum più appropriata per il cluster. Tuttavia, è consigliabile verificare la configurazione quorum dopo aver creato il cluster, prima di posizionare il cluster nell'ambiente di produzione. Per visualizzare la configurazione di quorum del cluster dettagliati, è possibile utilizzare la convalida di una configurazione guidata o il cmdlet [Test-Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps) Windows PowerShell, per eseguire il test di **Convalida configurazione Quorum** . In Gestione Cluster di Failover, verrà visualizzata la configurazione di base quorum nelle informazioni di riepilogo per il cluster selezionato o è possibile esaminare le informazioni sulle risorse di quorum che restituisce quando si esegue [Get-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusterquorum?view=win10-ps) Windows PowerShell cmdlet.

In qualsiasi momento, è possibile eseguire il test di **Convalida configurazione Quorum** per convalidare che la configurazione quorum è ottimale per il cluster. L'output di test indica se è consigliabile una modifica alla configurazione del quorum e le impostazioni che sono ottimali. Se è consigliata una modifica, è possibile utilizzare la configurazione guidata Quorum del Cluster per applicare le impostazioni consigliate.

Dopo che il cluster è nell'ambiente di produzione, non modificare la configurazione quorum solo dopo avere stabilito che la modifica sia appropriata per il cluster. Decidere di valuta la possibilità di modificare la configurazione del quorum nelle situazioni seguenti:

- Aggiunta o la rimozione di nodi
- Aggiunta o la rimozione di archiviazione
- Un errore di nodo o un a lungo termine
- Ripristino di un cluster in uno scenario di ripristino di emergenza multisito

Per ulteriori informazioni sulla convalida di un cluster di failover, vedi [La convalida dell'Hardware per un Cluster di Failover](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

## Configurare il quorum del cluster

È possibile configurare le impostazioni di quorum di cluster tramite Gestione Cluster di Failover o i cmdlet di Windows PowerShell cluster di Failover.

>[!IMPORTANT]
>È consigliabile utilizzare la configurazione quorum è consigliato per la configurazione guidata Quorum del Cluster. Ti consigliamo di personalizzare la configurazione quorum solo se si è stabilito che la modifica sia appropriata per il cluster. Per altre informazioni, vedi [le indicazioni generali per la configurazione quorum](#general-recommendations-for-quorum-configuration) in questo argomento.

### Configurare le impostazioni di quorum del cluster

È l'appartenenza al gruppo **Administrators** locale su ogni server di cluster o equivalente, le autorizzazioni minime necessarie per completare questa procedura. Inoltre, l'account che usi deve essere un account utente di dominio.

>[!NOTE]
>È possibile modificare la configurazione quorum del cluster senza interrompere il cluster o l'esecuzione di risorse del cluster in modalità offline.

### Modificare la configurazione quorum in un cluster di failover usando Gestione Cluster di Failover

1. In Gestione Cluster di Failover, seleziona o specificare il cluster che si desidera modificare.
2. Con il cluster selezionato, in **Azioni**, seleziona **Più azioni**e quindi seleziona **Configurare le impostazioni di Quorum del Cluster**. Viene visualizzata la configurazione guidata Quorum del Cluster. Seleziona **Avanti**.
3. Nella pagina **Seleziona l'opzione di configurazione Quorum** , seleziona una delle opzioni di tre configurazione e completare i passaggi per l'opzione corrispondente. Prima di configurare le impostazioni di quorum, è possibile esaminare le scelte. Per altre informazioni sulle opzioni, vedi [Panoramica del quorum in un cluster di failover](#overview-of-the-quorum-in-a-failover-cluster), in precedenza in questo argomento.

    - Per consentire il cluster per automaticamente le impostazioni di quorum ideali per la configurazione del cluster corrente, seleziona **le impostazioni tipiche di uso** e quindi completare la procedura guidata.
    - Per aggiungere o modificare il quorum di controllo, seleziona **Aggiungi o modifica il quorum di controllo**e quindi completare i passaggi seguenti. Per informazioni e considerazioni sulla configurazione quorum di controllo, vedi [configurazione del controllo](#witness-configuration) in precedenza in questo argomento.

      1. Nella pagina **Seleziona Quorum di controllo** , selezionare un'opzione per configurare un controllo disco o un controllo di condivisione file. La procedura guidata indica le opzioni di selezione di controllo che sono consigliate per il cluster.

          >[!NOTE]
          >Puoi anche selezionare **non configurare quorum di controllo** e quindi completare la procedura guidata. Se hai un numero pari voto nodi nel cluster, questo potrebbe non essere una configurazione consigliata.

      2. Se si seleziona l'opzione per configurare un controllo disco, nella pagina **Configura controllo di archiviazione** , seleziona il volume di archiviazione che si desidera assegnare come il controllo del disco e quindi completare la procedura guidata.
      3. Se si seleziona l'opzione per configurare un controllo di condivisione file, nella pagina **Configura controllo di condivisione File** , digitare o accedere a una condivisione di file che verrà utilizzata come risorsa di controllo e quindi completa la procedura guidata.

    - Per configurare le impostazioni di gestione di quorum e per aggiungere o modificare il quorum di controllo, selezionare **configurazione quorum avanzate e selezione di controllo**e quindi completare i passaggi seguenti. Per informazioni e considerazioni sulle impostazioni di configurazione avanzate quorum, vedere [voto l'assegnazione di nodo](#node-vote-assignment) e la [gestione di quorum dinamico](#dynamic-quorum-management) in precedenza in questo argomento.

      1. Nella pagina **Seleziona configurazione voto** , selezionare un'opzione di assegnare voti a nodi. Per impostazione predefinita, tutti i nodi vengono assegnati un voto. Tuttavia, per alcuni scenari, è possibile assegnare voti solo a un sottoinsieme dei nodi.

          >[!NOTE]
          >Puoi anche selezionare **I nodi No**. Questa operazione viene in genere sconsigliata, perché non consente i nodi per partecipare a quorum voto e richiede la configurazione di un controllo disco. Questo controllo disco diventa il singolo punto di errore per il cluster.

      2. Nella pagina **Configura gestione Quorum** , puoi abilitare o disabilitare l'opzione **Consenti cluster per gestire in modo dinamico l'assegnazione del nodo voti** . Seleziona questa opzione in genere aumenta la disponibilità del cluster. Per impostazione predefinita l'opzione è abilitata e si consiglia di non disabilitare questa opzione. Questa opzione consente al cluster di continuare l'esecuzione in scenari di errore che non sono possibili quando questa opzione è disabilitata.
      3. Nella pagina **Seleziona Quorum di controllo** , selezionare un'opzione per configurare un controllo disco o un controllo di condivisione file. La procedura guidata indica le opzioni di selezione di controllo che sono consigliate per il cluster.

          >[!NOTE]
          >Puoi anche selezionare **non configurare quorum di controllo**e quindi completare la procedura guidata. Se hai un numero pari voto nodi nel cluster, questo potrebbe non essere una configurazione consigliata.

      4. Se si seleziona l'opzione per configurare un controllo disco, nella pagina **Configura controllo di archiviazione** , seleziona il volume di archiviazione che si desidera assegnare come il controllo del disco e quindi completare la procedura guidata.
      5. Se si seleziona l'opzione per configurare un controllo di condivisione file, nella pagina **Configura controllo di condivisione File** , digitare o accedere a una condivisione di file che verrà utilizzata come risorsa di controllo e quindi completa la procedura guidata.

4. Seleziona **Avanti**. Verifica le selezioni nella pagina di conferma che viene visualizzato e quindi seleziona **successivo**.

Dopo la procedura guidata viene eseguito e verrà visualizzata la pagina di **Riepilogo** , se vuoi visualizzare un report delle attività eseguite dalla procedura guidata, seleziona **I Report di visualizzazione**. Il report più recente rimarrà nel *systemroot * * * \\Cluster\\Reports** cartella con il nome **QuorumConfiguration.mht**.

>[!NOTE]
>Dopo aver configurato il quorum del cluster, ti consigliamo di eseguire il test di **Convalida configurazione Quorum** per verificare le impostazioni di quorum aggiornato.

### Comandi equivalenti di Windows PowerShell

Gli esempi seguenti illustrano come usare il cmdlet [Set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum?view=win10-ps) e altri cmdlet Windows PowerShell per configurare il quorum del cluster.

L'esempio seguente modifica la configurazione del quorum in cluster *CONTOSO-FC1* a una configurazione della maggior parte del nodo semplice con nessun quorum di controllo.

```PowerShell
Set-ClusterQuorum –Cluster CONTOSO-FC1 -NodeMajority
```

L'esempio seguente modifica la configurazione del quorum nel cluster locale per la maggior parte nodo con configurazione del controllo. La risorsa disco denominata *Cluster Disk 2* è configurata come controllo disco.

```PowerShell
Set-ClusterQuorum -NodeAndDiskMajority "Cluster Disk 2"
```

L'esempio seguente modifica la configurazione del quorum nel cluster locale per la maggior parte nodo con configurazione del controllo. La risorsa di condivisione file denominata *\\\CONTOSO-FS\\fsw* è configurata come un controllo di condivisione file.

```PowerShell
Set-ClusterQuorum -NodeAndFileShareMajority "\\fileserver\fsw"
```

L'esempio seguente rimuove il voto quorum dal nodo *ContosoFCNode1* nel cluster locale.

```PowerShell
(Get-ClusterNode ContosoFCNode1).NodeWeight=0
```

L'esempio seguente aggiunge il voto quorum al nodo *ContosoFCNode1* nel cluster locale.

```PowerShell
(Get-ClusterNode ContosoFCNode1).NodeWeight=1
```

L'esempio seguente consente alla proprietà **DynamicQuorum** del cluster *CONTOSO-FC1* (se è stata disabilitata in precedenza):

```PowerShell
(Get-Cluster CONTOSO-FC1).DynamicQuorum=1
```

## Ripristinare un cluster tramite avvio senza quorum

Non verrà avviato un cluster di cui non è sufficiente voti quorum. Come primo passaggio, devi sempre verificare la configurazione quorum del cluster e analizzare il motivo per cui il cluster non ha più quorum. Questa situazione può verificarsi se si dispone di nodi che ha smesso di rispondere o se non è raggiungibile in un cluster multisito sito primario. Dopo aver identificato la causa principale per l'errore di cluster, è possibile utilizzare la procedura di ripristino descritta in questa sezione.

>[!NOTE]
> * Se il servizio Cluster si arresta perché viene perso quorum, verrà visualizzata 1177 ID evento nel Registro di sistema.
> * È sempre necessario analizzare il motivo per cui il quorum del cluster è stato perso.
> * È sempre preferibile per visualizzare un nodo o quorum di controllo a uno stato integro (aggiunta del cluster) invece di avvio del cluster senza quorum.

### Force avviare i nodi del cluster

Dopo aver determinato che non è possibile ripristinare il cluster introducendo i nodi o quorum di controllo a uno stato integro, forzare il cluster per avviare diventano necessari. Imporre l'avvio del cluster esegue l'override delle impostazioni di configurazione quorum di cluster e avvia il cluster in modalità **ForceQuorum** .

Potrebbe essere particolarmente utile in un cluster multisito forzare iniziare a quando non dispone di quorum del cluster. Considera uno scenario di ripristino di emergenza con un cluster che contiene separatamente trovano siti di primari e di backup, *il sito a* e *sito b*. Se non esiste una situazione di emergenza originale nel *sito a*, potrebbe richiedere una notevole quantità di tempo per il sito per sono ritornati online. È probabile che vuoi forzare *sito b* in linea, anche se non è disponibile quorum.

All'avvio di un cluster in modalità **ForceQuorum** e dopo che riprende voti quorum sufficienti, il cluster lascia automaticamente lo stato forzato e si comporta normalmente. Non è pertanto necessario di avviarsi nuovamente normalmente il cluster. Se un nodo di perde il cluster e perde il quorum, l'esecuzione offline nuovamente perché non è più nello stato imposto. Per riportare in linea quando non dispone di quorum richiede forzare l'avvio senza quorum del cluster.

>[!IMPORTANT]
>* Dopo che un cluster viene vigore di base, l'amministratore è in un controllo completo del cluster.
>* Il cluster Usa la configurazione del cluster nel nodo in cui il cluster è forza di base e la replica in tutti gli altri nodi disponibili.
>* Se si impone l'avvio senza quorum del cluster, tutte le impostazioni di configurazione quorum vengono ignorate durante il cluster rimane in modalità **ForceQuorum** . Ciò include le assegnazioni vota nodo specifico e le impostazioni di gestione di quorum dinamico.

### Impedire quorum in altri nodi del cluster

Dopo aver forza di base del cluster in un nodo, è necessario avviare tutti i nodi rimanenti nel cluster con un'impostazione per impedire quorum. Un nodo di base con un'impostazione che impedisce di quorum indica al servizio Cluster per l'aggiunta di un cluster esistente in esecuzione invece che formano una nuova istanza di cluster. Ciò impedisce che i nodi rimanenti che formano un divisione cluster che contiene due istanze concorrenti.

Questo diventa necessario quando devi ripristinare il cluster in alcuni scenari di ripristino di emergenza multisito dopo aver forza di base del cluster nel sito backup, *sito b*. Per aggiungere la forza avviato cluster nel *sito b*, i nodi nel sito primario, *il sito a*, devono essere avviata con il quorum non l'ha consentito.

>[!IMPORTANT]
>Dopo che un cluster viene vigore in un nodo di base, ti consigliamo di iniziare sempre nodi rimanenti con il quorum non l'ha consentito.

Ecco come ripristinare il cluster con gestione Cluster di Failover:

1. In Gestione Cluster di Failover, seleziona o specificare il cluster che si desidera ripristinare.
2. Con il cluster selezionato, in **Azioni**, seleziona **Forza avvio del Cluster**.

    Gestione Cluster di failover force inizia il cluster su tutti i nodi che sono raggiungibili. Il cluster utilizza la configurazione del cluster corrente all'avvio.

>[!NOTE]
>* Per impostare l'avvio in un nodo specifico che contiene una configurazione del cluster che si desidera utilizzare del cluster, è necessario utilizzare il cmdlet di Windows PowerShell o l'equivalenti strumenti da riga di comando come presentato dopo questa procedura. 
> * Usare Gestione Cluster di Failover per connettersi a un cluster che viene vigore di base, se si utilizza l'azione di **Avvio del servizio Cluster** per avviare un nodo, il nodo viene automaticamente avviato con l'impostazione che impedisce di quorum.

#### Comandi equivalenti di Windows PowerShell (Start-Clusternode)

L'esempio seguente mostra come usare il cmdlet **Start-ClusterNode** per forzare l'inizio del cluster su un nodo *ContosoFCNode1*.

```PowerShell
Start-ClusterNode –Node ContosoFCNode1 –FQ
```

In alternativa, è possibile digitare il comando seguente in locale nel nodo di:

```PowerShell
Net Start ClusSvc /FQ
```

L'esempio seguente mostra come usare il cmdlet **Start-ClusterNode** per avviare il servizio Cluster con il quorum in nodo *ContosoFCNode1*non l'ha consentito.

```PowerShell
Start-ClusterNode –Node ContosoFCNode1 –PQ
```

In alternativa, è possibile digitare il comando seguente in locale nel nodo di:

```PowerShell
Net Start ClusSvc /PQ
```

## Considerazioni sulla quorum per le configurazioni di ripristino di emergenza

In questa sezione sono riepilogate le caratteristiche e le configurazioni di quorum per due configurazioni di cluster multisito nelle distribuzioni di ripristino di emergenza. Linee guida per la configurazione quorum diverse a seconda se è necessario il failover automatico o il failover manuale per carichi di lavoro tra i siti. La configurazione dipende in genere i contratti di servizio (SLA) che sono presenti nell'organizzazione per fornire e supportare carichi di lavoro del cluster in caso di un errore o a un sito di emergenza.

### Failover automatico

In questa configurazione, il cluster è costituito da due o più siti che può ospitare ruoli cluster. Se si verifica un errore di qualsiasi sito, dovrebbero automaticamente il failover ai siti rimanenti i ruoli del cluster. Di conseguenza, il quorum del cluster deve essere configurato in modo che qualsiasi sito può sostenere un errore del sito completo.

La tabella seguente riepiloga considerazioni e suggerimenti per questa configurazione.

<table>
<thead>
<tr class="header">
<th>Elemento</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Numero di nodo voti per ogni sito</td>
<td>Deve essere uguale</td>
</tr>
<tr class="even">
<td>Assegnazione di nodo vota</td>
<td>Nodo voti non devono essere rimosso perché tutti i nodi sono ugualmente importanti</td>
</tr>
<tr class="odd">
<td>Gestione di quorum dinamico</td>
<td>Deve essere abilitato</td>
</tr>
<tr class="even">
<td>Configurazione del controllo</td>
<td>Si consiglia di controllo di condivisione file, configurato in un sito che è separato dai siti del cluster</td>
</tr>
<tr class="odd">
<td>Carichi di lavoro</td>
<td>I carichi di lavoro possono essere configurate su una qualsiasi di tutti i siti</td>
</tr>
</tbody>
</table>

#### Considerazioni aggiuntive per il failover automatico

- Il controllo di condivisione file di configurazione in un sito separato è necessaria dare la parità delle opportunità per sostenere ogni sito. Per altre informazioni, vedi [configurazione del controllo](#witness-configuration) in precedenza in questo argomento.

### Failover manuale

In questa configurazione, il cluster è costituito da un sito primario e *il sito a*un sito di backup (ripristino), *sito b*. Ruoli cluster sono ospitati nel *sito a*. A causa di configurazione quorum del cluster, se si verifica un errore in tutti i nodi della *il sito a*, il cluster smette di funzionare. In questo scenario l'amministratore deve eseguire il failover i servizi di cluster al *sito b* manualmente ed eseguire ulteriori passaggi per ripristinare il cluster.

La tabella seguente riepiloga considerazioni e suggerimenti per questa configurazione.

<table>
<thead>
<tr class="header">
<th>Elemento</th>
<th>Descrizione</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Numero di nodo voti per ogni sito</td>
<td>Può essere diverso</td>
</tr>
<tr class="even">
<td>Assegnazione di nodo vota</td>
<td>- Nodo voti non deve essere rimosso dalla nodi nel sito primario, <em>il sito a</em><br />
- Nodo voti deve essere rimosso dalla nodi nel sito di backup, <em>sito b</em><br />
- Se si verifica un'interruzione a lungo termine <em>il sito a</em>, voti devono essere assegnati ai nodi a <em>sito b</em> per abilitare la maggior parte di quorum in tale sito come parte di ripristino</td>
</tr>
<tr class="odd">
<td>Gestione di quorum dinamico</td>
<td>Deve essere abilitato</td>
</tr>
<tr class="even">
<td>Configurazione del controllo</td>
<td>- Configurare un controllo se non esiste un numero pari dei nodi a <em>il sito a</em><br />
- Se è necessario un controllo, configurare un controllo di condivisione file o un controllo di disco che sia accessibile solo per i nodi in <em>il sito a</em> (detto anche un controllo disco asimmetrica)</td>
</tr>
<tr class="odd">
<td>Carichi di lavoro</td>
<td>Usare i proprietari preferiti per mantenere i carichi di lavoro in esecuzione nei nodi a <em>il sito a</em></td>
</tr>
</tbody>
</table>

#### Considerazioni aggiuntive per il failover manuale

- Solo i nodi al *sito a* inizialmente sono configurati con voti quorum. Questo è necessario per assicurarsi che lo stato dei nodi a *sito b* non influisce il quorum del cluster.
- Passaggi per il ripristino possono variare a seconda se *il sito a* vittima di un errore temporaneo o un errore a lungo termine.

## Ulteriori informazioni

* [Clustering di failover](failover-clustering.md)
* [Cmdlet di Windows PowerShell cluster di failover](https://docs.microsoft.com/powershell/module/failoverclusters/?view=win10-ps)
* [Informazioni su Cluster e Pool Quorum](../storage/storage-spaces/understand-quorum.md)