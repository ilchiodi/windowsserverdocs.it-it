---
title: Distribuire dispositivi grafici con l'assegnazione di dispositivi discreti
description: Informazioni su come usare DDA per distribuire dispositivi grafici in Windows Server
ms.prod: windows-server-threshold
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.assetid: 67a01889-fa36-4bc6-841d-363d76df6a66
ms.openlocfilehash: 94ba561f35ea257a897f51cb3522196f7988eb71
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872105"
---
# <a name="deploy-graphics-devices-using-discrete-device-assignment"></a>Distribuire dispositivi grafici con l'assegnazione di dispositivi discreti

>Si applica a: Microsoft Hyper-V Server 2016, Windows Server 2016, Windows Server 2019 Microsoft Hyper-V Server 2019  

A partire da Windows Server 2016, è possibile usare l'assegnazione di dispositivi discreti, o DDA, per passare un intero dispositivo PCIe a una macchina virtuale.  Questo consentirà l'accesso ad alte prestazioni a dispositivi come l' [archiviazione NVMe](./Deploying-storage-devices-using-dda.md) o schede grafiche all'interno di una macchina virtuale e la possibilità di sfruttare i driver nativi dei dispositivi.  Visitare il [piano per la distribuzione di dispositivi con l'assegnazione di dispositivi discreti](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) per altri dettagli sul funzionamento dei dispositivi, quali sono le possibili implicazioni della sicurezza e così via.

Sono disponibili tre passaggi per l'uso di un dispositivo con l'assegnazione di dispositivi discreti:
-   Configurare la macchina virtuale per l'assegnazione di dispositivi discreti
-   Smontare il dispositivo dalla partizione host
-   Assegnazione del dispositivo alla macchina virtuale Guest

Tutti i comandi possono essere eseguiti nell'host in una console di Windows PowerShell come amministratore.

## <a name="configure-the-vm-for-dda"></a>Configurare la macchina virtuale per DDA
L'assegnazione di un dispositivo discreto impone alcune restrizioni alle macchine virtuali ed è necessario eseguire il passaggio seguente.

1.  Configurare l'azione di arresto automatico di una macchina virtuale da disattivare eseguendo

```
Set-VM -Name VMName -AutomaticStopAction TurnOff
```

### <a name="some-additional-vm-preparation-is-required-for-graphics-devices"></a>Per i dispositivi grafici è necessaria una preparazione aggiuntiva della macchina virtuale

Alcuni componenti hardware offrono prestazioni migliori se la macchina virtuale è configurata in un certo modo.  Per informazioni dettagliate sull'eventuale necessità o meno delle configurazioni seguenti per l'hardware, rivolgersi al fornitore dell'hardware. Ulteriori dettagli sono disponibili in [pianificare la distribuzione di dispositivi mediante l'assegnazione di dispositivi discreti](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) e in questo post di [Blog.](https://techcommunity.microsoft.com/t5/Virtualization/Discrete-Device-Assignment-GPUs/ba-p/382266)

1. Abilitare la combinazione di scrittura sulla CPU
   ```
   Set-VM -GuestControlledCacheTypes $true -VMName VMName
   ```
2. Configurare lo spazio MMIO a 32 bit
   ```
   Set-VM -LowMemoryMappedIoSpace 3Gb -VMName VMName
   ```
3. Configurare uno spazio MMIO superiore a 32 bit
   ```
   Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName VMName
   ```
   > [!TIP] 
   > I valori di spazio MMIO precedenti sono valori ragionevoli da impostare per la sperimentazione con una singola GPU.  Se dopo l'avvio della macchina virtuale, il dispositivo segnala un errore relativo a risorse insufficienti, è probabile che sia necessario modificare questi valori. Per informazioni su come calcolare con precisione i requisiti di MMIO, vedere [pianificare la distribuzione di dispositivi mediante l'assegnazione di dispositivi discreti](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) .

## <a name="dismount-the-device-from-the-host-partition"></a>Smontare il dispositivo dalla partizione host
### <a name="optional---install-the-partitioning-driver"></a>Facoltativo: installare il driver di partizionamento
L'assegnazione di un dispositivo discreto fornisce ai venditori hardware la possibilità di fornire un driver di mitigazione della sicurezza con i dispositivi.  Si noti che questo driver non corrisponde al driver di dispositivo che verrà installato nella macchina virtuale guest.  Il fornitore dell'hardware deve fornire questo driver, tuttavia, se lo fornisce, è necessario installarlo prima di smontare il dispositivo dalla partizione host.  Rivolgersi al fornitore dell'hardware per ulteriori informazioni su se hanno un driver di mitigazione
> Se non viene fornito alcun driver di partizionamento, durante lo smontaggio è `-force` necessario usare l'opzione per ignorare l'avviso di sicurezza. Per altre informazioni sulle implicazioni di sicurezza di questa operazione, vedere [pianificare la distribuzione di dispositivi con l'assegnazione di dispositivi discreti](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md).

### <a name="locating-the-devices-location-path"></a>Individuazione del percorso del dispositivo
Il percorso della posizione PCI è necessario per smontare e montare il dispositivo dall'host.  Un percorso di esempio è simile al seguente: `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`.  Altre informazioni sulla posizione del percorso sono disponibili qui: [Pianificare la distribuzione di dispositivi con l'assegnazione di dispositivi discreti](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md).

### <a name="disable-the-device"></a>Disabilitare il dispositivo
Con Device Manager o PowerShell, verificare che il dispositivo sia "disabilitato".  

### <a name="dismount-the-device"></a>Smontare il dispositivo
A seconda di se il fornitore ha fornito un driver di mitigazione, sarà necessario usare l'opzione "-Force".
- Se è stato installato un driver di mitigazione
  ```
  Dismount-VMHostAssignableDevice -LocationPath $locationPath
  ```
- Se non è stato installato alcun driver di mitigazione
  ```
  Dismount-VMHostAssignableDevice -force -LocationPath $locationPath
  ```

## <a name="assigning-the-device-to-the-guest-vm"></a>Assegnazione del dispositivo alla macchina virtuale Guest
Il passaggio finale consiste nell'indicare a Hyper-V che una macchina virtuale deve avere accesso al dispositivo.  Oltre al percorso trovato sopra, è necessario conoscerne il nome.

```
Add-VMAssignableDevice -LocationPath $locationPath -VMName VMName
```

## <a name="whats-next"></a>Argomenti successivi
Dopo che un dispositivo è stato montato correttamente in una macchina virtuale, è ora possibile avviare tale macchina virtuale e interagire con il dispositivo come si farebbe normalmente se fosse in esecuzione in un sistema bare metal.  Ciò significa che ora è possibile installare i driver del fornitore dell'hardware nella macchina virtuale e le applicazioni saranno in grado di visualizzare l'hardware presente.  È possibile verificare questo problema aprendo Gestione dispositivi nella macchina virtuale guest e osservando che l'hardware è ora visualizzato.

## <a name="removing-a-device-and-returning-it-to-the-host"></a>Rimozione di un dispositivo e relativa restituzione all'host
Se si vuole ripristinare lo stato originale del dispositivo, sarà necessario arrestare la macchina virtuale ed eseguire le operazioni seguenti:
```
#Remove the device from the VM
Remove-VMAssignableDevice -LocationPath $locationPath -VMName VMName
#Mount the device back in the host
Mount-VMHostAssignableDevice -LocationPath $locationPath
```
È quindi possibile riabilitare il dispositivo in gestione dispositivi e il sistema operativo host sarà in grado di interagire nuovamente con il dispositivo.

## <a name="examples"></a>Esempi

### <a name="mounting-a-gpu-to-a-vm"></a>Montaggio di una GPU in una macchina virtuale
In questo esempio viene usato PowerShell per configurare una macchina virtuale denominata "ddatest1" per portare la prima GPU disponibile dal produttore NVIDIA e assegnarla alla macchina virtuale.  
```
#Configure the VM for a Discrete Device Assignment
$vm =   "ddatest1"
#Set automatic stop action to TurnOff
Set-VM -Name $vm -AutomaticStopAction TurnOff
#Enable Write-Combining on the CPU
Set-VM -GuestControlledCacheTypes $true -VMName $vm
#Configure 32 bit MMIO space
Set-VM -LowMemoryMappedIoSpace 3Gb -VMName $vm
#Configure Greater than 32 bit MMIO space
Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName $vm

#Find the Location Path and disable the Device
#Enumerate all PNP Devices on the system
$pnpdevs = Get-PnpDevice -presentOnly
#Select only those devices that are Display devices manufactured by NVIDIA
$gpudevs = $pnpdevs |where-object {$_.Class -like "Display" -and $_.Manufacturer -like "NVIDIA"}
#Select the location path of the first device that's available to be dismounted by the host.
$locationPath = ($gpudevs | Get-PnpDeviceProperty DEVPKEY_Device_LocationPaths).data[0]
#Disable the PNP Device
Disable-PnpDevice  -InstanceId $gpudevs[0].InstanceId

#Dismount the Device from the Host
Dismount-VMHostAssignableDevice -force -LocationPath $locationPath

#Assign the device to the guest VM.
Add-VMAssignableDevice -LocationPath $locationPath -VMName $vm
```
