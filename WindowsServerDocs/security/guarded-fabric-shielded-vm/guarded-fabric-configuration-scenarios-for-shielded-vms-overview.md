---
title: Distribuire macchine virtuali schermate
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 5d1a06c9-24e1-4e14-9c9a-efb2adbfeddd
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: f7892dabb028b99cb4cb1c9045764a8e36aba7dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386773"
---
# <a name="deploy-shielded-vms"></a>Distribuire macchine virtuali schermate


>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Gli argomenti seguenti descrivono come un tenant può funzionare con le VM schermate.

1. Opzionale [Creare un disco modello di Windows](guarded-fabric-create-a-shielded-vm-template.md) o [creare un disco modello Linux](guarded-fabric-create-a-linux-shielded-vm-template.md). Il disco modello può essere creato dal tenant o dal provider di servizi di hosting. 

2. Opzionale [Convertire una macchina virtuale Windows esistente in una macchina virtuale schermata](guarded-fabric-vm-shielding-helper-vhd.md). 

3. [Creare dati di schermatura per definire una macchina virtuale schermata](guarded-fabric-tenant-creates-shielding-data.md).

    Per una descrizione e un diagramma di un file di dati di schermatura, vedere [che cos'è la schermatura dei dati e perché è necessaria?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary)
    
    Per informazioni sulla creazione di un file di risposte da includere in un file di dati schermato, vedere [VM schermate-generare un file di risposte usando la funzione New-ShieldingDataAnswerFile](guarded-fabric-sample-unattend-xml-file.md).

4. Creare una VM schermata:
 
    - Utilizzando **Windows Azure Pack**: [Distribuire una macchina virtuale schermata usando Windows Azure Pack](guarded-fabric-shielded-vm-windows-azure-pack.md)

    - Utilizzando **Virtual Machine Manager**: [Distribuire una macchina virtuale schermata usando Virtual Machine Manager](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Creare un modello di macchina virtuale schermata](guarded-fabric-create-a-shielded-vm-template.md)

## <a name="see-also"></a>Vedere anche

- [Infrastruttura sorvegliata e macchine virtuali schermate](guarded-fabric-and-shielded-vms-top-node.md)
