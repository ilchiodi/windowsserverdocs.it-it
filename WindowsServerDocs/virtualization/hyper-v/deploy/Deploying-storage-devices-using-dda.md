---
title: Distribuire dispositivi di archiviazione NVMe usando l'assegnazione di dispositivi discreti
description: Informazioni su come usare DDA per distribuire i dispositivi di archiviazione
ms.prod: windows-server
ms.technology: hyper-v
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.assetid: 1c36107e-78c9-4ec0-a313-6ed557ac0ffc
ms.openlocfilehash: 2b92b175a6e914b62b069f76f92255cb99d55d74
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860904"
---
# <a name="deploy-nvme-storage-devices-using-discrete-device-assignment"></a>Distribuire dispositivi di archiviazione NVMe usando l'assegnazione di dispositivi discreti

>Si applica a: Microsoft Hyper-V Server 2016, Windows Server 2016

A partire da Windows Server 2016, è possibile usare l'assegnazione di dispositivi discreti, o DDA, per passare un intero dispositivo PCIe a una macchina virtuale.  Questo consentirà l'accesso ad alte prestazioni a dispositivi come l'archiviazione NVMe o schede grafiche all'interno di una macchina virtuale e la possibilità di sfruttare i driver nativi dei dispositivi.  Visitare il [piano per la distribuzione di dispositivi con l'assegnazione di dispositivi discreti](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) per altri dettagli sul funzionamento dei dispositivi, quali sono le possibili implicazioni della sicurezza e così via. L'uso di un dispositivo con DDA prevede tre passaggi:
-   Configurare la macchina virtuale per DDA
-   Smontare il dispositivo dalla partizione host
-   Assegnazione del dispositivo alla macchina virtuale Guest

Tutti i comandi possono essere eseguiti nell'host in una console di Windows PowerShell come amministratore.

## <a name="configure-the-vm-for-dda"></a>Configurare la macchina virtuale per DDA
L'assegnazione di un dispositivo discreto impone alcune restrizioni alle macchine virtuali ed è necessario eseguire il passaggio seguente.

1.  Configurare l'azione di arresto automatico di una macchina virtuale da disattivare eseguendo

```
Set-VM -Name VMName -AutomaticStopAction TurnOff
```

## <a name="dismount-the-device-from-the-host-partition"></a>Smontare il dispositivo dalla partizione host

### <a name="locating-the-devices-location-path"></a>Individuazione del percorso del dispositivo
Il percorso della posizione PCI è necessario per smontare e montare il dispositivo dall'host.  Un percorso di esempio è simile al seguente: `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`.   Altre informazioni sulla posizione del percorso sono disponibili qui: pianificare la [distribuzione di dispositivi con l'assegnazione di dispositivi discreti](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md).

### <a name="disable-the-device"></a>Disabilitare il dispositivo
Con Device Manager o PowerShell, verificare che il dispositivo sia "disabilitato".  

### <a name="dismount-the-device"></a>Smontare il dispositivo
```
Dismount-VMHostAssignableDevice -LocationPath $locationPath
```

## <a name="assigning-the-device-to-the-guest-vm"></a>Assegnazione del dispositivo alla macchina virtuale Guest
Il passaggio finale consiste nell'indicare a Hyper-V che una macchina virtuale deve avere accesso al dispositivo.  Oltre al percorso trovato sopra, è necessario conoscerne il nome.

```
Add-VMAssignableDevice -LocationPath $locationPath -VMName VMName
```

## <a name="whats-next"></a>Operazioni successive
Dopo che un dispositivo è stato montato correttamente in una macchina virtuale, è ora possibile avviare tale macchina virtuale e interagire con il dispositivo come si farebbe normalmente se fosse in esecuzione in un sistema bare metal.  È possibile verificare questo problema aprendo Gestione dispositivi nella macchina virtuale guest e osservando che l'hardware è ora visualizzato.

## <a name="removing-a-device-and-returning-it-to-the-host"></a>Rimozione di un dispositivo e relativa restituzione all'host
Se si vuole ripristinare lo stato originale del dispositivo, sarà necessario arrestare la macchina virtuale ed eseguire le operazioni seguenti:
```
#Remove the device from the VM
Remove-VMAssignableDevice -LocationPath $locationPath -VMName VMName
#Mount the device back in the host
Mount-VMHostAssignableDevice -LocationPath $locationPath
```
È quindi possibile riabilitare il dispositivo in gestione dispositivi e il sistema operativo host sarà in grado di interagire nuovamente con il dispositivo.
