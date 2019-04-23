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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849322"
---
# <a name="storage-spaces-direct-hardware-requirements"></a>Requisiti hardware di Spazi di archiviazione diretta

> Si applica a: Windows Server 2016, Windows Server Insider Preview

Questo argomento descrive i requisiti hardware minimi per spazi di archiviazione diretta.

Per la produzione, Microsoft consiglia queste [Windows Server Software-Defined](https://microsoft.com/wssd) offre hardware e software dei partner, tra cui strumenti di distribuzione e procedure. Sono progettate, assemblati e convalidare l'architettura di riferimento per garantire rapidità e affidabilità, in modo da essere operativi e compatibilità. Altre informazioni, vedi [ https://microsoft.com/wssd ](https://microsoft.com/wssd).

![logo dei nostri partner di Windows Server Software Defined](media/hardware-requirements/wssd-partners.png)

   > [!TIP]
   > Vuole valutare spazi di archiviazione diretta, ma non dispone di hardware? Usare macchine virtuali di Azure o Hyper-V come descritto in [usando spazi di archiviazione diretta in cluster di macchine virtuali guest](storage-spaces-direct-in-vm.md).

## <a name="base-requirements"></a>Requisiti di base

I sistemi, componenti, dispositivi e driver deve essere **certificate di Windows Server 2016** per il [catalogo di Windows Server](https://www.windowsservercatalog.com). Inoltre, è consigliabile che i server, le unità, schede bus host e le schede di rete abbiano la **definita dal software Data Center (SDDC) Standard** e/o **definita dal software Data Center (SDDC) Premium** qualificazione aggiuntive (AQs), come illustrata di seguito. Sono disponibili oltre 1.000 componenti con la AQs SDDC.

![screenshot del catalogo di Windows Server che mostra il AQs SDDC](media/hardware-requirements/sddc-aqs.png)

Il cluster configurazione completa (server, rete e archiviazione) deve superare tutti [test di convalida cluster](https://technet.microsoft.com/library/cc732035(v=ws.10).aspx) per la procedura guidata in Gestione Cluster di Failover o con il `Test-Cluster` [cmdlet](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps) in PowerShell.

Inoltre, si applicano i requisiti seguenti:

## <a name="servers"></a>Server

- Minimo due server, massimo 16
- Consigliabile che tutti i server sia lo stesso produttore e modello

## <a name="cpu"></a>CPU

- Nehalem Intel o processore compatibile in un secondo momento; o
- AMD EPYC o in un secondo momento processore compatibile

## <a name="memory"></a>Memoria

- Memoria per Windows Server, le macchine virtuali e altre App o i carichi di lavoro; segno più
- 4 GB di RAM per ogni terabyte (TB) di capacità di unità di cache in ogni server, per i metadati di spazi di archiviazione diretta

## <a name="boot"></a>Avvio

- Qualsiasi dispositivo di avvio supportato da Windows Server, quali [include ora SATADOM](https://cloudblogs.microsoft.com/windowsserver/2017/08/30/announcing-support-for-satadom-boot-drives-in-windows-server-2016/)
- RAID 1 è mirror **non** necessari, ma è supportato per l'avvio
- Consigliato: Dimensioni minime pari a 200 GB

## <a name="networking"></a>Rete

Valore minimo (per node su scala ridotta 2 e 3)
- Interfaccia di rete 10 Gbps
- Connessione diretta (switchless) è supportato con 2 nodi

Consigliato (ad alte prestazioni, scalabilità o distribuzioni di nodi 4 +)
- Schede di rete che sono l'accesso diretto a memoria remota (RDMA) in grado di supportare, iWARP (scelta consigliata) o RoCE
- Due o più schede di rete per la ridondanza e prestazioni
- Interfaccia di rete di 25 Gbps o versione successiva

## <a name="drives"></a>Unità

Spazi di archiviazione diretta è compatibile con collegamento diretto SATA, SAS o NVMe unità fisicamente collegati a un solo server. Per ulteriori suggerimenti sulla scelta di unità, vedi l'argomento [Scelta delle unità](choosing-drives.md).

- Le unità SAS, SATA e NVMe (M.2 U.2 e Add-In-Card) sono supportate
- sono supportate 512n, 512e e unità native a 4 KB
- È necessario fornire le unità SSD [protezione contro la perdita dell'alimentazione](https://blogs.technet.microsoft.com/filecab/2016/11/18/dont-do-it-consumer-ssd/)
- Stesso numero e tipi di unità in ogni server – vedere [unità considerazioni simmetria](drive-symmetry-considerations.md)
- NVMe driver di Microsoft nella casella o NVMe driver aggiornato.
- Consigliato: Numero di unità di capacità è un multiplo intero del numero di unità di cache
- Consigliato: Le unità di cache devono avere resistenza scrittura elevata: almeno 3 unità di operazioni di scrittura-al giorno (DWPD) o almeno 4 terabyte scritti (TBW) al giorno: vedere [unità Understanding scrive al giorno (DWPD), terabyte scritto (TBW) e il valore minimo consigliato per l'archiviazione Gli spazi diretti](https://blogs.technet.microsoft.com/filecab/2017/08/11/understanding-dwpd-tbw/)

Ecco come le unità possono essere connesse per spazi di archiviazione diretta:

1. Direct-attached unità SATA
2. Unità NVMe collegato direttamente
3. Scheda di firma di accesso condiviso bus host (HBA) con unità SAS
4. Scheda di firma di accesso condiviso bus host (HBA) con unità SATA
5. **NON È SUPPORTATO:** RAID schede dei controller o SAN (Fibre Channel, iSCSI, FCoE) archiviazione. Bus host (HBA) schede devono implementare una semplice modalità pass-through.

![diagramma dell'unità supportate interconnessioni](media/hardware-requirements/drive-interconnect-support-1.png)

Le unità possono essere interne al server o in un dispositivo esterno che è connesso a un solo server. I servizi SES (SCSI Enclosure) è obbligatorio per l'identificazione e mapping degli slot. Ciascun alloggiamento esterno deve presentare un identificatore univoco (ID univoco).

1. Unità interna al server
2. Le unità in un dispositivo esterno "JBOD (") connesso a un server
3. **NON È SUPPORTATO:** Enclosure SAS condivisi connesso a più server o qualsiasi forma di Multipath i/o (MPIO) in cui le unità sono accessibili da più percorsi.

![diagramma dell'unità supportate interconnessioni](media/hardware-requirements/drive-interconnect-support-2.png)

### <a name="minimum-number-of-drives-excludes-boot-drive"></a>Numero minimo di unità (unità di avvio esclusi)

- Se ci sono delle unità utilizzate come cache, ce ne devono essere almeno due per server
- Ci devono essere almeno 4 unità di capacità (non cache) per server

| Tipi di unità presenti   | Numero minimo necessario |
|-----------------------|-------------------------|
| Tutte le unità NVMe (stesso modello) | 4 NVMe                  |
| Tutte le unità SSD (stesso modello)  | 4 SSD                   |
| Unità NVMe + SSD            | Due unità NVMe + quattro SSD          |
| Unità NVMe + HDD            | Due unità NVMe + quattro HDD          |
| Unità SSD + unità disco rigido             | Due unità SSD + quattro unità HDD           |
| Unità NVMe + SSD + HDD      | Due unità NVMe + quattro altre       |

   >[!NOTE]
   > Questa tabella presenta il valore minimo per le distribuzioni di hardware. Se si è virtualizzato e distribuzione con macchine virtuali di archiviazione, ad esempio in Microsoft Azure, vedere [usando spazi di archiviazione diretta in cluster di macchine virtuali guest](storage-spaces-direct-in-vm.md).

### <a name="maximum-capacity"></a>Capacità massima

- Consigliato: Capacità massima di archiviazione non elaborato 100 terabyte (TB) per ogni server
- 1 petabyte (1.000 TB) non elaborati capacità massima del pool di archiviazione
