---
title: CentOS è supportato e Red Hat Enterprise Linux macchine virtuali in Hyper-V
description: Elenca le versioni di Linux integration services per supportate CentOS e Red Hat Enterprise distribuzioni
ms.prod: windows-server-threshold
ms.service: na
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4bf8783d-dee5-4b3e-8cce-2b11b117c189
author: danihalfin
ms.author: daniha
ms.date: 12/20/2017
ms.openlocfilehash: 6bf15e5bfff4b875c4debd3c682bbdccd81a7bb6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869432"
---
# <a name="supported-centos-and-red-hat-enterprise-linux-virtual-machines-on-hyper-v"></a>CentOS è supportato e Red Hat Enterprise Linux macchine virtuali in Hyper-V

>Si applica a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

Le mappe di distribuzione di funzionalità seguenti indicano le funzionalità che sono presenti nelle versioni incorporate e scaricabili di Linux Integration Services. Dopo le tabelle sono elencate i problemi noti e soluzioni alternative per ogni distribuzione.

Il driver di Red Hat Enterprise Linux Integration Services incorporati per Hyper-V (disponibile dal Red Hat Enterprise Linux 6.4) sono sufficienti per gli ospiti di Red Hat Enterprise Linux per l'esecuzione con i dispositivi sintetici ad alte prestazioni negli host Hyper-V. Questi driver predefiniti sono certificati da Red Hat per questo utilizzo. Certified configurazioni possono essere visualizzate in questa pagina web di Red Hat: [Red Hat certificazione catalogo](https://access.redhat.com/ecosystem/search/#/ecosystem/Red%20Hat%20Enterprise%20Linux?sort=sortTitle%20asc&vendors=Microsoft&category=Server). Non è necessario scaricare e installare i pacchetti di Linux Integration Services da Microsoft Download Center e ciò potrebbe limitare così il supporto di Red Hat come descritto nell'articolo della Knowledge base di Red Hat 1067: [Red Hat Knowledgebase 1067](https://access.redhat.com/articles/1067).

A causa di potenziali conflitti tra il supporto LIS predefinito e il supporto LIS scaricabile quando si aggiorna il kernel, disabilitare gli aggiornamenti automatici, disinstallare i pacchetti scaricabili LIS, aggiornare il kernel, riavviare il computer e quindi installare LIS più recenti di rilascio e riavviare nuovamente.

>[!NOTE]
>Informazioni di certificazione ufficiali di Red Hat Enterprise Linux sono disponibile tramite il [Red Hat Customer Portal](https://access.redhat.com/ecosystem/search/#/category/Server?sort=sortTitle%20asc&query=windows%20server&ecosystem=Red%20Hat%20Enterprise%20Linux).

In questa sezione:

* [Serie RHEL/CentOS 7.x](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md#BKMK_7x)

* [Serie RHEL/CentOS 6.x](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md#BKMK_6x)

* [Serie RHEL/CentOS 5. x](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md#BKMK_5x)

* [Note](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md#BKMK_notes)

## <a name="table-legend"></a>Legenda tabella

* **Incorporata** -LIS sono inclusi come parte di questa distribuzione Linux. I numeri di versione del modulo del kernel per incorporato LIS (come illustrato da **lsmod**, ad esempio) sono diversi dal numero di versione del pacchetto di download LIS fornita da Microsoft. Una mancata corrispondenza non indicano che incorporato LIS è scaduto.

* & #10004; -Funzionalità disponibili

* (*vuoto*)-funzionalità non disponibile

## <a name="BKMK_7x"></a>Serie RHEL/CentOS 7.x

Questa serie dispone solo di kernel a 64 bit.

|**Funzionalità**|**Versione di Windows Server**|**7.5-7.6**|**7.3-7.4**|**7.0-7.2**|**7.5-7.6**|**7.4**|**7.3**|**7.2**|**7.1**|**7.0**|
|-|-|-|-|-|-|-|-|-|-|-|
|**Disponibilità**||[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|Incorporata|Incorporata|Incorporata|Incorporata|Incorporata|Incorporata||
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Ora esatta di Windows Server 2016|2019, 2016|&#10004;|&#10004;|||||||
|**[Funzionalità di rete](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Networking)**|||||||
|Frame jumbo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|La codifica VLAN e trunking|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Migrazione in tempo reale|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Indirizzo IP statico Injection|2019, 2016, 2012 R2, 2012|& #10004; Nota 2|& #10004; Nota 2|& #10004; Nota 2|& #10004; Nota 2|& #10004; Nota 2|& #10004; Nota 2|& #10004; Nota 2|& #10004; Nota 2|& #10004; Nota 2|
|RSS virtuale|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|Segmentazione di TCP e gli offload Checksum|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|SR-IOV|2019, 2016|&#10004;|&#10004;||&#10004;|&#10004;|&#10004;|||
|**[Archiviazione](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Storage)**||||
|Ridimensionamento di VHDX|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Fibre Channel virtuale|2019, 2016, 2012 R2|& #10004; Nota 3|& #10004; Nota 3|& #10004; Nota 3|& #10004; Nota 3|& #10004; Nota 3|& #10004; Nota 3|& #10004; Nota 3|& #10004; Nota 3|& #10004; Nota 3|
|Backup della macchina virtuale in tempo reale|2019, 2016, 2012 R2|& #10004; Nota 5|& #10004; Nota 5|& #10004; Nota 5|& #10004; Nota 4,5|& #10004; Nota 4, 5|& #10004; Nota 4, 5|& #10004; Nota 4, 5|& #10004; Nota 4, 5|& #10004; Nota 4, 5|
|Supporto per TRIM|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|||
|WWN SCSI|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|||||
|**[Memoria](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Memory)**||||
|Supporto PAE Kernel|2019, 2016, 2012 R2, 2012, 2008 R2|N/D|N/D|N/D|N/D|N/D|N/D|N/D|N/D|N/D|
|Configurazione di gap MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Memoria dinamica - aggiunta a caldo|2019, 2016, 2012 R2, 2012|& #10004; Si noti 8, 9, 10|& #10004; Si noti 8, 9, 10|& #10004; Si noti 8, 9, 10|& #10004; Nota 9, 10|& #10004; Nota 9, 10|& #10004; Nota 9, 10|& #10004; Nota 9, 10|& #10004; Nota 9, 10|& #10004; Si noti 8, 9, 10|
|Memoria dinamica - Ballooning|2019, 2016, 2012 R2, 2012|& #10004; Si noti 8, 9, 10|& #10004; Si noti 8, 9, 10|& #10004; Si noti 8, 9, 10|& #10004; Nota 9, 10|& #10004; Nota 9, 10|& #10004; Nota 9, 10|& #10004; Nota 9, 10|& #10004; Nota 9, 10|& #10004; Si noti 8, 9, 10|
|Ridimensionamento della memoria di runtime|2019, 2016|&#10004;|&#10004;|&#10004;|||||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Video)**||||
|Dispositivo video specifico Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|**[Varie](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Misc)**||||
|Coppia chiave-valore|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Interrupt non mascherabile|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Copiare i file dall'host al guest|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|comando lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|||||||
|Socket di Hyper-V|2019, 2016|&#10004;|&#10004;|&#10004;|||||||
|Pass-through/DDA PCI|2019, 2016|&#10004;|&#10004;||&#10004;|&#10004;|&#10004;||||
|**[Macchine virtuali di generazione 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_gen2)**|||||
|Avvio UEFI|2019, 2016, 2012 R2|& #10004; Nota 14|& #10004; Nota 14|& #10004; Nota 14|& #10004; Nota 14|& #10004; Nota 14|& #10004; Nota 14|& #10004; Nota 14|& #10004; Nota 14|& #10004; Nota 14|
|Avvio protetto|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|

## <a name="BKMK_6x"></a>Serie RHEL/CentOS 6.x

Il kernel a 32 bit per questa serie è PAE attivata. Non esiste alcun supporto predefinito di LIS per RHEL/CentOS 6.0-6.3.

|**Funzionalità**|**Versione di Windows Server**|**6.4-6.10**|**6.0-6.3**|**6.10, 6.9, 6.8**|**6.6, 6.7**|**6.5**|**6.4**|
|-|-|-|-|-|-|-|-|
|**Disponibilità**||[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|Incorporata|Incorporata|Incorporata|Incorporata|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Ora esatta di Windows Server 2016|2019, 2016||||||
|**[Funzionalità di rete](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Networking)**|
|Frame jumbo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|La codifica VLAN e trunking|2019, 2016, 2012 R2, 2012, 2008 R2|& #10004; Nota 1|& #10004; Nota 1|& #10004; Nota 1|& #10004; Nota 1|& #10004; Nota 1|& #10004; Nota 1|
|Migrazione in tempo reale|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Indirizzo IP statico Injection|2019, 2016, 2012 R2, 2012|& #10004; Nota 2|& #10004; Nota 2|& #10004; Nota 2|& #10004; Nota 2|& #10004; Nota 2|& #10004; Nota 2|
|RSS virtuale|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|Segmentazione di TCP e gli offload Checksum|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|SR-IOV|2019, 2016|||||||
|**[Archiviazione](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Storage)**|
|Ridimensionamento di VHDX|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|Fibre Channel virtuale|2019, 2016, 2012 R2|& #10004; Nota 3|& #10004; Nota 3|& #10004; Nota 3|& #10004; Nota 3|& #10004; Nota 3||
|Backup della macchina virtuale in tempo reale|2019, 2016, 2012 R2|& #10004; Nota 5|& #10004; Nota 5|& #10004; Nota 4, 5|& #10004; Nota 4, 5|& #10004; Nota 4, 5, 6|& #10004; Nota 4, 5, 6|
|Supporto per TRIM|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;||||
|WWN SCSI|2019, 2016, 2012 R2|&#10004;|&#10004;|||||
|**[Memoria](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Memory)**|
|Supporto PAE Kernel|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Configurazione di gap MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Memoria dinamica - aggiunta a caldo|2019, 2016, 2012 R2, 2012|& #10004; Si noti 7, 9, 10|& #10004; Si noti 7, 9, 10|& #10004; Si noti 7, 9, 10|& #10004; Si noti 7, 8, 9, 10|& #10004; Si noti 7, 8, 9, 10||
|Memoria dinamica - Ballooning|2019, 2016, 2012 R2, 2012|& #10004; Si noti 7, 9, 10|& #10004; Si noti 7, 9, 10|& #10004; Si noti 7, 9, 10|& #10004; Si noti 7, 9, 10|& #10004; Si noti 7, 9, 10|& #10004; Si noti 7, 9, 10, 11|
|Ridimensionamento della memoria di runtime|2019, 2016|||||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Video)**|
|Dispositivo video specifico Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|**[Varie](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Misc)**|
|Coppia chiave-valore|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|& #10004; Nota 12|& #10004; Nota 12|& #10004; Nota 12, 13|& #10004; Nota 12, 13|
|Interrupt non mascherabile|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Copiare i file dall'host al guest|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|comando lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;||||||
|Socket di Hyper-V|2019, 2016|&#10004;|&#10004;|||||
|Pass-through/DDA PCI|2019, 2016|||||||
|**[Macchine virtuali di generazione 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_gen2)**|
|Avvio UEFI|2012 R2|||||||
||2019, 2016|& #10004; Nota 14|& #10004; Nota 14|& #10004; Nota 14||||
|Avvio protetto|2019, 2016||||||

## <a name="BKMK_5x"></a>Serie RHEL/CentOS 5. x

Questa serie è supportato a 32 bit PAE kernel disponibile. Non esiste alcun supporto predefinito di LIS per RHEL/CentOS prima 5.9.

|**Funzionalità**|**Versione di Windows Server**|5.2 -5.11|**5.2-5.11**|**5.9 - 5.11**|
|-|-|-|-|-|
|**Disponibilità**||[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|[LIS 4.1](https://www.microsoft.com/download/details.aspx?id=51612)|Incorporata|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_core)**|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Ora esatta di Windows Server 2016|2019, 2016||||
|**[Funzionalità di rete](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Networking)**|
|Frame jumbo|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|La codifica VLAN e trunking|2019, 2016, 2012 R2, 2012, 2008 R2|& #10004; Nota 1|& #10004; Nota 1|& #10004; Nota 1|
|Migrazione in tempo reale|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Indirizzo IP statico Injection|2019, 2016, 2012 R2, 2012|& #10004; Nota 2|& #10004; Nota 2|& #10004; Nota 2|
|RSS virtuale|2019, 2016, 2012 R2||||
|Segmentazione di TCP e gli offload Checksum|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;||
|SR-IOV|2019, 2016||||||
|**[Archiviazione](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Storage)**|
|Ridimensionamento di VHDX|2019, 2016, 2012 R2|&#10004;|&#10004;||
|Fibre Channel virtuale|2019, 2016, 2012 R2|& #10004; Nota 3|& #10004; Nota 3||
|Backup della macchina virtuale in tempo reale|2019, 2016, 2012 R2|&#10004; Note 5, 15|& #10004; Nota 5|& #10004; Nota 4, 5, 6|
|Supporto per TRIM|2019, 2016, 2012 R2||||
|WWN SCSI|2019, 2016, 2012 R2||||
|**[Memoria](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Memory)**|
|Supporto PAE Kernel|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;|&#10004;|
|Configurazione di gap MMIO|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|
|Memoria dinamica - aggiunta a caldo|2019, 2016, 2012 R2, 2012||||
|Memoria dinamica - Ballooning|2019, 2016, 2012 R2, 2012|& #10004; Si noti 7, 9, 10, 11|& #10004; Si noti 7, 9, 10, 11||
|Ridimensionamento della memoria di runtime|2019, 2016||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Video)**|
|Dispositivo video specifico Hyper-V|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;||
|**[Varie](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Misc)**|
|Coppia chiave-valore|2019, 2016, 2012 R2, 2012, 2008 R2|&#10004;|&#10004;||
|Interrupt non mascherabile|2019, 2016, 2012 R2|&#10004;|&#10004;|&#10004;|
|Copiare i file dall'host al guest|2019, 2016, 2012 R2|&#10004;|&#10004;||
|comando lsvmbus|2019, 2016, 2012 R2, 2012, 2008 R2||||
|Socket di Hyper-V|2019, 2016||||
|Pass-through/DDA PCI|2019, 2016||||||
|**[Macchine virtuali di generazione 2](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_gen2)**|
|Avvio UEFI|2019, 2016, 2012 R2||||
|Avvio protetto|2019, 2016||||

## <a name="BKMK_notes"></a>Note

1. Per questa versione RHEL/CentOS, VLAN tag works ma non trunking VLAN.

2. Inserimento di IP statico potrebbe non funzionare se Gestione di rete è stata configurata per una scheda di rete sintetiche sulla macchina virtuale. Per garantire il corretto funzionamento di IP statico injection assicurarsi che l'amministratore di sistema è spento completamente o è stato disattivato per una scheda di rete specifico tramite il file ifcfg-ethX.

3. In Windows Server 2012 R2 durante l'utilizzo di dispositivi di virtuale fibre channel, assicurarsi che il numero di unità logica (LUN 0) 0 è stato popolato. Se non è stato popolato LUN 0, una macchina virtuale potrebbe non essere in grado di installare dispositivi di fibre channel in modo nativo.

4. Per incorporato LIS, deve essere installato il pacchetto "Hyper-v-daemon" per questa funzionalità.

5. Se sono presenti handle di file aperti durante un'operazione di backup di macchina virtuale, quindi in alcuni casi estremi, i dischi rigidi virtuali backup potrebbero essere necessario essere sottoposto a un controllo di coerenza di sistema di file (fsck) su ripristino. Operazioni di backup Live possano un esito negativo se la macchina virtuale dispone di un dispositivo iSCSI collegati o DAS (noto anche come disco pass-through).

6. Durante il download di Linux Integration Services preferito, il supporto di backup per 5.9 RHEL/CentOS live - 5.11/6.4/6.5 è disponibile anche tramite [Essentials Backup Hyper-V per Linux](https://github.com/LIS/backupessentials/tree/1.0).

7. Supporto della memoria dinamica è disponibile solo nelle macchine virtuali a 64 bit.

8. Aggiunta a caldo Supporto non è abilitato per impostazione predefinita in questa distribuzione. Per abilitare il supporto aggiunto a caldo che è necessario aggiungere una regola di udev in /etc/udev/rules.d/ come indicato di seguito:

   1. Creare un file **/etc/udev/rules.d/100-balloon.rules**. È possibile utilizzare qualsiasi altro nome per il file desiderato.

   2. Aggiungere il contenuto seguente al file: `SUBSYSTEM=="memory", ACTION=="add", ATTR{state}="online"`

   3. Riavviare il sistema per consentire l'aggiunta a caldo.

   Durante il download di Linux Integration Services crea questa regola su installazione, la regola è rimossi anche quando LIS viene disinstallato, la regola deve essere ricreato se è necessaria una quantità di memoria dinamica dopo la disinstallazione.

9. Operazioni di memoria dinamica possono non riuscire se il sistema operativo guest è troppo memoria insufficiente. Di seguito sono le procedure consigliate:

   * Memoria di avvio e di memoria minimo deve essere uguale o maggiore rispetto alla quantità di memoria in cui si consiglia il fornitore.

   * Le applicazioni che tendono a consumare l'intera memoria disponibile in un sistema sono limitate all'utilizzo di fino a 80% della RAM disponibile.

10. Se si utilizza la memoria dinamica in un sistema operativo Windows Server 2012 R2 o Windows Server 2016, specificare **memoria di avvio**, **memoria minima**, e **quantità massima di memoria** parametri in multipli di 128 megabyte (MB). In caso contrario, può comportare errori a caldo e non sarà possibile visualizzare qualsiasi memoria aumenta in un sistema operativo guest.

11. Alcune distribuzioni, incluse quelle in uso LIS 4.0 e 4.1, solo supportano Ballooning e non forniscono supporto per l'aggiunta a caldo. In questo caso, è possibile utilizzare la funzionalità memoria dinamica impostando il parametro di memoria di avvio su un valore che corrisponde al parametro di memoria massimo. Questo consente di tutta la memoria necessaria viene aggiunta a caldo alla macchina virtuale in fase di avvio e successivamente in base ai requisiti di memoria dell'host Hyper-V può liberamente allocare o deallocare la memoria dal guest utilizzando Ballooning. Configurare **memoria di avvio** e **memoria minima** uguale o superiore sul valore consigliato per la distribuzione.

12. Per abilitare l'infrastruttura (coppia chiave-VALORE) di coppia chiave/valore, installare il pacchetto rpm hypervkvpd o Hyper-v-daemon dall'ISO di RHEL. In alternativa il pacchetto può essere installato direttamente dal repository RHEL.

13. L'infrastruttura (coppia chiave-VALORE) di coppia chiave/valore potrebbe non funzionare correttamente senza un aggiornamento software Linux. Contattare il fornitore di distribuzione per ottenere l'aggiornamento software nel caso in cui noterete problemi con questa funzionalità.

14. In Windows Server 2012 R2 generazione 2 macchine virtuali presentano avvio protetto abilitato per impostazione predefinita e alcune macchine virtuali di Linux non verranno avviate se l'opzione di avvio protetto è disabilitata. È possibile disabilitare l'avvio protetto nel **Firmware** sezione delle impostazioni per la macchina virtuale in **di gestione di Hyper-V** o è possibile disabilitarlo con Powershell:

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```

   Il download di Linux Integration Services può essere applicato alle macchine virtuali di 2 generazione esistente ma non applicare la funzionalità di generazione 2.

15. Red Hat Enterprise Linux o CentOS 5.2, 5.3 e 5.4 il blocco di funzionalità del file System non è disponibile, in modo Live Backup della macchina virtuale non è inoltre disponibile.

Vedere anche

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [Macchine virtuali Debian supportate in Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali Oracle Linux supportate in Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali SUSE supportate in Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali Ubuntu supportate in Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali FreeBSD supportate in Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Forniscono le descrizioni per le macchine virtuali Linux e FreeBSD in Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Le procedure consigliate per l'esecuzione di Linux in Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Certificazione Hardware di Red Hat](https://hardware.redhat.com/&quicksearch=Microsoft)
