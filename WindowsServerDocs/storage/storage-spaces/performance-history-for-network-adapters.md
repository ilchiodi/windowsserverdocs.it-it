---
title: Cronologia delle prestazioni per le schede di rete
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Spazi di archiviazione diretta
ms.localizationpriority: medium
ms.openlocfilehash: 340999a8f440975d3736277b1a30dddbb942785d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849982"
---
# <a name="performance-history-for-network-adapters"></a>Cronologia delle prestazioni per le schede di rete

> Si applica a: Anteprima Windows Server Insider

Questo argomento secondario di [cronologia delle prestazioni per spazi di archiviazione diretta](performance-history.md) descrive in dettaglio la cronologia delle prestazioni raccolta per le schede di rete. Cronologia prestazioni scheda di rete è disponibile per ogni scheda di rete fisica in ogni server del cluster. Cronologia delle prestazioni di accesso diretto a memoria (RDMA) remota è disponibile per ogni scheda di rete fisica con RDMA abilitato.

   > [!NOTE]
   > Cronologia delle prestazioni non può essere raccolti per le schede di rete in un server che non è attivo. Raccolta riprenderà automaticamente quando il server torna allo stato attivo.

## <a name="series-names-and-units"></a>Unità e i nomi delle serie

Queste serie vengono raccolti per ogni scheda di rete idonea:

| serie                               | Unità            |
|--------------------------------------|-----------------|
| `netadapter.bandwidth.inbound`       | Bit al secondo |
| `netadapter.bandwidth.outbound`      | Bit al secondo |
| `netadapter.bandwidth.total`         | Bit al secondo |
| `netadapter.bandwidth.rdma.inbound`  | Bit al secondo |
| `netadapter.bandwidth.rdma.outbound` | Bit al secondo |
| `netadapter.bandwidth.rdma.total`    | Bit al secondo |

   > [!NOTE]
   > Cronologia prestazioni scheda di rete viene registrata nel **bits** al secondo, non in byte al secondo. Una scheda di rete 10 GbE può inviare e ricevere circa 1.000.000.000 bits = 125,000,000 byte = 1,25 GB al secondo alla massima teorica.

## <a name="how-to-interpret"></a>Come interpretare

| serie                               | Come interpretare                                                      |
|--------------------------------------|-----------------------------------------------------------------------|
| `netadapter.bandwidth.inbound`       | Frequenza dei dati ricevuti dalla scheda di rete.                         |
| `netadapter.bandwidth.outbound`      | Frequenza dei dati inviati dalla scheda di rete.                             |
| `netadapter.bandwidth.total`         | Frequenza totale di dati ricevuti o inviati dalla scheda di rete.           |
| `netadapter.bandwidth.rdma.inbound`  | Frequenza dei dati ricevuti tramite RDMA dalla scheda di rete.               |
| `netadapter.bandwidth.rdma.outbound` | Frequenza dei dati inviati tramite RDMA dalla scheda di rete.                   |
| `netadapter.bandwidth.rdma.total`    | Frequenza totale di dati ricevuti o inviati tramite RDMA dalla scheda di rete. |

## <a name="where-they-come-from"></a>Da dove provengono

Il `bytes.*` serie vengono raccolti dal `Network Adapter` contatore delle prestazioni impostato nel server in cui è installata la scheda di rete, un'istanza per ogni scheda di rete.

| serie                           | Contatore di origine           |
|----------------------------------|--------------------------|
| `netadapter.bandwidth.inbound`   | 8 × `Bytes Received/sec` |
| `netadapter.bandwidth.outbound`  | 8 × `Bytes Sent/sec`     |
| `netadapter.bandwidth.total`     | 8 × `Bytes Total/sec`    |

Il `rdma.*` serie vengono raccolti dal `RDMA Activity` contatore delle prestazioni impostato nel server in cui è installata la scheda di rete, un'istanza per ogni scheda di rete con RDMA abilitato.

| serie                               | Contatore di origine           |
|--------------------------------------|--------------------------|
| `netadapter.bandwidth.rdma.inbound`  | 8 × `Inbound bytes/sec`  |
| `netadapter.bandwidth.rdma.outbound` | 8 × `Outbound bytes/sec` |
| `netadapter.bandwidth.rdma.total`    | 8 × *somma dei valori precedenti*   |

   > [!NOTE]
   > I contatori vengono misurati su tutto l'intervallo, non campionato. Ad esempio, se la scheda di rete è inattiva per i trasferimenti bits 200 ma 9 secondi nella seconda 10th, relativo `netadapter.bandwidth.total` saranno registrate come 20 bit al secondo in Media durante questo intervallo di 10 secondi. In questo modo la cronologia delle prestazioni acquisisce tutte le attività e affidabile al rumore.

## <a name="usage-in-powershell"></a>Utilizzo in PowerShell

Usare la [Get-NetAdapter](https://docs.microsoft.com/powershell/module/netadapter/get-netadapter) cmdlet:

```PowerShell
Get-NetAdapter <Name> | Get-ClusterPerf
```

## <a name="see-also"></a>Vedere anche

- [Cronologia delle prestazioni per spazi di archiviazione diretta](performance-history.md)
