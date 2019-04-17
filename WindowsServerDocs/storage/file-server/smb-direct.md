---
title: Migliorare le prestazioni di un file server con SMB diretto
description: Descrive i SMB diretto delle funzionalità in Windows Server 2012 R2, Windows Server 2012 e Windows Server 2016.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: ed8fd5b4114fc9fd9c7dc278a98cea8cc67a8749
ms.sourcegitcommit: d31e266130b3b082372f7af4024e6089cb347d74
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/28/2018
ms.locfileid: "4239228"
---
# SMB diretto

>Si applica a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Windows Server 2012 R2, Windows Server 2012 e Windows Server 2016 includono una funzionalità denominata SMB diretto, che supporta l'uso di schede di rete che dispongono di funzionalità Remote diretta memoria accesso (RDMA). Le schede di rete che hanno RDMA possono funzionare alla velocità completo con latenza molto bassa, durante l'uso minimo della CPU. Per i carichi di lavoro, ad esempio Microsoft SQL Server o Hyper-V, in questo modo un file server remoto simile a quella di archiviazione locale. SMB diretto include:

- Aumentare la velocità effettiva: sfrutta la velocità effettiva completa delle reti ad alta velocità in cui le schede di rete coordinare il trasferimento di grandi quantità di dati alla velocità di riga.
- Bassa latenza: fornisce estremamente veloce risposte alle richieste di rete e, di conseguenza, rende archiviazione remota file utente come se si tratta di archiviazione a blocchi direttamente associata.
- Utilizzo della CPU a basso: utilizza un numero minore cicli di CPU quando il trasferimento dei dati attraverso la rete, che rende più alimentazione disponibili per le applicazioni server.

SMB diretto viene configurato automaticamente da Windows Server 2012 R2 e Windows Server 2012.

## SMB multicanale e SMB diretto

SMB multicanale è responsabile per rilevare le funzionalità RDMA di schede di rete per abilitare SMB diretto della funzionalità. Senza SMB multicanale, SMB Usa TCP/IP regolari con le schede di rete che supportano la RDMA (tutte le schede di rete forniscono uno stack TCP/IP insieme a nuovi stack RDMA).

Con i SMB multicanale, SMB rileva se dispone della funzionalità RDMA una scheda di rete e quindi crea più connessioni RDMA per tale sessione singola (due per ogni interface). In questo modo i SMB usare la velocità effettiva elevata, bassa latenza e basso utilizzo della CPU offerto da schede di rete RDMA compatibili. Inoltre, offre tolleranza di errore se usi più interfacce RDMA.

>[!NOTE]
>Si dovrebbero team schede di rete RDMA compatibili non se si prevede di usare la funzionalità RDMA delle schede di rete. Quando raggruppate, le schede di rete non supporterà RDMA.
>Dopo la creazione di almeno una connessione di rete RDMA, la connessione TCP/IP utilizzata per la negoziazione protocollo originale non viene più utilizzata. Tuttavia, la connessione TCP/IP viene mantenuta nel caso in cui le connessioni di rete RDMA esito negativo.

## Requisiti

SMB diretto richiede quanto segue:

- Almeno due computer che eseguono Windows Server 2012 R2 o Windows Server 2012
- Uno o più schede di rete con funzionalità RDMA.

### Considerazioni sull'uso SMB diretto

- È possibile utilizzare SMB diretto in un cluster di failover. Tuttavia, è necessario assicurarsi che le reti di cluster utilizzate per l'accesso client sono adeguate per SMB diretto. Supporta l'utilizzo di più reti per l'accesso client di clustering di failover, insieme a schede di rete che sono RSS (Receive Side Scaling)-che supportano e che supportano RDMA.
- È possibile utilizzare SMB diretto nel sistema operativo di gestione di Hyper-V per supportare utilizzando Hyper-V tramite SMB e per fornire l'archiviazione a una macchina virtuale che usa lo stack di archiviazione di Hyper-V. Tuttavia, le schede di rete che supportano la RDMA non vengono esposti direttamente a un client Hyper-V. Se una scheda di rete RDMA che supportano la connessione a un commutatore virtuale, le schede di rete virtuale dal commutatore non sarà in grado RDMA.
- Se disabiliti SMB multicanale, SMB diretto anche è disabilitato. Dato che i SMB multicanale rileva le funzionalità di scheda di rete e determina se una scheda di rete è in grado RDMA, SMB diretto non può essere utilizzato dal client se SMB multicanale è disabilitato.
- SMB diretto non è supportato in Windows RT SMB diretto richiede il supporto per le schede di rete che supportano la RDMA, che è disponibile solo in Windows Server 2012 R2 e Windows Server 2012.
- SMB diretto non è supportato nelle versioni di livello inferiore di Windows Server. È supportato solo in Windows Server 2012 R2 e Windows Server 2012.

## Abilitazione e disabilitazione SMB diretto

SMB diretto è abilitato per impostazione predefinita quando si installa Windows Server 2012 R2 o Windows Server 2012. Il client SMB rileva automaticamente e Usa le connessioni di rete più se viene identificata una configurazione appropriata.

### Disabilitare i SMB diretto

In genere, non è necessario disabilitare SMB diretto, tuttavia, è possibile disabilitarla eseguendo uno degli script di Windows PowerShell seguente.

Per disattivare RDMA per un'interfaccia specifica, digitare:

```PowerShell
Disable-NetAdapterRdma <name>
```

Per disattivare RDMA per tutte le interfacce, digitare:

```PowerShell
Set-NetOffloadGlobalSetting -NetworkDirect Disabled
```

Quando disabiliti RDMA sul client o server, i sistemi non usano. *Rete diretta* è il nome interno per Windows Server 2012 R2 e Windows Server 2012 base il supporto di rete per le interfacce RDMA.

### Abilitare nuovamente i SMB diretto

Dopo la disattivazione RDMA, è possibile riabilitarla eseguendo uno degli script di Windows PowerShell seguente.

Per abilitare nuovamente RDMA per un'interfaccia specifica, digitare:

```PowerShell
Enable-NetAdapterRDMA <name>
```

Per abilitare nuovamente RDMA per tutte le interfacce, digitare:

```PowerShell
Set-NetOffloadGlobalSetting -NetworkDirect Enabled
```

È necessario abilitare RDMA sul client e server per iniziare a usarlo nuovamente.

## Test delle prestazioni di SMB diretto

Puoi testare le prestazioni come funziona utilizzando una delle seguenti procedure.

### Confrontare la copia di un file con e senza usare SMB diretto

Ecco come misura la velocità effettiva maggiore di SMB diretto:

1. Configurare i SMB diretto
2. Misura la quantità di tempo per eseguire una copia di file di grandi dimensioni tramite SMB diretto.
3. Disabilitare RDMA nella scheda di rete, vedere [l'Abilitazione e disabilitazione SMB diretto](#enabling-and-disabling-smb-direct).
4. Misura la quantità di tempo per eseguire una copia di file di grandi dimensioni senza usare SMB diretto.
5. Abilitare nuovamente RDMA nella scheda di rete e quindi confrontare i risultati di due.
6. Per evitare l'impatto della memorizzazione nella cache, è necessario eseguire le operazioni seguenti:
    1. Copia una grande quantità di dati (più dati di memoria è in grado di gestire).
    2. Copiare i dati di due volte, con la prima copia come pratica e quindi tempistica la seconda copia.
    3. Riavviare il server sia il client prima di ogni test per assicurarti che operano in condizioni simili.

### Uno dei più schede di rete esito negativo durante una copia di file con SMB diretto

Ecco come confermare la funzionalità di failover di SMB diretto:

1. Assicurarsi che SMB diretto funziona in una configurazione della scheda di rete più.
2. Eseguire una copia di file di grandi dimensioni. Mentre viene eseguita la copia, simulare un errore di uno dei percorsi di rete disconnettendo uno dei cavi (o la disabilitazione di una delle schede di rete).
3. Verifica che la copia dei file continui utilizzando una delle schede di rete rimanente e che non sono presenti errori di copia di file.

>[!NOTE]
>Per evitare gli errori di un carico di lavoro che non usa SMB diretto, assicurati che non ci sono altri carichi di lavoro tramite il percorso di rete disconnesso.

## Ulteriori informazioni

- [Panoramica di Server Message Block](file-server-smb-overview.md)
- [Aumentare la disponibilità della rete, archiviazione e Server: panoramica dello Scenario](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831437(v%3dws.11)>)
- [Distribuire Hyper-V tramite SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
