---
title: Configurare e visualizzare le impostazioni VLAN sulle porte del commutatore virtuale Hyper-V
description: È possibile utilizzare questo argomento per apprendere le procedure consigliate per la configurazione e la visualizzazione delle impostazioni della rete locale virtuale (VLAN) in una porta del commutire virtuale Hyper-V in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: article
ms.assetid: 69e0e28a-98ae-4ade-bd27-ce2ad7eb310f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 28abdfe8295ad3f9fac29b8cc80aeebe2992392c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366867"
---
# <a name="configure-and-view-vlan-settings-on-hyper-v-virtual-switch-ports"></a>Configurare e visualizzare le impostazioni VLAN sulle porte del commutatore virtuale Hyper-V

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per apprendere le procedure consigliate per la configurazione e la visualizzazione delle impostazioni della rete locale virtuale (VLAN) in una porta del Commuter virtuale Hyper-V.

Per configurare le impostazioni VLAN nelle porte del Commuter virtuale Hyper-V, è possibile usare la console di gestione di Hyper-V di Windows @ no__t-0 Server 2016 o System Center Virtual Machine Manager (VMM).

Se si usa VMM, VMM usa il comando di Windows PowerShell seguente per configurare la porta di commutazione.

```
Set-VMNetworkAdapterIsolation <VM-name|-managementOS> -IsolationMode VLAN -DefaultIsolationID <vlan-value> -AllowUntaggedTraffic $True
```
Se non si usa VMM e si configura la porta di commutazione in Windows Server, è possibile usare la console di gestione di Hyper-V o il comando di Windows PowerShell seguente.
```
Set-VMNetworkAdapterVlan <VM-name|-managementOS> -Access -VlanID <vlan-value>
```

A causa di questi due metodi per la configurazione delle impostazioni VLAN sulle porte del Commuter virtuale Hyper-V, è possibile che, quando si tenta di visualizzare le impostazioni della porta di commutazione, venga visualizzato che le impostazioni VLAN non sono configurate, anche quando sono configurate.

## <a name="use-the-same-method-to-configure-and-view-switch-port-vlan-settings"></a>Usare lo stesso metodo per configurare e visualizzare le impostazioni VLAN della porta di commutazione

Per assicurarsi che non si verifichino questi problemi, è necessario usare lo stesso metodo per visualizzare le impostazioni VLAN della porta di commutazione usate per configurare le impostazioni VLAN della porta di commutazione.

Per configurare e visualizzare le impostazioni della porta del commutire VLAN, è necessario eseguire le operazioni seguenti:

- Se si usa VMM o il controller di rete per configurare e gestire la rete ed è stato distribuito il Software Defined Networking (SDN), è necessario usare i cmdlet di **VMNetworkAdapterIsolation** . 
- Se si usa la console di gestione di Hyper-V di Windows Server 2016 o i cmdlet di Windows PowerShell e non è stata distribuita la funzionalità SDN (Software Defined Networking), è necessario usare i cmdlet di **VMNetworkAdapterVlan** .

### <a name="possible-issues"></a>Possibili problemi

Se non si seguono queste linee guida, è possibile che si verifichino i seguenti problemi.

- Nei casi in cui è stato distribuito SDN e si usano VMM, il controller di rete o i cmdlet **VMNetworkAdapterIsolation** per configurare le impostazioni VLAN in una porta del commutatore virtuale Hyper-V: Se si usa la console di gestione di Hyper-V o si **ottiene VMNetworkAdapterVlan** per visualizzare le impostazioni di configurazione, nell'output del comando non vengono visualizzate le impostazioni VLAN. È invece necessario usare il cmdlet **Get-VMNetworkIsolation** per visualizzare le impostazioni VLAN.
- Nei casi in cui non è stata distribuita SDN, usare invece la console di gestione di Hyper-V o i cmdlet di **VMNetworkAdapterVlan** per configurare le impostazioni VLAN in una porta del commutatore virtuale Hyper-v: Se si usa il cmdlet **Get-VMNetworkIsolation** per visualizzare le impostazioni di configurazione, nell'output del comando non vengono visualizzate le impostazioni VLAN. È invece necessario usare il cmdlet **Get VMNetworkAdapterVlan** per visualizzare le impostazioni VLAN.

È anche importante non tentare di configurare le stesse impostazioni VLAN della porta di commutazione usando entrambi i metodi di configurazione. In tal caso, la porta di commutazione non è configurata correttamente e il risultato potrebbe essere un errore di comunicazione di rete.

### <a name="resources"></a>Risorse

Per ulteriori informazioni sui comandi di Windows PowerShell indicati in questo argomento, vedere quanto segue:

- [Set-VmNetworkAdapterIsolation](https://technet.microsoft.com/library/dn464283.aspx)
- [Get-VmNetworkAdapterIsolation](https://technet.microsoft.com/library/dn464277.aspx)
- [Set-VMNetworkAdapterVlan](https://technet.microsoft.com/library/hh848475.aspx)
- [Get-VMNetworkAdapterVlan](https://technet.microsoft.com/library/hh848516.aspx)





