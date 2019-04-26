---
title: Le prestazioni del Gateway HNV ottimizzazione nel Software definivano reti
description: Prestazioni del gateway HNV ottimizzazione le linee guida sul Software definito reti
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: grcusanz; AnPaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 217428b84a00b2e2231a15cb3878d0abcec1d9ad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827322"
---
# <a name="hnv-gateway-performance-tuning-in-software-defined-networks"></a>Le prestazioni del Gateway HNV ottimizzazione nel Software definivano reti

In questo argomento include specifiche hardware e consigli di configurazione per i server che eseguono Hyper-V e ospita le macchine virtuali di Windows Server Gateway, oltre ai parametri di configurazione per le macchine virtuali di Windows Server Gateway (macchine virtuali) . Per estrarre migliori prestazioni dalle macchine virtuali del gateway Windows Server, è probabile che vengano seguite le linee guida.
Nelle sezioni seguenti sono elencati i requisiti di configurazione e hardware quando si distribuisce Windows Server Gateway.
1. Consigli relativi all'hardware Hyper-V
2. Configurazione host di Hyper-V
3. Configurazione della macchina virtuale di Windows Server gateway

## <a name="hyper-v-hardware-recommendations"></a>Consigli relativi all'hardware Hyper-V

Di seguito è la configurazione hardware minima consigliata per ogni server che esegue Hyper-V e Windows Server 2016.

| Componente server               | Specifica                                                                                                                                                                                                                                                                   |
|--------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| CPU  | Nodi di Strumentazione gestione Windows (NUMA, non-Uniform Memory Architecture): 2 <br> Se sono presenti più gateway di Windows Server le macchine virtuali nell'host, per ottenere prestazioni ottimali, ogni VM del gateway devono avere accesso completo a un solo nodo NUMA. E deve essere diverso dal nodo NUMA utilizzato dalla scheda fisica dell'host. |
| Core per nodo NUMA            | 2                                                                                                                                                                                                                                                                               |
| Hyper-Threading                | Disattivato. Hyper-Threading non migliora le prestazioni di Windows Server Gateway.                                                                                                                                                                                           |
| Memoria ad accesso casuale (RAM)     | 48 GB                                                                                                                                                                                                                                                                           |
| Schede interfaccia di rete (NIC) | Due schede di rete da 10 GB, le prestazioni del gateway dipenderanno la velocità di riga. Se la velocità è inferiore a 10 Gbps, i numeri di velocità effettiva gateway tunnel passerà anche verso il basso per lo stesso fattore.                                                                                          |

Assicurarsi che il numero di processori virtuali assegnati a una macchina virtuale di Windows Server Gateway non superi il numero di processori presenti nel nodo NUMA. Ad esempio, se in un nodo NUMA sono presenti 8 core, il numero di processori virtuali deve essere inferiore o uguale a 8. Per prestazioni ottimali, deve essere 8. Per verificare il numero di nodi NUMA e il numero di core per ogni nodo NUMA, eseguire lo script di Windows PowerShell seguente in ogni host Hyper-V:

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

Una VM del gateway con otto processori virtuali e almeno 8GB di RAM è consigliabile quando si seleziona il numero di macchine virtuali gateway installare in ogni host Hyper-V quando ogni nodo NUMA sono presenti otto core. In questo caso, un nodo NUMA è dedicato al computer host.

## <a name="hyper-v-host-configuration"></a>Configurazione di Host Hyper-V

Di seguito è la configurazione consigliata per ogni server che esegue Windows Server 2016 e Hyper-V e il cui carico di lavoro consiste nell'eseguire macchine virtuali di Windows Server Gateway. Queste istruzioni di configurazione includono l'utilizzo di esempi di comandi di Windows PowerShell. Tali esempi contengono segnaposto per i valori effettivi che è necessario fornire quando si eseguono i comandi nel proprio ambiente. Ad esempio, i segnaposto di nome scheda di rete sono "NIC1? e "NIC2.? Quando si eseguono i comandi che utilizzano questi segnaposto, sostituirli con i nomi effettivi delle schede di rete dei server in uso. In caso contrario, i comandi non verranno eseguiti correttamente.

>[!Note]
> Per eseguire i comandi di Windows PowerShell seguenti, è necessario appartenere al gruppo Administrators.

| Elemento di configurazione                          | Configurazione di Windows Powershell                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Switch Embedded Teaming                     | Quando si crea un commutatore virtuale con più schede di rete, abilitato automaticamente switch embedded teaming per tali schede. <br> ```New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2"``` <br> Gruppo NIC tradizionali tramite LBFO non è supportato con SDN in Windows Server 2016. Passare al gruppo incorporato consente di usare lo stesso set di schede di rete per il traffico virtuale e il traffico RDMA. Ciò non è supportato con gruppo NIC base LBFO.                                                        |
| Regolazione di interrupt nelle schede NIC fisiche       | Utilizzare le impostazioni predefinite. Per controllare la configurazione, è possibile usare il comando Windows PowerShell seguente: ```Get-NetAdapterAdvancedProperty```                                                                                                                                                                                                                                                                                                                                                                    |
| Dimensioni buffer di ricezione nelle schede NIC fisiche       | È possibile verificare se le schede NIC fisiche supportano la configurazione di questo parametro eseguendo il comando ```Get-NetAdapterAdvancedProperty```. Se non supportano questo parametro, l'output del comando non include la proprietà "Receive Buffers.? Se le schede NIC supportano il parametro, è possibile utilizzare il comando di Windows PowerShell seguente per impostare le dimensioni del buffer di ricezione: <br>```Set-NetAdapterAdvancedProperty “NIC1�? –DisplayName “Receive Buffers�? –DisplayValue 3000``` <br>                          |
| Dimensione buffer invio nelle schede NIC fisiche          | È possibile verificare se le schede NIC fisiche supportano la configurazione di questo parametro eseguendo il comando ```Get-NetAdapterAdvancedProperty```. Se questo parametro non supportano le schede di rete, l'output del comando non include la proprietà "Send Buffers.? Se le schede NIC supportano il parametro, è possibile utilizzare il comando di Windows PowerShell seguente per impostare le dimensioni del buffer di invio: <br> ```Set-NetAdapterAdvancedProperty “NIC1�? –DisplayName “Transmit Buffers�? –DisplayValue 3000``` <br>                           |
| Receive Side Scaling (RSS) nelle schede NIC fisiche | È possibile verificare se le schede NIC fisiche è abilitato RSS eseguendo il comando di Windows PowerShell Get-NetAdapterRss. È possibile usare i comandi di Windows PowerShell seguenti per abilitare e configurare RSS nelle schede di rete: <br> ```Enable-NetAdapterRss “NIC1�?,�?NIC2�?```<br> ```Set-NetAdapterRss “NIC1�?,�?NIC2�? –NumberOfReceiveQueues 16 -MaxProcessors``` <br> NOTA: Pokud jsou VMMQ nebo VMQ, RSS non deve essere abilitata nelle schede di rete fisica. È possibile abilitarlo nelle schede di rete virtuale host |
| VMMQ                                        | Per abilitare VMMQ per una macchina virtuale, eseguire il comando seguente: <br> ```Set-VmNetworkAdapter -VMName <gateway vm name>,-VrssEnabled $true -VmmqEnabled $true``` <br> NOTA: Non tutte le schede di rete supportano VMMQ. Attualmente è supportata in serie 45xxx Chelsio T5 e T6 Mellanox CX-3 e 4 di CX e QLogic                                                                                                                                                                                                                                      |
| Coda macchine virtuali (VMQ) nel gruppo NIC | È possibile abilitare VMQ nel team SET usando il comando Windows PowerShell seguente: <br>```Enable-NetAdapterVmq``` <br> NOTA: Questa opzione deve essere abilitata solo se l'hardware non supporta VMMQ. Se supportato, VMMQ deve essere abilitata per ottenere prestazioni migliori.                                                                                                                                                                                                                                                               |
>[!Note]
> Coda macchine Virtuali e vRSS entrano in immagine solo quando il carico della macchina virtuale è elevato e la CPU viene utilizzata per il valore massimo. Solo allora sarà almeno un processore core max out. Coda macchine Virtuali e vRSS potranno quindi essere utile per ripartire il carico di elaborazione tra più core. Non applicabile per il traffico IPsec come traffico IPsec è limitato a un singolo core.

## <a name="windows-server-gateway-vm-configuration"></a>Configurazione delle macchine virtuali di Windows Server Gateway

In entrambi gli host Hyper-V, è possibile configurare più macchine virtuali configurate come gateway con Windows Server Gateway. È possibile utilizzare Gestione commutatori virtuali per creare un commutatore virtuale Hyper-V associato al gruppo NIC nell'host Hyper-V. Si noti che per ottenere prestazioni ottimali, è consigliabile distribuire un singolo gateway della macchina virtuale in un host Hyper-V.
Di seguito è illustrata la configurazione consigliata per ogni macchina virtuale di Windows Server Gateway.

| Elemento di configurazione                 | Configurazione di Windows Powershell                                                                                                                                                                                                                                                                                                                                                               |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Memoria                             | 8 GB                                                                                                                                                                                                                                                                                                                                                                                           |
| Numero di schede di rete virtuali | Usa 3 schede di rete con le specifiche seguenti: 1 per la gestione utilizzato dal sistema operativo di gestione, 1 esterno che fornisce l'accesso a reti esterne, 1 esterne1 interna che fornisce l'accesso solo alle reti interne.                                                                                                                                                            |
| Receive Side Scaling (RSS)         | È possibile mantenere il valore predefinito delle impostazioni di RSS per la scheda NIC di gestione. La configurazione dell'esempio seguente è applicata a una macchina virtuale con 8 processori virtuali. Per le schede NIC interna ed esterna, è possibile abilitare RSS con BaseProcNumber impostato su 0 e MaxRssProcessors impostato su 8 mediante il comando Windows PowerShell seguente: <br> ```Set-NetAdapterRss “Internal�?,�?External�? –BaseProcNumber 0 –MaxProcessorNumber 8``` <br> |
| Buffer sul lato di invio                   | È possibile mantenere il valore predefinito delle impostazioni di Send Side Buffer per il gruppo NIC di gestione. Per le interna ed esterna schede di rete è possibile configurare Send Side Buffer con 32 MB di RAM mediante il comando Windows PowerShell seguente: <br> ```Set-NetAdapterAdvancedProperty “Internal�?,�?External�? –DisplayName “Send Buffer Size�? –DisplayValue “32MB�?``` <br>                                                       |
| Buffer sul lato di ricezione                | È possibile mantenere il valore predefinito delle impostazioni di Receive Side Buffer per il gruppo NIC di gestione. Per le interna ed esterna schede di rete, è possibile configurare Receive Side Buffer con 16 MB di RAM mediante il comando Windows PowerShell seguente: <br> ```Set-NetAdapterAdvancedProperty “Internal�?,�?External�? –DisplayName “Receive Buffer Size�? –DisplayValue “16MB�?``` <br>                                            |
| Forward Optimization               | È possibile mantenere il valore predefinito delle impostazioni di Forward Optimization per la scheda NIC di gestione. Per le interna ed esterna schede di rete, è possibile abilitare Forward Optimization mediante il comando Windows PowerShell seguente: <br> ```Set-NetAdapterAdvancedProperty “Internal�?,�?External�? –DisplayName “Forward Optimization�? –DisplayValue “1�?``` <br>                                                                      |
