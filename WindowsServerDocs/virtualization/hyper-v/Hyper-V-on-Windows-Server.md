---
title: Hyper-V in Windows Server
description: Fornisce collegamenti ad articoli chiave su come provare, pianificare, distribuire e gestire Hyper-V
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: 0baef6b8-598c-4fe0-9f31-5869fc4e0f69
author: kbdazure
ms.author: kathydav
ms.date: 10/07/2016
ms.openlocfilehash: e75f61fb957929e668b3a1663402786cc399abe8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853254"
---
# <a name="hyper-v-on-windows-server"></a>Hyper-V in Windows Server

>Si applica a: Windows Server 2016, Windows Server 2019

Il ruolo Hyper-V in Windows Server consente creare un ambiente virtualizzato in cui è possibile creare e gestire macchine virtuali. È possibile eseguire più sistemi operativi in un computer fisico e isolare i sistemi operativi tra loro. Con questa tecnologia, è possibile migliorare l'efficienza delle risorse di elaborazione e liberare le risorse hardware.

Per ulteriori informazioni su Hyper-V in Windows Server, vedere gli argomenti nella tabella seguente.

## <a name="hyper-v-resources-for-it-pros"></a>Risorse di Hyper-V per i professionisti IT

|Attività |Risorse|
|---|---|
|![Segno di spunta e icona del documento per visualizzare i requisiti](media/All_Symbols_MeetsRequirements.png)|**Valuta Hyper-V**<p>[panoramica sulla tecnologia - Hyper-V](Hyper-V-Technology-Overview.md)<br />- Novità [di Hyper-V in Windows Server](What-s-new-in-Hyper-V-on-Windows.md)<br />- [requisiti di sistema per Hyper-V in Windows Server](System-requirements-for-Hyper-V-on-Windows.md)<br />- [sistemi operativi guest Windows supportati per Hyper-V](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md) <br />- [macchine virtuali Linux e FreeBSD supportate](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)<br />- la [compatibilità delle funzionalità per generazione e Guest](Hyper-V-feature-compatibility-by-generation-and-guest.md) <p>**Pianificare Hyper-V**<p>- è [consigliabile creare una macchina virtuale di prima o seconda generazione in Hyper-V?](plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md) <br />[piano di - per la scalabilità di Hyper-V in Windows Server](plan/plan-hyper-v-scalability-in-windows-server.md) <br />[piano di - per la rete Hyper-V in Windows Server](plan/plan-hyper-v-networking-in-windows-server.md) <br />[piano di - per la sicurezza di Hyper-V in Windows Server](plan/plan-hyper-v-security-in-windows-server.md)|
|![Icona del cursore e riflesso](media/All_Symbols_GetStarted.png)|**Introduzione a Hyper-V**<p>- [scaricare e installare Windows Server 2019](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019)<p>**Opzione di installazione Server Core o GUI di Windows Server 2019 come host macchina virtuale**<p>- [installare il ruolo Hyper-V in Windows Server](get-started/Install-the-Hyper-V-role-on-Windows-Server.md)<br />- [creare un commutire virtuale per macchine virtuali Hyper-V](get-started/Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)<br />- [creare una macchina virtuale in Hyper-V](get-started/Create-a-virtual-machine-in-Hyper-V.md)|
|![Icona persona e strumenti](media/All_Symbols_Administrator.png)|**Aggiornare gli host Hyper-V e le macchine virtuali**<p>- [aggiornare i nodi del cluster di Windows Server](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md)<br />- [aggiornare la versione della macchina virtuale](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md)<p>**Configurare e gestire Hyper-V**<p>- [configurare gli host per la migrazione in tempo reale senza clustering di failover](deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md)<br />- [gestione di nano server in modalità remota](../../get-started/manage-nano-server.md)<br />- [scegliere tra i checkpoint standard o di produzione](manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md)<br />- [abilitare o disabilitare i checkpoint](manage/Enable-or-disable-checkpoints-in-Hyper-V.md)<br />- [gestire le macchine virtuali Windows con PowerShell Direct](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md)<br />- [configurare la replica Hyper-V](manage/Set-up-Hyper-V-Replica.md)|
|![Icona bolle conversazione](media/All_Symbols_Chat.png)|**Blog**<p>Estrarre i post più recenti da responsabili del programma, responsabili di prodotto, gli sviluppatori e tester in team di Microsoft Virtualization e Hyper-V.<p>[Blog sulla virtualizzazione](https://blogs.technet.com/b/virtualization/) di - <br />Blog su - [Windows Server](https://blogs.technet.com/b/windowsserver/)<br />[Blog sulla virtualizzazione di - Ben Armstrong](https://blogs.msdn.com/b/virtual_pc_guy/) (archiviato)|
|![Icona del gruppo di utenti](media/All_Symbols_Users_Group.png)|**Forum e newsgroup**<p>Domande? Comunicare con i colleghi, MVP e il team del prodotto Hyper-V.<p>- [community di Windows Server](https://techcommunity.microsoft.com/t5/Windows-Server/ct-p/Windows-Server)<br />[Forum di TechNet su Windows Server Hyper-V](https://social.technet.microsoft.com/Forums/windowsserver/home?forum=winserverhyperv) - |

## <a name="related-technologies"></a>Tecnologie correlate

Nella tabella seguente elenca le tecnologie che è possibile che si desidera utilizzare nell'ambiente di elaborazione di virtualizzazione.

|Tecnologia|Descrizione|
|--------------|---------------|
|[Client Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/index)|Tecnologia di virtualizzazione, inclusa in Windows 8, Windows 8.1 e Windows 10 che è possibile installare tramite **programmi e funzionalità** nel **Pannello di controllo**.|
|[Clustering di failover](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering)|Funzionalità di Windows Server che garantisce un'elevata disponibilità per gli host Hyper-V e macchine virtuali.|
|[Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/overview)|Componente di System Center che fornisce una soluzione di gestione per i datacenter virtualizzati. È possibile configurare e gestire gli host di virtualizzazione, reti e risorse di archiviazione, pertanto è possibile creare e distribuire macchine virtuali e servizi nei cloud privati creati.|
|[Contenitori Windows](https://docs.microsoft.com/virtualization/windowscontainers/)|Utilizzare i contenitori di Windows Server e Hyper-V di fornire ambienti standard per i team di sviluppo, test e produzione.|