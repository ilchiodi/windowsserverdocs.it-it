---
title: Requisiti hardware di Spazi di archiviazione diretta
ms.prod: windows-server-threshold
description: Requisiti hardware minimi per i test di Spazi di archiviazione diretta.
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 08/05/2019
ms.localizationpriority: medium
ms.openlocfilehash: 2f3f8bff39550108b0417b9513bee4a248dca432
ms.sourcegitcommit: 0467b8e69de66e3184a42440dd55cccca584ba95
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2019
ms.locfileid: "69546377"
---
# <a name="storage-spaces-direct-hardware-requirements"></a>Requisiti hardware di Spazi di archiviazione diretta

> Si applica a Windows Server 2019, Windows Server 2016

In questo argomento vengono descritti i requisiti hardware minimi per Spazi di archiviazione diretta.

Per la produzione, Microsoft consiglia di acquistare una soluzione hardware/software convalidata dai partner, che include strumenti e procedure di distribuzione. Queste soluzioni sono progettate, assemblate e convalidate in base all'architettura di riferimento per garantire la compatibilità e l'affidabilità, per poter iniziare subito a funzionare. Per le soluzioni Windows Server 2019, visitare il [sito Web relativo alle soluzioni Azure stack HCI](https://azure.microsoft.com/overview/azure-stack/hci). Per le soluzioni Windows Server 2016, ulteriori informazioni sono disponibili in [Windows Server software-defined](https://microsoft.com/wssd).

   > [!TIP]
   > Si desidera valutare Spazi di archiviazione diretta senza hardware? Usare Hyper-V o macchine virtuali di Azure come descritto in [uso di spazi di archiviazione diretta in cluster di macchine virtuali guest](storage-spaces-direct-in-vm.md).

## <a name="base-requirements"></a>Requisiti di base

I sistemi, i componenti, i dispositivi e i driver devono essere **certificati Windows server 2016** in base al [Catalogo di Windows Server](https://www.windowsservercatalog.com). Inoltre, è consigliabile che i server, le unità, le schede bus host e le schede di rete dispongano di **SDDC (software-defined Data Center) standard** e/o di **SDDC (software-defined Data Center) Premium** (AQS), come illustrato sotto. Sono presenti più di 1.000 componenti con SDDC AQs.

![screenshot del catalogo di Windows Server che mostra il AQs SDDC](media/hardware-requirements/sddc-aqs.png)

Il cluster completamente configurato (server, rete e archiviazione) deve superare tutti i [test di convalida del cluster](https://technet.microsoft.com/library/cc732035(v=ws.10).aspx) per la procedura guidata in Gestione cluster di failover `Test-Cluster` o con il [cmdlet](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps) in PowerShell.

Inoltre, si applicano i requisiti seguenti:

## <a name="servers"></a>Server

- Minimo due server, massimo 16
- È consigliabile che tutti i server siano dello stesso produttore e modello

## <a name="cpu"></a>CPU

- Processore compatibile con Intel Nehalem o versioni successive; o
- Processore compatibile AMD EPYC o versioni successive

## <a name="memory"></a>Memoria

- Memoria per Windows Server, VM e altre applicazioni o carichi di lavoro; più
- 4 GB di RAM per terabyte (TB) di capacità dell'unità cache in ogni server, per Spazi di archiviazione diretta metadati

## <a name="boot"></a>Boot

- Qualsiasi dispositivo di avvio supportato da Windows Server, che [ora include SATADOM](https://cloudblogs.microsoft.com/windowsserver/2017/08/30/announcing-support-for-satadom-boot-drives-in-windows-server-2016/)
- Il mirroring RAID 1 **non** è necessario, ma è supportato per l'avvio
- Consigliato: dimensioni minime 200 GB

## <a name="networking"></a>Rete

Spazi di archiviazione diretta richiede una connessione di rete affidabile a larghezza di banda elevata e a bassa latenza tra ogni nodo.  

Interconnessione minima per un nodo 2-3 di dimensioni ridotte
- scheda di interfaccia di rete (NIC) a 10 Gbps o più veloce
- Due o più connessioni di rete da ogni nodo consigliato per la ridondanza e le prestazioni

Interconnessione consigliata per prestazioni elevate, scalabilità o distribuzioni di 4 + 
- Schede di rete che supportano accesso diretto a memoria remota (RDMA), iWARP (scelta consigliata) o RoCE
- Due o più connessioni di rete da ogni nodo consigliato per la ridondanza e le prestazioni
- NIC da 25 Gbps o più veloce

Interconnessioni del nodo Switched o Switched
- Passa È necessario configurare correttamente i commutatori di rete per gestire la larghezza di banda e il tipo di rete.  Se si usa RDMA che implementa il protocollo RoCE, la configurazione del dispositivo di rete e del Commuter è ancora più importante. 
- Senza interruttore I nodi possono essere interconnessi usando connessioni dirette, evitando l'uso di un'opzione.  È necessario che ogni nodo disponga di una connessione diretta a tutti gli altri nodi del cluster.


## <a name="drives"></a>Unità

Spazi di archiviazione diretta funziona con unità SATA, SAS o NVMe collegate direttamente, fisicamente collegate a un solo server. Per ulteriori suggerimenti sulla scelta di unità, vedi l'argomento [Scelta delle unità](choosing-drives.md).

- Tutte le unità SATA, SAS e NVMe (M. 2, U. 2 e Add-in-card) sono supportate
- tutte le unità native 512N, 512e e 4K sono supportate
- Le unità a stato solido devono garantire una [protezione da perdite di energia](https://blogs.technet.microsoft.com/filecab/2016/11/18/dont-do-it-consumer-ssd/)
- Stesso numero e tipi di unità in ogni server: vedere [considerazioni sulla simmetria delle unità](drive-symmetry-considerations.md)
- I dispositivi cache devono avere una dimensione di 32 GB o superiore
- Quando si usano dispositivi di memoria permanenti come dispositivi della cache, è necessario usare dispositivi NVMe o unità SSD (non è possibile usare HDD)
- Il driver NVMe è quello fornito da Microsoft incluso in Windows. (stornvme. sys)
- Consigliato: Il numero di unità di capacità è un multiplo totale del numero di unità cache
- Consigliato: Le unità cache devono avere una resistenza di scrittura elevata: almeno 3 unità-Scritture per giorno (DWPD) o almeno 4 terabyte scritti (TBW) al giorno [. vedere informazioni sulle Scritture di unità al giorno (DWPD), terabyte scritti (TBW) e il minimo consigliato per spazi di archiviazione diretta ](https://blogs.technet.microsoft.com/filecab/2017/08/11/understanding-dwpd-tbw/)

Ecco in che modo è possibile connettere le unità per Spazi di archiviazione diretta:

- Unità SATA collegate direttamente
- Unità NVMe collegate direttamente
- Scheda bus host SAS (HBA) con unità SAS
- Scheda bus host SAS (HBA) con unità SATA
- **NON SUPPORTATO:** Schede del controller RAID o SAN (Fibre Channel, iSCSI, FCoE). Le schede della scheda bus host (HBA) devono implementare una semplice modalità pass-through.

![diagramma delle interconnessioni tra unità supportate](media/hardware-requirements/drive-interconnect-support-1.png)

Le unità possono essere interne al server o in un'enclosure esterna connessa a un solo server. Il servizio chassis SCSI (SES) è necessario per l'identificazione e il mapping degli slot. Ogni enclosure esterna deve presentare un identificatore univoco (ID univoco).

- Unità interne al server
- Unità in uno chassis esterno ("JBOD") connesso a un server
- **NON SUPPORTATO:** Enclosure SAS condivise connesse a più server o qualsiasi tipo di i/o a percorsi multipli (MPIO) in cui le unità sono accessibili da più percorsi.

![diagramma delle interconnessioni tra unità supportate](media/hardware-requirements/drive-interconnect-support-2.png)

### <a name="minimum-number-of-drives-excludes-boot-drive"></a>Numero minimo di unità (esclusa l'unità di avvio)

- Se ci sono delle unità utilizzate come cache, ce ne devono essere almeno due per server
- Ci devono essere almeno 4 unità di capacità (non cache) per server

| Tipi di unità presenti   | Numero minimo necessario |
|-----------------------|-------------------------|
| Tutta la memoria persistente (stesso modello) | 4 memoria persistente |
| Tutte le unità NVMe (stesso modello) | 4 NVMe                  |
| Tutte le unità SSD (stesso modello)  | 4 SSD                   |
| Memoria persistente + NVMe o unità SSD | 2 memoria persistente + 4 NVMe o unità SSD |
| Unità NVMe + SSD            | Due unità NVMe + quattro SSD          |
| Unità NVMe + HDD            | Due unità NVMe + quattro HDD          |
| Unità SSD + unità disco rigido             | Due unità SSD + quattro unità HDD           |
| Unità NVMe + SSD + HDD      | Due unità NVMe + quattro altre       |

   >[!NOTE]
   > Questa tabella fornisce il valore minimo per le distribuzioni hardware. Se si esegue la distribuzione con macchine virtuali e archiviazione virtualizzata, ad esempio in Microsoft Azure, vedere [uso di spazi di archiviazione diretta in cluster di macchine virtuali guest](storage-spaces-direct-in-vm.md).

### <a name="maximum-capacity"></a>Capacità massima

| Valori massimi                | Windows Server 2019  | Windows Server 2016  |
| ---                     | ---------            | ---------            |
| Capacità non elaborata per server | 400 TB               | 100 TB               |
| Capacità pool           | 4 PB (4.000 TB)      | 1 PB                 |
