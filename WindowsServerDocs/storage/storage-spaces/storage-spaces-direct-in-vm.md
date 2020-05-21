---
title: Uso di Spazi di archiviazione diretta in una macchina virtuale
description: Come distribuire Spazi di archiviazione diretta in un cluster guest di macchine virtuali, ad esempio in Microsoft Azure.
ms.prod: windows-server
ms.author: eldenc
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 10/25/2017
ms.localizationpriority: medium
ms.openlocfilehash: 816e589cbb7ed4196411b8f5bab740c7ee5f7595
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436766"
---
# <a name="using-storage-spaces-direct-in-guest-virtual-machine-clusters"></a>Uso di Spazi di archiviazione diretta nei cluster di macchine virtuali guest

> Si applica a: Windows Server 2019, Windows Server 2016

È possibile distribuire Spazi di archiviazione diretta in un cluster di server fisici o in cluster guest di macchine virtuali, come descritto in questo argomento. Questo tipo di distribuzione offre archiviazione condivisa virtuale in un set di macchine virtuali in un cloud privato o pubblico, in modo che le soluzioni di disponibilità elevata dell'applicazione possano essere usate per aumentare la disponibilità delle applicazioni.

![](media/storage-spaces-direct-in-vm/storage-spaces-direct-in-vm.png)

## <a name="deploying-in-azure-iaas-vm-guest-clusters"></a>Distribuzione nei cluster guest di macchine virtuali IaaS di Azure

I [modelli di Azure](https://github.com/robotechredmond/301-storage-spaces-direct-md) sono stati pubblicati per ridurre la complessità, configurare le procedure consigliate e la velocità delle distribuzioni di spazi di archiviazione diretta in una VM IaaS di Azure. Si tratta della soluzione consigliata per la distribuzione in Azure.

<iframe src="https://channel9.msdn.com/Series/Microsoft-Hybrid-Cloud-Best-Practices-for-IT-Pros/Step-by-Step-Deploy-Windows-Server-2016-Storage-Spaces-Direct-S2D-Cluster-in-Microsoft-Azure/player" width="960" height="540" allowfullscreen></iframe>

## <a name="requirements"></a>Requisiti

Quando si distribuiscono Spazi di archiviazione diretta in un ambiente virtualizzato, si applicano le considerazioni seguenti.

> [!TIP]
> I modelli di Azure configureranno automaticamente le considerazioni seguenti e sono la soluzione consigliata per la distribuzione in macchine virtuali IaaS di Azure.

- Almeno 2 nodi e un massimo di 3 nodi

- le distribuzioni a 2 nodi devono configurare un server di controllo del mirroring (cloud Witness o condivisione file di controllo)

- le distribuzioni a 3 nodi possono tollerare un nodo inattivo e la perdita di uno o più dischi in un altro nodo.  Se due nodi vengono arrestati, i dischi virtuali risultano offline fino a quando uno dei nodi non restituisce.

- Configurare le macchine virtuali da distribuire tra domini di errore

    - Azure: configurare il set di disponibilità

    - Hyper-V: configurare AntiAffinityClassNames nelle VM per separare le macchine virtuali tra i nodi

    - VMware: configurare la regola di anti-affinità VM-VM creando una regola DRS di tipo ' macchine virtuali separate ' per separare le VM tra gli host ESX. I dischi presentati per l'uso con Spazi di archiviazione diretta devono usare l'adapter paravirtual SCSI (PVSCSI). Per il supporto di PVSCSI con Windows Server, vedere https://kb.vmware.com/s/article/1010398 .

- Sfruttare la bassa latenza/archiviazione ad alte prestazioni: sono necessari dischi gestiti di archiviazione Premium di Azure

- Distribuire una progettazione di archiviazione Flat senza dispositivi di memorizzazione nella cache configurata

- Almeno 2 dischi dati virtuali presentati a ogni macchina virtuale (VHD/VHDX/VMDK)

    Questo numero è diverso dalle distribuzioni bare metal perché i dischi virtuali possono essere implementati come file non suscettibili a errori fisici.

- Disabilitare le funzionalità di sostituzione automatica dell'unità nel Servizio integrità eseguendo il cmdlet di PowerShell seguente:

    ```powershell
          Get-storagesubsystem clus* | set-storagehealthsetting -name "System.Storage.PhysicalDisk.AutoReplace.Enabled" -value "False"
          ```

- To give greater resiliency to possible VHD / VHDX / VMDK storage latency in guest clusters, increase the Storage Spaces I/O timeout value:

    `HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\spaceport\\Parameters\\HwTimeout`

    `dword: 00007530`

    The decimal equivalent of Hexadecimal 7530 is 30000, which is 30 seconds. Note that the default value is 1770 Hexadecimal, or 6000 Decimal, which is 6 seconds.

## Not supported

- Host level virtual disk snapshot/restore

    Instead use traditional guest level backup solutions to backup and restore the data on the Storage Spaces Direct volumes.

- Host level virtual disk size change

    The virtual disks exposed through the virtual machine must retain the same size and characteristics. Adding more capacity to the storage pool can be accomplished by adding more virtual disks to each of the virtual machines and adding them to the pool. It's highly recommended to use virtual disks of the same size and characteristics as the current virtual disks.

## See also

- [Additional Azure Iaas VM templates for deploying Storage Spaces Direct, videos, and step-by-step guides](https://techcommunity.microsoft.com/t5/Failover-Clustering/Deploying-IaaS-VM-Guest-Clusters-in-Microsoft-Azure/ba-p/372126).

- [Additional Storage Spaces Direct Overview](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
