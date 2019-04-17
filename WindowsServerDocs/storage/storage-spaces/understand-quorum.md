---
title: Quorum di cluster e pool conoscenza
description: Comprendere i Cluster e Pool Quorum, con esempi specifici di passare attraverso le complicazioni.
keywords: Spazi di archiviazione diretta, Quorum, controllo, S2D, Cluster Quorum, Quorum Pool, Cluster, Pool
ms.prod: windows-server-threshold
ms.author: adagashe
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/18/2019
ms.localizationpriority: medium
ms.openlocfilehash: 24890b191db8bc6934132857e830d4f77c394b02
ms.sourcegitcommit: 28dc7c7f1e44fee7ab2112228af329a9ce0e02ba
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2019
ms.locfileid: "9024138"
---
# Quorum di cluster e pool conoscenza

>Si applica a: Windows Server 2019, Windows Server 2016

[Clustering di Failover di Windows Server](../../failover-clustering/failover-clustering-overview.md) offre una disponibilità elevata per carichi di lavoro. Queste risorse sono considerate disponibile elevata se i nodi che ospitano le risorse sono Tuttavia, il cluster in genere richiede più della metà dei nodi a essere in esecuzione, che è noto come avere *quorum*.

Quorum è progettato per impedire *"split Brain"* scenari che possono verificarsi quando è presente una partizione nella rete e a sottoinsiemi di nodi non comunicano tra loro. Ciò può provocare entrambi sottoinsiemi di nodi per provare a Me il carico di lavoro e scrivere sullo stesso disco che può causare problemi di numerosi. Tuttavia, questo non è consentito con concetto del Clustering di Failover di quorum che impone solo uno di questi gruppi di nodi di continuare l'esecuzione, in modo che solo uno di questi gruppi rimangono online.

Quorum determina il numero di errori che il cluster può sostenere pur rimanendo online. Quorum è progettata per gestire lo scenario quando si verifica un problema con la comunicazione tra sottoinsiemi di nodi del cluster, in modo che più server di non provare a contemporaneamente ospitare un gruppo di risorse e scrivere sullo stesso disco nello stesso momento. Richiedendo a questo concetto di quorum, il cluster verrà forzare l'arresto in uno dei sottoinsiemi di nodi e garantire che non esiste un solo vero proprietario di un gruppo di risorse specifico del servizio cluster. Una volta nodi cui sono stati arrestati ancora una volta possono comunicare con il gruppo principale dei nodi, verrà automaticamente ricostituzione del cluster e avviare il servizio cluster.

In Windows Server 2016, esistono due componenti del sistema che hanno i propri meccanismi di quorum:

- <strong>Il Quorum del cluster</strong>: questo opera a livello di cluster (ad esempio, è possibile perdere i nodi e cluster rimangano alto)
- <strong>Pool di Quorum</strong>: questo opera a livello di pool quando è abilitato spazi di archiviazione diretta (ad esempio, è possibile perdere i nodi e le unità e il pool di rimanere). I pool di archiviazione sono stati progettati per essere usato negli scenari di cluster e non in cluster, motivo per cui hanno un meccanismo di quorum diversi.

## Panoramica di quorum del cluster

La tabella seguente offre una panoramica dei risultati per ogni scenario Quorum del Cluster:

| Nodi del server | Può sostenere l'errore di un nodo server | Può sostenere un errore del nodo di un server, quindi un altro | Può supera due errori di nodo simultanei di server |
|--------------|-------------------------------------|---------------------------------------------------|----------------------------------------------------|
| 2            | 50/50                               | No                                                | No                                                 |
| 2 + controllo  | Sì                                 | No                                                | No                                                 |
| 3            | Sì                                 | 50/50                                             | No                                                 |
| 3 + controllo  | Sì                                 | Sì                                               | No                                                 |
| 4            | Sì                                 | Sì                                               | 50/50                                              |
| 4 + controllo  | Sì                                 | Sì                                               | Sì                                                |
| 5 e versioni successive  | Sì                                 | Sì                                               | Sì                                                |

### Consigli di quorum del cluster

- Se hai due nodi, è un controllo <strong>obbligatorio</strong>.
- Se si dispongano di tre o quattro nodi, di controllo è <strong>consigliamo vivamente</strong>.
- Se hai accesso a Internet, Usa un <strong> [cloud di controllo](../../failover-clustering/deploy-cloud-witness.md)</strong>
- Se sei in un ambiente IT con altri computer e condivisioni di file, Usa un controllo di condivisione file

## Funzionamento di quorum del cluster

Quando i nodi esito negativo o quando un subset dei nodi perde il contatto con un altro sottoinsieme, nodi sopravvissuti necessario verificare che costituiscono la *maggior parte* del cluster di rimanere in linea. Se essi non è possibile verificare che, essi verranno Vai offline.

Ma il concetto di *maggioranza* funziona solo nettamente quando il numero totale di nodi del cluster è dispari (ad esempio, tre nodi in un cluster di cinque nodo). Pertanto, cosa accade cluster con un numero pari nodi (ad esempio, un cluster di quattro nodi)?

Esistono due modi per il cluster può rendere il *numero totale di voti* dispari:

1. Prima di tutto, può passare *da* uno aggiungendo un *controllo* con un voto aggiuntivo. Questo utente questo sono necessarie.
2.  In alternativa, è possibile passare *verso il basso* una specifica azzeramento vota un sfortunato del nodo (avviene automaticamente in base alle esigenze).

Ogni volta che i nodi sopravvissuti verificare correttamente che sono la *maggior parte*, la definizione della *maggior parte* viene aggiornata per essere tra semplicemente i sopravvissuti. In questo modo il cluster di perdita di un nodo, quindi un altro e un altro e così via. Questo concetto di adattamento il *numero totale di voti* dopo errori successivi è noto come <strong> *quorum dinamica*</strong>.  

### Controllo dinamico

Controllo dinamico attiva o disattiva il voto del controllo per verificare che il *numero totale di voti* dispari. Se sono presenti un numero di voti dispari, il controllo non ha un voto. Se è presente un numero pari voti, il controllo ha un voto. Controllo dinamico riduce notevolmente il rischio che devono passare il cluster a causa di errori di controllo verso il basso. Il cluster stabilisce se usare il voto di controllo in base al numero di nodi votanti che sono disponibili nel cluster.

Quorum dinamico funziona con controllo dinamico nel modo descritto di seguito.

### Comportamento di quorum dinamico

- Se hai un <strong>anche</strong> numero di nodi e nessun controllo, *un nodo Ottiene il voto azzerato*. Ad esempio, solo tre dei quattro nodi ottenere voti, in modo che il *numero totale di voti* è tre e due sopravvissuti con voti sono considerate una maggior parte.
- Se hai un <strong>dispari</strong> numero di nodi e nessun controllo, *tutti ottenere voti*.
- Se hai un <strong>anche</strong> numero di nodi, oltre a di controllo, *il controllo di voti*, in modo che il totale è dispari.
- Se hai un <strong>dispari</strong> numero di nodi, oltre a di controllo, *il controllo non voto*.

Quorum dinamico consente la possibilità di assegnare un voto a un nodo in modo dinamico per evitare di perdere la maggior parte dei voti e per consentire l'esecuzione con un nodo (noto come ultimo-man permanente) del cluster. Ad esempio diamo un cluster di quattro nodi. Si supponga che quorum richiede 3 voti. 

In questo caso, il cluster sarà stati verso il basso in caso di perdita di due nodi.

![Diagramma che mostra quattro nodi del cluster, ognuno dei quali Ottiene un voto](media/understand-quorum/dynamic-quorum-base.png)

Tuttavia, quorum dinamico impedisce ciò accada. Il *numero totale di voti* necessari per quorum è ora determinato in base al numero di nodi disponibili. Pertanto, con dinamico quorum, il cluster rimangono backup anche in caso di perdita di tre nodi.

![Diagramma che mostra quattro nodi del cluster con nodi mancata uno alla volta e il numero di voti richiesti regolare dopo ogni errore.](media/understand-quorum/dynamic-quorum-step-through.png)

Lo scenario sopra si applica a un cluster generale che non dispone di spazi di archiviazione diretta abilitato. Tuttavia, quando spazi di archiviazione diretta è abilitato, il cluster può supportare solo due gli errori dei nodi. Questa operazione è illustrata più nella [sezione quorum pool](#poolQuorum).

### Esempi

#### Due nodi senza un controllo. 
Azzeramento vota del nodo, in modo che il voto della *maggior parte* è determinato all'esterno di un totale di <strong>1 vota</strong>. Se il nodo non voto diventa non disponibile in modo imprevisto, il cluster viene conservata superstite ha 1/1. Se il nodo voto diventa non disponibile in modo imprevisto, superstite è 0 o 1 e il cluster diventa non disponibile. Se il nodo voto normalmente è spento, il voto viene trasferito a altro nodo e viene conservata al cluster. *<strong>Questo è il motivo per cui è fondamentale per configurare un controllo.</strong>*

![Quorum illustrate nel caso con due nodi senza un controllo](media/understand-quorum/2-node-no-witness.png)

- Può sostenere un errore del server: <strong>probabilità che il 50%</strong>.
- Può sostenere errore di un server, quindi un altro: <strong>No</strong>.
- Può supera due errori di server in una sola volta: <strong>No</strong>. 

#### Due nodi con un controllo. 
Entrambi i nodi voto, oltre a voti il controllo, in modo che la *maggior parte* è determinato all'esterno di un totale di <strong>3 voti</strong>. Se dei nodi diventa non disponibile, il cluster viene conservata superstite ha 2/3.

![Quorum illustrate nel caso con due nodi con un controllo](media/understand-quorum/2-node-witness.png)

- Può sostenere un errore del server: <strong>Sì</strong>.
- Può sostenere errore di un server, quindi un altro: <strong>No</strong>.
- Può supera due errori di server in una sola volta: <strong>No</strong>. 

#### Tre nodi senza un controllo.
Tutti i nodi voto, in modo che la *maggior parte* è determinato all'esterno di un totale di <strong>3 voti</strong>. Se un nodo qualsiasi diventa non disponibile, i sopravvissuti sono 2 o 3 e viene conservata al cluster. Il cluster diventa due nodi senza un controllo, a questo punto, sei nello Scenario 1.

![Quorum illustrate nel caso con tre nodi senza un controllo](media/understand-quorum/3-node-no-witness.png)

- Può sostenere un errore del server: <strong>Sì</strong>.
- Può sostenere errore di un server, quindi un altro: <strong>probabilità che il 50%</strong>.
- Può supera due errori di server in una sola volta: <strong>No</strong>. 

#### Tre nodi con un controllo.
Tutti i nodi voto, in modo che il controllo non voto inizialmente. La *maggior parte* è determinato all'esterno di un totale di <strong>3 voti</strong>. Dopo un errore, il cluster dispone di due nodi con un controllo, che è tornato per lo Scenario 2. Pertanto, ora i due nodi e il controllo di votare.

![Quorum illustrate nel caso con tre nodi con un controllo](media/understand-quorum/3-node-witness.png)

- Può sostenere un errore del server: <strong>Sì</strong>.
- Può sostenere errore di un server, quindi un altro: <strong>Sì</strong>.
- Può supera due errori di server in una sola volta: <strong>No</strong>. 

#### Quattro nodi senza un controllo
Azzeramento vota del nodo, in modo che la *maggior parte* è determinato all'esterno di un totale di <strong>3 voti</strong>. Dopo un errore, il cluster diventa tre nodi, mentre sei nello Scenario 3.

![Quorum illustrate nel caso con quattro nodi senza un controllo](media/understand-quorum/4-node-no-witness.png)

- Può sostenere un errore del server: <strong>Sì</strong>.
- Può sostenere errore di un server, quindi un altro: <strong>Sì</strong>.
- Può supera due errori di server in una sola volta: <strong>probabilità che il 50%</strong>. 

#### Quattro nodi con un controllo.
Tutti i voti nodi e i voti di controllo, in modo che la *maggior parte* è determinato all'esterno di un totale di <strong>5 voti</strong>. Dopo un errore, sei in uno Scenario 4. Dopo due errori simultanei, ignorare verso il basso per lo Scenario 2.

![Quorum illustrate nel caso con quattro nodi con un controllo](media/understand-quorum/4-node-witness.png)

- Può sostenere un errore del server: <strong>Sì</strong>.
- Può sostenere errore di un server, quindi un altro: <strong>Sì</strong>.
- Può supera due errori di server in una sola volta: <strong>Sì</strong>. 

#### Cinque nodi e oltre.
Tutti i nodi voto o tutti i ma un voto, qualsiasi rende il numero totale di dispari. Spazi di archiviazione diretta non è possibile gestire più di due nodi verso il basso comunque, in modo a questo punto, nessun controllo è utile o necessarie.

![Quorum illustrate nel caso con cinque nodi e versioni successive](media/understand-quorum/5-nodes.png)

- Può sostenere un errore del server: <strong>Sì</strong>.
- Può sostenere errore di un server, quindi un altro: <strong>Sì</strong>.
- Può supera due errori di server in una sola volta: <strong>Sì</strong>. 

Ora che abbiamo comprendere il funzionamento di quorum, esaminiamo i tipi di testimoni quorum.

### Tipi di quorum di controllo

Clustering di failover supporta tre tipi di Quorum testimoni:

- <strong>[Cloud Witness](../../failover-clustering\deploy-cloud-witness.md) </strong> -Blob di archiviazione in Azure accessibile per tutti i nodi del cluster. Gestisce le informazioni di clustering in un file witness.log, ma non archiviare una copia del database del cluster.
- <strong>Controllo di condivisione di file</strong> -condivisione file SMB A che è configurato in un file server che esegue Windows Server. Gestisce le informazioni di clustering in un file witness.log, ma non archiviare una copia del database del cluster.
- <strong>Controllo del disco</strong> -un piccolo disco cluster che sia nel gruppo di Cluster di archiviazione disponibile. Il disco è altamente disponibile e possa eseguire il failover tra i nodi. Contiene una copia del database del cluster.  <strong>*Un controllo disco non è supportato con spazi di archiviazione diretta*</strong>.

## <a id="poolQuorum"></a>Panoramica di quorum di pool

Abbiamo appena parlato Quorum del Cluster, che opera a livello di cluster. A questo punto, esaminiamo Quorum Pool, che opera a livello di pool (ad esempio, è possibile perdite di nodi e unità e non il pool di rimanere). I pool di archiviazione sono stati progettati per essere usato negli scenari di cluster e non in cluster, motivo per cui hanno un meccanismo di quorum diversi.

La tabella seguente offre una panoramica dei risultati per ogni scenario Quorum Pool:

| Nodi del server | Può sostenere l'errore di un nodo server | Può sostenere un errore del nodo di un server, quindi un altro | Può supera due errori di nodo simultanei di server |
|--------------|-------------------------------------|---------------------------------------------------|----------------------------------------------------|
| 2            | No                                  | No                                                | No                                                 |
| 2 + controllo  | Sì                                 | No                                                | No                                                 |
| 3            | Sì                                 | No                                                | No                                                 |
| 3 + controllo  | Sì                                 | No                                                | No                                                 |
| 4            | Sì                                 | No                                                | No                                                 |
| 4 + controllo  | Sì                                 | Sì                                               | Sì                                                |
| 5 e versioni successive  | Sì                                 | Sì                                               | Sì                                                |

## Funzionamento di quorum di pool

Quando errori delle unità o quando un subset delle unità perde il contatto con un altro sottoinsieme, unità sopravvissute necessario verificare che costituiscono la *maggior parte* del pool di rimanere in linea. Se essi non è possibile verificare che, essi verranno Vai offline. Il pool è l'entità che è offline o rimanga online in base a se dispone di dischi sufficienti per quorum (pari al 50% + 1). Il proprietario di risorse di pool (nodo del cluster active) può essere il + 1.

Ma quorum pool funziona in modo diverso da quorum del cluster nei modi seguenti:

- il pool utilizza un nodo del cluster come controllo come un eventuali conflitti derivanti per sostenere la metà delle unità completata (questo nodo che è il proprietario di risorse di pool)
- il pool di non dispone di quorum dinamico
- il pool di non implementa la propria versione di rimuovere un voto

### Esempi

#### Quattro nodi con un layout simmetrico. 
Ognuna delle unità di 16 ha un voto e nodo due include anche un voto (dal momento che è il proprietario di risorse di pool). La *maggior parte* è determinato all'esterno di un totale di <strong>16 voti</strong>. Se i nodi 3 e 4 Vai verso il basso, il sottoinsieme sopravvissuto ha 8 unità e il proprietario risorsa pool, ovvero voti 9/16. Pertanto, il pool viene conservata.

![Quorum pool 1](media/understand-quorum/pool-1.png)

- Può sostenere un errore del server: <strong>Sì</strong>.
- Può sostenere errore di un server, quindi un altro: <strong>Sì</strong>.
- Può supera due errori di server in una sola volta: <strong>Sì</strong>. 

#### Quattro nodi con un errore di layout e unità simmetrico. 
Ognuna delle unità di 16 ha un voto e nodo 2 include anche un voto (dal momento che è il proprietario di risorse di pool). La *maggior parte* è determinato all'esterno di un totale di <strong>16 voti</strong>. Prima di tutto, 7 unità diventa non disponibile. Se i nodi 3 e 4 Vai verso il basso, il sottoinsieme sopravvissuto ha 7 unità e il proprietario risorsa pool, ovvero voti 8/16. Pertanto, il pool di non dispone di maggioranza e diventa non disponibile.

![Quorum pool 2](media/understand-quorum/pool-2.png)

- Può sostenere un errore del server: <strong>Sì</strong>.
- Può sostenere errore di un server, quindi un altro: <strong>No</strong>.
- Può supera due errori di server in una sola volta: <strong>No</strong>. 

#### Quattro nodi con un layout non simmetrica. 
Ognuna delle unità di 24 ha un voto e nodo due include anche un voto (dal momento che è il proprietario di risorse di pool). La *maggior parte* è determinato all'esterno di un totale di <strong>24 voti</strong>. Se i nodi 3 e 4 Vai verso il basso, il sottoinsieme sopravvissuto ha 8 unità e il proprietario risorsa pool, ovvero voti 9/24. Pertanto, il pool di non dispone di maggioranza e diventa non disponibile.

![Quorum pool 3](media/understand-quorum/pool-3.png)

- Può sostenere un errore del server: <strong>Sì</strong>.
- Può sostenere errore di un server, quindi un altro: <strong>dipendente </strong>(non può sostenere se entrambi i nodi 3 e 4 Vai verso il basso, ma possono sostenere tutti gli altri scenari.
- Può supera due errori di server in una sola volta: <strong>dipendente </strong>(non può sostenere se entrambi i nodi 3 e 4 Vai verso il basso, ma possono sostenere tutti gli altri scenari.

### Consigli di quorum di pool

- Assicurati che ogni nodo del cluster sia simmetrico (ogni nodo ha lo stesso numero di unità)
- Abilitare mirroring a tre vie o dalla doppia parità, in modo che si può tollerare un errori dei nodi e mantenere i dischi virtuali online. Vedi la [pagina di informazioni aggiuntive sul volume](plan-volumes.md) per altri dettagli.
- Se un disco in un altro nodo e nodi, due o più di due nodi sono verso il basso sono verso il basso, volumi potrebbero non avere accesso a tutte le tre copie dei dati relativi alla e pertanto impostare il modalità offline e non disponibile. Si consiglia di riportare i server o sostituire i dischi rapidamente per garantire che la maggior parte dei resilienza per tutti i dati in un volume.

## Ulteriori informazioni

- [Configurare e gestire il quorum](../../failover-clustering/manage-cluster-quorum.md)
- [Distribuire un cloud di controllo](../../failover-clustering/deploy-cloud-witness.md)
