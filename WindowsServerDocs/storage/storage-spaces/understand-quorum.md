---
title: Quorum del cluster e pool di comprensione
description: Informazioni su Cluster e Pool di Quorum, con esempi specifici, esaminare le complicazioni.
keywords: Spazi di archiviazione diretta, Quorum, controllo del mirroring, S2D, Cluster Quorum, quorum del Pool, Cluster, Pool
ms.prod: windows-server-threshold
ms.author: adagashe
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/18/2019
ms.localizationpriority: medium
ms.openlocfilehash: 24890b191db8bc6934132857e830d4f77c394b02
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879972"
---
# <a name="understanding-cluster-and-pool-quorum"></a>Quorum del cluster e pool di comprensione

>Si applica a: Windows Server 2019, Windows Server 2016

[Windows Server Failover Clustering](../../failover-clustering/failover-clustering-overview.md) garantisce un'elevata disponibilità per carichi di lavoro. Queste risorse sono considerate a disponibilità elevata se i nodi che ospitano le risorse sono attivi; Tuttavia, il cluster in genere richiede più della metà dei nodi sia in esecuzione, caratteristica nota come se avessero *quorum*.

Quorum è progettato per impedire *"split Brain"* scenari che possono verificarsi quando è presente una partizione nella rete e subset di nodi non possono comunicare tra loro. Ciò può provocare entrambi sottoinsiemi dei nodi per tentare il proprietario del carico di lavoro e scrivere sul disco stesso che può causare numerosi problemi. Tuttavia, non è possibile con concetto del Clustering di Failover di quorum che impone solo uno di questi gruppi di nodi per continuare l'esecuzione, in modo che solo uno di questi gruppi verrà rimanere online.

Quorum determina il numero di errori che il cluster può sostenere pur rimanendo online. Quorum è progettato per gestire lo scenario quando si verifica un problema con la comunicazione tra i subset di nodi del cluster, in modo che non tenti più server simultaneamente un gruppo di risorse host e scrivere lo stesso disco nello stesso momento. Con questo concetto del quorum, il cluster forzerà l'arresto in uno dei subset di nodi da verificare che sia presente solo un vero proprietario di un determinato gruppo di risorse del servizio cluster. Una volta che i nodi che sono stati arrestati ancora una volta possono comunicare con il gruppo principale dei nodi, verrà automaticamente ricostituzione del cluster e avviare il servizio cluster.

In Windows Server 2016, esistono due componenti del sistema che hanno i propri meccanismi di quorum:

- <strong>Il Quorum del cluster</strong>: Questo oggetto opera a livello di cluster (ad esempio è possibile perdere i nodi e cluster restare sempre aggiornato)
- <strong>Pool Quorum</strong>: Si opera a livello di pool quando spazi di archiviazione diretta è abilitato (ovvero è possibile perdere i nodi e le unità e il pool di restare sempre aggiornato). I pool di archiviazione sono stati progettati per essere usato in scenari sia cluster sia non cluster, motivo per cui dispongono di un meccanismo di quorum diverse.

## <a name="cluster-quorum-overview"></a>Panoramica del quorum del cluster

Nella tabella seguente offre una panoramica dei risultati del quorum del Cluster per ogni scenario:

| Nodi del server | Possono restare attive quando un errore del nodo server | Errore del nodo un server, poi un altro possono restare attive quando | Possono restare attive quando due errori dei nodi simultanee server |
|--------------|-------------------------------------|---------------------------------------------------|----------------------------------------------------|
| 2            | 50/50                               | No                                                | No                                                 |
| 2 + server di controllo  | Yes                                 | No                                                | No                                                 |
| 3            | Yes                                 | 50/50                                             | No                                                 |
| 3 + server di controllo  | Yes                                 | Yes                                               | No                                                 |
| 4            | Yes                                 | Yes                                               | 50/50                                              |
| 4 + server di controllo  | Yes                                 | Yes                                               | Yes                                                |
| 5 e versioni successive  | Yes                                 | Yes                                               | Yes                                                |

### <a name="cluster-quorum-recommendations"></a>Raccomandazioni del quorum del cluster

- Se si dispone di due nodi, è un controllo del mirroring <strong>obbligatorio</strong>.
- Se si dispone di tre o quattro nodi, è witness <strong>consigliabile</strong>.
- Se si ha accesso a Internet, usare una  <strong>[cloud di controllo](../../failover-clustering/deploy-cloud-witness.md)</strong>
- Se lavora in un ambiente IT con altri computer e le condivisioni file, usare una condivisione file di controllo

## <a name="how-cluster-quorum-works"></a>Funzionamento del quorum del cluster

Quando i nodi hanno esito negativo o quando alcuni subset di nodi perde il contatto con un altro subset, nodi restanti necessari verificare che costituiscano la *maggioranza* del cluster deve rimanere online. Se non è possibile verificare che, si rivolgeranno offline.

Il concetto di ma *maggioranza* solo funziona correttamente quando il numero totale di nodi del cluster sia dispari (ad esempio, tre nodi in un cluster con cinque nodi). Pertanto, per quanto riguarda i cluster con un numero pari di nodi (ad esempio, un cluster a quattro nodi)?

Esistono due modi per il cluster può rendere il *numero totale di voti* dispari:

1. In primo luogo, può passare *iscrizione* uno tramite l'aggiunta di un *witness* con un voto aggiuntivo. Ciò richiede la configurazione utente.
2.  In alternativa, è possibile accedere *verso il basso* uno azzerando uno sfortunato voto del nodo (viene eseguita automaticamente in base alle esigenze).

Ogni volta che nodi rimasti attivi correttamente, verificare che siano i *maggioranza*, la definizione di *maggioranza dei nodi* viene aggiornato per essere tra solo oggetti esclusi. In questo modo il cluster perde un nodo, quindi un altro, poi un altro e così via. Questo concetto del *numero totale di voti* adattamento dopo gli errori successivi è noto come  <strong>*dinamica del quorum*</strong>.  

### <a name="dynamic-witness"></a>Controllo del mirroring dinamico

Controllo dinamico attiva o disattiva il voto di controllo del mirroring per assicurarsi che il *numero totale di voti* è dispari. Se sono presenti un numero dispari di voti, il server di controllo non dispone di un voto. Se è presente un numero pari di voti, il controllo del mirroring dispone di un voto. Controllo dinamico riduce notevolmente il rischio che il cluster sarà disattivato a causa di errori di controllo del mirroring. Il cluster decide se utilizzare il voto di controllo in base al numero di nodi votanti disponibili nel cluster.

Dinamica del quorum funziona con server di controllo dinamico nel modo descritto di seguito.

### <a name="dynamic-quorum-behavior"></a>Comportamento di quorum dinamico

- Se si dispone di un <strong>anche</strong> numero di nodi e non del server di controllo *un nodo Ottiene proprio voto azzerato*. Ad esempio, solo tre dei quattro nodi ottenere voti, in modo che il *numero totale di voti* è la terza e due i superstiti con voti vengono considerati una maggioranza dei nodi.
- Se si dispone di un <strong>dispari</strong> numero di nodi e non del server di controllo *ricevono voti*.
- Se si dispone di un <strong>persino</strong> numero di nodi e del server di controllo *voti il controllo del mirroring*, pertanto il totale è dispari.
- Se si dispone di un <strong>dispari</strong> numero di nodi e del server di controllo *controllo del mirroring non vota*.

Quorum dinamico offre la possibilità di assegnare un voto a un nodo in modo dinamico per evitare di perdere la maggioranza di voti e consentire al cluster per l'esecuzione con un nodo (noto come permanente last-man). Diamo un cluster a quattro nodi come esempio. Si supponga di che cui il quorum richiede 3 voti. 

In questo caso, sarebbe stati nel cluster verso il basso se più di due nodi.

![Diagramma che mostra quattro nodi del cluster, ognuno dei quali Ottiene un voto](media/understand-quorum/dynamic-quorum-base.png)

Tuttavia, dinamica del quorum impedisce che questo accada. Il *numero totale di voti* necessari per quorum a questo punto è determinato in base al numero di nodi disponibili. Pertanto, con dinamica del quorum, il cluster rimarrà backup anche se si perdono tre nodi.

![Diagramma che mostra quattro nodi del cluster, con nodi di errori uno alla volta e il numero di voti necessari rettificare dopo ogni errore.](media/understand-quorum/dynamic-quorum-step-through.png)

Lo scenario riportato sopra si applica a un cluster generale che non dispone di spazi di archiviazione diretta abilitata. Tuttavia, quando spazi di archiviazione diretta è abilitato, il cluster può supportare solo due errori di nodo. Questo aspetto viene spiegato più il [sezione del quorum del pool](#poolQuorum).

### <a name="examples"></a>Esempi

#### <a name="two-nodes-without-a-witness"></a>Due nodi senza un server di controllo. 
Viene riempito di zeri voto del nodo, in modo che il *maggioranza* voto è determinato su un totale di <strong>1 voto</strong>. Se il nodo non di voto si arresta in modo imprevisto, superstite ha 1/1 e il cluster continua a funzionare. Se il voto nodo si arresta in modo imprevisto, superstite ha 0/1 e il cluster si arresta. Se il nodo voto normalmente è spento, il voto viene trasferito a altro nodo e continua a funzionare, il cluster. *<strong>È per questo motivo è fondamentale per configurare un server di controllo.</strong>*

![Quorum spiegato nel caso con due nodi senza un server di controllo](media/understand-quorum/2-node-no-witness.png)

- Possono restare attive quando un errore del server: <strong>Il 50 percento di probabilità</strong>.
- Errore di un server, poi un altro, possono restare attive quando: <strong>No</strong>.
- Possono restare attive quando due errori del server in una sola volta: <strong>No</strong>. 

#### <a name="two-nodes-with-a-witness"></a>Due nodi con un server di controllo. 
Votano entrambi i nodi, più voti il controllo del mirroring, in modo che il *maggioranza* viene determinato su un totale di <strong>3 voti</strong>. Se dei nodi diventa inattivo, superstite dispone di 2 o 3 e il cluster continua a funzionare.

![Quorum spiegato nel caso con due nodi con un server di controllo](media/understand-quorum/2-node-witness.png)

- Possono restare attive quando un errore del server: <strong>Sì</strong>.
- Errore di un server, poi un altro, possono restare attive quando: <strong>No</strong>.
- Possono restare attive quando due errori del server in una sola volta: <strong>No</strong>. 

#### <a name="three-nodes-without-a-witness"></a>Tre nodi senza un server di controllo.
Votano tutti i nodi, in modo che il *maggioranza* viene determinato su un totale di <strong>3 voti</strong>. Se qualsiasi nodo diventa inattivo, i superstiti sono 2 o 3 e il cluster continua a funzionare. Il cluster diventa due nodi senza un server di controllo: a questo punto, ci si trova in uno Scenario 1.

![Quorum spiegato nel caso con tre nodi senza un server di controllo](media/understand-quorum/3-node-no-witness.png)

- Possono restare attive quando un errore del server: <strong>Sì</strong>.
- Errore di un server, poi un altro, possono restare attive quando: <strong>Il 50 percento di probabilità</strong>.
- Possono restare attive quando due errori del server in una sola volta: <strong>No</strong>. 

#### <a name="three-nodes-with-a-witness"></a>Tre nodi con un server di controllo.
Votano tutti i nodi, in modo che il server di controllo non viene inizialmente votare. Il *maggioranza* viene determinato su un totale di <strong>3 voti</strong>. Dopo un errore, il cluster ha due nodi con un server di controllo – torna allo Scenario 2. Quindi, ora i due nodi e il controllo di votare.

![Quorum spiegato nel caso con tre nodi con un server di controllo](media/understand-quorum/3-node-witness.png)

- Possono restare attive quando un errore del server: <strong>Sì</strong>.
- Errore di un server, poi un altro, possono restare attive quando: <strong>Sì</strong>.
- Possono restare attive quando due errori del server in una sola volta: <strong>No</strong>. 

#### <a name="four-nodes-without-a-witness"></a>Quattro nodi senza un server di controllo
Viene riempito di zeri voto del nodo, in modo che il *maggioranza* viene determinato su un totale di <strong>3 voti</strong>. Dopo un errore, il cluster diventa tre nodi e ci si trova in uno Scenario 3.

![Quorum spiegato nel caso con quattro nodi senza un server di controllo](media/understand-quorum/4-node-no-witness.png)

- Possono restare attive quando un errore del server: <strong>Sì</strong>.
- Errore di un server, poi un altro, possono restare attive quando: <strong>Sì</strong>.
- Possono restare attive quando due errori del server in una sola volta: <strong>Il 50 percento di probabilità</strong>. 

#### <a name="four-nodes-with-a-witness"></a>Quattro nodi con un server di controllo.
Tutti i voti a nodi e i voti di controllo del mirroring, in modo che il *maggioranza* viene determinato su un totale di <strong>5 voti</strong>. Dopo un errore, viene visualizzato lo Scenario 4. Dopo due errori simultanei, passare direttamente allo Scenario 2.

![Quorum spiegato nel caso con quattro nodi con un server di controllo](media/understand-quorum/4-node-witness.png)

- Possono restare attive quando un errore del server: <strong>Sì</strong>.
- Errore di un server, poi un altro, possono restare attive quando: <strong>Sì</strong>.
- Possono restare attive quando due errori del server in una sola volta: <strong>Sì</strong>. 

#### <a name="five-nodes-and-beyond"></a>Cinque nodi e versioni successive.
Tutti tranne uno voto, qualsiasi effettua il totale dispari o votano tutti i nodi. Spazi di archiviazione diretta non possono gestire più di due nodi verso il basso, pertanto a questo punto, nessun controllo del mirroring è necessario o utile.

![Quorum spiegato nel caso con cinque nodi e altro ancora](media/understand-quorum/5-nodes.png)

- Possono restare attive quando un errore del server: <strong>Sì</strong>.
- Errore di un server, poi un altro, possono restare attive quando: <strong>Sì</strong>.
- Possono restare attive quando due errori del server in una sola volta: <strong>Sì</strong>. 

Ora che abbiamo capito di funzionamento del quorum, esaminiamo i tipi di server di controllo del quorum.

### <a name="quorum-witness-types"></a>Tipi di controllo del quorum

Clustering di failover supporta tre tipi di Quorum server di controllo:

- <strong>[Cloud Witness](../../failover-clustering\deploy-cloud-witness.md)</strong>  -Blob di archiviazione di Azure accessibili da tutti i nodi del cluster. Conserva le informazioni del clustering in un file witness log, ma non archivia una copia del database del cluster.
- <strong>Controllo di condivisione di file</strong> : condivisione di file SMB A cui è configurato in un file server che esegue Windows Server. Conserva le informazioni del clustering in un file witness log, ma non archivia una copia del database del cluster.
- <strong>Disco di controllo</strong> -un piccolo disco che si trova in gruppo di Cluster archiviazione disponibile. Questo disco è a disponibilità elevata e possibile eseguire il failover tra nodi. Contiene una copia del database del cluster.  <strong>*Un disco di controllo non è supportato con spazi di archiviazione diretta*</strong>.

## <a id="poolQuorum"></a>Panoramica del quorum del pool

Abbiamo appena detto quorum del Cluster, che opera a livello di cluster. A questo punto, passiamo il Quorum del Pool, che opera a livello di pool (ad esempio si possono perdere i nodi e le unità e disporre il pool di restare sempre aggiornato). I pool di archiviazione sono stati progettati per essere usato in scenari sia cluster sia non cluster, motivo per cui dispongono di un meccanismo di quorum diverse.

Nella tabella seguente offre una panoramica dei risultati del Quorum del Pool per ogni scenario:

| Nodi del server | Possono restare attive quando un errore del nodo server | Errore del nodo un server, poi un altro possono restare attive quando | Possono restare attive quando due errori dei nodi simultanee server |
|--------------|-------------------------------------|---------------------------------------------------|----------------------------------------------------|
| 2            | No                                  | No                                                | No                                                 |
| 2 + server di controllo  | Yes                                 | No                                                | No                                                 |
| 3            | Yes                                 | No                                                | No                                                 |
| 3 + server di controllo  | Yes                                 | No                                                | No                                                 |
| 4            | Yes                                 | No                                                | No                                                 |
| 4 + server di controllo  | Yes                                 | Yes                                               | Yes                                                |
| 5 e versioni successive  | Yes                                 | Yes                                               | Yes                                                |

## <a name="how-pool-quorum-works"></a>Funzionamento del quorum del pool

Quando le unità hanno esito negativo o quando un subset delle unità perde contatto con un altro sottoinsieme, superstite unità necessario verificare che costituiscano la *maggioranza* del pool di rimanere online. Se non è possibile verificare che, si rivolgeranno offline. Il pool è l'entità che viene portato offline o rimane in linea di base a eventuale numero sufficiente di dischi per il quorum (50% + 1). Il proprietario della risorsa del pool (nodo attivo del cluster) può essere il + 1.

Ma quorum del pool funziona in modo diverso dal quorum del cluster nei modi seguenti:

- il pool Usa un nodo del cluster come server di controllo come un spareggio necessaria per superare la metà delle unità verificata (in questo nodo è il proprietario della risorsa del pool)
- il pool non dispone di quorum dinamico
- il pool può neimplementuje METODU la propria versione della rimozione di un voto

### <a name="examples"></a>Esempi

#### <a name="four-nodes-with-a-symmetrical-layout"></a>Quattro nodi con un layout simmetrico. 
Ogni 16 unità dispone di un voto e nodo due dispone anche di un voto (perché è il proprietario della risorsa del pool). Il *maggioranza* viene determinato su un totale di <strong>16 voti</strong>. Se si disattivano tre e quattro i nodi, il subset superstito ha 8 unità e il proprietario della risorsa del pool, ovvero i voti 9/16. Pertanto, il pool continua a funzionare.

![Pool Quorum 1](media/understand-quorum/pool-1.png)

- Possono restare attive quando un errore del server: <strong>Sì</strong>.
- Errore di un server, poi un altro, possono restare attive quando: <strong>Sì</strong>.
- Possono restare attive quando due errori del server in una sola volta: <strong>Sì</strong>. 

#### <a name="four-nodes-with-a-symmetrical-layout-and-drive-failure"></a>Quattro nodi con un errore di layout e l'unità simmetrico. 
Ogni 16 unità dispone di un voto e il nodo 2 ha anche un voto (perché è il proprietario della risorsa del pool). Il *maggioranza* viene determinato su un totale di <strong>16 voti</strong>. In primo luogo, unità 7 si arresta. Se si disattivano tre e quattro i nodi, il sottoinsieme superstito ha 7 unità e il proprietario della risorsa del pool, ovvero i voti di 8 o 16. Pertanto, il pool non ha maggioranza dei nodi e si arresta.

![Pool Quorum 2](media/understand-quorum/pool-2.png)

- Possono restare attive quando un errore del server: <strong>Sì</strong>.
- Errore di un server, poi un altro, possono restare attive quando: <strong>No</strong>.
- Possono restare attive quando due errori del server in una sola volta: <strong>No</strong>. 

#### <a name="four-nodes-with-a-non-symmetrical-layout"></a>Quattro nodi con un layout non simmetrica. 
Ogni 24 unità dispone di un voto e nodo due dispone anche di un voto (perché è il proprietario della risorsa del pool). Il *maggioranza* viene determinato su un totale di <strong>24 voti</strong>. Se si disattivano tre e quattro i nodi, il sottoinsieme superstito ha 8 unità e il proprietario della risorsa del pool, ovvero i voti 9/24. Pertanto, il pool non ha maggioranza dei nodi e si arresta.

![Pool Quorum 3](media/understand-quorum/pool-3.png)

- Possono restare attive quando un errore del server: <strong>Sì</strong>.
- Errore di un server, poi un altro, possono restare attive quando: <strong>Dipende dal</strong> (non può sopravvivere se entrambi i nodi, tre e quattro diventano inattivi, ma possono restare attive quando tutti gli altri scenari.
- Possono restare attive quando due errori del server in una sola volta: <strong>Dipende dal</strong> (non può sopravvivere se entrambi i nodi, tre e quattro diventano inattivi, ma possono restare attive quando tutti gli altri scenari.

### <a name="pool-quorum-recommendations"></a>Raccomandazioni per i pool del quorum

- Assicurarsi che ogni nodo del cluster è simmetrico (ogni nodo ha lo stesso numero di unità)
- Abilitare tre vie o doppia parità in modo che è possibile tollerare i guasti di un nodo e mantenere online i dischi virtuali. Vedere la [pagina di informazioni aggiuntive sul volume](plan-volumes.md) per altri dettagli.
- Se un disco in un altro nodo e più di due nodi sono inattivi o due nodi sono inattivi, i volumi potrebbero non avere accesso a tutte le tre copie dei dati e pertanto essere portati offline e non essere disponibile. È consigliabile per riportare online il server o sostituire dischi rapidamente per garantire la resilienza la maggior parte per tutti i dati nel volume.

## <a name="more-information"></a>Altre informazioni

- [Configurazione e la gestione del quorum](../../failover-clustering/manage-cluster-quorum.md)
- [Distribuire un cloud di controllo](../../failover-clustering/deploy-cloud-witness.md)
