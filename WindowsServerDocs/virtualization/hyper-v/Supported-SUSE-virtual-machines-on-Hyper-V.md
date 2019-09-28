---
title: Macchine virtuali SUSE supportate in Hyper-V
description: Elenca i servizi di integrazione Linux e le funzionalità incluse in ogni versione
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7ec0e14c-4498-4bd9-8fe6-b94260198efc
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: 45517c1d381ba55c819b09b53ae563092e161b1e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366732"
---
# <a name="supported-suse-virtual-machines-on-hyper-v"></a>Macchine virtuali SUSE supportate in Hyper-V

>Si applica a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7,1, Windows 7

Di seguito è una mappa di distribuzione di funzionalità che indica le funzionalità di ogni versione. Dopo la tabella sono elencate i problemi noti e soluzioni alternative per ogni distribuzione.

Il driver di SUSE Linux Enterprise Service incorporati per Hyper-V sono certificati SUSE. In questo bollettino è possibile visualizzare una configurazione di esempio: [Bollettino di certificazione SUSE sì](https://www.suse.com/nbswebapp/yesBulletin.jsp?bulletinNumber=144176).

## <a name="table-legend"></a>Legenda tabella

* Le **funzionalità predefinite** di LIS sono incluse come parte di questa distribuzione Linux. Il pacchetto di download LIS fornito da Microsoft non funziona per questa distribuzione, pertanto non è necessario installarlo. I numeri di versione del modulo kernel per l'LIS incorporato, come illustrato da **lsmod**, ad esempio, sono diversi dal numero di versione nel pacchetto di download LIS fornito da Microsoft. Una mancata corrispondenza non indica che incorporato LIS è scaduto.

* &#10004; -Funzionalità disponibili

* (*vuoto*)-funzionalità non disponibile

SLES12 + è solo a 64 bit.

|**Funzionalità**|**Versione del sistema operativo Windows Server**|**SLES 15**|**SLES 12 SP3/SP4**|**SLES 12 SP2**|**SLES 12 SP1**|**SLES 11 SP4**|**SLES 11 SP3**|
|-|-|-|-|-|-|-|-|
|**Disponibilità**||Predefinito|Predefinito|Predefinito|Predefinito|Predefinito|Predefinito|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Ora esatta di Windows Server 2016|2019, 2016|&#10004;|&#10004;|&#10004;||||
|**[Rete](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**||||||||
|Frame jumbo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Assegnazione di tag e trunking VLAN|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Migrazione in tempo reale|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Inserimento IP statico|2019, 2016, 2012 R2, 2012|&#10004;Nota 1|&#10004;Nota 1|&#10004;Nota 1|&#10004;Nota 1|&#10004;Nota 1|&#10004;Nota 1|
|RSS virtuale|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|Offload di segmentazione e checksum TCP|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|SR-IOV|2019, 2016|&#10004;|&#10004;|&#10004;||||
|**[Archiviazione](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**||||||||
|Ridimensionamento VHDX|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Fibre Channel virtuale|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Backup della macchina virtuale in tempo reale|2019, 2016, 2012 R2|&#10004; Nota 2, 3, 8|&#10004; Nota 2, 3, 8|&#10004; Nota 2, 3, 8|&#10004; Nota 2, 3, 8|&#10004; Nota 2, 3, 8|&#10004; Nota 2, 3, 8|
|Supporto TRIM|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|WWN SCSI|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;||||
|**[Memoria](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**||||||||
|Supporto del kernel PAE|2019, 2016, 2012 R2, 2012, 2008 R2|N/D|N/D|N/D|N/D|&#10004;|&#10004;|
|Configurazione di MMIO Gap|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Memoria dinamica - aggiunta a caldo|2019, 2016, 2012 R2, 2012|&#10004; Nota 5, 6|&#10004; Nota 5, 6|&#10004; Nota 5, 6|&#10004; Nota 5, 6|&#10004; Nota 4, 5, 6|&#10004; Nota 4, 5, 6|
|Memoria dinamica - Ballooning|2019, 2016, 2012 R2, 2012|&#10004; Nota 5, 6|&#10004; Nota 5, 6|&#10004; Nota 5, 6|&#10004; Nota 5, 6|&#10004; Nota 4, 5, 6|&#10004; Nota 4, 5, 6|
|Ridimensionamento della memoria di runtime|2019, 2016|&#10004; Nota 5, 6|&#10004; Nota 5, 6|&#10004; Nota 5, 6||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**||||||||
|Dispositivo video specifico di Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|**[Varie](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**||||||||
|Coppia chiave/valore|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004; Nota 7|&#10004; Nota 7|
|Interrupt non mascherabile|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Copia di file da host a Guest|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|comando lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;||||
|Socket di Hyper-V|2019, 2016|&#10004;|&#10004;|||||
|Pass-through/DDA PCI|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|||
|**[Macchine virtuali di seconda generazione](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**||||||||
|Avvio tramite UEFI|2019, 2016, 2012 R2|&#10004; Nota 9|&#10004; Nota 9|&#10004; Nota 9|&#10004; Nota 9|&#10004; Nota 9||
|Avvio protetto|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|||

## <a name="BKMK_notes"></a>Note

1. Inserimento di IP statico potrebbe non funzionare se **Gestione rete** è stato configurato per una scheda di rete specifici di Hyper-V specificata nella macchina virtuale. Per garantire il funzionamento uniforme dell'indirizzo IP statico injection, assicurarsi che Gestione di rete è disattivato completamente o è stato disattivato per una scheda di rete specifico tramite il relativo **ifcfg ethX** file.

2. Se sono presenti handle di file aperti durante un'operazione di backup di macchina virtuale, quindi in alcuni casi estremi, i dischi rigidi virtuali backup potrebbero essere necessario essere sottoposto a un controllo di coerenza di sistema di file (fsck) su ripristino.

3. Operazioni di backup Live possano un esito negativo se la macchina virtuale dispone di un dispositivo iSCSI collegati o DAS (noto anche come disco pass-through).

4. Operazioni di memoria dinamica possono non riuscire se il sistema operativo guest è troppo memoria insufficiente. Di seguito sono le procedure consigliate:

   * Memoria di avvio e di memoria minimo deve essere uguale o maggiore rispetto alla quantità di memoria in cui si consiglia il fornitore.

   * Le applicazioni che tendono a consumare l'intera memoria disponibile in un sistema sono limitate all'utilizzo di fino a 80% della RAM disponibile.

5. Supporto della memoria dinamica è disponibile solo nelle macchine virtuali a 64 bit.

6. Se si utilizza la memoria dinamica in sistemi operativi Windows Server 2016 o Windows Server 2012, specificare **memoria di avvio**, **memoria minima**, e **quantità massima di memoria** parametri in multipli di 128 megabyte (MB). In caso contrario, può causare errori aggiunta a caldo di, e non sarà possibile visualizzare qualsiasi memoria aumenta in un sistema operativo guest.

7. In Windows Server 2016 o Windows Server 2012 R2, l'infrastruttura di coppia chiave/valore potrebbe non funzionare correttamente senza un aggiornamento software Linux. Contattare il fornitore di distribuzione per ottenere l'aggiornamento software nel caso in cui noterete problemi con questa funzionalità.

8. Backup VSS non riuscirà se una singola partizione è montata più volte.

9. In Windows Server 2012 R2, le macchine virtuali di seconda generazione hanno l'avvio protetto abilitato per impostazione predefinita e le macchine virtuali Linux di seconda generazione non vengono avviate a meno che l'opzione di avvio protetto non sia disabilitata. È possibile disabilitare l'avvio protetto nella sezione **Firmware** delle impostazioni della macchina virtuale nella console di gestione di Hyper-V Manager oppure è possibile disabilitarlo con Powershell:

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```

## <a name="see-also"></a>Vedere anche

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [Macchine virtuali CentOS e Red Hat Enterprise Linux supportate in Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali Debian supportate in Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali Oracle Linux supportate in Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali Ubuntu supportate in Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali FreeBSD supportate in Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descrizioni delle funzionalità per le macchine virtuali Linux e FreeBSD in Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Procedure consigliate per l'esecuzione di Linux in Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)
