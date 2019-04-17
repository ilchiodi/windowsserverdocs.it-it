---
title: Uso di spazi di archiviazione diretta in una macchina virtuale
ms.prod: windows-server-threshold
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 10/25/2017
description: Come distribuire spazi di archiviazione diretta in un cluster di macchina virtuale guest, ad esempio, in Microsoft Azure.
ms.localizationpriority: medium
ms.openlocfilehash: a741475d09ab7f7ab29f158ef55378ca9a37beac
ms.sourcegitcommit: 52883945fd6e6bb5c24bd81944ca4bc0c5e6a216
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2018
ms.locfileid: "8964613"
---
# Uso di spazi di archiviazione diretta in cluster di macchina virtuale guest

> Si applica a: Windows Server 2019, Windows Server 2016

È possibile distribuire spazi di archiviazione diretta in un cluster di server fisici o nei cluster guest macchina virtuale come illustrato in questo argomento. Questo tipo di distribuzione offre virtuale archiviazione condivisa tra un set di macchine virtuali sopra un cloud privato o pubblico, in modo che le soluzioni di disponibilità elevata di applicazione possono essere usate per aumentare la disponibilità delle applicazioni.

![](media/storage-spaces-direct-in-vm/storage-spaces-direct-in-vm.png)

## Distribuzione di cluster guest della macchina virtuale di Azure Iaas

[Modelli di Azure](https://github.com/robotechredmond/301-storage-spaces-direct-md) sono state pubblicate riduzione della complessità, configurare meglio procedure consigliate e la velocità delle distribuzioni di spazi di archiviazione diretta in una macchina virtuale di Azure Iaas. Questa è la soluzione consigliata per la distribuzione in Azure.

<iframe src="https://channel9.msdn.com/Series/Microsoft-Hybrid-Cloud-Best-Practices-for-IT-Pros/Step-by-Step-Deploy-Windows-Server-2016-Storage-Spaces-Direct-S2D-Cluster-in-Microsoft-Azure/player" width="960" height="540" allowfullscreen></iframe>

## Requisiti

Le considerazioni seguenti si applicano durante la distribuzione di spazi di archiviazione diretta in un ambiente virtualizzato.

> [!TIP]
> Modelli di Azure automaticamente Configura le seguenti considerazioni per te e sono la soluzione consigliata durante la distribuzione in macchine virtuali di Azure IaaS.

-   Almeno 2 nodi e massimo di 3 nodi

-   le distribuzioni di 2 nodi devono configurare un controllo (controllo di condivisione File o un controllo Cloud)

-   le distribuzioni di 3 nodi possono tollerare 1 nodo verso il basso e la perdita di 1 o più dischi in un altro nodo.  Se 2 nodi sono arresto quindi i dischi virtuali, essere offline fino a quando non restituisce uno dei nodi.  

-   Configurare le macchine virtuali per essere distribuito in domini di errore

    -   Azure-configurare il Set di disponibilità

    -   Hyper-V: Configura AntiAffinityClassNames su macchine virtuali per separare le macchine virtuali tra i nodi

    -   VMware – regola Configura macchina anti-affinità creando una Rule DRS di tipo ' Separate le macchine virtuali "per separare le macchine virtuali tra host ESX. I dischi presentati da usare con spazi di archiviazione diretta devono Usa la scheda Paravirtual SCSI (PVSCSI). Per il supporto PVSCSI con Windows Server, consultare https://kb.vmware.com/s/article/1010398.

-   Sfruttare a bassa latenza / archiviazione ad alte prestazioni - archiviazione Premium di Azure gestiti i dischi sono necessari

-   Distribuire una struttura di archiviazione flat con senza memorizzazione nella cache i dispositivi configurati

-   Minimo di 2 dischi di dati virtuale presentati per ogni macchina virtuale (VHD / VHDX / VMDK)

    Questo numero è diverso rispetto alle distribuzioni bare metal perché i dischi virtuali possono essere implementati come file che non sono soggetti a errori fisici.

-   Disabilitare le funzionalità sostituzione dell'unità automatica nel servizio integrità eseguendo il cmdlet PowerShell seguente:

    ```powershell
    Get-storagesubsystem clus* | set-storagehealthsetting -name “System.Storage.PhysicalDisk.AutoReplace.Enabled” -value “False”
    ```

-   Non supportato: ospitare livello disco virtuale snapshot/ripristino

    Usare invece soluzioni di backup livello guest tradizionale per il backup e ripristinare i dati nei volumi spazi di archiviazione diretta.

-   Per dare maggiore resilienza a possibili VHD / VHDX / latenza di archiviazione VMDK in cluster guest, aumentare il valore di timeout dei / o spazi di archiviazione:

    `HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\spaceport\\Parameters\\HwTimeout`

    `dword: 00007530`

    L'equivalente decimale di 7530 esadecimale è 30000, che è di 30 secondi. Tieni presente che il valore predefinito è 1770 6000 o esadecimale decimale, ovvero 6 secondi.

## Vedi anche

[Modelli aggiuntivi Azure Iaas VM per distribuire spazi di archiviazione diretta, video e guide dettagliate](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure/).

[Spazi di archiviazione aggiuntivi diretta Panoramica] (https://docs.microsoft.com/en-us/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
