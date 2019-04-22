---
title: Macchine virtuali FreeBSD supportate in Hyper-V
description: Elenca le funzionalità incluse in ogni versione di Linux integration services
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 930e758f-bd50-46b4-a3a4-9857110f17b4
author: shirgall
ms.author: kathydav
ms.date: 08/30/2017
ms.openlocfilehash: 013328953321bc66b3fd30759e5be321eea32dde
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824232"
---
# <a name="supported-freebsd-virtual-machines-on-hyper-v"></a>Macchine virtuali FreeBSD supportate in Hyper-V

>Si applica a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

La mappa di distribuzione di funzionalità seguente indica le funzionalità di ogni versione. Dopo la tabella sono elencate i problemi noti e soluzioni alternative per ogni distribuzione.

## <a name="table-legend"></a>Legenda tabella

* **Compilato** -BIS (FreeBSD Integration Service) sono inclusi come parte di questa versione di FreeBSD.

* & #10004; -Funzionalità disponibili

* (*vuoto*)-funzionalità non disponibile

|**Funzionalità**|**Versione del sistema operativo Windows Server**|**11.1/11.2**|**11.0**|**10.3**|**10.2**|**10.0 - 10.1**|**9.1 - 9.3, 8.4**|
|-|-|-|-|-|-|-|-|
|**Disponibilità**||Incorporata|Incorporata|Incorporata|Incorporata|Incorporata|[Porte](https://svnweb.freebsd.org/ports/branches/2015Q1/emulators/hyperv-is/) |
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_core)**|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004; |
|Ora esatta di Windows Server 2016|2016|&#10004;||||||
|**[Funzionalità di rete](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Networking)**||||||||
|Frame jumbo|2016, 2012 R2, 2012, 2008 R2|& #10004; Nota 3|& #10004; Nota 3|& #10004; Nota 3|& #10004; Nota 3|& #10004; Nota 3|& #10004; Nota 3|
|La codifica VLAN e trunking|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Migrazione in tempo reale|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Indirizzo IP statico Injection|2016, 2012 R2, 2012|& #10004; Nota 4|& #10004; Nota 4|& #10004; Nota 4|& #10004; Nota 4|& #10004; Nota 4|&#10004;|
|RSS virtuale|2016, 2012 R2|&#10004;|&#10004;|||||
|Segmentazione di TCP e gli offload Checksum|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|Grandi ricezione Offload (e)|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;||||
|SR-IOV|2016|||||||
|**[Archiviazione](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Storage)**||Nota 1|Nota 1|Nota 1|Nota 1|Nota 1, 2|Nota 1, 2|
|Ridimensionamento di VHDX|2016, 2012 R2|& #10004; Nota 7|& #10004; Nota 7|||||
|Fibre Channel virtuale|2016, 2012 R2|||||||
|Backup della macchina virtuale in tempo reale|2016, 2012 R2|&#10004;||||||
|Supporto per TRIM|2016, 2012 R2|&#10004;||||||
|WWN SCSI|2016, 2012 R2|||||||
|**[Memoria](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Memory)**||||||||
|Supporto PAE Kernel|2016, 2012 R2, 2012, 2008 R2|||||||
|Configurazione di gap MMIO|2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Memoria dinamica - aggiunta a caldo|2016, 2012 R2, 2012|||||||
|Memoria dinamica - Ballooning|2016, 2012 R2, 2012|||||||
|Ridimensionamento della memoria di runtime|2016|||||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Video)**||||||||
|Dispositivo video specifico Hyper-V|2016, 2012 R2, 2012, 2008 R2|||||||
|**[Varie](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Misc)**||||||||
|Coppia chiave/valore|2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;Nota 6|& #10004; Nota 5, 6|&#10004;Nota 6|
|Interrupt non mascherabile|2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Copiare i file dall'host al guest|2016, 2012 R2|||||||
|comando lsvmbus|2016, 2012 R2, 2012, 2008 R2|||||||
|Socket di Hyper-V|2016|||||||
|Pass-through/DDA PCI|2016|&#10004;||||||
|**[Macchine virtuali di generazione 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_gen2)**||||||||
|Avvio UEFI|2016, 2012 R2|&#10004;||||||
|Avvio protetto|2016|||||||

## <a name="BKMK_notes"></a>Note

1. Per suggerire [dispositivi disco etichetta]( https://www.freebsd.org/doc/handbook/geom-glabel.html) per evitare errori di montaggio radice durante l'avvio.

2. L'unità DVD virtuale potrebbe non essere riconosciuta quando vengono caricati i driver BIS in FreeBSD 8.x e 9.x a meno che non si abilita il driver legacy di ATA tramite il comando seguente.
    ```sh
    # echo ‘hw.ata.disk_enable=1’ >> /boot/loader.conf
    # shutdown -r now
    ```

3. 9126 è che la massima supportata per la dimensione MTU.

4. In uno scenario di failover, è possibile impostare un indirizzo IPv6 statico nel server di replica. Utilizzare un indirizzo IPv4.

5. Coppia chiave-valore viene fornito da porte FreeBSD 10.0. Vedere le [porte FreeBSD 10.0](https://svnweb.freebsd.org/ports/branches/2015Q1/emulators/hyperv-is/) su FreeBSD.org per ulteriori informazioni.

6. Coppia chiave-VALORE potrebbe non funzionare in Windows Server 2008 R2.

7. Per garantire il funzionamento di ridimensionamento online VHDX correttamente in FreeBSD 11.0, è necessario risolvere un bug di geometria che è stato risolto in 11.0 +, dopo che l'host viene ridimensionato il disco VHDX: aprire il disco per scrittura ed eseguire "gpart recover" come indicato di seguito un passaggio manuale speciale.
    ```sh
    # dd if=/dev/da1 of=/dev/da1 count=0
    # gpart recover da1
    ```
**Note aggiuntive**: Matrice di funzionalità di stable 10 e 11 stabile è lo stessa con versione FreeBSD 11.1. In aggiunta, FreeBSD 10.2 e versioni precedenti (10.1, 10.0, 9.x, 8.x) di vita. Consultare [qui](https://security.freebsd.org/) per un elenco aggiornato delle versioni supportate e gli avvisi di sicurezza più recenti.

**Note aggiuntive**: Matrice di funzionalità di stable 10 e 11 stabile è lo stessa con versione FreeBSD 11.1. In aggiunta, FreeBSD 10.2 e versioni precedenti (10.1, 10.0, 9.x, 8.x) di vita. Consultare [qui](https://security.freebsd.org/) per un elenco aggiornato delle versioni supportate e gli avvisi di sicurezza più recenti.

## <a name="see-also"></a>Vedere anche

* [Forniscono le descrizioni per le macchine virtuali Linux e FreeBSD in Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)
* [Le procedure consigliate per l'esecuzione di FreeBSD in Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)
