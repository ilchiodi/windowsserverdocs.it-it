---
title: Requisiti hardware di Spazi di archiviazione diretta
ms.prod: windows-server-threshold
description: Requisiti hardware minimi per i test di Spazi di archiviazione diretta.
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 04/12/2018
ms.localizationpriority: medium
ms.openlocfilehash: 84d10ab3e25500720dd13e2ba057dc3c5bf05a6f
ms.sourcegitcommit: 515b4fd5c40fcbae0e12a2c30090384533972353
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/29/2018
ms.locfileid: "8232530"
---
# Requisiti hardware di Spazi di archiviazione diretta

> Si applica a: Windows Server 2016, Windows Server Insider Preview

Questo argomento descrive i requisiti hardware minimi per spazi di archiviazione diretta.

Per la produzione, Microsoft consiglia di queste offerte hardware/software di [Windows Server Software-Defined](https://microsoft.com/wssd) dai nostri partner, che includono le procedure e strumenti di distribuzione. Sono progettate, assemblati e convalidati sulla base delle nostra architettura di riferimento per garantire la compatibilità e affidabilità, in modo che ottenere fino e diventare subito operativi. Altre informazioni consultando [https://microsoft.com/wssd](https://microsoft.com/wssd).

![logo dei nostri partner di Windows Server Software Defined](media/hardware-requirements/wssd-partners.png)

   > [!TIP]
   > Vuole valutare spazi di archiviazione diretta, ma non dispone di hardware? Usare Hyper-V o macchine virtuali di Azure come descritto in [Con spazi di archiviazione diretta in cluster di macchina virtuale guest](storage-spaces-direct-in-vm.md).

## Requisiti di base

I sistemi, componenti, dispositivi e driver deve essere **Certificazione di Windows Server 2016** per ogni [Catalogo di Windows Server](https://www.windowsservercatalog.com). Inoltre, è consigliabile schede di rete, unità, schede bus host e i server di **Standard Software-Defined Data Center (SDDC)** e/o **Premium Software-Defined Data Center (SDDC)** requisiti aggiuntivi (ricerca avanzata), come illustrato nella figura di seguito. Ci sono componenti oltre 1.000 con la ricerca avanzata SDDC.

![screenshot del catalogo di Windows Server che mostra la ricerca avanzata SDDC](media/hardware-requirements/sddc-aqs.png)

Il cluster completamente configurato (server, rete e archiviazione) deve superare tutti i [test di convalida del cluster](https://technet.microsoft.com/library/cc732035(v=ws.10).aspx) per la procedura guidata in Gestione Cluster di Failover o con il `Test-Cluster` [cmdlet](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps) di PowerShell.

Inoltre, si applicano i requisiti seguenti:

## Server

- Minimo due server, massimo 16
- Consigliabile che tutti i server sia lo stesso produttore e modello

## CPU

- Processore Intel Nehalem o versione successiva compatibile; o
- In un secondo momento processore compatibile o AMD EPYC

## Memoria

- Memoria per Windows Server, le macchine virtuali e altre App o carichi di lavoro; Plus
- 4 GB di RAM per terabyte (TB) di capacità di unità cache su ciascun server, per i metadati di spazi di archiviazione diretta

## Avvio

- Qualsiasi dispositivo di avvio supportato da Windows Server, che [include ora SATADOM](https://cloudblogs.microsoft.com/windowsserver/2017/08/30/announcing-support-for-satadom-boot-drives-in-windows-server-2016/)
- RAID 1 mirroring è **non** necessari, ma è supportata per l'avvio
- Consigliato: dimensione minima di 200 GB

## Reti

Importo minimo (per il nodo di piccole dimensioni di scala 2-3)
- Interfaccia di rete di 10 Gbps
- Connessione diretta (switchless) è supportato con 2-nodi

Consigliato (ad alte prestazioni, la scala o le distribuzioni di 4 + nodi)
- Schede NIC che sono accesso diretto a memoria remota (RDMA) in grado, (scelta consigliata) iWARP o RoCE
- Due o più schede di rete per la ridondanza e prestazioni
- Interfaccia di rete 25 Gbps o superiore

## Unità

Spazi di archiviazione diretta interagisce con collegate SATA, SAS o NVMe unità che sono fisicamente associate a un solo server. Per ulteriori suggerimenti sulla scelta di unità, vedi l'argomento [Scelta delle unità](choosing-drives.md).

- Le unità SAS, SATA e NVMe (M.2 U.2 e Add-In-Card) tutti supportate
- 512n, 512 e unità nativo 4K tutti supportate
- Le unità SSD devono fornire [protezione interruzioni dell'alimentazione](https://blogs.technet.microsoft.com/filecab/2016/11/18/dont-do-it-consumer-ssd/)
- Stesso numero e dei tipi di unità in ogni server, vedere [Considerazioni simmetria di unità](drive-symmetry-considerations.md)
- NVMe driver è integrato Microsoft o NVMe driver aggiornato.
- Consigliato: Numero di unità di capacità sia un multiplo del numero di unità cache intero
- Consigliato: Unità Cache deve essere minore resistenza in scrittura elevata: almeno 3 drive-writes-per-day (dwpd drive) o almeno 4 terabyte scritti (Terabytes) al giorno-Vedi unità conoscenza [scrive al giorno dwpd (drive), terabyte scritti (Terabytes) e il valore minimo consigliato per l'archiviazione Spazi diretta](https://blogs.technet.microsoft.com/filecab/2017/08/11/understanding-dwpd-tbw/)

Ecco come unità possono essere connessi spazi di archiviazione diretta:

1. Unità SATA collegate
2. Unità NVMe collegate
3. Scheda SAS bus host (HBA) con le unità SAS
4. Scheda SAS bus host (HBA) con unità SATA
5. **Non supportato:** RAID schede controller o SAN (Fibre Channel, iSCSI, FCoE) archiviazione. Bus host (HBA) schede devono implementare la modalità semplice pass-through.

![diagramma di lavoro supportate interconnessioni](media/hardware-requirements/drive-interconnect-support-1.png)

Le unità possono essere interne al server o in un allegato esterno che è connesso a un solo server. Servizi SES (SCSI Enclosure Services) è necessaria per l'identificazione e mapping slot. Ciascuna enclosure esterna deve presentare un identificatore univoco (ID univoco).

1. Unità interne al server
2. Le unità in un enclosure esterna ("JBOD") collegata a un server
3. **Non supportato:** Alloggiamenti SAS condivisi è connesso a più server o qualsiasi forma di Multipath i/o (MPIO) in cui sono accessibili da più percorsi di unità.

![diagramma di lavoro supportate interconnessioni](media/hardware-requirements/drive-interconnect-support-2.png)

### Numero minimo di unità (esclude l'unità di avvio)

- Se ci sono delle unità utilizzate come cache, ce ne devono essere almeno due per server
- Ci devono essere almeno 4 unità di capacità (non cache) per server

| Tipi di unità presenti   | Numero minimo necessario |
|-----------------------|-------------------------|
| Tutte le unità NVMe (stesso modello) | 4 NVMe                  |
| Tutte le unità SSD (stesso modello)  | 4 SSD                   |
| Unità NVMe + SSD            | Due unità NVMe + quattro SSD          |
| Unità NVMe + HDD            | Due NVMe + quattro HDD          |
| SSD + HDD             | Due SSD + quattro HDD           |
| NVMe + SSD + HDD      | Due NVMe + quattro altre       |

   >[!NOTE]
   > Questa tabella fornisce il valore minimo per le distribuzioni di hardware. Se eseguire la distribuzione con le macchine virtuali e virtualizzate di archiviazione, ad esempio in Microsoft Azure, vedere [Con spazi di archiviazione diretta in cluster di macchina virtuale guest](storage-spaces-direct-in-vm.md).

### Capacità massima

- Consigliato: Capacità di archiviazione non elaborato di massimo 100 terabyte (TB) per ogni server
- Massimo 1 petabyte (1000 TB) non elaborati la capacità nel pool di archiviazione
