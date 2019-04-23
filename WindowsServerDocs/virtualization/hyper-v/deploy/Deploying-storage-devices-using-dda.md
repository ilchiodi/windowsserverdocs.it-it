---
title: Distribuire i dispositivi di archiviazione NVMe usando discreti dispositivo assegnazione
description: Informazioni su come usare DDA per distribuire i dispositivi di archiviazione
ms.prod: windows-server-threshold
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.assetid: 1c36107e-78c9-4ec0-a313-6ed557ac0ffc
ms.openlocfilehash: d6fe54789d37386d5dc782ef8a2ca26b47adc69e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841362"
---
# <a name="deploy-nvme-storage-devices-using-discrete-device-assignment"></a>Distribuire i dispositivi di archiviazione NVMe usando discreti dispositivo assegnazione

>Si applica a: Microsoft Hyper-V Server 2016, Windows Server 2016

A partire da Windows Server 2016, è possibile utilizzare discreti dispositivo assegnazione o DDA, passare un intero dispositivo PCIe in una macchina virtuale.  Ciò consentirà l'accesso ad alte prestazioni per i dispositivi, ad esempio archiviazione NVMe o schede grafiche all'interno di una macchina virtuale pur essendo in grado di sfruttare il driver di dispositivi nativi.  Visitare il [pianificare la distribuzione di dispositivi utilizzando discreti dispositivo assegnazione](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) per altri dettagli su quali dispositivi di lavoro, quali sono le implicazioni di sicurezza e così via. Esistono tre passaggi per l'uso di un dispositivo con DDA:
-   Configurare la macchina virtuale per DDA
-   Smontare il dispositivo nella partizione Host
-   Assegnazione di dispositivo per la macchina virtuale Guest

Comando tutto può essere eseguito nell'Host in una console di Windows PowerShell come amministratore.

## <a name="configure-the-vm-for-dda"></a>Configurare la macchina virtuale per DDA
Assegnazione dispositivo discreti impone alcune limitazioni relative alle macchine virtuali ed è necessario eseguire il passaggio seguente.

1.  Configurare il "azione arresto automatico" di una macchina virtuale di spegnimento eseguendo

```
Set-VM -Name VMName -AutomaticStopAction TurnOff
```

## <a name="dismount-the-device-from-the-host-partition"></a>Smontare il dispositivo nella partizione Host

### <a name="locating-the-devices-location-path"></a>Individuazione Location Path del dispositivo
Il percorso del PCI è necessario smontare e montare il dispositivo dall'Host.  Un esempio di percorso di percorso simile al seguente: `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`.   Altre informazioni sul percorso il percorso è reperibile qui: [Pianificare la distribuzione di dispositivi utilizzando discreti dispositivo assegnazione](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md).

### <a name="disable-the-device"></a>Disabilitare il dispositivo
Usa PowerShell o Gestione dispositivi, verificare che il dispositivo viene "disabilitato".  

### <a name="dismount-the-device"></a>Smontare il dispositivo
```
Dismount-VMHostAssignableDevice -LocationPath $locationPath
```

## <a name="assigning-the-device-to-the-guest-vm"></a>Assegnazione di dispositivo per la macchina virtuale Guest
Il passaggio finale consiste nell'indicare Hyper-V che una macchina virtuale deve avere accesso al dispositivo.  Oltre al percorso trovato sopra, è necessario conoscere il nome della macchina virtuale.

```
Add-VMAssignableDevice -LocationPath $locationPath -VMName VMName
```

## <a name="whats-next"></a>Quali sono le novità
Dopo che un dispositivo è stato montato in una macchina virtuale, sarà ora possibile avviare tale macchina virtuale e interagire con il dispositivo come si farebbe normalmente se è in esecuzione in un sistema bare metal.  È possibile verificarlo aprendo Gestione dispositivi nella VM Guest e vedere che l'hardware viene ora visualizzato.

## <a name="removing-a-device-and-returning-it-to-the-host"></a>Rimozione di un dispositivo e la restituzione all'Host
Se si desidera restituire egli dispositivo torna allo stato originale, è necessario arrestare la macchina virtuale ed emettere la seguente:
```
#Remove the device from the VM
Remove-VMAssignableDevice -LocationPath $locationPath -VMName VMName
#Mount the device back in the host
Mount-VMHostAssignableDevice -LocationPath $locationPath
```
È quindi possibile abilitare nuovamente il dispositivo in Gestione dispositivi e il sistema operativo host saranno in grado di interagire con il dispositivo nuovamente.
