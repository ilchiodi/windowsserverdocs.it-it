---
title: Microsoft Hyper-V Server 2016
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: f815ada0-4c63-4e73-9c24-dc5eb21526c7
author: KBDAzure
ms.author: kathydav
ms.date: 07/26/2017
ms.openlocfilehash: 9a1b6ea7b9abc94f63a1390b6fa18e4c8d4a1822
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839372"
---
# <a name="microsoft-hyper-v-server-2016"></a>Microsoft Hyper-V Server 2016

Microsoft Hyper-V Server 2016 è un supporto\-prodotto autonomo che include solo Windows hypervisor, un modello di driver di Windows Server e componenti di virtualizzazione. Fornisce una soluzione di virtualizzazione affidabile e semplice che consentono di migliorare l'utilizzo dei server riducendo i costi.

La tecnologia hypervisor di Windows in Microsoft Hyper-V Server 2016 equivale a quali sono le novità di Hyper\-ruolo V in Windows Server 2016. Pertanto, gran parte del contenuto disponibile per l'Hyper\-ruolo V in Windows Server 2016 si applica anche a Microsoft Hyper-V Server 2016.

## <a name="hyper-v-server-resources-for-it-pros"></a>Hyper\-risorse Server V per i professionisti IT

|Attività|Risorse|
|-|-|
|![Soddisfa i requisiti simbolo](media/All_Symbols_MeetsRequirements.png)|**Valutazione di Hyper-V**<br /><br />-   [Panoramica della tecnologia Hyper-V](hyper-v-technology-overview.md)<br />- [Quali sono le novità in Hyper-V in Windows Server 2016](what-s-new-in-hyper-v-on-windows.md)<br />-   [Requisiti di sistema per Hyper-V in Windows Server 2016](system-requirements-for-hyper-v-on-windows.md)<br />-   [Sistemi operativi guest supportati Windows per Hyper-V](supported-windows-guest-operating-systems-for-hyper-v-on-windows.md)<br />-   [Supportate Linux e FreeBSD le macchine virtuali](supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)<br />-   [Compatibilità con la funzionalità per la generazione e guest](hyper-v-feature-compatibility-by-generation-and-guest.md)<br /><br />**Piano per Hyper-V**<br /><br />-Decidere quale [generazione della macchina virtuale](plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md) soddisfa le proprie esigenze. <br/>-Se si sta spostando o l'importazione di macchine virtuali, per decidere quando [l'aggiornamento della versione](deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md). <br />- [Scalabilità](plan/plan-hyper-v-scalability-in-windows-server.md) <br />- [Funzionalità di rete](plan/plan-hyper-v-networking-in-windows-server.md) <br />- [Sicurezza](plan/plan-hyper-v-security-in-windows-server.md)|
|![Ottenere simbolo avviata](media/All_Symbols_GetStarted.png)|**Introduzione a Hyper-V Server**<br /><br />[Scaricare e installare Microsoft Hyper\-V Server 2016](https://www.microsoft.com/evalcenter/evaluate-hyper-v-server-2016). Ciò consente di installare l'hypervisor di Windows, un modello di driver di Windows Server e componenti di virtualizzazione. È simile a in esecuzione l'opzione di installazione Server Core di Windows Server 2016 e il Hyper\-ruolo V.|
|![Gestire i simboli](media/All_Symbols_Administrator.png)|**Configurare e gestire i Server Hyper-V**<br /><br />Hyper\-V Server non dispone di un'interfaccia utente grafica \(GUI\). È possibile usare gli strumenti seguenti per configurare e gestire Hyper\-Server V.<br /><br />-   [Configurare un'installazione Server Core di Windows Server 2016 con sconfig. cmd](../../get-started/sconfig-on-ws2016.md) per aggiornare le impostazioni di dominio o gruppo di lavoro, modificare Windows aggiornare le impostazioni, abilitare la gestione remota e altro ancora.<br />-Usare un normale [prompt dei comandi](../../administration/windows-commands/windows-commands.md) per i comandi non è disponibili in Sconfig.<br />-Usare [Hyper\-Manager V](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/remote_host_management) oppure [Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm) per la gestione remota di Hyper\-Server V. Utilizzare Hyper\-di gestione di V [installazione di Hyper\-ruolo V in Windows 10](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v) oppure [Windows Server 2016](get-started/install-the-hyper-v-role-on-windows-server.md).<br />-Vedere [installare Server Core](../../get-started/getting-started-with-server-core.md) per le opzioni di gestione aggiuntiva per le funzioni di base del server che non sono specifiche di Hyper\-V. La maggior parte dei metodi di gestione sono documentati funziona anche con Hyper\-Server V.<br /><br />**Configurare e gestire Hyper\-macchine virtuali V**<br /><br />-   [Creare un commutatore virtuale per le macchine virtuali Hyper-V](get-started/create-a-virtual-switch-for-hyper-v-virtual-machines.md)<br />-   [Creare una macchina virtuale in Hyper-V](get-started/create-a-virtual-machine-in-hyper-v.md)<br />-   [Scegliere tra i checkpoint standard o di produzione](manage/choose-between-standard-or-production-checkpoints-in-hyper-v.md)<br />-   [Abilitare o disabilitare i checkpoint](manage/enable-or-disable-checkpoints-in-hyper-v.md)<br />-   [Gestire le macchine virtuali Windows con PowerShell Direct](manage/manage-windows-virtual-machines-with-powershell-direct.md) <br /><br />**Distribuzione**<br /><br />-   [Configurare gli host per live migration senza Clustering di Failover](deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)<br />- [Aggiornare i nodi del cluster Windows Server](../../failover-clustering/cluster-operating-system-rolling-upgrade.md)<br />- [Versione aggiornamento macchina virtuale](deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md)<br />|
