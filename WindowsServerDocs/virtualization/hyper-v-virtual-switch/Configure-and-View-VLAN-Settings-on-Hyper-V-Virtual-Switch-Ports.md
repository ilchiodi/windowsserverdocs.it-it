---
title: Configurare e visualizzare le impostazioni VLAN sulle porte del commutatore virtuale Hyper-V
description: È possibile utilizzare questo argomento per apprendere le procedure consigliate per la configurazione e la visualizzazione delle impostazioni di rete LAN (VLAN) virtuali su una porta di commutatore virtuale Hyper-V in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: article
ms.assetid: 69e0e28a-98ae-4ade-bd27-ce2ad7eb310f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1e4843b0ffee86d728736ae212b953bb7c8552c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820552"
---
# <a name="configure-and-view-vlan-settings-on-hyper-v-virtual-switch-ports"></a>Configurare e visualizzare le impostazioni VLAN sulle porte del commutatore virtuale Hyper-V

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per apprendere le procedure consigliate per la configurazione e la visualizzazione delle impostazioni di rete LAN (VLAN) virtuali su una porta di commutatore virtuale Hyper-V.

Quando si desidera configurare le impostazioni VLAN sulle porte di commutazione virtuale Hyper-V, è possibile usare entrambi Windows&reg; Server 2016 Hyper-V Manager o System Center Virtual Machine Manager (VMM).

Se si usa VMM, VMM Usa il comando Windows PowerShell seguente per configurare la porta del commutatore.

```
Set-VMNetworkAdapterIsolation <VM-name|-managementOS> -IsolationMode VLAN -DefaultIsolationID <vlan-value> -AllowUntaggedTraffic $True
```
Se non si usa VMM e si sta configurando la porta del commutatore in Windows Server, è possibile usare la console di gestione di Hyper-V o il comando seguente di Windows PowerShell.
```
Set-VMNetworkAdapterVlan <VM-name|-managementOS> -Access -VlanID <vlan-value>
```

A causa di questi due metodi per configurare le impostazioni VLAN sulle porte con commutatore virtuale Hyper-V, è possibile che quando si prova a visualizzare le impostazioni della porta commutatore, viene visualizzato all'utente che le impostazioni VLAN sono configurate non - anche quando sono configurati.

## <a name="use-the-same-method-to-configure-and-view-switch-port-vlan-settings"></a>Usare il metodo di stesso per configurare e visualizzare le impostazioni VLAN porta del commutatore

Per garantire che non si verifichino questi problemi, è necessario usare lo stesso metodo per visualizzare le impostazioni VLAN della porta commutatore usato per configurare le impostazioni VLAN della porta commutatore.

Per configurare e visualizzare le impostazioni porta del commutatore VLAN, è necessario eseguire le operazioni seguenti:

- Se si usa VMM o un Controller di rete per configurare e gestire la rete ed è stata distribuita una rete SDN (Software Defined), è necessario usare il **VMNetworkAdapterIsolation** cmdlet. 
- Se si usano i cmdlet di Windows Server 2016 Hyper-V Manager o Windows PowerShell e di rete SDN (Software Defined) non sono stati distribuiti, è necessario usare il **VMNetworkAdapterVlan** cmdlet.

### <a name="possible-issues"></a>Possibili problemi

Se non si seguono queste linee guida si potrebbero verificarsi i problemi seguenti.

- Nei casi in cui sono stati distribuiti SDN e si usa VMM, Controller di rete o la **VMNetworkAdapterIsolation** cmdlet per configurare le impostazioni VLAN in una porta del commutatore virtuale Hyper-V: Se si usa Hyper-V Manager oppure **ottenere VMNetworkAdapterVlan** per visualizzare le impostazioni di configurazione, l'output del comando non visualizzerà le impostazioni VLAN. È invece necessario usare il **Get-VMNetworkIsolation** cmdlet per visualizzare le impostazioni VLAN.
- Nei casi in cui non sono distribuiti SDN e invece utilizzare Gestione Hyper-V o il **VMNetworkAdapterVlan** cmdlet per configurare le impostazioni VLAN in una porta del commutatore virtuale Hyper-V: Se si usa la **Get-VMNetworkIsolation** cmdlet per visualizzare le impostazioni di configurazione, l'output del comando non visualizzerà le impostazioni VLAN. È invece necessario usare il **VMNetworkAdapterVlan ottenere** cmdlet per visualizzare le impostazioni VLAN.

È anche importante non dovrà tentare di configurare le impostazioni VLAN della porta commutatore stesso usando entrambi i metodi di configurazione. In questo caso, la porta di commutazione non è configurata correttamente e il risultato potrebbe trattarsi di un errore nella comunicazione di rete.

### <a name="resources"></a>Risorse

Per altre informazioni sui comandi di Windows PowerShell menzionati in questo argomento, vedere gli argomenti seguenti:

- [Set-VmNetworkAdapterIsolation](https://technet.microsoft.com/library/dn464283.aspx)
- [Get-VmNetworkAdapterIsolation](https://technet.microsoft.com/library/dn464277.aspx)
- [Set-VMNetworkAdapterVlan](https://technet.microsoft.com/library/hh848475.aspx)
- [Get-VMNetworkAdapterVlan](https://technet.microsoft.com/library/hh848516.aspx)





