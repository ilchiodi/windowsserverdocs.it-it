---
title: Macchine virtuali Debian supportate in Hyper-V
description: Elenca le funzionalità incluse in ogni versione di Linux integration services
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3cc62c10-02a3-4633-960c-23bf91a45bd5
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: 6ec089f501a0999a4460501dbc4d03428d36af40
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863832"
---
# <a name="supported-debian-virtual-machines-on-hyper-v"></a>Macchine virtuali Debian supportate in Hyper-V

>Si applica a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

La mappa di distribuzione di funzionalità seguente indica le funzionalità presenti in ogni versione. Dopo la tabella sono elencate i problemi noti e soluzioni alternative per ogni distribuzione.

## <a name="table-legend"></a>Legenda tabella

* **Incorporata** -LIS sono inclusi come parte di questa distribuzione Linux. Il pacchetto di download LIS fornita da Microsoft non funziona per la distribuzione in modo da non installare. I numeri di versione del modulo del kernel per incorporato LIS (come illustrato da **lsmod**, ad esempio) sono diversi dal numero di versione del pacchetto di download LIS fornita da Microsoft. Una mancata corrispondenza non indicano che incorporato LIS è scaduto.

* & #10004; -Funzionalità disponibili

* (*vuoto*)-funzionalità non disponibile

|**Funzionalità**|**Versione del sistema operativo Windows Server**|**9.0-9.6 (stretch)**|**8.0-8.11 (jessie)**|**7.0-7.11 (wheezy)**|
|-|-|-|-|-|
|**Disponibilità**||Incorporata|Incorporata|Compilato (nota 6)|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Ora esatta di Windows Server 2016|2019, 2016|&#10004; Note 8||
|**[Funzionalità di rete](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Networking)**|
|Frame jumbo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|La codifica VLAN e trunking|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Migrazione in tempo reale|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Indirizzo IP statico Injection|2019, 2016, 2012 R2, 2012|||
|RSS virtuale|2019, 2016, 2012 R2|&#10004; Note 8|||
|Segmentazione di TCP e gli offload Checksum|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004; Note 8|||
|SR-IOV|2019, 2016|&#10004; Note 8||
|**[Archiviazione](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Storage)**|
|Ridimensionamento di VHDX|2019, 2016, 2012 R2|& #10004; Nota 1|& #10004; Nota 1|& #10004; Nota 1|
|Fibre Channel virtuale|2019, 2016, 2012 R2|||
|Backup della macchina virtuale in tempo reale|2019, 2016, 2012 R2|& #10004; Nota 4,5|& #10004; Nota 4,5|& #10004; Nota 4|
|Supporto per TRIM|2019, 2016, 2012 R2|&#10004; Note 8|||
|WWN SCSI|2019, 2016, 2012 R2|&#10004; Note 8||
|**[Memoria](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Memory)**|
|Supporto PAE Kernel|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Configurazione di gap MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|
|Memoria dinamica - aggiunta a caldo|2019, 2016, 2012 R2, 2012|&#10004; Note 8|||
|Memoria dinamica - Ballooning|2019, 2016, 2012 R2, 2012|&#10004; Note 8|||
|Ridimensionamento della memoria di runtime|2019, 2016|&#10004; Note 8|||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Video)**|
|Dispositivo video specifico Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;||
|**[Varie](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Misc)**|
|Coppia chiave-valore|2019, 2016, 2012 R2, 2012, 2008 R2|& #10004; Nota 4|& #10004; Nota 4||
|Interrupt non mascherabile|2019, 2016, 2012 R2|&#10004;|&#10004;|
|Copiare i file dall'host al guest|2019, 2016, 2012 R2|& #10004; Nota 4|& #10004; Nota 4||
|comando lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2|||
|Socket di Hyper-V|2019, 2016|&#10004; Note 8|||
|Pass-through/DDA PCI|2019, 2016|&#10004; Note 8|||
|**[Macchine virtuali di generazione 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_gen2)**|
|Avvio UEFI|2019, 2016, 2012 R2|& #10004; Nota 7|& #10004; Nota 7||
|Avvio protetto|2019, 2016|||

## <a name="BKMK_notes"></a>Note

1. Creazione di file System in dischi rigidi virtuali superiori a 2TB non è supportata.

2. In Windows Server 2008 R2 SCSI dischi creare 8 voci diverse in dev/sd *.

3. Windows Server 2012 R2 una macchina Virtuale con 8 core o più avranno tutti gli interrupt indirizzati a una singola CPU virtuale.

4. A partire da 8.3 Debian installati manualmente pacchetto Debian "Hyper-v-daemon" contiene la coppia chiave-valore, fcopy e il daemon VSS. 7. x Debian e 8.0 8.2 il pacchetto di Hyper-v-daemon deve provenire da [backports Debian](https://wiki.debian.org/Backports).

5. Backup di macchina virtuale non funzionerà con ext2 file System. Il layout predefinito creato dal programma di installazione Debian include i file System ext2, è necessario personalizzare il layout per non creare questo tipo di file System.

6. Debian 7. x è più supportata e Usa un kernel precedente, il kernel incluso in [backports Debian](https://wiki.debian.org/Backports) Debian 7. x è migliorato le funzionalità di Hyper-V.

7. In Windows Server 2012 R2 generazione 2 macchine virtuali presentano avvio protetto abilitato per impostazione predefinita e alcune macchine virtuali di Linux non verranno avviate se l'opzione di avvio protetto è disabilitata. È possibile disabilitare l'avvio protetto nel **Firmware** sezione delle impostazioni per la macchina virtuale in **di gestione di Hyper-V** o è possibile disabilitarlo con Powershell:

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```
8. Le funzionalità più recenti di kernel upstream sono disponibili solo usando il kernel incluso [backports Debian](https://wiki.debian.org/Backports).

Vedere anche

* [CentOS è supportato e Red Hat Enterprise Linux macchine virtuali in Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali Oracle Linux supportate in Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali SUSE supportate in Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali Ubuntu supportate in Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali FreeBSD supportate in Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Forniscono le descrizioni per le macchine virtuali Linux e FreeBSD in Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Le procedure consigliate per l'esecuzione di Linux in Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)
