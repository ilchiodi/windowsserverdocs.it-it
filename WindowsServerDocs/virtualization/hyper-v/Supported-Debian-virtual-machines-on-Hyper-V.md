---
title: Macchine virtuali Debian supportate in Hyper-V
description: Elenca i servizi di integrazione Linux e le funzionalità incluse in ogni versione
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: 3cc62c10-02a3-4633-960c-23bf91a45bd5
author: shirgall
ms.author: kathydav
ms.date: 04/07/2020
ms.openlocfilehash: e5483b9547e67414bd66b3daad1a4b07c3cb7cfc
ms.sourcegitcommit: 7b1ebc4934998af2472962ca8cce1c872f39946f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2020
ms.locfileid: "80994509"
---
# <a name="supported-debian-virtual-machines-on-hyper-v"></a>Macchine virtuali Debian supportate in Hyper-V

>Si applica a: Windows Server 2019, Hyper-V Server 2019, Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows 10, Windows 8.1

La mappa di distribuzione di funzionalità seguente indica le funzionalità presenti in ogni versione. Dopo la tabella sono elencate i problemi noti e soluzioni alternative per ogni distribuzione.

## <a name="table-legend"></a>Legenda tabella

* **Incorporata** -LIS sono inclusi come parte di questa distribuzione Linux. Il pacchetto di download LIS fornita da Microsoft non funziona per la distribuzione in modo da non installare. I numeri di versione del modulo del kernel per incorporato LIS (come illustrato da **lsmod**, ad esempio) sono diversi dal numero di versione del pacchetto di download LIS fornita da Microsoft. Una mancata corrispondenza non indicano che incorporato LIS è scaduto.

* & #10004; -Funzionalità disponibili

* (*vuoto*)-funzionalità non disponibile

| **Funzionalità**                                                                                                                                  | **Versione del sistema operativo Windows Server** | **10.0-10.3 (Buster)** | **9.0-9.12 (stretch)** | **8.0-8.11 (Jessie)** | **7.0-7.11 (wheezy)** |
|----------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------|-----------------------|-----------------------|-----------------------|-----------------------|
| **Disponibilità**                                                                                                                             |                                             | Incorporata              | Incorporata              | Incorporata              | Incorporato (nota 5)     |
| **[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**                                                   | 2019, 2016, 2012 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Ora esatta di Windows Server 2016                                                                                                            | 2019, 2016                                  | & #10004; Nota 4       | & #10004; Nota 4       |                       |                       |
| **[Rete](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**                                       |                                             |                       |                       |                       |                       |
| Frame jumbo                                                                                                                                 | 2019, 2016, 2012 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Assegnazione di tag e trunking VLAN                                                                                                                    | 2019, 2016, 2012 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Migrazione in tempo reale                                                                                                                               | 2019, 2016, 2012 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Inserimento IP statico                                                                                                                          | 2019, 2016, 2012 R2                   |                       |                       |                       |                       |
| RSS virtuale                                                                                                                                         | 2019, 2016, 2012 R2                         | & #10004; Nota 4       | & #10004; Nota 4       |                       |                       |
| Offload di segmentazione e checksum TCP                                                                                                       | 2019, 2016, 2012 R2          | & #10004; Nota 4       | & #10004; Nota 4       |                       |                       |
| SR-IOV                                                                                                                                       | 2019, 2016                                  | & #10004; Nota 4       | & #10004; Nota 4       |                       |                       |
| **[Archiviazione](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**                                             |                                             |                       |                       |                       |                       |
| Ridimensionamento VHDX                                                                                                                                  | 2019, 2016, 2012 R2                         | & #10004; Nota 1       | & #10004; Nota 1       | & #10004; Nota 1       | & #10004; Nota 1       |
| Fibre Channel virtuale                                                                                                                        | 2019, 2016, 2012 R2                         |                       |                       |                       |                       |
| Backup della macchina virtuale in tempo reale                                                                                                                  | 2019, 2016, 2012 R2                         | &#10004;Nota2 | &#10004;Nota2 | &#10004;Nota2 | &#10004;Nota2 |
| Supporto TRIM                                                                                                                                 | 2019, 2016, 2012 R2                         | & #10004; Nota 4       | & #10004; Nota 4       |                       |                       |
| WWN SCSI                                                                                                                                     | 2019, 2016, 2012 R2                         | & #10004; Nota 4       | & #10004; Nota 4       |                       |                       |
| **[Memoria](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**                                               |                                             |                       |                       |                       |                       |
| Supporto del kernel PAE                                                                                                                           | 2019, 2016, 2012 R2          | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Configurazione di MMIO Gap                                                                                                                    | 2019, 2016, 2012 R2                         | &#10004;              | &#10004;              | &#10004;              | &#10004;              |
| Memoria dinamica - aggiunta a caldo                                                                                                                     | 2019, 2016, 2012 R2                   | & #10004; Nota 4       | & #10004; Nota 4       |                       |                       |
| Memoria dinamica - Ballooning                                                                                                                  | 2019, 2016, 2012 R2                   | & #10004; Nota 4       | & #10004; Nota 4       |                       |                       |
| Ridimensionamento della memoria di runtime                                                                                                                        | 2019, 2016                                  | & #10004; Nota 4       | & #10004; Nota 4       |                       |                       |
| **[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**                                                 |                                             |                       |                       |                       |                       |
| Dispositivo video specifico di Hyper-V                                                                                                                | 2019, 2016, 2012 R2          | &#10004;              | &#10004;              | &#10004;              |                       |
| **[Varie](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**                                 |                                             |                       |                       |                       |                       |
| Coppia chiave-valore                                                                                                                               | 2019, 2016, 2012 R2          | & #10004; Nota 2       | & #10004; Nota 2       | & #10004; Nota 2       |                       |
| Interrupt non mascherabile                                                                                                                       | 2019, 2016, 2012 R2                         | &#10004;              | &#10004;              | &#10004;              |                       |
| Copia di file da host a Guest                                                                                                                 | 2019, 2016, 2012 R2                         | & #10004; Nota 2       | & #10004; Nota 2       | & #10004; Nota 2       |                       |
| comando lsvmbus                                                                                                                              | 2019, 2016, 2012 R2          |                       |                       |                       |                       |
| Socket di Hyper-V                                                                                                                              | 2019, 2016                                  | & #10004; Nota 4       | & #10004; Nota 4       |                       |                       |
| Pass-through/DDA PCI                                                                                                                          | 2019, 2016                                  | & #10004; Nota 4       | & #10004; Nota 4       |                       |                       |
| **[Macchine virtuali di seconda generazione](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)** |                                             |                       |                       |                       |                       |
| Avvio tramite UEFI                                                                                                                              | 2019, 2016, 2012 R2                         | & #10004; Nota 3       | & #10004; Nota 3       | & #10004; Nota 3       |                       |
| Avvio protetto                                                                                                                                  | 2019, 2016                                  | &#10004;              |                       |                       |                       |


## <a name="notes"></a>Note

1. Creazione di file System in dischi rigidi virtuali superiori a 2TB non è supportata.

2. A partire da 8.3 Debian installati manualmente pacchetto Debian "Hyper-v-daemon" contiene la coppia chiave-valore, fcopy e il daemon VSS. 7\. x Debian e 8.0 8.2 il pacchetto di Hyper-v-daemon deve provenire da [backports Debian](https://wiki.debian.org/Backports).

3. Nelle macchine virtuali Windows Server 2012 R2 di seconda generazione l'avvio protetto è abilitato per impostazione predefinita e alcune macchine virtuali Linux non vengono avviate a meno che l'opzione di avvio protetto non sia disabilitata. È possibile disabilitare l'avvio protetto nel **Firmware** sezione delle impostazioni per la macchina virtuale in **di gestione di Hyper-V** o è possibile disabilitarlo con Powershell:

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off
   ```
4. Le funzionalità del kernel upstream più recenti sono disponibili solo tramite il kernel che include i [backport Debian](https://wiki.debian.org/Backports).

5. Sebbene Debian 7. x non sia più supportato e usi un kernel precedente, il kernel incluso nei backport Debian per Debian 7. x ha migliorato le funzionalità di Hyper-V.

Vedere anche

* [Macchine virtuali CentOS e Red Hat Enterprise Linux supportate in Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali Oracle Linux supportate in Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali SUSE supportate in Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali Ubuntu supportate in Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali FreeBSD supportate in Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descrizioni delle funzionalità per le macchine virtuali Linux e FreeBSD in Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Procedure consigliate per l'esecuzione di Linux in Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)
