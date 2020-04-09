---
title: Procedure consigliate per l'esecuzione di Linux in Hyper-V
description: Fornisce consigli per l'esecuzione di Linux in una macchina virtuale
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: a08648eb-eea0-4e2b-87fb-52bfe8953491
author: shirgall
ms.author: kathydav
ms.date: 3/1/2019
ms.openlocfilehash: 7baf71af401b8318ccd136fe12d6eb810cf9434e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853304"
---
# <a name="best-practices-for-running-linux-on-hyper-v"></a>Procedure consigliate per l'esecuzione di Linux in Hyper-V

>Si applica a: Windows Server 2019, Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7,1, Windows 7

Questo argomento contiene un elenco di consigli per l'esecuzione di macchine virtuali Linux in Hyper-V.

## <a name="tuning-linux-file-systems-on-dynamic-vhdx-files"></a>Ottimizzazione dei file system Linux in file VHDX dinamici

Alcuni file system Linux possono utilizzare quantità significative di spazio su disco reale anche quando il file system è quasi vuoto. Per ridurre la quantità di spazio su disco reale utilizzato per i file VHDX dinamici, considerare i consigli seguenti:

* Quando si crea il VHDX, usare 1 MB BlockSizeBytes (da 32 MB predefiniti) in PowerShell, ad esempio:

```Powershell
PS > New-VHD -Path C:\MyVHDs\test.vhdx -SizeBytes 127GB -Dynamic -BlockSizeBytes 1MB
```

* Il formato ext4 è preferibile a ext3 perché ext4 è più efficiente dello spazio di ext3 quando viene usato con file VHDX dinamici.

* Quando si crea il file System, specificare il numero di gruppi da 4096, ad esempio:

```bash
# mkfs.ext4 -G 4096 /dev/sdX1

```

## <a name="grub-menu-timeout-on-generation-2-virtual-machines"></a>Timeout del menu grub sulle macchine virtuali di seconda generazione

A causa dell'hardware legacy rimosso dall'emulazione nelle macchine virtuali di seconda generazione, il timer del conto alla rovescia del menu GRUB viene conteggiato troppo rapidamente per visualizzare il menu grub, caricando immediatamente la voce predefinita. Fino a quando GRUB non è stato corretto per l'uso del timer supportato da EFI, modificare **/boot/grub/grub.conf**,/**etc/default/grub**oppure equivalente a "timeout = 100000" invece che al valore predefinito "timeout = 5".

## <a name="pxe-boot-on-generation-2-virtual-machines"></a>Avvio PxE sulle macchine virtuali di seconda generazione

Poiché il timer PIT non è presente nelle macchine virtuali di seconda generazione, le connessioni di rete al server TFTP PxE possono essere terminate in anticipo e impedire al bootloader di leggere la configurazione di GRUB e di caricare un kernel dal server.

In RHEL 6. x, è possibile usare il bootloader di GRUB v 0.97 EFI legacy anziché GRUB2, come descritto qui: [https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/s1-netboot-pxe-config-efi.html](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/s1-netboot-pxe-config-efi.html)

Nelle distribuzioni di Linux diverse da RHEL 6. x, è possibile seguire una procedura simile per configurare GRUB v 0.97 per caricare i kernel Linux da un server PxE.

Inoltre, nell'input della tastiera e del mouse RHEL/CentOS 6,6 non funzionerà con il kernel di pre-installazione che impedisce di specificare le opzioni di installazione nel menu. Una console seriale deve essere configurata in modo da consentire la scelta delle opzioni di installazione.

* Nel file **efidefault** del server PxE aggiungere il seguente parametro del kernel **"console = ttyS1"**

* Nella macchina virtuale in Hyper-V configurare una porta COM usando questo cmdlet di PowerShell:

```Powershell
Set-VMComPort -VMName <Name> -Number 2 -Path \\.\pipe\dbg1

```

Se si specifica un file kickstart per il kernel di pre-installazione, si eviteranno anche le richieste di input da tastiera e mouse durante l'installazione.

## <a name="use-static-mac-addresses-with-failover-clustering"></a>Usare gli indirizzi MAC statici con il clustering di failover

Le macchine virtuali Linux che verranno distribuite tramite il clustering di failover devono essere configurate con un indirizzo MAC Media Access Control statico per ogni scheda di rete virtuale. In alcune versioni di Linux la configurazione di rete potrebbe andare persa dopo il failover perché un nuovo indirizzo MAC viene assegnato alla scheda di rete virtuale. Per evitare di perdere la configurazione di rete, verificare che ogni scheda di rete virtuale disponga di un indirizzo MAC statico. È possibile configurare l'indirizzo MAC modificando le impostazioni della macchina virtuale nella console di gestione di Hyper-V o Gestione cluster di failover.

## <a name="use-hyper-v-specific-network-adapters-not-the-legacy-network-adapter"></a>Usare schede di rete specifiche di Hyper-V e non la scheda di rete legacy

Configurare e usare la scheda Ethernet virtuale, che è una scheda di rete specifica di Hyper-V con prestazioni migliorate. Se entrambe le schede di rete legacy e Hyper-V sono collegate a una macchina virtuale, i nomi di rete nell'output di **ifconfig-a** possono mostrare valori casuali, ad esempio **_tmp12000801310**. Per evitare questo problema, rimuovere tutte le schede di rete legacy quando si usano schede di rete specifiche di Hyper-V in una macchina virtuale Linux.

## <a name="use-io-scheduler-noop-for-better-disk-io-performance"></a>Usare l'utilità di pianificazione di I/O NOOP per migliorare le prestazioni di I/O su disco

Il kernel Linux ha quattro diverse utilità di pianificazione di I/O per riordinare le richieste con algoritmi diversi. NOOP è una coda First-in First-out che passa la decisione di pianificazione che deve essere eseguita dall'hypervisor. Si consiglia di usare NOOP come utilità di pianificazione durante l'esecuzione di una macchina virtuale Linux in Hyper-V. Per modificare l'utilità di pianificazione per un dispositivo specifico, nella configurazione del caricatore di avvio (ad esempio,/etc/grub.conf) aggiungere **Elevator = NOOP** ai parametri del kernel e quindi riavviare.

## <a name="numa"></a>NUMA

Le versioni del kernel Linux precedenti a 2.6.37 non supportano NUMA in Hyper-V con dimensioni di VM maggiori. Questo problema influisca principalmente sulle distribuzioni precedenti che usano il kernel Red Hat 2.6.32 upstream ed è stato risolto in Red Hat Enterprise Linux (RHEL) 6,6 (kernel-2.6.32-504). I sistemi che eseguono kernel personalizzati precedenti a 2.6.37 o i kernel basati su RHEL precedenti a 2.6.32-504 devono impostare il parametro di avvio `numa=off` nella riga di comando del kernel in grub. conf. Per ulteriori informazioni, vedere la pagina relativa alla [KB 436883 di Red Hat](https://access.redhat.com/solutions/436883).

## <a name="reserve-more-memory-for-kdump"></a>Riservare ulteriore memoria per kdump

Nel caso in cui il kernel di acquisizione del dump venga interrotto con un panico all'avvio, riservare più memoria per il kernel. Modificare ad esempio il parametro **crashkernel = 384 MB-: 128M** in **crashkernel = 384 MB-: 256M** nel file di configurazione di Ubuntu grub.

## <a name="shrinking-vhdx-or-expanding-vhd-and-vhdx-files-can-result-in-erroneous-gpt-partition-tables"></a>La compattazione di VHDX o l'espansione di file VHD e VHDX può comportare tabelle di partizione GPT errate

Hyper-V consente di compattare i file del disco virtuale (VHDX) senza considerare le strutture di dati di partizioni, volumi o file system che possono esistere sul disco. Se il VHDX viene compattato fino alla fine della VHDX prima della fine di una partizione, è possibile che i dati vadano persi, che la partizione possa essere danneggiata o che siano restituiti dati non validi quando la partizione viene letta.

Dopo il ridimensionamento di un disco rigido virtuale o VHDX, gli amministratori devono utilizzare un'utilità come fdisk o una parte per aggiornare le strutture Partition, volume e file system in modo da riflettere le modifiche apportate alle dimensioni del disco. La compattazione o l'espansione delle dimensioni di un disco rigido virtuale o di una VHDX che dispone di una tabella di partizione GUID (GPT) genererà un avviso quando viene utilizzato uno strumento di gestione delle partizioni per verificare il layout della partizione e l'amministratore riceverà un avviso per correggere le intestazioni GPT iniziali e secondarie. Questo passaggio manuale è sicuro per l'esecuzione senza perdita di dati.

## <a name="see-also"></a>Vedere anche

* [Macchine virtuali Linux e FreeBSD supportate per Hyper-V in Windows](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)

* [Procedure consigliate per l'esecuzione di FreeBSD in Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)

* [Distribuire un cluster Hyper-V](https://technet.microsoft.com/library/jj863389.aspx)

* [Creare immagini Linux per Azure](https://docs.microsoft.com/azure/virtual-machines/linux/create-upload-generic)

* [Ottimizzare la VM Linux in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/optimization)
