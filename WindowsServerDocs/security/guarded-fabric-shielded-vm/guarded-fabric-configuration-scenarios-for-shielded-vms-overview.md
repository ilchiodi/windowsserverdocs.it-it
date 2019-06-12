---
title: Distribuire macchine virtuali schermate
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 5d1a06c9-24e1-4e14-9c9a-efb2adbfeddd
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 9badbfcb709c29451425aaecc56b46ac98837e18
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443798"
---
# <a name="deploy-shielded-vms"></a>Distribuire macchine virtuali schermate


>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

Gli argomenti seguenti descrivono il funzionamento di un tenant con le macchine virtuali schermate.

1. (Facoltativo) [Creare un disco modello Windows](guarded-fabric-create-a-shielded-vm-template.md) oppure [creare un disco modello Linux](guarded-fabric-create-a-linux-shielded-vm-template.md). È possibile creare il disco modello per il tenant o il provider di servizi di hosting. 

2. (Facoltativo) [Convertire una macchina virtuale Windows esistente in una macchina virtuale schermata](guarded-fabric-vm-shielding-helper-vhd.md). 

3. [Creare i dati di schermatura per definire una macchina virtuale schermata](guarded-fabric-tenant-creates-shielding-data.md).

    Per una descrizione e un diagramma di un file di dati di schermatura, vedere [cosa sia i dati di schermatura e perché sono necessari?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary)
    
    Per informazioni sulla creazione di un file di risposte da includere in un file di dati schermati, vedere [macchine virtuali schermate: generare un file di risposte usando la funzione New-ShieldingDataAnswerFile](guarded-fabric-sample-unattend-xml-file.md).

4. Creare una VM schermata:
 
    - Usando **Windows Azure Pack**: [Distribuire una macchina virtuale schermata usando Microsoft Azure Pack](guarded-fabric-shielded-vm-windows-azure-pack.md)

    - Usando **Virtual Machine Manager**: [Distribuire una macchina virtuale schermata usando Virtual Machine Manager](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Creare un modello di macchina virtuale schermata](guarded-fabric-create-a-shielded-vm-template.md)

## <a name="see-also"></a>Vedere anche

- [Infrastruttura sorvegliata e macchine virtuali schermate](guarded-fabric-and-shielded-vms-top-node.md)
