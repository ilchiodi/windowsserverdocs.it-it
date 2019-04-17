---
title: Cronologia delle prestazioni per le schede di rete
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 02/02/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: 340999a8f440975d3736277b1a30dddbb942785d
ms.sourcegitcommit: d31e266130b3b082372f7af4024e6089cb347d74
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/28/2018
ms.locfileid: "4239238"
---
# Cronologia delle prestazioni per le schede di rete

> Si applica a: Windows Server Insider Preview

Questo argomento secondario della [cronologia delle prestazioni di spazi di archiviazione diretta](performance-history.md) descrive in dettaglio la cronologia delle prestazioni raccolti per le schede di rete. Cronologia delle prestazioni di scheda di rete è disponibile per ogni scheda di rete fisica in ogni server del cluster. Cronologia delle prestazioni di accesso diretto alla memoria (RDMA) remoto è disponibile per ogni scheda di rete fisiche con RDMA abilitate.

   > [!NOTE]
   > Non è possibile raccogliere la cronologia delle prestazioni per le schede di rete in un server che è verso il basso. Raccolta riprenderà automaticamente quando il server torna allo stato attivo.

## Le unità e i nomi di serie

Queste serie vengono raccolti per ogni scheda di rete idonei:

| Serie                               | Unit            |
|--------------------------------------|-----------------|
| `netadapter.bandwidth.inbound`       | bit al secondo |
| `netadapter.bandwidth.outbound`      | bit al secondo |
| `netadapter.bandwidth.total`         | bit al secondo |
| `netadapter.bandwidth.rdma.inbound`  | bit al secondo |
| `netadapter.bandwidth.rdma.outbound` | bit al secondo |
| `netadapter.bandwidth.rdma.total`    | bit al secondo |

   > [!NOTE]
   > Cronologia delle prestazioni di scheda di rete viene registrata in **bit** al secondo, non byte al secondo. Una scheda di rete 10 GbE possa inviare e ricevere circa 1.000.000.000 bit = 125,000,000 byte = 1,25 GB al secondo alla massima teorica.

## Come interpretare

| Serie                               | Come interpretare                                                      |
|--------------------------------------|-----------------------------------------------------------------------|
| `netadapter.bandwidth.inbound`       | Tasso di dati ricevuti dalla scheda di rete.                         |
| `netadapter.bandwidth.outbound`      | Frequenza di dati inviati dalla scheda di rete.                             |
| `netadapter.bandwidth.total`         | Frequenza totale dei dati ricevuti o inviati dalla scheda di rete.           |
| `netadapter.bandwidth.rdma.inbound`  | Tasso di dati ricevuti su RDMA dalla scheda di rete.               |
| `netadapter.bandwidth.rdma.outbound` | Tasso di dati inviati RDMA dalla scheda di rete.                   |
| `netadapter.bandwidth.rdma.total`    | Frequenza totale dei dati ricevuti o inviati tramite RDMA dalla scheda di rete. |

## La provenienza

Il `bytes.*` serie vengono raccolti dal `Network Adapter` contatore delle prestazioni impostato nel server in cui è installata la scheda di rete, un'istanza per ogni scheda di rete.

| Serie                           | Contatore di origine           |
|----------------------------------|--------------------------|
| `netadapter.bandwidth.inbound`   | 8 × `Bytes Received/sec` |
| `netadapter.bandwidth.outbound`  | 8 × `Bytes Sent/sec`     |
| `netadapter.bandwidth.total`     | 8 × `Bytes Total/sec`    |

Il `rdma.*` serie vengono raccolti dal `RDMA Activity` contatore delle prestazioni impostato nel server in cui è installata la scheda di rete, un'istanza per ogni scheda di rete con RDMA abilitate.

| Serie                               | Contatore di origine           |
|--------------------------------------|--------------------------|
| `netadapter.bandwidth.rdma.inbound`  | 8 × `Inbound bytes/sec`  |
| `netadapter.bandwidth.rdma.outbound` | 8 × `Outbound bytes/sec` |
| `netadapter.bandwidth.rdma.total`    | 8 × *somma delle condizioni precedenti*   |

   > [!NOTE]
   > Contatori vengono misurati nell'intero intervallo, non campionati. Ad esempio, se la scheda di rete è rimasto inattiva per 9 secondi, ma i trasferimenti bits 200 nella seconda 10, il relativo `netadapter.bandwidth.total` verrà registrato come 20 bit al secondo in Media durante questo intervallo di 10 secondi. Ciò garantisce la cronologia delle prestazioni acquisisce tutte le attività e robusto al rumore.

## Utilizzo di PowerShell

Usa il cmdlet [Get-NetAdapter](https://docs.microsoft.com/powershell/module/netadapter/get-netadapter) :

```PowerShell
Get-NetAdapter <Name> | Get-ClusterPerf
```

## Vedi anche

- [Cronologia delle prestazioni di spazi di archiviazione diretta](performance-history.md)
