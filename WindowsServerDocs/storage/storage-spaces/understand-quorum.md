---
title: Informazioni sul quorum del cluster e del pool
description: Informazioni sul quorum del cluster e del pool, con esempi specifici per approfondire le complessità.
keywords: Spazi di archiviazione diretta, quorum, server di controllo del mirroring, S2D, quorum del cluster, quorum del pool, cluster, pool
ms.prod: windows-server-threshold
ms.author: adagashe
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/18/2019
ms.localizationpriority: medium
ms.openlocfilehash: 962a4edc1a171167a6af336d4fb32188a526f455
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872129"
---
# <a name="understanding-cluster-and-pool-quorum"></a>Informazioni sul quorum del cluster e del pool

>Si applica a Windows Server 2019, Windows Server 2016

[Windows Server failover clustering](../../failover-clustering/failover-clustering-overview.md) fornisce disponibilità elevata per i carichi di lavoro. Queste risorse vengono considerate a disponibilità elevata se i nodi che ospitano risorse sono attivati; Tuttavia, il cluster richiede in genere più della metà dei nodi in esecuzione, che è noto come *quorum*.

Il quorum è progettato per impedire scenari *Split Brain* che possono verificarsi quando è presente una partizione nella rete e i subset di nodi non possono comunicare tra loro. Questo può causare l'esecuzione di entrambi i subset di nodi per provare a eseguire il proprietario del carico di lavoro e scrivere nello stesso disco che può causare numerosi problemi. Tuttavia, questo viene impedito con il concetto di quorum del clustering di failover che impone la continua esecuzione solo di uno di questi gruppi di nodi, quindi solo uno di questi gruppi rimarrà online.

Il quorum determina il numero di errori che il cluster può sostenere rimanendo online. Il quorum è progettato per gestire lo scenario quando si verifica un problema di comunicazione tra subset di nodi del cluster, in modo che più server non tentino di ospitare simultaneamente un gruppo di risorse e di scrivere sullo stesso disco nello stesso momento. Con questo concetto di quorum, il cluster forza l'arresto del servizio cluster in uno dei subset di nodi per garantire che esista un solo proprietario reale di un determinato gruppo di risorse. Una volta che i nodi che sono stati interrotti possono comunicare di nuovo con il gruppo principale di nodi, verranno aggiunti automaticamente al cluster e avvierà il servizio cluster.

In Windows Server 2019 e Windows Server 2016 sono disponibili due componenti del sistema con i propri meccanismi di quorum:

- **Quorum del cluster**: Questa operazione funziona a livello di cluster, ad esempio se è possibile perdere i nodi e il cluster rimane attivo
- **Quorum pool**: Questa operazione funziona a livello di pool quando Spazi di archiviazione diretta è abilitato, ovvero è possibile perdere i nodi e le unità e fare in caso di rimanere attivi. I pool di archiviazione sono stati progettati per essere utilizzati in scenari in cluster e non in cluster ed è per questo motivo che hanno un meccanismo di quorum diverso.

## <a name="cluster-quorum-overview"></a>Panoramica del quorum del cluster

La tabella seguente offre una panoramica dei risultati del quorum del cluster per ogni scenario:

| Nodi server | Può sopravvivere a un errore del nodo server | Può sopravvivere a un errore del nodo server e quindi a un altro | Può sopravvivere a due errori simultanei del nodo server |
|--------------|-------------------------------------|---------------------------------------------------|----------------------------------------------------|
| 2            | 50/50                               | No                                                | No                                                 |
| 2 + server di controllo  | Yes                                 | No                                                | No                                                 |
| 3            | Yes                                 | 50/50                                             | No                                                 |
| 3 + server di controllo  | Yes                                 | Sì                                               | No                                                 |
| 4            | Yes                                 | Yes                                               | 50/50                                              |
| 4 + server di controllo  | Yes                                 | Yes                                               | Yes                                                |
| 5 e versioni successive  | Yes                                 | Yes                                               | Yes                                                |

### <a name="cluster-quorum-recommendations"></a>Raccomandazioni del quorum del cluster

- Se si dispone di due nodi, è **necessario**un server di controllo del mirroring.
- Se si dispone di tre o quattro nodi, il server di controllo del mirroring è **fortemente consigliato**.
- Se si dispone di accesso a Internet, usare un  **[cloud](../../failover-clustering/deploy-cloud-witness.md) di controllo**
- Se ci si trova in un ambiente IT con altri computer e condivisioni file, usare una condivisione file di controllo

## <a name="how-cluster-quorum-works"></a>Funzionamento del quorum del cluster

Quando i nodi hanno esito negativo o quando un subset di nodi perde il contatto con un altro subset, i nodi superstiti devono verificare che costituiscano la *maggior parte* del cluster per rimanere online. Se non riescono a verificarlo, passeranno offline.

Tuttavia, il concetto di *maggioranza* funziona solo quando il numero totale di nodi nel cluster è dispari, ad esempio tre nodi in un cluster a cinque nodi. Quindi, cosa accade per i cluster con un numero pari di nodi (ad indicare un cluster a quattro nodi)?

Esistono due modi in cui il cluster può rendere dispari il *numero totale di voti* :

1. Per prima cosa, può *diventare uno aggiungendo* un server di controllo del *mirroring* con un voto aggiuntivo. Questa operazione richiede la configurazione dell'utente.
2.  In alternativa, è possibile *che si verifichi uno,* azzerando il voto di un nodo sfortunato (si verifica automaticamente in base alle esigenze).

Ogni volta che i nodi superstiti verificano la *maggior parte della maggioranza*, la definizione della *maggioranza* viene aggiornata in modo da essere tra i soli superstiti. In questo modo, il cluster può perdere un nodo, un altro, un altro e così via. Questo concetto del *numero totale di voti* che si adattano dopo errori successivi è noto come ***quorum dinamico***.  

### <a name="dynamic-witness"></a>Server di controllo dinamico

Il controllo dinamico consente di impostare il voto del server di controllo del mirroring per assicurarsi che il *numero totale di voti* sia dispari. Se è presente un numero dispari di voti, il server di controllo del mirroring non dispone di un voto. Se è presente un numero pari di voti, il server di controllo del mirroring ha un voto. Il server di controllo dinamico riduce significativamente il rischio che il cluster si arresti a causa di un errore del server di controllo. Il cluster decide se usare il voto del server di controllo del mirroring in base al numero di nodi votanti disponibili nel cluster.

Il quorum dinamico funziona con il server di controllo dinamico nel modo descritto di seguito.

### <a name="dynamic-quorum-behavior"></a>Comportamento del quorum dinamico

- Se è presente un numero **pari** di nodi e nessun server di controllo del mirroring, *il voto viene azzerato da un nodo*. Ad esempio, solo tre dei quattro nodi ottengono voti, quindi il *numero totale di voti* è tre e due superstiti con voti sono considerati di maggioranza.
- Se è presente un numero **dispari** di nodi e nessun server di controllo del mirroring, *tutti ricevono voti*.
- Se si dispone di un numero **pari** di nodi più Witness, il server di controllo del *mirroring*, il totale è dispari.
- Se si dispone di un numero **dispari** di nodi e di un server di controllo del mirroring, *il server*di controllo

Il quorum dinamico consente di assegnare un voto a un nodo in modo dinamico per evitare di perdere la maggior parte dei voti e di consentire l'esecuzione del cluster con un nodo (noto come ultimo uomo). Si prenda come esempio un cluster a quattro nodi. Si supponga che il quorum richieda 3 voti. 

In questo caso, il cluster sarebbe diminuito se si perdessero due nodi.

![Diagramma che mostra quattro nodi del cluster, ognuno dei quali ottiene un voto](media/understand-quorum/dynamic-quorum-base.png)

Tuttavia, il quorum dinamico impedisce che ciò accada. Il *numero totale di voti* richiesti per il quorum è ora determinato in base al numero di nodi disponibili. Pertanto, con il quorum dinamico, il cluster resterà attivo anche se si perdono tre nodi.

![Diagramma che mostra quattro nodi del cluster, con i nodi non riusciti uno alla volta e il numero di voti necessari per la regolazione dopo ogni errore.](media/understand-quorum/dynamic-quorum-step-through.png)

Lo scenario precedente si applica a un cluster generale in cui non è abilitato Spazi di archiviazione diretta. Tuttavia, quando Spazi di archiviazione diretta è abilitato, il cluster può supportare solo due errori del nodo. Questa operazione è illustrata più nella [sezione quorum del pool](#pool-quorum-overview).

### <a name="examples"></a>Esempi

#### <a name="two-nodes-without-a-witness"></a>Due nodi senza un server di controllo del mirroring. 
Il voto di un nodo viene azzerato, quindi il voto di *maggioranza* è determinato da un totale di **1 voto**. Se il nodo che non vota il voto si interrompe in modo imprevisto, il superstite ha 1/1 e il cluster sopravvive. Se il nodo votante si arresta in modo imprevisto, il superstite ha 0/1 e il cluster diventa inattivo. Se il nodo votante è spento normalmente, il voto viene trasferito all'altro nodo e il cluster sopravvive. ***Questo è il motivo per cui è essenziale configurare un server di controllo del mirroring.***

![Spiegazione del quorum nel caso di due nodi senza un server di controllo del mirroring](media/understand-quorum/2-node-no-witness.png)

- Può sopravvivere a un errore del server: **50% di probabilità**.
- Può sopravvivere a un errore del server e quindi a un altro: **No**.
- Può sopravvivere a due errori del server contemporaneamente: **No**. 

#### <a name="two-nodes-with-a-witness"></a>Due nodi con un server di controllo del mirroring. 
Entrambi i nodi votano, oltre ai voti del witness, quindi la *maggior parte* è determinata da un totale di **3 voti**. Se uno dei nodi diventa inattivo, il superstite ha 2/3 e il cluster sopravvive.

![Spiegazione del quorum nel caso di due nodi con un server di controllo del mirroring](media/understand-quorum/2-node-witness.png)

- Può sopravvivere a un errore del server: **Sì**.
- Può sopravvivere a un errore del server e quindi a un altro: **No**.
- Può sopravvivere a due errori del server contemporaneamente: **No**. 

#### <a name="three-nodes-without-a-witness"></a>Tre nodi senza un server di controllo del mirroring.
Tutti i nodi votano, quindi la *maggior parte* è determinata da un totale di **3 voti**. Se un nodo diventa inattivo, i superstiti sono 2/3 e il cluster sopravvive. Il cluster diventa due nodi senza un server di controllo del mirroring. a questo punto, si trova nello scenario 1.

![Spiegazione del quorum nel caso di tre nodi senza un server di controllo del mirroring](media/understand-quorum/3-node-no-witness.png)

- Può sopravvivere a un errore del server: **Sì**.
- Può sopravvivere a un errore del server e quindi a un altro: **50% di probabilità**.
- Può sopravvivere a due errori del server contemporaneamente: **No**. 

#### <a name="three-nodes-with-a-witness"></a>Tre nodi con un server di controllo del mirroring.
Tutti i nodi votano, quindi il server di controllo del mirroring non vota inizialmente. La *maggior parte* è determinata da un totale di **3 voti**. Dopo un errore, il cluster ha due nodi con un server di controllo del mirroring, che è riportato allo scenario 2. Quindi, ora i due nodi e il voto del server di controllo del mirroring.

![Spiegazione del quorum nel caso di tre nodi con un server di controllo del mirroring](media/understand-quorum/3-node-witness.png)

- Può sopravvivere a un errore del server: **Sì**.
- Può sopravvivere a un errore del server e quindi a un altro: **Sì**.
- Può sopravvivere a due errori del server contemporaneamente: **No**. 

#### <a name="four-nodes-without-a-witness"></a>Quattro nodi senza un server di controllo del mirroring
Il voto di un nodo viene azzerato, quindi la *maggior parte* è determinata da un totale di **3 voti**. Dopo un errore, il cluster diventa tre nodi e si trova nello scenario 3.

![Il quorum è stato illustrato nel caso di quattro nodi senza un server di controllo del mirroring](media/understand-quorum/4-node-no-witness.png)

- Può sopravvivere a un errore del server: **Sì**.
- Può sopravvivere a un errore del server e quindi a un altro: **Sì**.
- Può sopravvivere a due errori del server contemporaneamente: **50% di probabilità**. 

#### <a name="four-nodes-with-a-witness"></a>Quattro nodi con un server di controllo del mirroring.
Tutti i nodi votano e i voti del witness, quindi la *maggior parte* è determinata da un totale di **5 voti**. In seguito a un errore, lo scenario è 4. Dopo due errori simultanei, passare allo scenario 2.

![Spiegazione del quorum nel caso di quattro nodi con un server di controllo del mirroring](media/understand-quorum/4-node-witness.png)

- Può sopravvivere a un errore del server: **Sì**.
- Può sopravvivere a un errore del server e quindi a un altro: **Sì**.
- Può sopravvivere a due errori del server contemporaneamente: **Sì**. 

#### <a name="five-nodes-and-beyond"></a>Cinque nodi e oltre.
Tutti i nodi votano, o tutti i voti tranne uno, qualunque sia il totale dispari. Spazi di archiviazione diretta non può gestire comunque più di due nodi, quindi, a questo punto, non è necessario o utile alcun server di controllo del mirroring.

![Spiegazione del quorum nel caso di cinque nodi e oltre](media/understand-quorum/5-nodes.png)

- Può sopravvivere a un errore del server: **Sì**.
- Può sopravvivere a un errore del server e quindi a un altro: **Sì**.
- Può sopravvivere a due errori del server contemporaneamente: **Sì**. 

Dopo aver compreso il funzionamento del quorum, verranno esaminati i tipi di testimoni del quorum.

### <a name="quorum-witness-types"></a>Tipi di server di controllo del quorum

Il clustering di failover supporta tre tipi di testimoni del quorum:

- **[Cloud Witness](../../failover-clustering/deploy-cloud-witness.md)** -archiviazione BLOB in Azure accessibile da tutti i nodi del cluster. Mantiene le informazioni di clustering in un file witness. log, ma non archivia una copia del database del cluster.
- **Condivisione file** di controllo: una condivisione file SMB configurata in un file server che esegue Windows Server. Mantiene le informazioni di clustering in un file witness. log, ma non archivia una copia del database del cluster.
- **Disco** di controllo: un disco in cluster di piccole dimensioni che si trova nel gruppo di archiviazione disponibile nel cluster. Questo disco è a disponibilità elevata ed è in grado di failover tra i nodi. Contiene una copia del database del cluster.  ***Il disco di controllo non è supportato con spazi di archiviazione diretta***.

## <a name="pool-quorum-overview"></a>Panoramica del quorum del pool

Abbiamo appena parlato del quorum del cluster, che funziona a livello di cluster. Esaminiamo ora il quorum del pool, che funziona a livello di pool, ovvero è possibile perdere i nodi e le unità e fare in modo che il pool sia sempre attivo. I pool di archiviazione sono stati progettati per essere utilizzati in scenari in cluster e non in cluster ed è per questo motivo che hanno un meccanismo di quorum diverso.

La tabella seguente offre una panoramica dei risultati del quorum del pool per ogni scenario:

| Nodi server | Può sopravvivere a un errore del nodo server | Può sopravvivere a un errore del nodo server e quindi a un altro | Può sopravvivere a due errori simultanei del nodo server |
|--------------|-------------------------------------|---------------------------------------------------|----------------------------------------------------|
| 2            | No                                  | No                                                | No                                                 |
| 2 + server di controllo  | Yes                                 | No                                                | No                                                 |
| 3            | Yes                                 | No                                                | No                                                 |
| 3 + server di controllo  | Yes                                 | No                                                | No                                                 |
| 4            | Yes                                 | No                                                | No                                                 |
| 4 + server di controllo  | Yes                                 | Yes                                               | Yes                                                |
| 5 e versioni successive  | Yes                                 | Yes                                               | Yes                                                |

## <a name="how-pool-quorum-works"></a>Funzionamento del quorum del pool

Quando le unità hanno esito negativo o quando un subset di unità perde il contatto con un altro subset, le unità superstiti devono verificare che costituiscano la *maggior parte* del pool per rimanere online. Se non riescono a verificarlo, passeranno offline. Il pool è l'entità che passa alla modalità offline o rimane online in base alla presenza di dischi sufficienti per il quorum (50% + 1). Il proprietario della risorsa del pool (nodo del cluster attivo) può essere + 1.

Tuttavia, il quorum del pool funziona in modo diverso rispetto al quorum del cluster nei modi seguenti:

- il pool usa un nodo nel cluster come server di controllo del mirroring come un tie-breaker per sopravvivere alla metà delle unità (questo nodo che è il proprietario della risorsa del pool)
- il pool non dispone del quorum dinamico
- il pool non implementa la propria versione della rimozione di un voto

### <a name="examples"></a>Esempi

#### <a name="four-nodes-with-a-symmetrical-layout"></a>Quattro nodi con layout simmetrico. 
Ognuna delle 16 unità ha un voto e il nodo due ha anche un voto, perché è il proprietario della risorsa del pool. La *maggior parte* è determinata da un totale di **16 voti**. Se i nodi tre e quattro si arrestano, il subset superstite ha 8 unità e il proprietario della risorsa del pool, che corrisponde a 9/16 voti. Quindi, il pool non è più presente.

![Quorum pool 1](media/understand-quorum/pool-1.png)

- Può sopravvivere a un errore del server: **Sì**.
- Può sopravvivere a un errore del server e quindi a un altro: **Sì**.
- Può sopravvivere a due errori del server contemporaneamente: **Sì**. 

#### <a name="four-nodes-with-a-symmetrical-layout-and-drive-failure"></a>Quattro nodi con un layout simmetrico e un errore di unità. 
Ognuna delle 16 unità ha un voto e il nodo 2 dispone anche di un voto, dal momento che si tratta del proprietario della risorsa del pool. La *maggior parte* è determinata da un totale di **16 voti**. In primo luogo, l'unità 7 diventa inattiva. Se i nodi tre e quattro si arrestano, il subset superstite include 7 unità e il proprietario della risorsa del pool, che corrisponde a 8/16 voti. Quindi, il pool non ha la maggioranza e diventa inattivo.

![Quorum del pool 2](media/understand-quorum/pool-2.png)

- Può sopravvivere a un errore del server: **Sì**.
- Può sopravvivere a un errore del server e quindi a un altro: **No**.
- Può sopravvivere a due errori del server contemporaneamente: **No**. 

#### <a name="four-nodes-with-a-non-symmetrical-layout"></a>Quattro nodi con layout non simmetrico. 
Ognuna delle 24 unità ha un voto e il nodo due ha anche un voto (poiché si tratta del proprietario della risorsa del pool). La *maggior parte* è determinata da un totale di **24 voti**. Se i nodi tre e quattro si arrestano, il subset superstite ha 8 unità e il proprietario della risorsa del pool, che corrisponde a 9/24 voti. Quindi, il pool non ha la maggioranza e diventa inattivo.

![Quorum del pool 3](media/understand-quorum/pool-3.png)

- Può sopravvivere a un errore del server: **Sì**.
- Può sopravvivere a un errore del server, un altro: * * dipende * * (non può sopravvivere se entrambi i nodi tre e quattro si arrestano, ma possono sopravvivere a tutti gli altri scenari.
- Può sopravvivere a due errori del server contemporaneamente: * * dipende * * (non può sopravvivere se entrambi i nodi tre e quattro si arrestano, ma possono sopravvivere a tutti gli altri scenari.

### <a name="pool-quorum-recommendations"></a>Raccomandazioni del quorum del pool

- Verificare che ogni nodo del cluster sia simmetrico (ogni nodo ha lo stesso numero di unità)
- Abilitare il mirroring a tre vie o la doppia parità per poter tollerare gli errori dei nodi e tenere online i dischi virtuali. Per ulteriori informazioni, vedere la pagina relativa alle [linee guida sui volumi](plan-volumes.md) .
- Se più di due nodi sono inattivi o se due nodi e un disco in un altro nodo sono inattivi, i volumi potrebbero non avere accesso a tutte e tre le copie dei dati e quindi essere portati offline e non disponibili. È consigliabile ripristinare i server o sostituirli rapidamente per garantire la massima resilienza per tutti i dati del volume.

## <a name="more-information"></a>Altre informazioni

- [Configurare e gestire il quorum](../../failover-clustering/manage-cluster-quorum.md)
- [Distribuire un cloud di controllo](../../failover-clustering/deploy-cloud-witness.md)
