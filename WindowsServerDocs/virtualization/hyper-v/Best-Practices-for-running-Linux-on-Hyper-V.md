---
title: Procedure consigliate per l'esecuzione di Linux in Hyper-V
description: Fornisce indicazioni per l'esecuzione di Linux in una macchina virtuale
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a08648eb-eea0-4e2b-87fb-52bfe8953491
author: shirgall
ms.author: kathydav
ms.date: 3/1/2019
ms.openlocfilehash: a24e2b1a1d79d52c1cc16f9e7c1b253d9b477aae
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284446"
---
# <a name="best-practices-for-running-linux-on-hyper-v"></a>Procedure consigliate per l'esecuzione di Linux in Hyper-V

>Si applica a: Windows Server 2019, Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

In questo argomento contiene un elenco di raccomandazioni per l'esecuzione di macchina virtuale Linux in Hyper-V.

## <a name="tuning-linux-file-systems-on-dynamic-vhdx-files"></a>Ottimizzare i sistemi di File di Linux su file VHDX dinamici

Alcuni file System di Linux potrebbero usare grandi quantità di spazio su disco effettivo, anche quando il file system è pressoché vuoto. Per ridurre la quantità di spazio su disco effettivo dei file VHDX dinamici, tenere presente quanto segue:

* Quando si crea il file VHDX, usare 1 MB BlockSizeBytes (dal valore predefinito 32MB) in PowerShell, ad esempio:

```Powershell
PS > New-VHD -Path C:\MyVHDs\test.vhdx -SizeBytes 127GB -Dynamic -BlockSizeBytes 1MB
```

* Il formato ext4 è preferibile a ext3 perché ext4 spazio più efficiente rispetto a ext3 quando usato con file VHDX dinamici.

* Quando la creazione del file System specifica il numero di gruppi in modo 4096, ad esempio:

```bash
# mkfs.ext4 -G 4096 /dev/sdX1

```

## <a name="grub-menu-timeout-on-generation-2-virtual-machines"></a>Timeout del Menu di GRUB in macchine virtuali di generazione 2

A causa di hardware legacy da rimuovere dall'emulazione nelle macchine virtuali di generazione 2, il timer del conto alla rovescia menu grub conta in senso decrescente troppo rapidamente per il menu di grub da visualizzare, caricare immediatamente la voce predefinita. Finché non viene risolto grub per utilizzare il timer supportato da EFI, modificare **/boot/grub/grub.conf**, /**via/predefinita/grub**, o equivalente di "timeout = 100000" anziché il valore predefinito "timeout = 5".

## <a name="pxe-boot-on-generation-2-virtual-machines"></a>Avvio PxE in macchine virtuali di generazione 2

Poiché il timer PIT non è presente nelle macchine virtuali di generazione 2, le connessioni di rete per il server TFTP PxE possono venire terminate in modo anomalo e impediscono il caricatore di avvio dalla lettura di configurazione di Grub e il caricamento di un kernel dal server.

In RHEL 6.x, il caricatore di avvio grub legacy EFI v0.97 può essere usato invece grub2 come descritto di seguito: [https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/s1-netboot-pxe-config-efi.html](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/s1-netboot-pxe-config-efi.html)

Nelle distribuzioni Linux diverse da RHEL 6.x, una procedura simile può essere seguita per configurare v0.97 grub per caricare i kernel Linux da un server PxE.

Inoltre, RHEL/CentOS 6.6 tastiera e mouse input non funzioneranno con il kernel di pre-installazione che impedisce che specificano le opzioni di installazione nel menu di scelta. Una console seriale deve essere configurata per consentire la scelta di opzioni di installazione.

* Nel **efidefault** sul server PxE, aggiungere il parametro del kernel seguenti **"console = ttyS1"**

* Nella macchina virtuale in Hyper-V, configurare una porta COM utilizzando il seguente cmdlet di PowerShell:

```Powershell
Set-VMComPort -VMName <Name> -Number 2 -Path \\.\pipe\dbg1

```

Specifica di un file kickstart al kernel di pre-installazione inoltre evita la necessità di tastiera e mouse durante l'installazione di input.

## <a name="use-static-mac-addresses-with-failover-clustering"></a>Usare gli indirizzi MAC statici con il clustering di failover

Macchine virtuali Linux che verrà distribuite tramite clustering di failover deve essere configurate con un indirizzo statico media access control (MAC) per ogni scheda di rete virtuale. In alcune versioni di Linux, la configurazione di rete vadano persa dopo il failover perché un nuovo indirizzo MAC viene assegnato alla scheda di rete virtuale. Per evitare di perdere la configurazione di rete, assicurarsi che ogni scheda di rete virtuale abbia un indirizzo MAC statico. È possibile configurare l'indirizzo MAC modificando le impostazioni della macchina virtuale in Hyper-V Manager o gestione Cluster di Failover.

## <a name="use-hyper-v-specific-network-adapters-not-the-legacy-network-adapter"></a>Utilizzare schede di rete specifici di Hyper-V, non la scheda di rete legacy

Configurare e usare la scheda Ethernet virtuale, ovvero una scheda di rete specifici di Hyper-V con prestazioni migliorate. Se entrambe le schede di rete specifici di Hyper-V e legacy sono collegate a una macchina virtuale, i nomi di rete nell'output del **ifconfig - a** potrebbe visualizzare valori casuali, ad esempio **_tmp12000801310**. Per evitare questo problema, rimuovere tutte le schede di rete legacy quando si usano schede di rete specifici di Hyper-V in una macchina virtuale Linux.

## <a name="use-io-scheduler-noop-for-better-disk-io-performance"></a>Utilizzo i/o utilità di pianificazione NOOP per le prestazioni dei / o del disco migliorate

Il kernel Linux ha quattro utilità di pianificazione dei / o diverso per riordinare le richieste con algoritmi diversi. NOOP è una coda first in, First Out che passa la decisione pianificazione deve risultare dall'hypervisor. È consigliabile usare NOOP come utilità di pianificazione durante l'esecuzione di macchina virtuale Linux in Hyper-V. Per modificare l'utilità di pianificazione per un dispositivo specifico, nella configurazione del caricatore di avvio (/ etc/grub.conf, ad esempio), aggiungere **elevator = noop** ai parametri del kernel e quindi riavviare.

## <a name="numa"></a>NUMA

Le versioni del kernel Linux precedenti alla 2.6.37 non supportano NUMA in Hyper-V con dimensioni maggiori della VM. Questo problema influisce principalmente sulle distribuzioni precedenti usando l'upstream kernel, Red Hat 2.6.32 ed è stato risolto in Red Hat Enterprise Linux (RHEL) 6.6 (kernel-2.6.32-504). I sistemi che eseguono kernel personalizzati precedenti alla 2.6.37 o kernel basati su RHEL precedenti rispetto a 2.6.32-504 deve impostare il parametro di avvio `numa=off` nella riga di comando del kernel in RUB. Per altre informazioni, vedere [Red Hat KB 436883](https://access.redhat.com/solutions/436883).

## <a name="reserve-more-memory-for-kdump"></a>Riservare memoria aggiuntiva per kdump

Nel caso in cui il kernel di acquisizione dump finisce per avere un panic in fase di avvio, prenotare maggiore quantità di memoria per il kernel. Ad esempio, modificare il parametro **crashkernel = 384M-:128M** al **crashkernel = 384M-:256M** nel file di configurazione di grub Ubuntu.

## <a name="shrinking-vhdx-or-expanding-vhd-and-vhdx-files-can-result-in-erroneous-gpt-partition-tables"></a>Nelle tabelle di partizione GPT errate può causare VHDX di riduzione o espansione di file VHD e VHDX

Hyper-V consente la compattazione dei file di disco virtuale (VHDX) senza tener conto di qualsiasi partizione, volume o strutture di dati file system che potrebbero esistere sul disco. Se il file VHDX viene compattato fino a qui entra nel disco VHDX a fondo prima della fine di una partizione, i dati possono andare persi, che partizioni possono diventare dati danneggiati o non validi possono essere restituiti quando viene letta la partizione.

Dopo il ridimensionamento di un VHD o VHDX, gli amministratori devono usare un'utilità come fdisk o separate per aggiornare la partizione, volume e struttura del file system in modo da riflettere la modifica delle dimensioni del disco. Riduzione o espansione di un VHD o VHDX che dispone di una tabella di partizione GUID (GPT) verrà generato un avviso quando uno strumento di gestione partizione consente di controllare il layout della partizione e all'amministratore verrà richiesto di risolvere le intestazioni GPT prima e secondarie. Questo passaggio manuale è possibile eseguire senza perdita di dati.

## <a name="see-also"></a>Vedere anche

* [Linux e FreeBSD macchine virtuali supportate per Hyper-V in Windows](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)

* [Le procedure consigliate per l'esecuzione di FreeBSD in Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)

* [Distribuire un Cluster Hyper-V](https://technet.microsoft.com/library/jj863389.aspx)

* [Creare immagini di Linux per Azure](https://docs.microsoft.com/azure/virtual-machines/linux/create-upload-generic)

* [Ottimizzare la VM Linux in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/optimization)
