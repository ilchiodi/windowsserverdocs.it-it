---
title: Compatibilità con la funzionalità di Hyper-V per la generazione e guest
description: Elenca le generazioni e sistemi operativi compatibili con le funzionalità principali di Hyper-V
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 81c1f32d-7814-4992-8a66-dd4b77c939b4
author: KBDAzure
ms.author: kathydav
ms.date: 12/05/2016
ms.openlocfilehash: 5950a75da4569979794a5848bd41ab349dc34676
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812660"
---
# <a name="hyper-v-feature-compatibility-by-generation-and-guest"></a>Compatibilità con la funzionalità di Hyper-V per la generazione e guest

>Si applica a: Windows Server 2016
  
Le tabelle in questo articolo mostrano le generazioni e sistemi operativi compatibili con alcune delle funzionalità di Hyper-V, raggruppate per categorie. In generale, si otterrà la migliore disponibilità di funzionalità con una macchina virtuale di generazione 2 che esegue il sistema operativo più recente.  
  
Tenere presente che alcune funzionalità si basano su hardware o un'altra infrastruttura. Per dettagli sull'hardware, vedere [requisiti di sistema per Hyper-V in Windows Server 2016](System-requirements-for-Hyper-V-on-Windows.md). In alcuni casi, una funzionalità è utilizzabile con qualsiasi sistema operativo guest supportato. Per informazioni dettagliate in cui sono supportati i sistemi operativi, vedere:  
  
* [Macchine virtuali Linux e FreeBSD supportate](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  
* [Sistemi operativi guest di Windows supportati](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md)  
  
## <a name="availability-and-backup"></a>Backup e disponibilità  
  
Funzionalità  | Generazione | Sistema operativo guest  
------------- | ------------- | -----------  
Checkpoint | 1 e 2 | Qualsiasi guest supportati  
Clustering dei guest | 1 e 2 | Utenti guest che eseguono applicazioni compatibili con i cluster e che sia installato software di destinazione iSCSI  
Replica | 1 e 2 | Qualsiasi guest supportati  
Controller di dominio | 1 e 2 | Qualsiasi supportato guest Windows Server usando solo i checkpoint di produzione. Vedere [sistemi operativi guest supportati Windows Server](https://docs.microsoft.com/windows-server/virtualization/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows#supported-windows-server-guest-operating-systems)   
  
## <a name="compute"></a>Calcolo  
  
Funzionalità  | Generazione | Sistema operativo guest  
------------- | ------------- | -----------  
Memoria dinamica | 1 e 2 | Versioni specifiche di guest supportati. Vedere [Panoramica della memoria dinamica di Hyper-V](https://technet.microsoft.com/library/hh831766.aspx) per le versioni precedenti a Windows 10 e Windows Server 2016.  
Hot aggiunta/rimozione di memoria | 1 e 2 | Windows Server 2016, Windows 10  
NUMA virtuale | 1 e 2 | Qualsiasi guest supportati  
  
## <a name="development-and-test"></a>Sviluppo e test  
Funzionalità  | Generazione | Sistema operativo guest  
------------- | ------------- | -----------  
Porte COM/seriali | 1 e 2 <br>**Nota:** Per la generazione 2, usare Windows PowerShell per configurare. Per informazioni dettagliate, vedere [aggiungere una porta COM per il debug del kernel](./plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md#add-a-com-port-for-kernel-debugging). | Qualsiasi guest supportati  
  
## <a name="mobility"></a>Mobilità  
  
Funzionalità  | Generazione | Sistema operativo guest  
------------- | ------------- | -----------  
Migrazione in tempo reale  | 1 e 2 |  Qualsiasi guest supportati  
Importazione/esportazione | 1 e 2 |  Qualsiasi guest supportati  
  
## <a name="networking"></a>Rete  
  
Funzionalità  | Generazione | Sistema operativo guest  
------------- | ------------- | -----------  
Hot aggiunta/rimozione della scheda di rete virtuale | 2 | Qualsiasi guest supportati  
Scheda di rete legacy virtuale | 1 | Qualsiasi guest supportati  
Virtualizzazione di input/output radice (SR-IOV) | 1 e 2 | utenti guest di Windows a 64 bit, a partire da Windows Server 2012 e Windows 8.  
Coda di più macchine virtuali (VMMQ) | 1 e 2  | Qualsiasi guest supportati  
  
## <a name="remote-connection-experience"></a>Esperienza di connessione remota  
  
Funzionalità  | Generazione | Sistema operativo guest  
------------- | ------------- | -----------  
Assegnazione di dispositivi discreti (DDA) | 1 e 2 | Windows Server 2016, Windows Server 2012 R2 solo con 3133690 aggiornamento installato, Windows 10 <br> **Nota:** Per informazioni dettagliate sull'aggiornamento 3133690, vedere [ciò](https://support.microsoft.com/kb/3133690) articolo del supporto tecnico.  
Modalità sessione avanzata | 1 e 2 | Windows Server 2016, Windows Server 2012 R2, Windows 10 e Windows 8.1, con Servizi Desktop remoto abilitato <br>**Nota**: Si potrebbe essere necessario anche configurare l'host. Per informazioni dettagliate, vedere [utilizzare le risorse locali nella macchina virtuale Hyper-V con VMConnect](./learn-more/Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md).  
RemoteFx | 1 e 2 | Generazione 1 su versioni di Windows a 32 bit e 64 bit a partire da Windows 8. <br> Generazione 2 su versioni a 64-bit Windows 10  
  
## <a name="security"></a>Sicurezza  
  
Funzionalità  | Generazione | Sistema operativo guest  
------------- | ------------- | -----------  
Avvio protetto | 2 | **Linux**: Ubuntu 14.04 e versioni successive, SUSE Linux Enterprise Server 12 e successive, Red Hat Enterprise Linux 7.0 e versioni successive e CentOS 7.0 e versioni successive<br>**Windows**: Tutte le versioni supportate che è possono eseguire in una macchina virtuale di generazione 2  
Macchine virtuali schermate | 2 | **Windows**: Tutte le versioni supportate che è possono eseguire in una macchina virtuale di generazione 2  
  
## <a name="storage"></a>Archiviazione  
  
Funzionalità  | Generazione | Sistema operativo guest  
------------- | ------------- | -----------  
Dischi rigidi virtuali condivisi (VHDX solo) | 1 e 2  | Windows Server 2016, Windows Server 2012 R2, Windows Server 2012  
SMB3 | 1 e 2 | Tutto ciò che supportano SMB3  
Spazi di archiviazione diretti | 2 | Windows Server 2016  
Fibre Channel virtuale | 1 e 2 | Windows Server 2016, Windows Server 2012 R2, Windows Server 2012  
Formato VHDX | 1 e 2 | Qualsiasi guest supportati   
  
  
  
  
    


