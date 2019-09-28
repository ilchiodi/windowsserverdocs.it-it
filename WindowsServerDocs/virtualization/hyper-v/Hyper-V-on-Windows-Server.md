---
title: Hyper-V in Windows Server
description: Fornisce collegamenti ad articoli chiave su come provare, pianificare, distribuire e gestire Hyper-V
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0baef6b8-598c-4fe0-9f31-5869fc4e0f69
author: KBDAzure
ms.author: kathydav
ms.date: 10/07/2016
ms.openlocfilehash: 558733e3472578d1df413fe220f1846debc0f358
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366838"
---
# <a name="hyper-v-on-windows-server"></a>Hyper-V in Windows Server

>Si applica a: Windows Server 2016, Windows Server 2019

Il ruolo Hyper-V in Windows Server consente creare un ambiente virtualizzato in cui è possibile creare e gestire macchine virtuali. È possibile eseguire più sistemi operativi in un computer fisico e isolare i sistemi operativi tra loro. Con questa tecnologia, è possibile migliorare l'efficienza delle risorse di elaborazione e liberare le risorse hardware.

Per ulteriori informazioni su Hyper-V in Windows Server, vedere gli argomenti nella tabella seguente.

## <a name="hyper-v-resources-for-it-pros"></a>Risorse di Hyper-V per i professionisti IT

|Attività |Risorse|
|---|---|
|![Segno di spunta e icona del documento per visualizzare i requisiti](media/All_Symbols_MeetsRequirements.png)|**Valutazione di Hyper-V**<br /><br />[Panoramica della tecnologia Hyper-V](Hyper-V-Technology-Overview.md) - <br />-  Novità[di Hyper-V in Windows Server](What-s-new-in-Hyper-V-on-Windows.md)<br />[requisiti di sistema -  per Hyper-V in Windows Server](System-requirements-for-Hyper-V-on-Windows.md)<br />- [supporta i sistemi operativi guest Windows per Hyper-V](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md) <br />[macchine virtuali Linux e FreeBSD supportate](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md) da - <br />compatibilità delle funzionalità - [per generazione e Guest](Hyper-V-feature-compatibility-by-generation-and-guest.md) <br /><br />**Pianificare Hyper-V**<br /><br />-  è[consigliabile creare una macchina virtuale di prima o seconda generazione in Hyper-V?](plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md) <br />Piano @no__t 0[per la scalabilità di Hyper-V in Windows Server](plan/plan-hyper-v-scalability-in-windows-server.md) <br />Piano @no__t 0[per la rete Hyper-V in Windows Server](plan/plan-hyper-v-networking-in-windows-server.md) <br />Piano @no__t 0[per la sicurezza di Hyper-V in Windows Server](plan/plan-hyper-v-security-in-windows-server.md)|
|![Icona del cursore e riflesso](media/All_Symbols_GetStarted.png)|**Introduzione a Hyper-V**<br /><br />- [scaricare e installare Windows Server 2019](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019)<br /><br />**Opzione di installazione Server Core o GUI di Windows Server 2019 come host macchina virtuale**<br /><br />- [installare il ruolo Hyper-V in Windows Server](get-started/Install-the-Hyper-V-role-on-Windows-Server.md)<br />- [creare un commutire virtuale per le macchine virtuali Hyper-V](get-started/Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)<br />- [creare una macchina virtuale in Hyper-V](get-started/Create-a-virtual-machine-in-Hyper-V.md)|
|![Icona persona e strumenti](media/All_Symbols_Administrator.png)|**Aggiornare gli host Hyper-V e le macchine virtuali**<br /><br />- [aggiornare i nodi del cluster di Windows Server](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md)<br />versione - [aggiornamento macchina virtuale](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)<br /><br />**Configurare e gestire Hyper-V**<br /><br />- [configurare gli host per la migrazione in tempo reale senza clustering di failover](deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md)<br />- [gestione di nano server in modalità remota](../../get-started/manage-nano-server.md)<br />- [scegliere tra i checkpoint standard o di produzione](manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md)<br />- [Abilita o Disabilita i checkpoint](manage/Enable-or-disable-checkpoints-in-Hyper-V.md)<br />- [gestire le macchine virtuali Windows con PowerShell Direct](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md)<br />- [configurare la replica Hyper-V](manage/Set-up-Hyper-V-Replica.md)|
|![Icona bolle conversazione](media/All_Symbols_Chat.png)|**Blog**<br /><br />Estrarre i post più recenti da responsabili del programma, responsabili di prodotto, gli sviluppatori e tester in team di Microsoft Virtualization e Hyper-V.<br /><br />[Blog sulla virtualizzazione](https://blogs.technet.com/b/virtualization/) @no__t 0<br />[Blog di Windows Server](https://blogs.technet.com/b/windowsserver/) - <br />[Blog sulla virtualizzazione di Ben Armstrong](https://blogs.msdn.com/b/virtual_pc_guy/) -  (archiviato)|
|![Icona del gruppo di utenti](media/All_Symbols_Users_Group.png)|**Forum e newsgroup**<br /><br />Domande? Comunicare con i colleghi, MVP e il team del prodotto Hyper-V.<br /><br />[community di Windows Server](https://techcommunity.microsoft.com/t5/Windows-Server/ct-p/Windows-Server) - <br />[forum TechNet di Windows Server Hyper-V](https://social.technet.microsoft.com/Forums/windowsserver/home?forum=winserverhyperv) - |

## <a name="related-technologies"></a>Tecnologie correlate

Nella tabella seguente elenca le tecnologie che è possibile che si desidera utilizzare nell'ambiente di elaborazione di virtualizzazione.

|Tecnologia|Descrizione|
|--------------|---------------|
|[Client Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/index)|Tecnologia di virtualizzazione, inclusa in Windows 8, Windows 8.1 e Windows 10 che è possibile installare tramite **programmi e funzionalità** nel **Pannello di controllo**.|
|[Clustering di failover](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering)|Funzionalità di Windows Server che garantisce un'elevata disponibilità per gli host Hyper-V e macchine virtuali.|
|[Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/overview)|Componente di System Center che fornisce una soluzione di gestione per i datacenter virtualizzati. È possibile configurare e gestire gli host di virtualizzazione, reti e risorse di archiviazione, pertanto è possibile creare e distribuire macchine virtuali e servizi nei cloud privati creati.|
|[Contenitori Windows](https://docs.microsoft.com/virtualization/windowscontainers/)|Utilizzare i contenitori di Windows Server e Hyper-V di fornire ambienti standard per i team di sviluppo, test e produzione.|