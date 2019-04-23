---
title: Scenari di ripristino di emergenza per l'infrastruttura Iperconvergente
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.date: 03/29/2018
description: Questo articolo vengono descritti gli scenari attualmente disponibili per il ripristino di emergenza di Microsoft HCI (spazi di archiviazione diretta)
ms.localizationpriority: medium
ms.openlocfilehash: 32bbf02ca78d5c6a2147162768c984d0e0b27e36
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879592"
---
# <a name="disaster-recovery-with-storage-spaces-direct"></a>Ripristino di emergenza con spazi di archiviazione diretta

> Si applica a: Windows Server 2019, Windows Server 2016

In questo argomento fornisce scenari in modalità infrastruttura iperconvergente (HCL) possono essere configurati per il ripristino di emergenza.

Numerose aziende eseguono soluzioni iperconvergente e pianificazione di emergenza offre la possibilità di rimanere in o tornare nell'ambiente di produzione rapidamente se si verifica una situazione di emergenza. Esistono diversi modi per configurare l'uomo per il ripristino di emergenza e in questo documento illustra le opzioni che sono attualmente disponibili per l'utente.

Quando le discussioni di disponibilità di ripristino se si verifica un'emergenza vertono ciò che è noto come obiettivo del tempo di ripristino (RTO). Si tratta della durata di tempo in cui i servizi devono essere ripristinati per evitare conseguenze inaccettabili all'azienda di destinazione. In alcuni casi, questo processo può avvenire automaticamente con la produzione ripristinata quasi immediatamente. In altri casi, deve verificarsi l'intervento dell'amministratore manuale per ripristinare i servizi.

Le opzioni di ripristino di emergenza con un iperconvergente oggi sono:

1. Più cluster che utilizzano la Replica di archiviazione
2. Replica Hyper-V tra cluster
3. Backup e ripristino

## <a name="multiple-clusters-utilizing-storage-replica"></a>Più cluster che utilizzano la Replica di archiviazione

[Replica archiviazione](../storage-replica/storage-replica-overview.md) abilita la replica dei volumi e supporta la replica sincrona e asincrona. Quando si sceglie tra l'uso di replica sincrona o asincrona, si dovrebbe prendere in considerazione l'obiettivo punto di ripristino (RPO). Obiettivo del punto di ripristino è la quantità di perdita di dati che è disposti a sostenere prima che venga considerato grosse perdite. Se si procede con la replica sincrona, in modo sequenziale scriverà entrambe le estremità allo stesso tempo. Se si procede con asincroni, operazioni di scrittura replicherà molto veloce ma potrebbe comunque essere persi. È opportuno considerare l'utilizzo dell'applicazione o un file per trovare quello ottimale per te.

Replica di archiviazione è un meccanismo di copia a livello di blocco e a livello di file; vale a dire, non è importante sui tipi di dati in fase di replica. Questo rende un'opzione diffusa per infrastruttura iperconvergente. Replica di archiviazione può usare anche diversi tipi di unità tra i partner di replica, quindi, inserirli tutti di spazio di archiviazione di un tipo in un uomo e un altro spazio di archiviazione di tipo in altro è assolutamente adeguato. 

Un'importante funzionalità di Replica di archiviazione è che può essere eseguito in Azure, nonché in locale. È possibile impostare in locale a locale, Azure ad Azure, o addirittura in locale in Azure (o viceversa).

In questo scenario, esistono due cluster indipendenti separato. Per la configurazione della Replica di archiviazione tra uomo, è possibile seguire i passaggi descritti in [replica di archiviazione da Cluster a cluster](../storage-replica/cluster-to-cluster-storage-replication.md).

![Diagramma di replica archiviazione](media\storage-spaces-direct-disaster-recovery\Disaster-Recovery-Figure1.png)

Le considerazioni seguenti si applicano quando si distribuisce la Replica di archiviazione. 

1.  Configurazione della replica viene eseguita di fuori del Clustering di Failover. 
2.  La scelta del metodo di replica saranno dipende la latenza di rete e i requisiti di RPO. Sincrono replica i dati su reti a bassa latenza con la coerenza di arresto anomalo del sistema per verificare che nessuna perdita di dati in un momento dell'errore. Replica asincrona i dati su reti con latenze più elevate, ma ogni sito non abbia copie identiche in una fase di errore. 
3.  In caso di emergenza, i failover tra i cluster non sono automatici e dovranno essere orchestrato manualmente tramite i cmdlet di PowerShell di Replica di archiviazione. Nel diagramma precedente, ClusterA è la replica primaria e ClusterB è quello secondario. Se ClusterA diventa inattivo, è necessario impostare manualmente ClusterB come primario prima di connettere le risorse di. Una volta ClusterA eseguire il backup, è necessario renderlo secondario. Dopo che sono già stati sincronizzati tutti i dati di backup, apportare la modifica e scambiare i ruoli nuovamente al modo in cui che sono state originariamente impostate.
4.  Poiché Replica archiviazione è la replica solo i dati, una nuova macchina virtuale o scalabilità Out File Server di scalabilità orizzontale (SOFS) che usano i dati dovrà essere creato all'interno di gestione di Cluster di Failover nel partner di replica.

Replica di archiviazione è utilizzabile se si dispone di macchine virtuali o un SOFS in esecuzione nel cluster. Portare online le risorse nella replica uomo può essere manuali o automatizzati tramite l'utilizzo di script di PowerShell.

## <a name="hyper-v-replica"></a>Replica Hyper-V

[Replica Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/set-up-hyper-v-replica) fornisce la replica a livello di macchina virtuale di ripristino di emergenza in infrastrutture iperconvergente. Operazioni che è possibile eseguire la Replica Hyper-V consiste nel disconnettere una macchina virtuale ed eseguirne la replica per un sito secondario o in Azure (replica). Quindi dal sito secondario, la Replica Hyper-V può replicare la macchina virtuale in una terza (replica estesa).

![Diagramma di replica Hyper-V](media\storage-spaces-direct-disaster-recovery\Disaster-Recovery-Figure2.png)

Con la Replica Hyper-V, la replica viene preso in considerazione da Hyper-V. Quando si attiva prima di tutto una macchina virtuale per la replica, sono disponibili tre opzioni per la modalità d' la copia iniziale da inviare per il cluster di replica corrispondente.

1.  Inviare la copia iniziale sulla rete
2.  Inviare la copia iniziale su un supporto esterno in modo che possono essere copiato manualmente nel server
3.  Usare una macchina virtuale esistente già creata in host della replica

L'altra opzione viene generato quando si vuole eseguita la replica iniziale.

1.  Avviare immediatamente la replica
2.  Pianificare un orario per quando la replica iniziale venga eseguita. 

Altre considerazioni che saranno necessari sono:

- Del file VHD/VHDX si desidera replicare. È possibile scegliere di replicare tutti gli elementi o solo uno di essi.
- Numero di punti di ripristino che si desidera salvare. Se desideri sono disponibili diverse opzioni su ciò che punto nel tempo che si desidera ripristinare, quindi è preferibile specificare il numero desiderato. Se si vuole solo un punto di ripristino, è possibile che anche.
- Quanto spesso si vuole che il servizio Copia Shadow del Volume (VSS) eseguire la replica di una copia shadow incrementale.
- Quanto spesso le modifiche vengono replicate (30 secondi, 5 minuti, 15 minuti).

UOMO fa parte di Replica Hyper-V, è necessario che sia la [Hyper-V Replica Broker](https://blogs.technet.microsoft.com/virtualization/2012/03/27/why-is-the-hyper-v-replica-broker-required/) risorsa creata in ogni cluster. Questa risorsa ha vari scopi:

1.  Offre un unico spazio dei nomi per ogni cluster per la Replica Hyper-V a cui connettersi.
2.  Determina quale nodo all'interno di tale cluster di replica (o replica estesa) sarà collocati nel quando riceve la copia.
3.  Tiene traccia di quale nodo proprietario della replica (o replica estesa) nel caso in cui la macchina virtuale passa a un altro nodo. È necessario tenere traccia dell'operazione in modo che quando la replica venga eseguita, può inviare le informazioni sul nodo appropriato.

## <a name="backup-and-restore"></a>Backup e ripristino

Un'opzione di ripristino di emergenza tradizionali che non è parlata moltissimo, ma è altrettanto importante è il negativo dell'intero cluster o un nodo del cluster. Entrambe le opzioni con questo scenario viene utilizzato Windows NT Backup. 

È sempre consigliabile disporre di backup periodico dell'infrastruttura iperconvergente. Mentre è in esecuzione il servizio Cluster, se si esegue un Backup dello stato del sistema, il database di registro cluster sarà parte del backup desiderato. Il ripristino del cluster o il database dispone di due metodi diversi (Non-Authoritative e autorevole).

### <a name="non-authoritative"></a>Non autorevole

Un ripristino autorevole Non è possibile usare Backup di Windows NT e corrisponde a un ripristino completo del solo il nodo del cluster stesso. Se è sufficiente ripristinare un nodo del cluster (e il database del Registro di sistema del cluster) e tutti i cluster le informazioni correnti valida, si ripristina usando non autorevole. Le operazioni di ripristino non autorevole può avvenire tramite l'interfaccia di Windows NT Backup o la riga di comando WBADMIN. FILE EXE.

Dopo aver ripristinato il nodo, lasciarla di connettersi al cluster. Cosa accade è che verranno inviate al cluster in esecuzione esistente e aggiornare tutte le informazioni con i dati attualmente presenti.

### <a name="authoritative"></a>Autorevole

Un ripristino autorevole di configurazione del cluster, d'altra parte, richiede la configurazione del cluster indietro nel tempo. Questo tipo di ripristino deve essere eseguito solo quando sono stato perso e deve ripristinare le informazioni del cluster stesso. Ad esempio, un utente eliminato per errore un File Server che conteneva più di 1000 condivisioni e ti servono nuovamente. Il completamento di un ripristino autorevole del cluster richiede che il Backup venga eseguita dalla riga di comando.

Quando viene avviato un ripristino autorevole in un nodo del cluster, il servizio cluster viene arrestato su tutti gli altri nodi nella visualizzazione del cluster e la configurazione del cluster è stata bloccata. È per questo motivo è fondamentale che il servizio cluster nel nodo in cui è stato eseguito il ripristino è possibile avviare prima di tutto in modo che il cluster viene formato utilizzando la nuova copia della configurazione del cluster.

Per eseguire un ripristino autorevole, la procedura seguente può essere eseguita.

1.  Eseguire WBADMIN. EXE da un prompt dei comandi amministrativo per ottenere la versione più recente di backup che si desidera installare e verificare che lo stato del sistema è uno dei componenti è possibile ripristinare.

    ```powershell
    Wbadmin get versions
    ```

2.  Determinare se il backup di versione che è necessario contiene le informazioni del Registro di sistema del cluster come un componente. Esistono un paio gli elementi che necessari da questo comando, la versione e il componente dell'applicazione o per l'uso nel passaggio 3. Per la versione, ad esempio, è stato eseguito il backup il 3 gennaio 2018 alle 04 2:00 e questo è quello che è necessario ripristinata.

    ```powershell
    wbadmin get items -backuptarget:\\backupserver\location
    ```

3.  Avviare il ripristino autorevole per ripristinare solo la versione del Registro di sistema cluster che è necessario. 

    ```powershell
    wbadmin start recovery -version:01/03/2018-02:04 -itemtype:app -items:cluster
    ```

Dopo il ripristino ha avuto luogo, questo nodo deve essere quello in cui avviare il servizio Cluster prima di tutto e formare il cluster. Tutti gli altri nodi dovrà quindi essere avviato e connettersi al cluster.

## <a name="summary"></a>Riepilogo 

Per questa somma attaccarli tutti, il ripristino di emergenza iperconvergente è qualcosa che deve essere pianificato con cura. Esistono diversi scenari che possono meglio si adatta alle proprie esigenze e devono essere verificati. Un elemento degno di nota è che se si ha familiarità con i cluster di Failover in passato, i cluster estesi siano stati un'opzione molto diffusa negli anni. Si è verificato un po' di una modifica di progettazione con la soluzione iperconvergente e si basa sulla resilienza. Se si perdono due nodi in un cluster iperconvergente, l'intero cluster sarà disattivato. Questo è il caso, in un ambiente iperconvergente, l'istanza di stretch scenario non è supportato.


