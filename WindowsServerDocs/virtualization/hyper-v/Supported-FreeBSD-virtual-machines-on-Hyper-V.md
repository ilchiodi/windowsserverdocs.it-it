---
title: Macchine virtuali FreeBSD supportate in Hyper-V
description: Elenca i servizi di integrazione Linux e le funzionalità incluse in ogni versione
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
ms.openlocfilehash: 3ca1de87469c30a8cadbf047e77aff441145a499
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869478"
---
# <a name="supported-freebsd-virtual-machines-on-hyper-v"></a>Macchine virtuali FreeBSD supportate in Hyper-V

>Si applica a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7,1, Windows 7

La mappa di distribuzione di funzionalità seguente indica le funzionalità di ogni versione. Dopo la tabella sono elencate i problemi noti e soluzioni alternative per ogni distribuzione.

## <a name="table-legend"></a>Legenda tabella

* **Built in** -bis (FreeBSD Integration Service) è incluso in questa versione di FreeBSD.

* &#10004; -Funzionalità disponibili

* (*vuoto*)-funzionalità non disponibile

|**Funzionalità**|**Versione del sistema operativo Windows Server**|**11.1/11.2**|**11.0**|**10.3**|**10.2**|**10.0 - 10.1**|**9,1-9,3, 8,4**|
|-|-|-|-|-|-|-|-|
|**Disponibilità**||Incorporata|Incorporata|Incorporata|Incorporata|Incorporata|[Porte](https://svnweb.freebsd.org/ports/branches/2015Q1/emulators/hyperv-is/) |
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004; |
|Ora esatta di Windows Server 2016|2019, 2016|&#10004;||||||
|**[Rete](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**||||||||
|Frame jumbo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004; Nota 3|&#10004; Nota 3|&#10004; Nota 3|&#10004; Nota 3|&#10004; Nota 3|&#10004; Nota 3|
|Assegnazione di tag e trunking VLAN|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Migrazione in tempo reale|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Inserimento IP statico|2019, 2016, 2012 R2, 2012|&#10004; Nota 4|&#10004; Nota 4|&#10004; Nota 4|&#10004; Nota 4|&#10004; Nota 4|&#10004;|
|RSS virtuale|2019, 2016, 2012 R2|&#10004;|&#10004;|||||
|Offload di segmentazione e checksum TCP|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|Grandi ricezione Offload (e)|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;||||
|SR-IOV|2019, 2016|||||||
|**[Archiviazione](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**||Nota 1|Nota 1|Nota 1|Nota 1|Nota 1, 2|Nota 1, 2|
|Ridimensionamento VHDX|2019, 2016, 2012 R2|&#10004; Nota 7|&#10004; Nota 7|||||
|Fibre Channel virtuale|2019, 2016, 2012 R2|||||||
|Backup della macchina virtuale in tempo reale|2019, 2016, 2012 R2|&#10004;||||||
|Supporto TRIM|2019, 2016, 2012 R2|&#10004;||||||
|WWN SCSI|2019, 2016, 2012 R2|||||||
|**[Memoria](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**||||||||
|Supporto del kernel PAE|2019, 2016, 2012 R2, 2012, 2008 R2|||||||
|Configurazione di MMIO Gap|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Memoria dinamica - aggiunta a caldo|2019, 2016, 2012 R2, 2012|||||||
|Memoria dinamica - Ballooning|2019, 2016, 2012 R2, 2012|||||||
|Ridimensionamento della memoria di runtime|2019, 2016|||||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**||||||||
|Dispositivo video specifico Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|||||||
|**[Varie](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**||||||||
|Coppia chiave/valore|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;Nota 6|&#10004; Nota 5, 6|&#10004;Nota 6|
|Interrupt non mascherabile|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Copia di file da host a Guest|2019, 2016, 2012 R2|||||||
|comando lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2|||||||
|Socket di Hyper-V|2019, 2016|||||||
|Pass-through/DDA PCI|2019, 2016|&#10004;||||||
|**[Macchine virtuali di seconda generazione](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**||||||||
|Avvio tramite UEFI|2019, 2016, 2012 R2|&#10004;||||||
|Avvio protetto|2019, 2016|||||||

## <a name="BKMK_notes"></a>Note

1. Suggerire di [etichettare i dispositivi disco]( https://www.freebsd.org/doc/handbook/geom-glabel.html) per evitare errori di montaggio radice durante l'avvio.

2. È possibile che l'unità DVD virtuale non venga riconosciuta quando i driver BIS vengono caricati in FreeBSD 8. x e 9. x, a meno che non si abiliti il driver ATA legacy tramite il comando seguente.
    ```sh
    # echo ‘hw.ata.disk_enable=1' >> /boot/loader.conf
    # shutdown -r now
    ```

3. 9126 è la dimensione MTU massima supportata.

4. In uno scenario di failover, è possibile impostare un indirizzo IPv6 statico nel server di replica. Utilizzare un indirizzo IPv4.

5. KVP viene fornito dalle porte in FreeBSD 10,0. Per ulteriori informazioni, vedere le [porte FreeBSD 10,0](https://svnweb.freebsd.org/ports/branches/2015Q1/emulators/hyperv-is/) in FreeBSD.org.

6. Coppia chiave-VALORE potrebbe non funzionare in Windows Server 2008 R2.

7. Per far funzionare correttamente il ridimensionamento di VHDX online in FreeBSD 11,0, è necessario eseguire un passaggio manuale speciale per aggirare un bug GEOM risolto in 11.0 +, dopo che l'host ha ridimensionato il disco VHDX-aprire il disco per la scrittura ed eseguire "gpart Recover" come riportato di seguito.
    ```sh
    # dd if=/dev/da1 of=/dev/da1 count=0
    # gpart recover da1
    ```
   **Note aggiuntive**: La matrice di funzionalità di 10 stabili e 11 stabili è uguale alla versione di FreeBSD 11,1. Inoltre, FreeBSD 10,2 e le versioni precedenti (10,1, 10,0, 9. x, 8. x) sono alla fine della vita. Per un elenco aggiornato delle versioni supportate e degli avvisi di sicurezza più recenti, fai riferimento a [questa](https://security.freebsd.org/) pagina.

**Note aggiuntive**: La matrice di funzionalità di 10 stabili e 11 stabili è uguale alla versione di FreeBSD 11,1. Inoltre, FreeBSD 10,2 e le versioni precedenti (10,1, 10,0, 9. x, 8. x) sono alla fine della vita. Per un elenco aggiornato delle versioni supportate e degli avvisi di sicurezza più recenti, fai riferimento a [questa](https://security.freebsd.org/) pagina.

## <a name="see-also"></a>Vedere anche

* [Descrizioni delle funzionalità per le macchine virtuali Linux e FreeBSD in Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)
* [Procedure consigliate per l'esecuzione di FreeBSD in Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)
