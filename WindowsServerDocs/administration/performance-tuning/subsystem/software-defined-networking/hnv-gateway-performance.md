---
title: Ottimizzazione delle prestazioni del gateway HNV in reti definite da software
description: Linee guida per l'ottimizzazione delle prestazioni del gateway HNV su reti definite da software
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: grcusanz; AnPaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 907b160b143af18a8ede3a9a7975fa8b22753118
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383501"
---
# <a name="hnv-gateway-performance-tuning-in-software-defined-networks"></a>Ottimizzazione delle prestazioni del gateway HNV in reti definite da software

In questo argomento vengono fornite le specifiche hardware e le indicazioni di configurazione per i server che eseguono Hyper-V e che ospitano macchine virtuali Windows Server gateway, oltre ai parametri di configurazione per le macchine virtuali (VM) gateway Windows Server . Per estrarre le prestazioni ottimali dalle macchine virtuali del gateway Windows Server, si prevede che queste linee guida verranno seguite.
Nelle sezioni seguenti sono elencati i requisiti di configurazione e hardware quando si distribuisce Windows Server Gateway.
1. Consigli relativi all'hardware Hyper-V
2. Configurazione host di Hyper-V
3. Configurazione della macchina virtuale del gateway Windows Server

## <a name="hyper-v-hardware-recommendations"></a>Consigli relativi all'hardware Hyper-V

Di seguito è riportata la configurazione hardware minima consigliata per ogni server che esegue Windows Server 2016 e Hyper-V.

| Componente server               | Specifiche                                                                                                                                                                                                                                                                   |
|--------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| CPU  | Nodi NUMA (non-Uniform Memory Architecture): 2 <br> Se nell'host sono presenti più macchine virtuali del gateway Windows Server, per ottenere prestazioni ottimali, ogni macchina virtuale del gateway deve avere accesso completo a un nodo NUMA. E deve essere diverso dal nodo NUMA utilizzato dalla scheda fisica host. |
| Core per nodo NUMA            | 2                                                                                                                                                                                                                                                                               |
| Hyper-Threading                | Disattivato. Hyper-Threading non migliora le prestazioni di Windows Server Gateway.                                                                                                                                                                                           |
| Memoria ad accesso casuale (RAM)     | 48 GB                                                                                                                                                                                                                                                                           |
| Schede interfaccia di rete (NIC) | Schede NIC da 2 10 GB, le prestazioni del gateway variano a seconda della velocità di riga. Se la frequenza di riga è inferiore a 10Gbps, i numeri di velocità effettiva del tunnel del gateway diminuiscono anche per lo stesso fattore.                                                                                          |

Assicurarsi che il numero di processori virtuali assegnati a una macchina virtuale di Windows Server Gateway non superi il numero di processori presenti nel nodo NUMA. Ad esempio, se in un nodo NUMA sono presenti 8 core, il numero di processori virtuali deve essere inferiore o uguale a 8. Per ottenere prestazioni ottimali, deve essere 8. Per verificare il numero di nodi NUMA e il numero di core per ogni nodo NUMA, eseguire lo script di Windows PowerShell seguente in ogni host Hyper-V:

```PowerShell
$nodes = [object[]] $(gwmi –Namespace root\virtualization\v2 -Class MSVM_NumaNode)
$cores = ($nodes | Measure-Object NumberOfProcessorCores -sum).Sum
$lps = ($nodes | Measure-Object NumberOfLogicalProcessors -sum).Sum


Write-Host "Number of NUMA Nodes: ", $nodes.count
Write-Host ("Total Number of Cores: ", $cores)
Write-Host ("Total Number of Logical Processors: ", $lps)
```

>[!Important]
> L'allocazione di processori virtuali tra i nodi NUMA potrebbe influire negativamente sulle prestazioni di Windows Server Gateway. L'esecuzione di più macchine virtuali, ciascuna delle quali dispone di processori virtuali di un nodo NUMA, consente di ottenere prestazioni aggregate migliori di quelle di una singola macchina virtuale a cui sono assegnati tutti i processori virtuali.

Quando si seleziona il numero di macchine virtuali del gateway da installare in ogni host Hyper-V quando ogni nodo NUMA ha otto core, è consigliabile usare una VM del gateway con otto processori virtuali e almeno 8GB di RAM. In questo caso, un nodo NUMA è dedicato al computer host.

## <a name="hyper-v-host-configuration"></a>Configurazione host Hyper-V

Di seguito è riportata la configurazione consigliata per ogni server che esegue Windows Server 2016 e Hyper-V e il cui carico di lavoro consiste nell'eseguire macchine virtuali Windows Server gateway. Queste istruzioni di configurazione includono l'utilizzo di esempi di comandi di Windows PowerShell. Tali esempi contengono segnaposto per i valori effettivi che è necessario fornire quando si eseguono i comandi nel proprio ambiente. I segnaposto per i nomi delle schede di rete, ad esempio, sono "NIC1" e "NIC2". Quando si eseguono i comandi che utilizzano questi segnaposto, sostituirli con i nomi effettivi delle schede di rete dei server in uso. In caso contrario, i comandi non verranno eseguiti correttamente.

>[!Note]
> Per eseguire i comandi di Windows PowerShell seguenti, è necessario appartenere al gruppo Administrators.

| Elemento di configurazione                          | Configurazione di Windows PowerShell                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Switch Embedded Teaming                     | Quando si crea un vswitch con più schede di rete, il switch Embedded Teaming viene abilitato automaticamente per gli adapter. <br> ```New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2"``` <br> Il gruppo tradizionale tramite LBFO non è supportato con SDN in Windows Server 2016. Switch Embedded Teaming consente di usare lo stesso set di schede di rete per il traffico virtuale e il traffico RDMA. Questa operazione non è supportata con gruppo NIC basato su LBFO.                                                        |
| Regolazione di interrupt nelle schede NIC fisiche       | Utilizzare le impostazioni predefinite. Per controllare la configurazione, è possibile usare il comando seguente di Windows PowerShell: ```Get-NetAdapterAdvancedProperty```                                                                                                                                                                                                                                                                                                                                                                    |
| Dimensioni buffer di ricezione nelle schede NIC fisiche       | È possibile verificare se le schede di rete fisiche supportano la configurazione di questo parametro eseguendo il comando ```Get-NetAdapterAdvancedProperty```. Se questo parametro non è supportato, l'output del comando non includerà la proprietà "buffer di ricezione". Se le schede NIC supportano il parametro, è possibile utilizzare il comando di Windows PowerShell seguente per impostare le dimensioni del buffer di ricezione: <br>```Set-NetAdapterAdvancedProperty "NIC1" –DisplayName "Receive Buffers" –DisplayValue 3000``` <br>                          |
| Dimensione buffer invio nelle schede NIC fisiche          | È possibile verificare se le schede di rete fisiche supportano la configurazione di questo parametro eseguendo il comando ```Get-NetAdapterAdvancedProperty```. Se le schede NIC non supportano questo parametro, l'output del comando non includerà la proprietà "buffer di invio". Se le schede NIC supportano il parametro, è possibile utilizzare il comando di Windows PowerShell seguente per impostare le dimensioni del buffer di invio: <br> ```Set-NetAdapterAdvancedProperty "NIC1" –DisplayName "Transmit Buffers" –DisplayValue 3000``` <br>                           |
| Receive Side Scaling (RSS) nelle schede NIC fisiche | È possibile verificare se le schede di rete fisiche dispongono di RSS abilitato eseguendo il comando di Windows PowerShell Get-NetAdapterRss. Per abilitare e configurare RSS nelle schede di rete, è possibile utilizzare i comandi di Windows PowerShell seguenti: <br> ```Enable-NetAdapterRss "NIC1","NIC2"```<br> ```Set-NetAdapterRss "NIC1","NIC2" –NumberOfReceiveQueues 16 -MaxProcessors``` <br> NOTA: Se VMMQ o VMQ è abilitato, non è necessario abilitare RSS sulle schede di rete fisiche. È possibile abilitarlo nelle schede di rete virtuali dell'host |
| VMMQ                                        | Per abilitare VMMQ per una macchina virtuale, eseguire il comando seguente: <br> ```Set-VmNetworkAdapter -VMName <gateway vm name>,-VrssEnabled $true -VmmqEnabled $true``` <br> NOTA: Non tutte le schede di rete supportano VMMQ. Attualmente è supportata nelle serie Chelsio T5 e T6, Mellanox CX-3 e CX-4 e QLogic 45xxx                                                                                                                                                                                                                                      |
| Coda macchine virtuali (VMQ) nel gruppo NIC | È possibile abilitare VMQ nel team di SET usando il comando di Windows PowerShell seguente: <br>```Enable-NetAdapterVmq``` <br> NOTA: Questa operazione deve essere abilitata solo se il HW non supporta VMMQ. Se supportato, è necessario abilitare VMMQ per ottenere prestazioni migliori.                                                                                                                                                                                                                                                               |
>[!Note]
> VMQ e vRSS sono disponibili solo quando il carico della VM è elevato e la CPU viene utilizzata al massimo. Solo a questo punto, sarà presente almeno un core del processore. VMQ e vRSS saranno utili per consentire la distribuzione del carico di elaborazione tra più core. Questa operazione non è applicabile al traffico IPsec perché il traffico IPsec è limitato a un singolo core.

## <a name="windows-server-gateway-vm-configuration"></a>Configurazione delle macchine virtuali di Windows Server Gateway

In entrambi gli host Hyper-V è possibile configurare più macchine virtuali configurate come gateway con Windows Server gateway. È possibile utilizzare Gestione commutatori virtuali per creare un commutatore virtuale Hyper-V associato al gruppo NIC nell'host Hyper-V. Si noti che per ottenere prestazioni ottimali, è consigliabile distribuire una singola VM del gateway in un host Hyper-V.
Di seguito è illustrata la configurazione consigliata per ogni macchina virtuale di Windows Server Gateway.

| Elemento di configurazione                 | Configurazione di Windows PowerShell                                                                                                                                                                                                                                                                                                                                                               |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Memoria                             | 8 GB                                                                                                                                                                                                                                                                                                                                                                                           |
| Numero di schede di rete virtuali | 3 NIC con i seguenti usi specifici: 1 per la gestione utilizzata dal sistema operativo di gestione, 1 esterno che fornisce l'accesso a reti esterne, 1 interno che fornisce l'accesso solo alle reti interne.                                                                                                                                                            |
| Receive Side Scaling (RSS)         | È possibile salvare le impostazioni RSS predefinite per la scheda di interfaccia di rete di gestione. La configurazione dell'esempio seguente è applicata a una macchina virtuale con 8 processori virtuali. Per le schede di rete esterne e interne, è possibile abilitare RSS con BaseProcNumber impostato su 0 e MaxRssProcessors impostato su 8 usando il comando di Windows PowerShell seguente: <br> ```Set-NetAdapterRss "Internal","External" –BaseProcNumber 0 –MaxProcessorNumber 8``` <br> |
| Buffer sul lato di invio                   | È possibile usare le impostazioni predefinite del buffer di invio per la scheda di interfaccia di rete di gestione. Per le schede NIC interne ed esterne è possibile configurare il buffer sul lato di invio con 32 MB di RAM usando il comando seguente di Windows PowerShell: <br> ```Set-NetAdapterAdvancedProperty "Internal","External" –DisplayName "Send Buffer Size" –DisplayValue "32MB"``` <br>                                                       |
| Buffer sul lato di ricezione                | È possibile usare le impostazioni predefinite del buffer di ricezione per la scheda di interfaccia di rete di gestione. Per le schede NIC interne ed esterne, è possibile configurare il buffer sul lato di ricezione con 16 MB di RAM usando il comando di Windows PowerShell seguente: <br> ```Set-NetAdapterAdvancedProperty "Internal","External" –DisplayName "Receive Buffer Size" –DisplayValue "16MB"``` <br>                                            |
| Forward Optimization               | È possibile usare le impostazioni di ottimizzazione diretta predefinite per la scheda di interfaccia di rete di gestione. Per le schede di rete interne ed esterne, è possibile abilitare l'ottimizzazione in diretta usando il comando di Windows PowerShell seguente: <br> ```Set-NetAdapterAdvancedProperty "Internal","External" –DisplayName "Forward Optimization" –DisplayValue "1"``` <br>                                                                      |
