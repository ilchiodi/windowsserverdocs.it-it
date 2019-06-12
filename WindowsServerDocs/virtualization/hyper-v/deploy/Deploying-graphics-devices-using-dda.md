---
title: Distribuire i dispositivi di grafica usando discreti dispositivo assegnazione
description: Informazioni su come usare DDA per distribuire i dispositivi di grafica in Windows Server
ms.prod: windows-server-threshold
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.assetid: 67a01889-fa36-4bc6-841d-363d76df6a66
ms.openlocfilehash: 6c528535fd34f57957a37992843933d4cd9f8824
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447877"
---
# <a name="deploy-graphics-devices-using-discrete-device-assignment"></a>Distribuire i dispositivi di grafica usando discreti dispositivo assegnazione

>Si applica a: Microsoft Hyper-V Server 2016, Windows Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019  

A partire da Windows Server 2016, è possibile utilizzare discreti dispositivo assegnazione o DDA, passare un intero dispositivo PCIe in una macchina virtuale.  Ciò consentirà l'accesso ad alte prestazioni per i dispositivi, ad esempio [NVMe archiviazione](./Deploying-storage-devices-using-dda.md) o schede grafiche all'interno di una macchina virtuale durante la possibilità di sfruttare il driver di dispositivi nativi.  Visitare il [pianificare la distribuzione di dispositivi utilizzando discreti dispositivo assegnazione](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) per altri dettagli su quali dispositivi di lavoro, quali sono le implicazioni di sicurezza e così via.

Esistono tre passaggi per l'uso di un dispositivo con l'assegnazione dispositivo discreti:
-   Configurare la macchina virtuale per l'assegnazione dispositivo discreti
-   Smontare il dispositivo nella partizione Host
-   Assegnazione di dispositivo per la macchina virtuale Guest

Comando tutto può essere eseguito nell'Host in una console di Windows PowerShell come amministratore.

## <a name="configure-the-vm-for-dda"></a>Configurare la macchina virtuale per DDA
Assegnazione dispositivo discreti impone alcune limitazioni relative alle macchine virtuali ed è necessario eseguire il passaggio seguente.

1.  Configurare il "azione arresto automatico" di una macchina virtuale di spegnimento eseguendo

```
Set-VM -Name VMName -AutomaticStopAction TurnOff
```

### <a name="some-additional-vm-preparation-is-required-for-graphics-devices"></a>Alcune attività di preparazione VM aggiuntiva è necessaria per i dispositivi di grafica

Alcuni dispositivi hardware offre prestazioni migliori se la macchina virtuale in configurata in un determinato modo.  Per informazioni dettagliate sulla necessità o meno le seguenti configurazioni per l'hardware, contattare il fornitore dell'hardware. Sono disponibili dettagli aggiuntivi sul [pianificare la distribuzione di dispositivi Usa l'assegnazione dispositivo discreti](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) e in questo [post di blog.](https://blogs.technet.microsoft.com/virtualization/2015/11/23/discrete-device-assignment-gpus/)

1. Abilitare la combinazione di scrittura sulla CPU
   ```
   Set-VM -GuestControlledCacheTypes $true -VMName VMName
   ```
2. Configurare lo spazio MMIO a 32 bit
   ```
   Set-VM -LowMemoryMappedIoSpace 3Gb -VMName VMName
   ```
3. Configurare maggiore spazio MMIO a 32 bit
   ```
   Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName VMName
   ```
   Si noti che i valori di spazio MMIO riportato sopra sono valori accettabili da impostare per la sperimentazione con una singola GPU.  Se dopo l'avvio della macchina virtuale, il dispositivo segnala un errore relativo a risorse insufficienti, si sarà probabilmente necessario modificare questi valori.  Inoltre, se si assegnano più GPU, è necessario aumentare anche questi valori.

## <a name="dismount-the-device-from-the-host-partition"></a>Smontare il dispositivo nella partizione Host
### <a name="optional---install-the-partitioning-driver"></a>Facoltativo: installare il Driver di partizionamento
Assegnazione dispositivo discreti offrono Vendor di hardware la possibilità di fornire un driver di mitigazione dei rischi di sicurezza con i propri dispositivi.  Si noti che questo driver non è lo stesso come il driver di dispositivo che verrà installato nella macchina virtuale guest.  Ha fino a discrezione del fornitore dell'hardware per fornire questo driver, tuttavia, se offrono, installarlo prima per smontare il dispositivo dalla partizione host.  Contattare il fornitore dell'hardware per altre informazioni su se dispongono di un driver di mitigazione
> Se non viene fornito alcun driver di partizionamento, durante lo smontaggio è necessario usare il `-force` opzione per ignorare l'avviso di sicurezza. Per altre informazioni sulle implicazioni di sicurezza di questa operazione sul [pianificare la distribuzione di dispositivi utilizzando discreti dispositivo assegnazione](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md).

### <a name="locating-the-devices-location-path"></a>Individuazione Location Path del dispositivo
Il percorso del PCI è necessario smontare e montare il dispositivo dall'Host.  Un esempio di percorso di percorso simile al seguente: `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`.  Altre informazioni sul percorso il percorso è reperibile qui: [Pianificare la distribuzione di dispositivi utilizzando discreti dispositivo assegnazione](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md).

### <a name="disable-the-device"></a>Disabilitare il dispositivo
Usa PowerShell o Gestione dispositivi, verificare che il dispositivo viene "disabilitato".  

### <a name="dismount-the-device"></a>Smontare il dispositivo
A seconda se il fornitore ha fornito un driver di mitigazione, ovvero dovrai usare la "-force" opzione oppure No.
- Se è stato installato un Driver di mitigazione
  ```
  Dismount-VMHostAssignableDevice -LocationPath $locationPath
  ```
- Se non è stato installato un Driver di mitigazione
  ```
  Dismount-VMHostAssignableDevice -force -LocationPath $locationPath
  ```

## <a name="assigning-the-device-to-the-guest-vm"></a>Assegnazione di dispositivo per la macchina virtuale Guest
Il passaggio finale consiste nell'indicare Hyper-V che una macchina virtuale deve avere accesso al dispositivo.  Oltre al percorso trovato sopra, è necessario conoscere il nome della macchina virtuale.

```
Add-VMAssignableDevice -LocationPath $locationPath -VMName VMName
```

## <a name="whats-next"></a>Quali sono le novità
Dopo che un dispositivo è stato montato in una macchina virtuale, sarà ora possibile avviare tale macchina virtuale e interagire con il dispositivo come si farebbe normalmente se è in esecuzione in un sistema bare metal.  Ciò significa che a questo punto è in grado di installare i driver del fornitore dell'Hardware nella macchina virtuale e le applicazioni saranno in grado di visualizzare l'hardware presenti.  È possibile verificarlo aprendo Gestione dispositivi nella VM Guest e vedere che l'hardware viene ora visualizzato.

## <a name="removing-a-device-and-returning-it-to-the-host"></a>Rimozione di un dispositivo e la restituzione all'Host
Se si desidera restituire egli dispositivo torna allo stato originale, è necessario arrestare la macchina virtuale ed emettere la seguente:
```
#Remove the device from the VM
Remove-VMAssignableDevice -LocationPath $locationPath -VMName VMName
#Mount the device back in the host
Mount-VMHostAssignableDevice -LocationPath $locationPath
```
È quindi possibile abilitare nuovamente il dispositivo in Gestione dispositivi e il sistema operativo host saranno in grado di interagire con il dispositivo nuovamente.

## <a name="examples"></a>Esempi

### <a name="mounting-a-gpu-to-a-vm"></a>Montaggio di una GPU a una macchina virtuale
In questo esempio è usare PowerShell per configurare una macchina virtuale denominata "ddatest1" per richiedere prima GPU disponibili dal produttore NVIDIA e assegnarlo alla macchina virtuale.  
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
