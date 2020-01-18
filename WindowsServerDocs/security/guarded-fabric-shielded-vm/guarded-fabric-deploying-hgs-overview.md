---
title: Distribuzione del servizio sorveglianza host
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 310b63d9-5ac7-4961-98ef-103af45d706a
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 01/14/2020
ms.openlocfilehash: e66e7f365553f3aa106abbebf372492e0cc08386
ms.sourcegitcommit: c5709021aa98abd075d7a8f912d4fd2263db8803
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/18/2020
ms.locfileid: "76265943"
---
# <a name="deploying-the-host-guardian-service"></a>Distribuzione del servizio sorveglianza host 

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Uno degli obiettivi più importanti di fornire un ambiente ospitato è garantire la sicurezza delle macchine virtuali in esecuzione nell'ambiente. L'infrastruttura sorvegliata consente ai provider di servizi cloud o agli amministratori del cloud privato aziendale di offrire un ambiente più sicuro per le macchine virtuali. Un'infrastruttura sorvegliata è costituita da un Servizio Sorveglianza host (Host Guardian Service - HGS), in genere, un cluster di tre nodi, oltre a uno o più host sorvegliati e un set di macchine virtuali schermate (VM).

## <a name="video-deploying-a-guarded-fabric"></a>Video: distribuzione di un'infrastruttura sorvegliata 

> [!VIDEO https://www.microsoft.com/videoplayer/embed/dcd8e99f-36f1-4bc8-b3d2-9576da38d9f1?autoplay=false]

## <a name="deployment-tasks-for-guarded-fabrics-and-shielded-vms"></a>Attività di distribuzione per infrastrutture sorvegliate e VM schermate

La tabella seguente suddivide le attività per distribuire un'infrastruttura sorvegliata e creare VM schermate in base ai diversi ruoli di amministratore. Si noti che quando l'amministratore di HGS configura HGS con gli host Hyper-V autorizzati, un amministratore dell'infrastruttura raccoglierà e fornirà le informazioni di identificazione sugli host nello stesso momento.    

|<img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-hgs-administrator-tasks.png" alt="Host Guardian Service administrator tasks" width="238" height="62" align="left" /> | <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-fabric-administrator-tasks.png" alt="Fabric administrator tasks" width="300" height="62" align="left" /> | <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-tenant-administrator-tasks.png" alt="Tenant administrator tasks" width="184" height="66" align="left" /> |
|-------------------------------------|--------------------------------|-----------------------------------------|
|(1) [verificare i prerequisiti di HGS](guarded-fabric-prepare-for-hgs.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-verify.png" alt="Step 1" hspace="8" align="right" />| | |
|(2) [configurare il primo nodo&nbsp;HGS](guarded-fabric-choose-where-to-install-hgs.md)&nbsp;<img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-first-hgs-node.png" alt="Step 2" hspace="8" align="right" />| | |
|(3) [configurare altri nodi di&nbsp;HGS](guarded-fabric-configure-additional-hgs-nodes.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-secondary-hgs-nodes.png" alt="Step 3" hspace="8" align="right" />| | |
| &nbsp; |(4) [configurare DNS infrastruttura](guarded-fabric-configuring-fabric-dns.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-fabric-dns.png" alt="Step 4" hspace="8" align="right" />| |
| &nbsp; |(5) [verificare i prerequisiti dell'host&nbsp;(chiave)](guarded-fabric-guarded-host-prerequisites.md#host-key-attestation)<br>[Verificare i prerequisiti del&nbsp;host (TPM)](guarded-fabric-guarded-host-prerequisites.md#tpm-trusted-attestation)<img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-verify.png" alt="Step 5" hspace="8" align="right" />| |
|(7) [configurare HGS con le informazioni sull'host](guarded-fabric-add-host-information-to-hgs.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-hgs-with-host-info.png" alt="Step 7" hspace="8" align="right" />|(6) [creare la chiave host (chiave)](guarded-fabric-create-host-key.md)<br>[Raccogli informazioni host (TPM)](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-collect-info-from-hosts.png" alt="Step 6" hspace="8" align="right" />| |
| &nbsp; |(8) [confermare che gli host possono attestare](guarded-fabric-confirm-hosts-can-attest-successfully.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-confirm-hosts-attest.png" alt="Step 8" hspace="8" align="right" />| |
| &nbsp; |(9) [configurare VMM (facoltativo)](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-overview) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-configure-vmm.png" alt="Step 9" hspace="8" align="right" />| |
| &nbsp; |(10) [creare dischi modello](guarded-fabric-create-a-shielded-vm-template.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-create-template-disk.png" alt="Step 10" hspace="8" align="right" />| |
| &nbsp; |(11) [creare un disco dell'helper di schermatura della macchina virtuale per VMM (facoltativo)](guarded-fabric-vm-shielding-helper-vhd.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-create-helper-disk.png" alt="Step 11" hspace="8" align="right" />| |
| &nbsp; |(12) [configurare Windows Azure Pack (facoltativo)](guarded-fabric-shielded-vm-windows-azure-pack.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-windows-azure-pack.png" alt="Step 12" hspace="8" align="right" />| |
| &nbsp; | &nbsp; |(13) [creare un file di dati di schermatura](guarded-fabric-tenant-creates-shielding-data.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-shielding-data-file.png" alt="Step 13" hspace="8" align="right" />|
| &nbsp; | &nbsp; |(14) [creare VM schermate usando Windows Azure Pack](guarded-fabric-shielded-vm-windows-azure-pack.md) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-shielded-vms.png" alt="Step 14" hspace="8" align="right" /><br>[Creare VM schermate con VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/guarded-vms) <img src="../media/Guarded-Fabric-Shielded-VM/guarded-host-shielded-vms.png" alt="Step 15" hspace="8" align="right" />|


## <a name="see-also"></a>Vedi anche

- [Infrastruttura sorvegliata e macchine virtuali schermate](guarded-fabric-and-shielded-vms-top-node.md)
