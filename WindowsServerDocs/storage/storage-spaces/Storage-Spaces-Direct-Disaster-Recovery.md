---
title: Scenari di ripristino di emergenza per dell'infrastruttura Iperconvergente
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: johnmarlin-msft
ms.date: 03/29/2018
description: Questo articolo descrive gli scenari attualmente disponibili per il ripristino di emergenza di Microsoft HCI (spazi di archiviazione diretta)
ms.localizationpriority: medium
ms.openlocfilehash: 32bbf02ca78d5c6a2147162768c984d0e0b27e36
ms.sourcegitcommit: dfd25348ea3587e09ea8c2224237a3e8078422ae
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/16/2018
ms.locfileid: "4678610"
---
# Ripristino di emergenza con spazi di archiviazione diretta

> Si applica a: Windows Server 2019, Windows Server 2016

Questo argomento fornisce scenari sul modo in cui un'infrastruttura iperconvergente (HCI) possono essere configurati per il ripristino di emergenza.

Numerose aziende eseguono le soluzioni iperconvergenti e pianificazione di una situazione di emergenza offre la possibilità di rimanere in o da tornare alla produzione rapidamente se fosse necessario verificarsi una situazione di emergenza. Esistono diversi modi per configurare HCI per il ripristino di emergenza e questo documento illustra le opzioni che sono attualmente disponibili per gli utenti.

Quando discussioni di ripristino disponibilità in caso di emergenza riguardano noto come ripristino ora obiettivo (RTO). Si tratta della durata della destinazione in cui servizi devono essere ripristinati per evitare inaccettabile conseguenze per l'azienda. In alcuni casi, questo processo può verificarsi automaticamente con produzione ripristinati quasi immediatamente. In altri casi, l'intervento manuale amministratore deve verificarsi per ripristinare i servizi.

Opzioni di ripristino di emergenza con un iperconvergente oggi sono:

1. Più cluster tramite Replica archiviazione
2. Replica Hyper-V tra i cluster
3. Backup e ripristino

## Più cluster tramite Replica archiviazione

[Replica di archiviazione](../storage-replica/storage-replica-overview.md) consente la replica dei volumi e supporta la replica sincrona e asincrona. Quando si sceglie tra utilizzando la replica sincrona o asincrona, è necessario considerare l'obiettivo di punto di ripristino (RPO). Obiettivo di punto di ripristino è la quantità di perdita di dati possibili che sei disposto ad comportano prima che sia considerato perdita principale. Se Vai con la replica sincrona, in sequenza scriverà entrambe le estremità nello stesso momento. Se Vai con asincrona, le scritture replicheranno molto rapidamente, ma potrebbero continuerà a essere perse. È consigliabile l'utilizzo dell'applicazione o un file per vedere qual è il migliore per te.

Replica di archiviazione è un meccanismo di copia livello di blocco rispetto a livello di file. vale a dire, non è importante sui tipi di dati replicati. In questo modo un'opzione più popolari per dell'infrastruttura iperconvergente. Replica di archiviazione può anche usare diversi tipi di unità tra i partner di replica, quindi avere tutti dell'archiviazione di un tipo su uno HCI e un altro tipo archiviazione sull'altro è ottimale. 

Una funzione importante di Replica di archiviazione è che possono essere eseguito in Azure, nonché in locale. È possibile impostare in locale in locale, Azure per Azure, o persino in locale in Azure (o viceversa).

In questo scenario, esistono due cluster distinti indipendenti. Per la configurazione di Replica di archiviazione tra HCI, puoi seguire i passaggi descritti in [replica archiviazione da Cluster a cluster](../storage-replica/cluster-to-cluster-storage-replication.md).

![Diagramma di replica archiviazione](media\storage-spaces-direct-disaster-recovery\Disaster-Recovery-Figure1.png)

Le considerazioni seguenti si applicano durante la distribuzione di Replica di archiviazione. 

1.  Configurazione della replica viene eseguita di fuori del Clustering di Failover. 
2.  Scelta del metodo della replica sarà variano in base ai requisiti di RPO e latenza della rete. Sincrono replica i dati su reti a bassa latenza con coerenza di arresto anomalo del sistema per non garantire perdita di dati in un momento dell'errore. Replica asincrona i dati in rete con latenze più elevate, ma ogni sito potrebbero non dispongano di copie identiche alla volta dell'errore. 
3.  Nel caso di emergenza, tra i cluster di failover non sono automatico e devono essere orchestrati manualmente tramite i cmdlet di PowerShell di Replica di archiviazione. Nel diagramma precedente, ClusterA è primario e ClusterB è secondaria. Se ClusterA diventa non disponibile, dovrai impostare manualmente ClusterB come principale prima che si possono visualizzare le risorse. Una volta ClusterA verso l'alto, dovresti per renderlo secondario. Una volta che tutti i dati è stata sincronizzati alto, apporta le modifiche e i ruoli di tornare al modo in cui che sono stati impostati originariamente di scambio.
4.  Poiché Replica di archiviazione viene eseguita la replica solo i dati, una nuova macchina virtuale o Scale Out File Server (SOFS) che utilizzano questi dati dovrà essere create all'interno di gestione di Cluster di Failover nel partner di replica.

Replica di archiviazione può essere usata se si dispone di macchine virtuali o un SOFS in esecuzione nel cluster. Eseguire la migrazione di risorse online della replica HCI può essere manuale o automatica attraverso l'uso degli script di PowerShell.

## Replica Hyper-V

[Replica Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/set-up-hyper-v-replica) fornisce la replica di livello macchina virtuale per il ripristino di emergenza infrastrutture iperconvergenti. Cosa fare Replica Hyper-V è una macchina virtuale e replicare a un sito secondario o Azure (replica). Quindi dal sito secondario, Replica Hyper-V possono replicare la macchina virtuale in un terzo (replica estesa).

![Diagramma di replica Hyper-V](media\storage-spaces-direct-disaster-recovery\Disaster-Recovery-Figure2.png)

Replica Hyper-V, la replica è gestita da Hyper-V. Quando abiliti prima di tutto una macchina virtuale per la replica, ci sono tre opzioni per come desideri la copia iniziale da inviare al cluster replica corrispondente.

1.  Inviare la copia iniziale attraverso la rete
2.  Inviare la copia iniziale su un supporto esterno, in modo che possono essere copiato manualmente nel server
3.  Utilizzare una macchina virtuale esistente già creata nell'host di replica

L'altra opzione è per quando desideri che deve avvenire questa replica iniziale.

1.  Avviare immediatamente la replica
2.  Pianificare un orario per quando viene eseguita la replica iniziale. 

Altre considerazioni che ti servirà sono:

- Del file VHD/VHDX che si desidera replicare. Puoi scegliere di replicare tutti gli elementi o solo uno di essi.
- Numero di punti di ripristino che si desidera salvare. Se si desidera a disposizione varie opzioni su cosa punto nel tempo che si desidera ripristinare, quindi potresti specificare quanti desiderato. Se vuoi solo un punto di ripristino, è possibile che anche.
- Frequenza con cui si desidera avere il servizio Copia Shadow del Volume (VSS) replicare una copia shadow incrementale.
- Frequenza con cui le modifiche vengono replicate (30 secondi, 5 minuti, 15 minuti).

Quando HCI partecipa Replica Hyper-V, è necessario che la risorsa [Broker Replica Hyper-V](https://blogs.technet.microsoft.com/virtualization/2012/03/27/why-is-the-hyper-v-replica-broker-required/) creata in ogni cluster. Questa risorsa consente di diversi aspetti:

1.  Offre un singolo spazio dei nomi per ogni cluster per la Replica Hyper-V a cui connettersi.
2.  Determina quale nodo all'interno di tale cluster di replica (o replica estesa) sarà conservati su quando riceve la copia.
3.  Tiene traccia del quale nodo del proprietario della replica (o replica estesa) nel caso in cui la macchina virtuale si sposta in un altro nodo. Deve tenere traccia in modo che quando la replica viene eseguita, può inviare le informazioni per il nodo appropriato.

## Backup e ripristino

Un'opzione di ripristino di emergenza tradizionali che non è parlata molto simile, ma è importante è l'errore dell'intero cluster o un nodo del cluster. Entrambe le opzioni con questo scenario viene utilizzato Windows NT Backup. 

È sempre un suggerimento di disporre di backup periodici dell'infrastruttura iperconvergente. Mentre è in esecuzione il servizio Cluster, se eseguire un Backup dello stato del sistema, il database del Registro di sistema del cluster sarebbe una parte di questo backup. Ripristino del cluster o al database include due metodi diversi (Non-autorevoli e autorevoli).

### Non attendibile

Un ripristino Non attendibile, è possibile utilizzare Windows NT Backup ed equivale a un ripristino completo del semplicemente il nodo del cluster. Se è necessario solo ripristinare un nodo del cluster (e il database del Registro di sistema del cluster) e tutti i cluster informazioni aggiornate buona, sarebbe ripristino usando non attendibile. Non autorevole possono essere eseguite tramite l'interfaccia di Windows NT Backup o la riga di comando WBADMIN. FILE EXE.

Una volta che il ripristino del nodo, restare al cluster. Che cosa accadrà è verrà inviata al cluster in esecuzione esistenti e aggiornare tutte le informazioni di ciò che è attualmente presente.

### Autorevole

Il ripristino della configurazione del cluster, d'altro canto, richiede la configurazione del cluster in tempo. Questo tipo di ripristino deve essere eseguito solo quando le informazioni di cluster stesso sono state smarrite ed esigenze ripristinate. Ad esempio, un utente eliminato per un File Server di contenuti condivisioni oltre 1000 e ti servono indietro. Completamento autorevole del cluster richiede che il Backup venga eseguita dalla riga di comando.

Quando un ripristino autorevole viene avviato in un nodo del cluster, il servizio cluster viene arrestato in tutti gli altri nodi nella visualizzazione cluster e la configurazione del cluster è bloccata. Ecco perché è fondamentale che il servizio cluster nel nodo in cui è stato eseguito il ripristino essere avviati prima di tutto in modo che il cluster è ben con la nuova copia della configurazione del cluster.

Per eseguire attraverso autorevole, è possono eseguire i passaggi seguenti.

1.  Eseguire WBADMIN. File EXE da un prompt dei comandi amministrativo per ottenere la versione più recente di backup che si desidera installare e assicurati che lo stato del sistema è uno dei componenti è possibile ripristinare.

    ```powershell
    Wbadmin get versions
    ```

2.  Determinare se il backup di versione che è stato le informazioni del Registro di sistema del cluster in esso come componente. Ci sono due gli elementi che necessari da questo comando, la versione e l'applicazione/componente da usare nel passaggio 3. Per la versione, ad esempio, che il backup è stato eseguito 3 gennaio 2018 a 2:04 am e questo è quello che devi ripristinato.

    ```powershell
    wbadmin get items -backuptarget:\\backupserver\location
    ```

3.  Avvia il ripristino attendibile per recuperare solo la versione di registro di sistema del cluster che è necessario. 

    ```powershell
    wbadmin start recovery -version:01/03/2018-02:04 -itemtype:app -items:cluster
    ```

Dopo il ripristino ha avuto luogo, questo nodo deve essere quello di avviare il servizio Cluster prima di tutto e formare il cluster. Tutti gli altri nodi quindi dovrà essere avviati e al cluster.

## Riepilogo 

Per questo somma tutti massimo, il ripristino di emergenza iper-convergenti è qualcosa che deve essere pianificato con attenzione. Esistono diversi scenari che possono più adatto alle tue esigenze e devono essere accuratamente testati. Un elemento da notare è che se hai familiarità con i cluster di Failover in passato, cluster estesi sono state un'opzione molto diffuso negli anni. Si è verificato un po' di una modifica di progettazione con la soluzione iperconvergente e si basa sulla resilienza. In caso di perdita di due nodi in un cluster iperconvergente, l'intero cluster illustreranno verso il basso. Con questa fase il caso, in un ambiente iperconvergente, lo scenario esteso non è supportato.


