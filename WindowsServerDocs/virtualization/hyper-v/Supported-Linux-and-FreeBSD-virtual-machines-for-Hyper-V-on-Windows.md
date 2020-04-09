---
title: Linux e FreeBSD macchine virtuali supportate per Hyper-V in Windows
description: Elenca i servizi di integrazione Linux e le funzionalità incluse in ogni versione
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: 990ff94a-30fb-434b-b4a2-3804a5245ba6
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: dff7a0f829f11a92f2c87305da806b9be43f42fe
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855944"
---
# <a name="supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows"></a>Linux e FreeBSD macchine virtuali supportate per Hyper-V in Windows

>Si applica a: Windows Server 2019, Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7,1, Windows 7

Hyper-V supporta dispositivi emulati e specifici di Hyper-V per le macchine virtuali Linux e FreeBSD. Quando si esegue con dispositivi emulati, è necessario installare alcun software aggiuntivo. Dispositivi emulati tuttavia non forniscono prestazioni elevate e non possono sfruttare l'infrastruttura di gestione di macchina virtuale avanzato che offre la tecnologia Hyper-V. Per poter utilizzare completamente tutti i vantaggi che offre la tecnologia Hyper-V, si consiglia di utilizzare i dispositivi specifici di Hyper-V per Linux e FreeBSD. L'insieme di driver che sono necessari per eseguire i dispositivi specifici di Hyper-V sono noti come Linux Integration Services (LIS) o FreeBSD Integration Services (BIS).

LIS è stato aggiunto il kernel Linux e viene aggiornato per le nuove versioni. Ma le distribuzioni di Linux basate su kernel precedenti potrebbero non disporre di miglioramenti o correzioni più recenti. Microsoft fornisce un download contenente i driver LIS installabili per alcune installazioni di Linux su questi kernel precedenti. Poiché i fornitori di distribuzione includono le versioni di Linux Integration Services, è consigliabile installare l'ultima versione scaricabile di LIS, se applicabile, per l'installazione.

Per altre distribuzioni di Linux LIS modifiche regolarmente sono integrate nel kernel del sistema operativo e applicazioni quindi non è necessaria alcuna installazione o un download separato.

Per le versioni precedenti di FreeBSD (prima 10.0), Microsoft fornisce porte che contengono i driver installabili BIS e daemon corrispondente per le macchine virtuali FreeBSD. Per le versioni più recenti FreeBSD BIS incorporato nel sistema operativo FreeBSD e alcun download separato o l'installazione non è necessario ad eccezione di un download di porte di coppia chiave-VALORE che è necessaria per FreeBSD 10.0.

> [!TIP]
> - Scaricare [Windows Server 2019](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019) da Evaluation Center.

L'obiettivo di questo contenuto è fornire informazioni che agevolano la distribuzione di Linux o FreeBSD in Hyper-V. Dettagli specifici includono:

* Versioni di FreeBSD che richiedono il download e installazione dei driver LIS o BIS o distribuzioni di Linux.

* Versioni di FreeBSD che contengono i driver LIS o BIS incorporati o distribuzioni di Linux.

* Mappe di distribuzione di funzionalità che indicano le caratteristiche di maggior parte delle distribuzioni di Linux o rilascia FreeBSD.

* Problemi noti e soluzioni alternative per ogni distribuzione o rilascio.

* Descrizione delle funzionalità per ogni funzionalità LIS o BIS.

**Si vuole suggerire suggerimenti sulle funzionalità e sulle funzionalità?** È possibile avremmo potuto fare di meglio? È possibile utilizzare il sito [Windows Server User Voice](https://windowsserver.uservoice.com/forums/295062-linux-support) per suggerire nuove funzionalità e funzionalità per le macchine virtuali Linux e FreeBSD in Hyper-V e per vedere quali altri utenti stanno dicendo.

## <a name="in-this-section"></a>Contenuto della sezione

* [Macchine virtuali CentOS e Red Hat Enterprise Linux supportate in Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali Debian supportate in Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali Oracle Linux supportate in Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali SUSE supportate in Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali Ubuntu supportate in Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Macchine virtuali FreeBSD supportate in Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Descrizioni delle funzionalità per le macchine virtuali Linux e FreeBSD in Hyper-V](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Procedure consigliate per l'esecuzione di Linux in Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Procedure consigliate per l'esecuzione di FreeBSD in Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)
