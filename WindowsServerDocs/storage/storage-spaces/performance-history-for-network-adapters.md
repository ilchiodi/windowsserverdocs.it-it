---
title: Cronologia prestazioni per le schede di rete
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
ms.localizationpriority: medium
ms.openlocfilehash: e2379ce540cb26c02bc79f591d2a597874ab287c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856214"
---
# <a name="performance-history-for-network-adapters"></a>Cronologia prestazioni per le schede di rete

> Si applica a: Windows Server 2019

Questo argomento secondario della [cronologia delle prestazioni per spazi di archiviazione diretta](performance-history.md) descrive in dettaglio la cronologia delle prestazioni raccolta per le schede di rete. La cronologia delle prestazioni della scheda di rete è disponibile per ogni scheda di rete fisica in ogni server del cluster. La cronologia delle prestazioni di accesso diretto a memoria remota (RDMA) è disponibile per ogni scheda di rete fisica con RDMA abilitato.

   > [!NOTE]
   > Non è possibile raccogliere la cronologia delle prestazioni per le schede di rete in un server che non è attivo. La raccolta verrà ripresa automaticamente quando il server verrà riavviato.

## <a name="series-names-and-units"></a>Unità e nomi di serie

Queste serie vengono raccolte per ogni scheda di rete idonea:

| Serie                               | Unità            |
|--------------------------------------|-----------------|
| `netadapter.bandwidth.inbound`       | bit al secondo |
| `netadapter.bandwidth.outbound`      | bit al secondo |
| `netadapter.bandwidth.total`         | bit al secondo |
| `netadapter.bandwidth.rdma.inbound`  | bit al secondo |
| `netadapter.bandwidth.rdma.outbound` | bit al secondo |
| `netadapter.bandwidth.rdma.total`    | bit al secondo |

   > [!NOTE]
   > La cronologia delle prestazioni della scheda di rete viene registrata in **bit** al secondo, non in byte al secondo. La scheda di rete 1 10 GbE può inviare e ricevere circa 1 miliardo bit = 125 milioni byte = 1,25 GB al secondo al massimo teorico.

## <a name="how-to-interpret"></a>Come interpretare

| Serie                               | Come interpretare                                                      |
|--------------------------------------|-----------------------------------------------------------------------|
| `netadapter.bandwidth.inbound`       | Frequenza dei dati ricevuti dalla scheda di rete.                         |
| `netadapter.bandwidth.outbound`      | Frequenza dei dati inviati dalla scheda di rete.                             |
| `netadapter.bandwidth.total`         | Velocità totale dei dati ricevuti o inviati dalla scheda di rete.           |
| `netadapter.bandwidth.rdma.inbound`  | Frequenza dei dati ricevuti tramite RDMA dalla scheda di rete.               |
| `netadapter.bandwidth.rdma.outbound` | Frequenza dei dati inviati tramite RDMA dalla scheda di rete.                   |
| `netadapter.bandwidth.rdma.total`    | Frequenza totale dei dati ricevuti o inviati tramite RDMA dalla scheda di rete. |

## <a name="where-they-come-from"></a>Da dove provengono

La serie di `bytes.*` viene raccolta dal contatore delle prestazioni `Network Adapter` impostato sul server in cui è installata la scheda di rete, un'istanza per ogni scheda di rete.

| Serie                           | Contatore di origine           |
|----------------------------------|--------------------------|
| `netadapter.bandwidth.inbound`   | 8 × `Bytes Received/sec` |
| `netadapter.bandwidth.outbound`  | 8 × `Bytes Sent/sec`     |
| `netadapter.bandwidth.total`     | 8 × `Bytes Total/sec`    |

La serie `rdma.*` viene raccolta dal contatore delle prestazioni `RDMA Activity` impostato nel server in cui è installata la scheda di rete, un'istanza per scheda di rete con RDMA abilitato.

| Serie                               | Contatore di origine           |
|--------------------------------------|--------------------------|
| `netadapter.bandwidth.rdma.inbound`  | 8 × `Inbound bytes/sec`  |
| `netadapter.bandwidth.rdma.outbound` | 8 × `Outbound bytes/sec` |
| `netadapter.bandwidth.rdma.total`    | 8 × *somma dei precedenti*   |

   > [!NOTE]
   > I contatori sono misurati nell'intero intervallo, non campionati. Ad esempio, se la scheda di rete è inattiva per 9 secondi, ma trasferisce 200 bit nel decimo secondo, il `netadapter.bandwidth.total` verrà registrato come 20 bit al secondo in media durante questo intervallo di 10 secondi. Ciò garantisce che la cronologia delle prestazioni acquisisce tutte le attività ed è affidabile per il rumore.

## <a name="usage-in-powershell"></a>Utilizzo in PowerShell

Usare il cmdlet [Get-NetAdapter](https://docs.microsoft.com/powershell/module/netadapter/get-netadapter) :

```PowerShell
Get-NetAdapter <Name> | Get-ClusterPerf
```

## <a name="see-also"></a>Vedere anche

- [Cronologia prestazioni per Spazi di archiviazione diretta](performance-history.md)
