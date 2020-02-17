---
title: Migliorare le prestazioni di un file server con SMB diretto
description: Descrive la funzionalità SMB diretto in Windows Server 2012 R2, Windows Server 2012 e Windows Server 2016.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 41126aa0d054607449d57928c1777679e5087e73
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394463"
---
# <a name="smb-direct"></a>SMB diretto

>Si applica a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Windows Server 2012 R2, Windows Server 2012 e Windows Server 2016 includono una funzionalità denominata SMB diretto, che supporta l'uso di schede di rete dotate della funzionalità Accesso diretto a memoria remota (RDMA). Le schede di rete con RDMA sono in grado di funzionare alla massima velocità con una latenza molto bassa e un utilizzo minimo della CPU. Nei carichi di lavoro come Hyper-V o Microsoft SQL Server, ciò consente di assimilare un file server remoto a un archivio locale. SMB diretto include:

- Velocità effettiva maggiore: sfrutta tutta la velocità effettiva delle reti ad alta velocità in cui le schede di rete coordinano il trasferimento di grandi quantità di dati alla velocità della linea.
- Bassa latenza: offre tempi di risposta alle richieste di rete molto rapidi, rendendo l'archiviazione file remota veloce quanto l'archiviazione a blocchi collegata direttamente.
- Basso utilizzo della CPU: usa meno cicli della CPU durante il trasferimento dei dati in rete, lasciando più potenza a disposizione delle applicazioni server.

SMB diretto viene configurato automaticamente da Windows Server 2012 R2 e Windows Server 2012.

## <a name="smb-multichannel-and-smb-direct"></a>SMB multicanale ed SMB diretto

SMB multicanale è il nome della funzione che rileva le funzionalità RDMA delle schede di rete per abilitare SMB diretto. In mancanza della funzione SMB multicanale, SMB usa il normale protocollo TCP/IP insieme alle schede di rete con supporto per RDMA (tutte le schede di rete, insieme al nuovo stack RDMA, offrono uno stack TCP/IP).

Con la funzione SMB multicanale, il protocollo SMB rileva se una data scheda di rete è dotata o meno di funzionalità RDMA, quindi crea più connessioni RDMA per quella sessione (due per interfaccia). In questo modo, il protocollo SMB è in grado di sfruttare la velocità di trasmissione elevata, la bassa latenza e l'utilizzo minimo della CPU offerti dalle schede di rete con supporto per RDMA. Inoltre, se si utilizzano più interfacce RDMA, garantisce tolleranza di errore.

>[!NOTE]
>Se si intende sfruttare la funzionalità RDMA delle schede di rete, è sconsigliato utilizzare in team le schede di rete con supporto per RDMA. Se utilizzate in team, infatti, le schede di rete non supportano la funzionalità RDMA.
>Dopo la creazione di almeno una connessione di rete RDMA, la connessione TCP/IP utilizzata per la negoziazione del protocollo originale non sarà più utilizzata. Tuttavia, qualora le connessioni di rete RDMA non riescano, la connessione TCP/IP viene mantenuta.

## <a name="requirements"></a>Requisiti

SMB diretto presenta i requisiti seguenti:

- Almeno due computer che eseguono Windows Server 2012 R2 o Windows Server 2012
- Una o più schede di rete con funzionalità RDMA.

### <a name="considerations-when-using-smb-direct"></a>Considerazioni sull'utilizzo di SMB diretto

- È possibile utilizzare SMB diretto in un cluster di failover, ma occorre prima verificare che le reti di cluster per l'accesso client consentano l'utilizzo di SMB diretto. Il clustering di failover supporta l'utilizzo di più reti per l'accesso client, oltre a schede di rete con supporto per RSS (Receive Side Scaling) e RDMA.
- È possibile utilizzare la funzionalità SMB diretto sui sistemi operativi di gestione di Hyper-V a supporto dell'uso di Hyper-V su SMB e come archivio per una macchina virtuale che utilizza lo stack di archiviazione Hyper-V. Le schede di rete con supporto per RDMA, comunque, non sono esposte direttamente a un client Hyper-V. Se si connette a un commutatore virtuale una scheda di rete con supporto per RDMA, questa non sarà più dotata di supporto per RDMA.
- Se si disabilita la funzionalità SMB multicanale, viene disabilitata anche la funzione SMB diretto. Poiché SMB multicanale rileva le funzionalità delle schede di rete e stabilisce se una scheda di rete offre o meno il supporto per RDMA, il client non potrà utilizzare la funzionalità SMB diretto se la funzionalità SMB multicanale è disabilitata.
- SMB diretto non è supportato in Windows RT. SMB diretto richiede il supporto per schede di rete abilitate per RDMA, disponibili solo in Windows Server 2012 R2 e Windows Server 2012.
- SMB diretto non è supportata dalle versioni di livello inferiore di Windows Server, ma solo da Windows Server 2012 R2 e Windows Server 2012.

## <a name="enabling-and-disabling-smb-direct"></a>Abilitazione e disabilitazione di SMB diretto

SMB diretto è abilitato per impostazione predefinita se è installato Windows Server 2012 R2 o Windows Server 2012. Se viene identificata una configurazione appropriata, il client SMB rileva e utilizza automaticamente più connessioni di rete.

### <a name="disable-smb-direct"></a>Disabilitare SMB diretto

In genere, non vi è alcuna necessità di disabilitare SMB diretto, ma è comunque possibile disattivare questa funzionalità eseguendo uno degli script di Windows PowerShell indicati di seguito.

Per disabilitare RDMA solo per un determinato tipo di interfaccia:

```PowerShell
Disable-NetAdapterRdma <name>
```

Per disabilitare RDMA per tutti i tipi di interfaccia:

```PowerShell
Set-NetOffloadGlobalSetting -NetworkDirect Disabled
```

Se RDMA viene disabilitata sul client o sul server, non può essere utilizzata dai sistemi. *Rete diretta* è il nome interno del supporto di rete di base di Windows Server 2012 R2 e Windows Server 2012 per interfacce RDMA.

### <a name="re-enable-smb-direct"></a>Riabilitare SMB diretto

Dopo averla disabilitata, è possibile riabilitare la funzionalità RDMA eseguendo uno degli script di Windows PowerShell indicati di seguito.

Per riabilitare RDMA solo per un determinato tipo di interfaccia:

```PowerShell
Enable-NetAdapterRDMA <name>
```

Per riabilitare RDMA per tutti i tipi di interfaccia:

```PowerShell
Set-NetOffloadGlobalSetting -NetworkDirect Enabled
```

Per poter utilizzare nuovamente la funzione RDMA è necessario abilitarla sia sul client che sul server.

## <a name="test-performance-of-smb-direct"></a>Testare le prestazioni di SMB diretto

Per testare il funzionamento delle prestazioni è possibile utilizzare una delle procedure seguenti.

### <a name="compare-a-file-copy-with-and-without-using-smb-direct"></a>Confrontare la copia di un file con e senza la funzionalità SMB diretto

Per misurare l'aumento della velocità effettiva di SMB diretto, seguire questa procedura.

1. Configurazione di SMB diretto
2. Misurare il tempo necessario per eseguire la copia di un file di grandi dimensioni con l'impostazione SMB diretto abilitata.
3. Disabilitare RDMA sulla scheda di rete, vedere [Enabling and disabling SMB Direct](#enabling-and-disabling-smb-direct).
4. Misurare il tempo necessario per eseguire la copia di un file di grandi dimensioni con l'impostazione SMB diretto disabilitata.
5. Riabilitare RDMA sulla scheda di rete e mettere a confronto i risultati ottenuti.
6. Per evitare gli effetti della memorizzazione sulla cache, procedere come segue:
    1. Copiare una grande quantità di dati (superiore alla capacità di gestione della memoria).
    2. Copiare due volte i dati, dove la prima copia è per fare pratica, e controllando il tempo necessario per la seconda copia.
    3. Riavviare sia server che client prima di ciascun test per assicurarsi che il funzionamento avvenga in condizioni simili.

### <a name="fail-one-of-multiple-network-adapters-during-a-file-copy-with-smb-direct"></a>Mettere in stato di errore una delle schede di rete durante la copia di un file con SMB diretto

Per confermare la capacità di failover di SMB diretto, seguire questa procedura.

1. Assicurarsi che SMB diretto sia utilizzata nell'ambito di una configurazione con più schede di rete.
2. Eseguire la copia di un file di grandi dimensioni. Durante la copia, scollegare un cavo o disabilitare una scheda di rete per simulare un percorso di rete errato.
3. Verificare che la copia del file prosegua utilizzando una delle schede di rete rimanenti e che non vi siano errori durante la copia.

>[!NOTE]
>Per evitare errori a eventuali carichi di lavoro che non utilizzano SMB diretto, assicurarsi che non ve ne siano altri che utilizzano il percorso di rete scollegato.

## <a name="more-information"></a>Ulteriori informazioni

- [Panoramica di SMB (Server Message Block)](file-server-smb-overview.md)
- [Aumento della disponibilità di server, archiviazione e rete: Panoramica dello scenario](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831437(v%3dws.11)>)
- [Distribuire Hyper-V tramite SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
