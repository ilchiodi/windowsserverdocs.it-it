---
title: Panoramica della migrazione in tempo reale
description: Offre una panoramica delle funzionalità di migrazione in tempo reale in Windows Server 2016.
ms.prod: windows-server-threshold
ms.service: na
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5cc875ab-05c4-439e-b27d-6bfc77054660
author: johncslack
ms.author: joslack
ms.date: 06/27/2017
ms.openlocfilehash: 2bbe897ffb8b200a72fac5a662e518d4a4be1131
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887852"
---
# <a name="live-migration-overview"></a>Panoramica della migrazione in tempo reale

Migrazione in tempo reale è una funzionalità di Hyper-V in Windows Server.  Consente di spostare in modo trasparente macchine virtuali in esecuzione da un host Hyper-V a altro senza tempi di inattività percepito.  Il vantaggio principale della migrazione in tempo reale è la flessibilità; Macchine virtuali in esecuzione non sono associate a un singolo computer host.  In questo modo le azioni, ad esempio lo svuotamento di un host specifico di macchine virtuali prima della rimozione delle autorizzazioni o l'aggiornamento.  Se usato con il Clustering di Failover di Windows, migrazione in tempo reale consente la creazione della disponibilità elevata e l'errore sistemi tolleranza d'errore. 

## <a name="related-technologies-and-documentation"></a>Documentazione e le tecnologie correlate

Migrazione in tempo reale viene spesso usata in combinazione con alcune tecnologie correlate, come il Clustering di Failover e System Center Virtual Machine Manager.  Se si usa Live Migration tramite queste tecnologie, ecco i puntatori alla relativa documentazione più recente:
* [Clustering di failover](../../../failover-clustering/failover-clustering-overview.md) (Windows Server 2016) 
* [System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/) (System Center 2016) 

Se si usano le versioni precedenti di Windows Server, oppure occorre informazioni dettagliate sulle funzionalità introdotte in versioni precedenti di Windows Server, ecco i puntatori alla documentazione cronologico: 
* [Migrazione in tempo reale](https://technet.microsoft.com/library/ee815293(v=ws.10).aspx) (Windows Server 2008 R2)  
* [Live Migration](https://technet.microsoft.com/library/hh831435(v=ws.11).aspx) (Windows Server 2012 R2) 
* [Failover Clustering](https://technet.microsoft.com/library/hh831579(v=ws.11).aspx) (Windows Server 2012 R2)
* [Clustering di failover](https://technet.microsoft.com/library/ff182338(v=ws.10).aspx) (Windows Server 2008 R2)
* [System Center Virtual Machine Manager](https://technet.microsoft.com/library/gg610610.aspx) (System Center 2012 R2)
* [System Center Virtual Machine Manager](https://technet.microsoft.com/library/cc917964.aspx) (System Center 2008 R2)

## <a name="live-migration-in-windows-server-2016"></a>Migrazione in tempo reale in Windows Server 2016

In Windows Server 2016, esistono meno restrizioni sulla distribuzione di migrazione in tempo reale.  Ora funziona senza Clustering di Failover.  Altre funzionalità rimane invariata rispetto alle versioni precedenti di migrazione in tempo reale.  Per altre informazioni su come configurare e utilizzare live migration senza Clustering di Failover: 
* [Configurare gli host per live migration senza Clustering di Failover](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)
* [Utilizzare live migration senza Clustering di Failover per spostare una macchina virtuale](use-live-migration-without-failover-clustering-to-move-a-virtual-machine.md)