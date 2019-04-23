---
title: Infrastruttura sorvegliata e macchine virtuali schermate
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 5c7ada81-2d97-41d4-87cf-1a7ccf06cd20
manager: dongill
author: rpsqrd
ms.author: justinha
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: c2c16574439569eeb1181977f23bae238f43865c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857432"
---
# <a name="guarded-fabric-and-shielded-vms"></a>Infrastruttura sorvegliata e macchine virtuali schermate

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Uno degli obiettivi più importanti di fornire un ambiente ospitato è per garantire la sicurezza delle macchine virtuali in esecuzione nell'ambiente. L'infrastruttura sorvegliata consente ai provider di servizi cloud o agli amministratori del cloud privato aziendale di offrire un ambiente più sicuro per le macchine virtuali. Un'infrastruttura sorvegliata è costituita da un Servizio Sorveglianza host (Host Guardian Service - HGS), in genere, un cluster di tre nodi, oltre a uno o più host sorvegliati e un set di macchine virtuali schermate (VM).

> [!IMPORTANT]
> Assicurarsi di aver installato l'aggiornamento cumulativo più recente prima di distribuire le macchine virtuali schermate nell'ambiente di produzione.

## <a name="videos-blog-and-overview-topic-about-guarded-fabrics-and-shielded-vms"></a>Argomento introduttivo, blog e video circa sorvegliati le infrastrutture e macchine virtuali schermate

- Video: [Come proteggere l'infrastruttura di virtualizzazione da minacce interne con Windows Server 2019](https://myignite.techcommunity.microsoft.com/sessions/64690)
- Video: [Introduzione a macchine virtuali schermate in Windows Server 2016](https://channel9.msdn.com/Shows/Mechanics/Introduction-to-Shielded-Virtual-Machines-in-Windows-Server-2016)
- Video: [Approfondimento su macchine virtuali schermate in Windows Server 2016 Hyper-V](https://channel9.msdn.com/events/Ignite/2016/BRK3124)
- Video: [Distribuzione di macchine virtuali schermate e un'infrastruttura sorvegliata con Windows Server 2016](https://mva.microsoft.com/en-US/training-courses/deploying-shielded-vms-and-a-guarded-fabric-with-windows-server-2016-17131?l=WFLef7vUD_4604300474)
- Blog: [Data Center e blog sulla sicurezza del Cloud privato Microsoft](https://blogs.technet.microsoft.com/datacentersecurity/)
- Panoramica: [Panoramica di macchine virtuali schermata e dell'infrastruttura sorvegliata](Guarded-Fabric-and-Shielded-VMs.md)

## <a name="planning-topics"></a>Argomenti sulla pianificazione

- [Guida alla pianificazione di servizi di hosting](guarded-fabric-planning-for-hosters.md)
- [Guida alla pianificazione per i tenant](guarded-fabric-shielded-vm-planning-for-tenants.md)

## <a name="deployment-topics"></a>Argomenti relativi alla distribuzione

- [Guida alla distribuzione](guarded-fabric-deploying-hgs-overview.md)
    - [Guida introduttiva](guarded-fabric-deployment-overview.md)
    - [Distribuire HGS](guarded-fabric-setting-up-the-host-guardian-service-hgs.md)
    - [Distribuire host sorvegliati](guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)
        - [Configurazione dell'infrastruttura DNS per gli host che diventeranno host sorvegliati](guarded-fabric-configuring-fabric-dns.md)
        - [Distribuire un host sorvegliato utilizzando la modalità Active Directory](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md)
        - [Distribuire un host sorvegliato utilizzando la modalità TPM](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md)
        - [Verificare che consente di attestare gli host sorvegliati](guarded-fabric-confirm-hosts-can-attest-successfully.md)
        - [Macchine virtuali schermate: provider di servizi consente di distribuire host sorvegliati in VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-hosts)
    - [Distribuire VM schermate](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
        - [Creare un modello di macchina virtuale schermata](guarded-fabric-create-a-shielded-vm-template.md)
        - [Preparare un helper di schermatura della macchina virtuale, disco rigido virtuale](guarded-fabric-vm-shielding-helper-vhd.md)
        - [Configurare Windows Azure Pack](guarded-fabric-hoster-sets-up-windows-azure-pack.md)
        - [Creare un file di dati di schermatura](guarded-fabric-tenant-creates-shielding-data.md)
        - [Distribuire una macchina virtuale schermata usando Microsoft Azure Pack](guarded-fabric-shielded-vm-windows-azure-pack.md)
        - [Distribuire una macchina virtuale schermata usando Virtual Machine Manager](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)

## <a name="operations-and-management-topic"></a>Argomento relativo alla gestione e operazioni

- [La gestione del servizio sorveglianza Host](guarded-fabric-manage-hgs.md)
