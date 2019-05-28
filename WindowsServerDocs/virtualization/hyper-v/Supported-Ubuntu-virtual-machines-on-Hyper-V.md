---
title: Macchine virtuali Ubuntu supportate in Hyper-V
description: Elenca le funzionalità incluse in ogni versione di Linux integration services
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95ea5f7c-25c6-494b-8ffd-2a77f631ee94
author: shirgall
ms.author: shirgall
ms.date: 11/19/2018
ms.openlocfilehash: b58193ec570cf0d94b6c95018b8c00c813331986
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222647"
---
# <a name="supported-ubuntu-virtual-machines-on-hyper-v"></a>Macchine virtuali Ubuntu supportate in Hyper-V

>Si applica a: Windows Server 2019, 2016, Hyper-V Server 2019, 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

A partire da Ubuntu 12.04, caricamento del pacchetto "virtuale linux" Installa un kernel appropriato per l'uso come una macchina virtuale guest. Questo pacchetto sempre dipende dall'immagine più recente del kernel generico minimo e intestazioni utilizzate per le macchine virtuali. Durante l'utilizzo è facoltativo, il kernel linux virtuale caricherà un minor numero di driver e potrebbe avvio più veloce e meno overhead di memoria rispetto a un'immagine generica.

Per sfruttare appieno di Hyper-V, installare i pacchetti linux-cloud-strumenti per installare gli strumenti e daemon per l'utilizzo con le macchine virtuali e linux-strumenti appropriati. Quando si utilizza il kernel linux virtuale, caricare linux-strumenti-linux-cloud-strumenti-virtuali e.

La mappa di distribuzione di funzionalità seguente indica le funzionalità di ogni versione. Dopo la tabella sono elencate i problemi noti e soluzioni alternative per ogni distribuzione.

## <a name="table-legend"></a>Legenda tabella

* **Incorporata** -LIS sono inclusi come parte di questa distribuzione Linux. Il pacchetto di download LIS fornita da Microsoft non funziona per la distribuzione, in modo da non installare il file. I numeri di versione del modulo del kernel per incorporato LIS (come illustrato da **lsmod**, ad esempio) sono diversi dal numero di versione del pacchetto di download LIS fornita da Microsoft. Una mancata corrispondenza non indica che incorporato LIS è scaduto.

* &#10004; -Funzionalità disponibili

* (*vuoto*)-funzionalità non disponibile

|**Funzionalità**|**Versione del sistema operativo Windows Server**|**18.10**|**18.04 LTS**|**16.04 LTS**|**14.04 LTS**|**12.04 LTS**|
|-|-|-|-|-|-|-|
|**Disponibilità**||Predefinito|Predefinito|Predefinito|Predefinito|Predefinito|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Ora esatta di Windows Server 2016|2019, 2016|&#10004;|&#10004;|&#10004;|||
|**[Funzionalità di rete](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**|||||||
|Frame jumbo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|La codifica VLAN e trunking|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Migrazione in tempo reale|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Indirizzo IP statico Injection|2019, 2016, 2012 R2, 2012|&#10004; Nota 1|&#10004; Nota 1|&#10004; Nota 1|&#10004; Nota 1|&#10004; Nota 1|
|RSS virtuale|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|Segmentazione di TCP e gli offload Checksum|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|SR-IOV|2019, 2016|&#10004;|&#10004;|&#10004;|||
|**[Archiviazione](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**||||||
|Ridimensionamento di VHDX|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|Fibre Channel virtuale|2019, 2016, 2012 R2|&#10004; Nota 2|&#10004; Nota 2|&#10004; Nota 2|&#10004; Nota 2||
|Backup della macchina virtuale in tempo reale|2019, 2016, 2012 R2|&#10004;Prendere nota 3, 4, 6|&#10004; Nota 3, 4, 5|&#10004; Nota 3, 4, 5|&#10004; Nota 3, 4, 5||
|Supporto per TRIM|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|WWN SCSI|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|**[Memoria](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**||||||
|Supporto PAE Kernel|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Configurazione di gap MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Memoria dinamica - aggiunta a caldo|2019, 2016, 2012 R2, 2012|&#10004; Si noti 7, 8, 9|&#10004; Si noti 7, 8, 9|&#10004; Si noti 7, 8, 9|&#10004; Si noti 7, 8, 9||
|Memoria dinamica - Ballooning|2019, 2016, 2012 R2, 2012|&#10004; Si noti 7, 8, 9|&#10004; Si noti 7, 8, 9|&#10004; Si noti 7, 8, 9|&#10004; Si noti 7, 8, 9||
|Ridimensionamento della memoria di runtime|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**|||||||
|Dispositivo video specifico Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|**[Varie](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**||||||
|Coppia chiave/valore|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;Nota 6, 10|&#10004; Nota 5, 10|&#10004; Nota 5, 10|&#10004; Nota 5, 10|&#10004; Nota 5, 10|
|Interrupt non mascherabile|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Copiare i file dall'host al guest|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|comando lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|Socket di Hyper-V|2019, 2016||||||
|Pass-through/DDA PCI|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;||
|**[Macchine virtuali di generazione 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**||||||
|Avvio UEFI|2019, 2016, 2012 R2|&#10004; Nota 11, 12|&#10004; Nota 11, 12|&#10004; Nota 11, 12|&#10004; Nota 11, 12||
|Avvio protetto|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;||

## <a name="notes"></a>Note

1. Inserimento di IP statico potrebbe non funzionare se **Gestione rete** è stato configurato per una scheda di rete specifici di Hyper-V specificata nella macchina virtuale. Per garantire il funzionamento uniforme dell'indirizzo IP statico injection, assicurarsi che Gestione di rete è disattivato completamente o è stato disattivato per una scheda di rete specifico tramite il relativo **ifcfg ethX** file.

2. Quando si utilizzano dispositivi di virtual fiber channel, assicurarsi che il numero di unità logica (LUN 0) 0 è stato popolato. Se non è stato popolato LUN 0, una macchina virtuale Linux potrebbe non essere in grado di installare dispositivi di fiber channel in modo nativo.

3. Se non vi sono aperte gli handle di file durante un'operazione di backup di macchina virtuale, quindi in alcuni casi estremi, i dischi rigidi virtuali backup potrebbero essere necessario eseguire una verifica coerenza file system (`fsck`) su ripristino.

4. Operazioni di backup Live possano un esito negativo se la macchina virtuale dispone di un dispositivo iSCSI collegati o DAS (noto anche come disco pass-through).

5. Sul supporto a lungo termine (LTS) versioni utilizzano più recente del kernel Hardware attivazione (HWE) virtuale per aggiornati Linux Integration Services.

   Per installare il kernel Azure ottimizzato su 14.04, 16.04 e 18.04, eseguire i comandi seguenti come radice (o sudo):

   ```bash
   # apt-get update
   # apt-get install linux-azure

   ```

   12.04 non è un kernel virtuale separato. Per installare il kernel HWE generico 12.04, eseguire i comandi seguenti come radice (o sudo):

   ```bash
   # apt-get update
   # apt-get install linux-generic-lts-trusty

   ```

   Ubuntu 12.04 il daemon di Hyper-V seguenti sono in un pacchetto installato separatamente:

   * **Daemon Snapshot VSS** -il daemon è necessaria per creare backup della macchina virtuale Linux live.
   * **Daemon di coppia chiave-VALORE** -il daemon consente di impostare e l'esecuzione di query intrinseche ed estrinseci coppie chiave-valore.
   * **daemon fcopy** -il daemon implementa un servizio tra l'host e guest di copia dei file.

   Per installare il daemon di coppia chiave-valore 12.04, eseguire i comandi seguenti come radice (o sudo).

   ```bash
   # apt-get install hv-kvp-daemon-init linux-tools-lts-trusty linux-cloud-tools-generic-lts-trusty

   ```

   Ogni volta che viene aggiornato il kernel, è necessario riavviare la macchina virtuale per poterlo utilizzare.

6. In Ubuntu 18.10, usare il kernel virtuale più recente per disporre di funzionalità di Hyper-V aggiornate.

   Per installare il kernel virtuale 18.10, eseguire i comandi seguenti come radice (o sudo):

   ```bash
   # apt-get update
   # apt-get install linux-azure

   ```

   Ogni volta che viene aggiornato il kernel, è necessario riavviare la macchina virtuale per poterlo utilizzare.

7. Supporto della memoria dinamica è disponibile solo nelle macchine virtuali a 64 bit.

8. Le operazioni di memoria dinamiche possono non riuscire se il sistema operativo guest è troppo memoria insufficiente. Di seguito sono le procedure consigliate:

   * Memoria di avvio e di memoria minimo deve essere uguale o maggiore rispetto alla quantità di memoria in cui si consiglia il fornitore.

   * Le applicazioni che tendono a consumare l'intera memoria disponibile in un sistema sono limitate all'utilizzo di fino a 80% della RAM disponibile.

9. Se si utilizza la memoria dinamica in Windows Server 2019, Windows Server 2016 o sistemi operativi Windows Server 2012 o 2012 R2, specificare **memoria di avvio**, **memoria minima**, e **massimo memoria** parametri in multipli di 128 megabyte (MB). In caso contrario, può causare errori aggiunta a caldo di, e che non appare alcuna memoria aumenta in un sistema operativo guest.

10. In Windows Server 2019, Windows Server 2016 o Windows Server 2012 R2, l'infrastruttura di coppia chiave/valore potrebbe non funzionare correttamente senza un aggiornamento software Linux. Contattare il fornitore di distribuzione per ottenere l'aggiornamento software nel caso in cui noterete problemi con questa funzionalità.

11. In Windows Server 2012 R2, macchine virtuali di generazione 2 è avvio protetto abilitato per impostazione predefinita e alcune Linux macchine virtuali non verranno avviate se l'opzione di avvio protetto è disabilitata. È possibile disabilitare l'avvio protetto nel **Firmware** sezione delle impostazioni per la macchina virtuale in **di gestione di Hyper-V** o è possibile disabilitarlo con Powershell:

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```

12. Prima di tentare di copiare il disco rigido Virtuale di una macchina virtuale di generazione 2 VHD esistente per creare nuove macchine virtuali di generazione 2, seguire questi passaggi:

   1. Accedere alla macchina virtuale esistente generazione 2.

   2. Cambiare directory nella directory di avvio EFI:

      ```bash
      # cd /boot/efi/EFI

      ```

   3. Copiare la directory ubuntu in una nuova directory denominata avvio:

      ```bash
      # sudo cp -r ubuntu/ boot

      ```

   4. Passare alla directory nella directory di avvio appena creata:

      ```bash
      # cd boot

      ```

   5. Rinominare il file shimx64.efi:

      ```bash
      # sudo mv shimx64.efi bootx64.efi

      ```

## <a name="see-also"></a>Vedere anche

* [CentOS è supportato e Red Hat Enterprise Linux macchine virtuali in Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali Debian supportate in Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali Oracle Linux supportate in Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali SUSE supportate in Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Forniscono le descrizioni per le macchine virtuali Linux e FreeBSD in Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Le procedure consigliate per l'esecuzione di Linux in Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [Ubuntu 14.04 nella generazione 2 VM - Blog di virtualizzazione di Ben Armstrong](https://blogs.msdn.com/b/virtual_pc_guy/archive/2014/06/09/ubuntu-14-04-in-a-generation-2-vm.aspx)
