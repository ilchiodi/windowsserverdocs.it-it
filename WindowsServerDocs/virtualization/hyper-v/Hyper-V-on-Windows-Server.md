---
title: Hyper-V in Windows Server
description: Vengono forniti collegamenti ad articoli importanti sulla valutazione, pianificazione, distribuzione e gestione di Hyper-V
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0baef6b8-598c-4fe0-9f31-5869fc4e0f69
author: KBDAzure
ms.author: kathydav
ms.date: 10/07/2016
ms.openlocfilehash: 1a658b611b68d7ecde64bdf0f8a318cc1af2804c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880262"
---
# <a name="hyper-v-on-windows-server"></a>Hyper-V in Windows Server

>Si applica a: Windows Server 2016, Windows Server 2019

Il ruolo Hyper-V in Windows Server consente creare un ambiente virtualizzato in cui è possibile creare e gestire macchine virtuali. È possibile eseguire più sistemi operativi in un computer fisico e isolare i sistemi operativi tra loro. Con questa tecnologia, è possibile migliorare l'efficienza delle risorse di elaborazione e liberare le risorse hardware.

Vedere gli argomenti nella tabella seguente per altre informazioni su Hyper-V in Windows Server.

## <a name="hyper-v-resources-for-it-pros"></a>Risorse di Hyper-V per i professionisti IT

|Attività |Risorse|
|---|---|
|![Segno di spunta e documento icona per visualizzare i requisiti siano soddisfatti.](media/All_Symbols_MeetsRequirements.png)|**Valutazione di Hyper-V**<br /><br />- [Panoramica della tecnologia Hyper-V](Hyper-V-Technology-Overview.md)<br />- [Quali sono le novità in Hyper-V in Windows Server](What-s-new-in-Hyper-V-on-Windows.md)<br />- [Requisiti di sistema per Hyper-V in Windows Server](System-requirements-for-Hyper-V-on-Windows.md)<br />- [Sistemi operativi guest supportati Windows per Hyper-V](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md) <br />- [Macchine virtuali Linux e FreeBSD supportate](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)<br />- [Compatibilità con la funzionalità per la generazione e guest](Hyper-V-feature-compatibility-by-generation-and-guest.md) <br /><br />**Piano per Hyper-V**<br /><br />- [È consigliabile creare una macchina virtuale di generazione 1 o 2 in Hyper-V?](plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md) <br />- [Pianificare la scalabilità di Hyper-V in Windows Server](plan/plan-hyper-v-scalability-in-windows-server.md) <br />- [Pianificare la rete di Hyper-V in Windows Server](plan/plan-hyper-v-networking-in-windows-server.md) <br />- [Pianificare la sicurezza di Hyper-V in Windows Server](plan/plan-hyper-v-security-in-windows-server.md)|
|![Icona del cursore e riflesso](media/All_Symbols_GetStarted.png)|**Introduzione a Hyper-V**<br /><br />- [Scaricare e installare Windows Server 2019](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019)<br /><br />**Opzione di installazione di Server Core o interfaccia utente grafica di Windows Server 2019 come host macchina virtuale**<br /><br />- [Installare il ruolo Hyper-V in Windows Server](get-started/Install-the-Hyper-V-role-on-Windows-Server.md)<br />- [Creare un commutatore virtuale per le macchine virtuali Hyper-V](get-started/Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)<br />- [Creare una macchina virtuale in Hyper-V](get-started/Create-a-virtual-machine-in-Hyper-V.md)|
|![Icona persona e strumenti](media/All_Symbols_Administrator.png)|**Eseguire l'aggiornamento host Hyper-V e macchine virtuali**<br /><br />- [Aggiornare i nodi del cluster Windows Server](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md)<br />- [Versione aggiornamento macchina virtuale](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)<br /><br />**Configurare e gestire Hyper-V**<br /><br />- [Configurare gli host per live migration senza Clustering di Failover](deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md)<br />- [Gestione remota di Nano Server](../../get-started/manage-nano-server.md)<br />- [Scegliere tra i checkpoint standard o di produzione](manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md)<br />- [Abilitare o disabilitare i checkpoint](manage/Enable-or-disable-checkpoints-in-Hyper-V.md)<br />- [Gestire le macchine virtuali Windows con PowerShell Direct](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md)<br />- [Configurare la Replica Hyper-V](manage/Set-up-Hyper-V-Replica.md)|
|![Icona bolle conversazione](media/All_Symbols_Chat.png)|**Blogs**<br /><br />Estrarre i post più recenti da responsabili del programma, responsabili di prodotto, gli sviluppatori e tester in team di Microsoft Virtualization e Hyper-V.<br /><br />- [Virtualization Blog](https://blogs.technet.com/b/virtualization/)<br />- [Blog di Windows Server](https://blogs.technet.com/b/windowsserver/)<br />- [Blog di virtualizzazione di Ben Armstrong](https://blogs.msdn.com/b/virtual_pc_guy/) (archiviati)|
|![Icona del gruppo di utenti](media/All_Symbols_Users_Group.png)|**Forum e newsgroup**<br /><br />Domande? Comunicare con i colleghi, MVP e il team del prodotto Hyper-V.<br /><br />- [Windows Server Community](https://techcommunity.microsoft.com/t5/Windows-Server/ct-p/Windows-Server)<br />- [Forum TechNet di Windows Server Hyper-V](https://social.technet.microsoft.com/Forums/windowsserver/home?forum=winserverhyperv)|

## <a name="related-technologies"></a>Tecnologie correlate

Nella tabella seguente elenca le tecnologie che è possibile che si desidera utilizzare nell'ambiente di elaborazione di virtualizzazione.

|Tecnologia|Descrizione|
|--------------|---------------|
|[Hyper-V client](https://docs.microsoft.com/virtualization/hyper-v-on-windows/index)|Tecnologia di virtualizzazione, inclusa in Windows 8, Windows 8.1 e Windows 10 che è possibile installare tramite **programmi e funzionalità** nel **Pannello di controllo**.|
|[Clustering di failover](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering)|Funzionalità di Windows Server che garantisce un'elevata disponibilità per gli host Hyper-V e macchine virtuali.|
|[Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/overview)|Componente di System Center che fornisce una soluzione di gestione per i datacenter virtualizzati. È possibile configurare e gestire gli host di virtualizzazione, reti e risorse di archiviazione, pertanto è possibile creare e distribuire macchine virtuali e servizi nei cloud privati creati.|
|[Contenitori Windows](https://docs.microsoft.com/virtualization/windowscontainers/)|Utilizzare i contenitori di Windows Server e Hyper-V di fornire ambienti standard per i team di sviluppo, test e produzione.|