---
title: Scenari di ripristino di emergenza per l'infrastruttura iperconvergente
ms.prod: windows-server
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.date: 03/29/2018
description: Questo articolo descrive gli scenari attualmente disponibili per il ripristino di emergenza di Microsoft HCI (Spazi di archiviazione diretta)
ms.localizationpriority: medium
ms.openlocfilehash: 8e6372ec7b4759f672c13f4bd822172afaf3faf3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393746"
---
# <a name="disaster-recovery-with-storage-spaces-direct"></a>Ripristino di emergenza con Spazi di archiviazione diretta

> Si applica a: Windows Server 2019, Windows Server 2016

Questo argomento illustra gli scenari in cui è possibile configurare l'infrastruttura iperconvergente (HCI) per il ripristino di emergenza.

Numerose società eseguono soluzioni iperconvergenti e la pianificazione di una situazione di emergenza offre la possibilità di rimanere o tornare alla produzione rapidamente in caso di emergenza. Esistono diversi modi per configurare HCI per il ripristino di emergenza e in questo documento sono illustrate le opzioni attualmente disponibili.

Quando si verificano problemi di ripristino della disponibilità in caso di emergenza, è possibile aggirare il noto come obiettivo del tempo di ripristino (RTO). Si tratta della durata del tempo in cui è necessario ripristinare i servizi per evitare conseguenze inaccettabili per l'azienda. In alcuni casi, questo processo può essere eseguito automaticamente con la produzione ripristinata quasi immediatamente. In altri casi, l'intervento dell'amministratore manuale deve verificarsi per ripristinare i servizi.

Le opzioni di ripristino di emergenza con iperconvergenza oggi sono:

1. Più cluster che usano la replica di archiviazione
2. Replica Hyper-V tra cluster
3. Backup e ripristino

## <a name="multiple-clusters-utilizing-storage-replica"></a>Più cluster che usano la replica di archiviazione

[Replica archiviazione](../storage-replica/storage-replica-overview.md) consente la replica dei volumi e supporta la replica sincrona e asincrona. Quando si sceglie di utilizzare una replica sincrona o asincrona, è necessario considerare l'obiettivo del punto di ripristino (RPO). L'obiettivo del punto di ripristino è la quantità di possibile perdita di dati che si vuole sostenere prima che venga considerata una perdita significativa. Se si utilizza la replica sincrona, la scrittura sequenziale in entrambe le estremità viene completata nello stesso momento. Se si utilizza asincrono, le Scritture verranno replicate molto rapidamente, ma potrebbero comunque andare perse. È consigliabile prendere in considerazione l'utilizzo dell'applicazione o del file per vedere quale funziona meglio.

Replica archiviazione è un meccanismo di copia a livello di blocco rispetto a livello file; Ciò significa che i tipi di dati replicati non sono importanti. Questo lo rende un'opzione molto diffusa per l'infrastruttura iperconvergente. Replica archiviazione può anche usare diversi tipi di unità tra i partner di replica, quindi la presenza di un solo tipo di archiviazione in un HCI e di un altro tipo di archiviazione nell'altra è perfettamente accettabile. 

Una funzionalità importante di replica archiviazione è che può essere eseguita in Azure e in locale. È possibile configurare da locale a locale, da Azure ad Azure o anche da locale ad Azure (o viceversa).

In questo scenario sono presenti due cluster indipendenti distinti. Per la configurazione della replica di archiviazione tra HCI, è possibile seguire la procedura descritta nella [replica di archiviazione da cluster a cluster](../storage-replica/cluster-to-cluster-storage-replication.md).

![Diagramma della replica di archiviazione](media/storage-spaces-direct-disaster-recovery/Disaster-Recovery-Figure1.png)

Quando si distribuisce replica di archiviazione, si applicano le considerazioni seguenti. 

1.  La configurazione della replica viene eseguita al di fuori del clustering di failover. 
2.  La scelta del metodo di replica dipende dalla latenza di rete e dai requisiti di RPO. Sincrona replica i dati nelle reti a bassa latenza con coerenza di arresto anomalo per evitare perdite di dati in caso di errore. Asincrono replica i dati su reti con latenze più elevate, ma ogni sito potrebbe non avere copie identiche in un momento in cui si è verificato l'errore. 
3.  In caso di emergenza, i failover tra i cluster non sono automatici ed è necessario orchestrarli manualmente tramite i cmdlet di PowerShell per replica archiviazione. Nel diagramma precedente, ClusterA è il database primario e ClusterB è il database secondario. Se ClusterA diventa inattivo, è necessario impostare manualmente ClusterB come primario prima di poter riportare le risorse. Una volta eseguito il backup del ClusterA, è necessario impostarlo come secondario. Una volta che tutti i dati sono stati sincronizzati, apportare la modifica e scambiare di nuovo i ruoli con il modo in cui sono stati originariamente impostati.
4.  Poiché replica di archiviazione esegue solo la replica dei dati, è necessario creare una nuova macchina virtuale o un file server Scale Out (SOFS) utilizzando questi dati all'interno Gestione cluster di failover nel partner di replica.

La replica di archiviazione può essere usata se sono presenti macchine virtuali o un SOFS in esecuzione nel cluster. La possibilità di portare online le risorse nell'HCI della replica può essere manuale o automatizzata tramite l'uso di script di PowerShell.

## <a name="hyper-v-replica"></a>Replica Hyper-V

La [replica Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/set-up-hyper-v-replica) fornisce la replica a livello di macchina virtuale per il ripristino di emergenza in infrastrutture iperconvergenti. Che cosa può fare la replica Hyper-V consiste nell'eseguire una macchina virtuale e replicarla in un sito secondario o in Azure (replica). Quindi, dal sito secondario, la replica Hyper-V può replicare la macchina virtuale in una terza (replica estesa).

![Diagramma della replica Hyper-V](media/storage-spaces-direct-disaster-recovery/Disaster-Recovery-Figure2.png)

Con la replica Hyper-V, la replica viene eseguita da Hyper-V. Quando si Abilita per la prima volta una macchina virtuale per la replica, sono disponibili tre opzioni per il modo in cui si desidera che la copia iniziale venga inviata ai corrispondenti cluster di replica.

1.  Invia la copia iniziale sulla rete
2.  Inviare la copia iniziale a un supporto esterno per poterla copiare manualmente nel server
3.  Usare una macchina virtuale esistente già creata negli host di replica

L'altra opzione è quando si desidera che la replica iniziale venga eseguita.

1.  Avvia immediatamente la replica
2.  Pianificare un periodo di tempo in cui si verifica la replica iniziale. 

Altre considerazioni che saranno necessarie sono:

- Quali VHD/VHDX si desidera replicare. È possibile scegliere di replicare tutti i o solo uno di essi.
- Numero di punti di ripristino che si desidera salvare. Se si desidera disporre di diverse opzioni relative al momento in cui si desidera eseguire il ripristino, è necessario specificare il numero desiderato. Se si desidera un solo punto di ripristino, è possibile scegliere anche questa.
- Frequenza con cui si desidera che il Servizio Copia Shadow del volume (VSS) replica una copia shadow incrementale.
- Frequenza con cui vengono replicate le modifiche (30 secondi, 5 minuti, 15 minuti).

Quando HCI partecipa alla replica Hyper-V, è necessario che la risorsa [gestore di replica Hyper-v](https://blogs.technet.microsoft.com/virtualization/2012/03/27/why-is-the-hyper-v-replica-broker-required/) sia stata creata in ogni cluster. Questa risorsa esegue diverse operazioni:

1.  Fornisce un solo spazio dei nomi per ogni cluster per cui la replica Hyper-V si connette a.
2.  Determina il nodo all'interno del cluster in cui risiederà la replica (o la replica estesa) al momento della prima ricezione della copia.
3.  Tiene traccia del nodo proprietario della replica (o replica estesa) nel caso in cui la macchina virtuale si sposti in un altro nodo. Deve tenere traccia di questo in modo che, al momento della replica, possa inviare le informazioni al nodo appropriato.

## <a name="backup-and-restore"></a>Backup e ripristino

Un'opzione di ripristino di emergenza tradizionale che non parla molto, ma è altrettanto importante è l'errore dell'intero cluster o di un nodo del cluster. Una delle due opzioni con questo scenario prevede l'uso del backup di Windows NT. 

È sempre consigliabile eseguire backup periodici dell'infrastruttura iperconvergente. Mentre il servizio cluster è in esecuzione, se si esegue un backup dello stato del sistema, il database del registro di sistema del cluster fa parte di tale backup. Il ripristino del cluster o del database è costituito da due metodi diversi, ovvero non autorevole e autorevole.

### <a name="non-authoritative"></a>Non autorevole

Un ripristino non autorevole può essere eseguito utilizzando Windows NT backup e equivale a un ripristino completo solo del nodo del cluster. Se è necessario ripristinare solo un nodo del cluster (e il database del registro di sistema del cluster) e tutte le informazioni correnti sul cluster, è consigliabile eseguire il ripristino usando non autorevole. I ripristini non autorevoli possono essere eseguiti tramite l'interfaccia di backup di Windows NT o la riga di comando WBADMIN. EXE.

Una volta ripristinato il nodo, è necessario aggiungerlo al cluster. Ciò che accadrà è che verrà indirizzato al cluster in esecuzione esistente e aggiornerà tutte le relative informazioni con le informazioni attualmente presenti.

### <a name="authoritative"></a>Autorevole

Un ripristino autorevole della configurazione del cluster, d'altra parte, riprende la configurazione del cluster indietro nel tempo. Questo tipo di ripristino deve essere eseguito solo quando le informazioni del cluster sono andate perse ed è necessario ripristinarle. Ad esempio, qualcuno ha accidentalmente eliminato un file server che conteneva più di 1000 condivisioni ed è necessario ripristinarlo. Il completamento di un ripristino autorevole del cluster richiede che il backup venga eseguito dalla riga di comando.

Quando si avvia un ripristino autorevole in un nodo del cluster, il servizio cluster viene arrestato in tutti gli altri nodi della visualizzazione cluster e la configurazione del cluster è bloccata. Per questo motivo è importante che il servizio cluster nel nodo in cui è stato eseguito il ripristino venga avviato per primo, in modo che il cluster venga creato usando la nuova copia della configurazione del cluster.

Per eseguire un ripristino autorevole, è possibile eseguire i passaggi seguenti.

1.  Eseguire WBADMIN. EXE da un prompt dei comandi amministrativo per ottenere la versione più recente dei backup che si desidera installare e assicurarsi che lo stato del sistema sia uno dei componenti che è possibile ripristinare.

    ```powershell
    Wbadmin get versions
    ```

2.  Determinare se per il backup della versione sono presenti le informazioni del registro di sistema del cluster come componente. Questo comando richiede un paio di elementi, la versione e l'applicazione/componente da usare nel passaggio 3. Per la versione, ad esempio, si può dire che il backup è stato eseguito il 3 gennaio 2018 alle 2:04AM e questo è quello che è necessario ripristinare.

    ```powershell
    wbadmin get items -backuptarget:\\backupserver\location
    ```

3.  Avviare il ripristino autorevole per ripristinare solo la versione del registro di sistema del cluster necessaria. 

    ```powershell
    wbadmin start recovery -version:01/03/2018-02:04 -itemtype:app -items:cluster
    ```

Una volta eseguito il ripristino, questo nodo deve essere quello per avviare prima il servizio cluster e formare il cluster. Tutti gli altri nodi dovranno quindi essere avviati e aggiunti al cluster.

## <a name="summary"></a>Riepilogo 

Per sommare questa situazione, il ripristino di emergenza iperconvergente è un elemento che deve essere pianificato con attenzione. Esistono diversi scenari che possono essere più adatti alle proprie esigenze e che devono essere testati accuratamente. Un elemento da notare è che se si ha familiarità con i cluster di failover in passato, i cluster estesi sono stati un'opzione molto diffusa negli anni. Si è verificata una modifica della progettazione con la soluzione iperconvergente ed è basata sulla resilienza. Se si perdono due nodi in un cluster iperconvergente, l'intero cluster si arresterà. In questo caso, in un ambiente iperconvergente lo scenario di estensione non è supportato.


