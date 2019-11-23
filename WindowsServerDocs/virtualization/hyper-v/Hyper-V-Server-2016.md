---
title: Microsoft Hyper-V Server 2016
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: f815ada0-4c63-4e73-9c24-dc5eb21526c7
author: KBDAzure
ms.author: kathydav
ms.date: 07/26/2017
ms.openlocfilehash: c9e1b5e2d30276bf89bc2d56482ec76d1c2241a2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365580"
---
# <a name="microsoft-hyper-v-server-2016"></a>Microsoft Hyper-V Server 2016

Microsoft Hyper-V Server 2016 è un prodotto autonomo\-che contiene solo Windows Hypervisor, un modello di driver di Windows Server e componenti di virtualizzazione. Offre una soluzione di virtualizzazione semplice e affidabile che consente di migliorare l'utilizzo del server e ridurre i costi.

La tecnologia Windows Hypervisor in Microsoft Hyper-V Server 2016 è identica a quella del ruolo Hyper\-V in Windows Server 2016. Quindi, gran parte del contenuto disponibile per il ruolo Hyper\-V in Windows Server 2016 si applica anche a Microsoft Hyper-V Server 2016.

## <a name="hyper-v-server-resources-for-it-pros"></a>Risorse di Hyper\-V Server per professionisti IT

|Attività|Risorse|
|-|-|
|![Il simbolo soddisfa i requisiti](media/All_Symbols_MeetsRequirements.png)|**Valutazione di Hyper-V**<br /><br />[panoramica sulla tecnologia -   Hyper-V](hyper-v-technology-overview.md)<br />- Novità [di Hyper-V in Windows Server 2016](what-s-new-in-hyper-v-on-windows.md)<br />-   [requisiti di sistema per Hyper-V in Windows Server 2016](system-requirements-for-hyper-v-on-windows.md)<br />-   [sistemi operativi guest Windows supportati per Hyper-V](supported-windows-guest-operating-systems-for-hyper-v-on-windows.md)<br />-   [macchine virtuali Linux e FreeBSD supportate](supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)<br />-   la [compatibilità delle funzionalità per generazione e Guest](hyper-v-feature-compatibility-by-generation-and-guest.md)<br /><br />**Pianificare Hyper-V**<br /><br />-Decidere quale [generazione di macchine virtuali](plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md) soddisfi le proprie esigenze. <br/>-Se si stanno migrando o importando macchine virtuali, decidere quando [aggiornare la versione](deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md). <br />[scalabilità](plan/plan-hyper-v-scalability-in-windows-server.md) -  <br />[rete](plan/plan-hyper-v-networking-in-windows-server.md) -  <br />[sicurezza](plan/plan-hyper-v-security-in-windows-server.md) - |
|![Icona inizia](media/All_Symbols_GetStarted.png)|**Introduzione a Hyper-V Server**<br /><br />[Scaricare e installare Microsoft Hyper\-V Server 2016](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2016). In questo modo vengono installati Windows Hypervisor, un modello di driver di Windows Server e componenti di virtualizzazione. È simile all'esecuzione dell'opzione di installazione dei componenti di base del server di Windows Server 2016 e del ruolo Hyper\-V.|
|![Gestisci simbolo](media/All_Symbols_Administrator.png)|**Configurare e gestire il server Hyper-V**<br /><br />Hyper\-V Server non dispone di un'interfaccia utente grafica \(\)GUI. È possibile usare gli strumenti seguenti per configurare e gestire Hyper\-V Server.<br /><br />-   [configurare un'installazione dei componenti di base del server di Windows server 2016 con Sconfig. cmd](../../get-started/sconfig-on-ws2016.md) per aggiornare le impostazioni del dominio o del gruppo di lavoro, modificare le impostazioni di Windows Update, abilitare la gestione remota e altro ancora.<br />-Usare un [prompt](../../administration/windows-commands/windows-commands.md) dei comandi comune per i comandi non disponibili in sconfig.<br />-Usare la console di [gestione di hyper\-v](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/remote_host_management) o [Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm) per gestire in remoto Hyper\-v Server. Per usare la console di gestione di Hyper\-V, [installare il ruolo hyper\-v in Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v) o [Windows Server 2016](get-started/install-the-hyper-v-role-on-windows-server.md).<br />-Vedere [Install Server Core](../../get-started/getting-started-with-server-core.md) per altre opzioni di gestione per le funzioni server di base non specifiche di Hyper\-V. La maggior parte dei metodi di gestione documentati funziona anche con Hyper\-V Server.<br /><br />**Configurare e gestire macchine virtuali Hyper\-V**<br /><br />-   [creare un commutire virtuale per macchine virtuali Hyper-V](get-started/create-a-virtual-switch-for-hyper-v-virtual-machines.md)<br />-   [creare una macchina virtuale in Hyper-V](get-started/create-a-virtual-machine-in-hyper-v.md)<br />-   [scegliere tra i checkpoint standard o di produzione](manage/choose-between-standard-or-production-checkpoints-in-hyper-v.md)<br />-   [abilitare o disabilitare i checkpoint](manage/enable-or-disable-checkpoints-in-hyper-v.md)<br />-   [gestire le macchine virtuali Windows con PowerShell Direct](manage/manage-windows-virtual-machines-with-powershell-direct.md) <br /><br />**Distribuzione**<br /><br />-   [configurare gli host per la migrazione in tempo reale senza clustering di failover](deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)<br />- [aggiornare i nodi del cluster di Windows Server](../../failover-clustering/cluster-operating-system-rolling-upgrade.md)<br />- [aggiornare la versione della macchina virtuale](deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)<br />|
