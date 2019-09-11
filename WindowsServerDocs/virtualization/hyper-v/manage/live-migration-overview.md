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
ms.openlocfilehash: 2842435f1bdb0aeb82bbcf1ae02be66e242dc9eb
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872048"
---
# <a name="live-migration-overview"></a>Panoramica della migrazione in tempo reale

Live Migration è una funzionalità di Hyper-V in Windows Server.  Consente di spostare in modo trasparente le macchine virtuali in esecuzione da un host Hyper-V a un altro senza tempi di inattività.  Il vantaggio principale della migrazione in tempo reale è la flessibilità. le macchine virtuali in esecuzione non sono collegate a un singolo computer host.  Ciò consente di eseguire azioni come lo svuotamento di un host specifico di macchine virtuali prima di rimuovere le autorizzazioni o di aggiornarlo.  Una volta abbinato a clustering di failover di Windows, Live Migration consente la creazione di sistemi a disponibilità elevata e a tolleranza di errore. 

## <a name="related-technologies-and-documentation"></a>Documentazione e tecnologie correlate

La migrazione in tempo reale viene spesso utilizzata in combinazione con alcune tecnologie correlate come il clustering di failover e System Center Virtual Machine Manager.  Se si usa Live Migration tramite queste tecnologie, di seguito sono riportati i puntatori alla documentazione più recente:
* [Clustering di failover](../../../failover-clustering/failover-clustering-overview.md) (Windows Server 2016) 
* [System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/) (System Center 2016) 

Se si usano versioni precedenti di Windows Server o se sono necessari dettagli sulle funzionalità introdotte nelle versioni precedenti di Windows Server, di seguito sono riportati i puntatori alla documentazione cronologica: 
* [Live Migration](https://technet.microsoft.com/library/ee815293(v=ws.10).aspx) (Windows Server 2008 R2)  
* [Live Migration](https://technet.microsoft.com/library/hh831435(v=ws.11).aspx) (Windows Server 2012 R2) 
* [Clustering di failover](https://technet.microsoft.com/library/hh831579(v=ws.11).aspx) (Windows Server 2012 R2)
* [Clustering di failover](https://technet.microsoft.com/library/ff182338(v=ws.10).aspx) (Windows Server 2008 R2)
* [System Center Virtual Machine Manager](https://technet.microsoft.com/library/gg610610.aspx) (System Center 2012 R2)
* [System Center Virtual Machine Manager](https://technet.microsoft.com/library/cc917964.aspx) (System Center 2008 R2)

## <a name="live-migration-in-windows-server-2016"></a>Live Migration in Windows Server 2016

In Windows Server 2016, la distribuzione della migrazione in tempo reale presenta un minor numero di restrizioni.  Ora funziona senza clustering di failover.  Altre funzionalità rimangono invariate rispetto alle versioni precedenti di Live Migration.  Per ulteriori informazioni sulla configurazione e sull'utilizzo della migrazione in tempo reale senza clustering di failover: 
* [Configurare gli host per la migrazione in tempo reale senza clustering di failover](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)
* [Usare la migrazione in tempo reale senza clustering di failover per spostare una macchina virtuale](use-live-migration-without-failover-clustering-to-move-a-virtual-machine.md)