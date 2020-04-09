---
title: Distribuire macchine virtuali schermate
ms.prod: windows-server
ms.topic: article
ms.assetid: 5d1a06c9-24e1-4e14-9c9a-efb2adbfeddd
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: f4550f8a92330c8f483e332ab9e4b36fda853b0a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856864"
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
 
    - Uso di **Windows Azure Pack**: [distribuire una macchina virtuale schermata usando Windows Azure Pack](guarded-fabric-shielded-vm-windows-azure-pack.md)

    - Uso di **Virtual Machine Manager**: [distribuire una macchina virtuale schermata usando Virtual Machine Manager](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Creare un modello di macchina virtuale schermata](guarded-fabric-create-a-shielded-vm-template.md)

## <a name="see-also"></a>Vedere anche

- [Infrastruttura sorvegliata e macchine virtuali schermate](guarded-fabric-and-shielded-vms-top-node.md)
