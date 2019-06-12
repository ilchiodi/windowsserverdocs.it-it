---
title: Utilizzo di spazi di archiviazione diretta in una macchina virtuale
ms.prod: windows-server-threshold
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 10/25/2017
description: Come distribuire spazi di archiviazione diretta in un cluster guest di macchina virtuale, ad esempio, in Microsoft Azure.
ms.localizationpriority: medium
ms.openlocfilehash: b99e750b78654df48ad3b412269511d047e3057c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447816"
---
# <a name="using-storage-spaces-direct-in-guest-virtual-machine-clusters"></a>Utilizzo di spazi di archiviazione diretta in cluster di macchine virtuali guest

> Si applica a: Windows Server 2019, Windows Server 2016

È possibile distribuire spazi di archiviazione diretta in un cluster di server fisici o in cluster guest di macchina virtuale come descritto in questo argomento. Questo tipo di distribuzione offre archiviazione condivisa virtuale in un set di macchine virtuali all'inizio di un cloud privato o pubblico soluzioni a disponibilità elevata dell'applicazione possono essere usati per migliorare la disponibilità delle applicazioni.

![](media/storage-spaces-direct-in-vm/storage-spaces-direct-in-vm.png)

## <a name="deploying-in-azure-iaas-vm-guest-clusters"></a>Distribuzione in cluster guest di VM Iaas di Azure

[I modelli di Azure](https://github.com/robotechredmond/301-storage-spaces-direct-md) è stati pubblicati riduzione della complessità, configurare migliori procedure consigliate e velocità delle distribuzioni di spazi di archiviazione diretta in una macchina virtuale Iaas di Azure. Questa è la soluzione consigliata per la distribuzione in Azure.

<iframe src="https://channel9.msdn.com/Series/Microsoft-Hybrid-Cloud-Best-Practices-for-IT-Pros/Step-by-Step-Deploy-Windows-Server-2016-Storage-Spaces-Direct-S2D-Cluster-in-Microsoft-Azure/player" width="960" height="540" allowfullscreen></iframe>

## <a name="requirements"></a>Requisiti

Le considerazioni seguenti riguardano la distribuzione di spazi di archiviazione diretta in un ambiente virtualizzato.

> [!TIP]
> I modelli di Azure configura automaticamente le seguenti considerazioni per l'utente e sono la soluzione consigliata per la distribuzione in macchine virtuali IaaS di Azure.

-   Almeno 2 nodi e un massimo di 3 nodi

-   le distribuzioni di 2 nodi devono configurare un server di controllo (Cloud di controllo o controllo di condivisione File)

-   le distribuzioni di 3 nodi in grado di tollerare 1 nodo verso il basso e la perdita di 1 o più dischi in un altro nodo.  Se 2 nodi vengono arrestati quindi i dischi virtuali sarà offline fino a quando non restituisce uno dei nodi.  

-   Configurare le macchine virtuali da distribuire nei domini di errore

    -   Azure-configurare il Set di disponibilità

    -   Hyper-V: Configura AntiAffinityClassNames nelle VM per separare le macchine virtuali tra nodi

    -   VMware – regola Configura anti-affinità delle macchine Virtuali della macchina virtuale creando una Rule DRS di tipo ' macchine virtuali Separate "per separare le macchine virtuali tra host ESX. Dischi presentati per l'uso con spazi di archiviazione diretta devono utilizzare l'adapter Paravirtual SCSI (PVSCSI). Per il supporto PVSCSI con Windows Server, consultare https://kb.vmware.com/s/article/1010398.

-   Sfruttare la bassa latenza / archiviazione ad alte prestazioni, archiviazione Premium di Azure managed disks sono necessari

-   Distribuire una progettazione di archiviazione semplice senza dispositivi di memorizzazione nella cache configurata

-   Minimo 2 dischi di dati virtuale presentati a ogni macchina virtuale (VHD / VHDX / VMDK)

    Questo numero è diverso rispetto alle distribuzioni bare metal perché i dischi virtuali possono essere implementati come i file che non sono soggetti a errori di fisici.

-   Disabilitare le funzionalità di sostituzione automatica dell'unità del servizio integrità eseguendo il cmdlet di PowerShell seguente:

    ```powershell
    Get-storagesubsystem clus* | set-storagehealthsetting -name “System.Storage.PhysicalDisk.AutoReplace.Enabled” -value “False”
    ```

-   Non supportati: Snapshot del disco virtuale a livello di host/ripristino

    Per eseguire il backup e ripristinare i dati nei volumi di spazi di archiviazione diretta, utilizzare soluzioni di backup a livello di guest tradizionale.

-   Per consentire maggiore resilienza alle possibili VHD / VHDX / latenza di archiviazione di file VMDK in cluster guest, aumentare il valore di timeout dei / o spazi di archiviazione:

    `HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\spaceport\\Parameters\\HwTimeout`

    `dword: 00007530`

    L'equivalente decimale 7530 esadecimale è 30000, ovvero 30 secondi. Si noti che il valore predefinito è 1770 esadecimale o 6000 decimale, ovvero 6 secondi.

## <a name="see-also"></a>Vedere anche

[Modelli di macchina virtuale Iaas di Azure aggiuntivi per la distribuzione di spazi di archiviazione diretta, video e guide dettagliate](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/).

[Panoramica di spazi di ulteriore spazio di archiviazione diretta](https://docs.microsoft.com/en-us/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
