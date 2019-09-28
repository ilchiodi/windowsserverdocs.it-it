---
title: Accesso diretto a memoria remota (RDMA) e Switch Embedded Teaming (SET)
description: In questo argomento vengono fornite informazioni sulla configurazione delle interfacce di accesso diretto a memoria remota (RDMA) con Hyper-V in Windows Server 2016, oltre a informazioni su switch Embedded Teaming (SET).
manager: brianlic
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 68c35b64-4d24-42be-90c9-184f2b5f19be
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b39cac842f115a1828c666eec52f17f80971510c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365686"
---
# <a name="remote-direct-memory-access-rdma-and-switch-embedded-teaming-set"></a>Accesso diretto a memoria remota \(RDMA @ no__t-1 e switch Embedded Teaming \(IMPOSTARE @ no__t-3

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Questo argomento fornisce informazioni sulla configurazione di accesso diretto a memoria remota \(RDMA @ no__t-1 interfacce con Hyper-V in Windows Server 2016, oltre a informazioni su switch Embedded Teaming \(IMPOSTARE @ no__t-3.  

> [!NOTE]
> Oltre a questo argomento, è disponibile il seguente switch Embedded Teaming content. 
> - Download della raccolta TechNet: [Guida dell'utente di Windows Server 2016 e switch Embedded Teaming](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0)

## <a name="bkmk_rdma"></a>Configurazione delle interfacce RDMA con Hyper-V  

In Windows Server 2012 R2, l'uso di RDMA e Hyper-V nello stesso computer delle schede di rete che forniscono i servizi RDMA non può essere associato a un Commuter virtuale Hyper-V. In questo modo si aumenta il numero di schede di rete fisiche che è necessario installare nell'host Hyper-V.

>[!TIP]
>Nelle edizioni di Windows Server precedenti a Windows Server 2016, non è possibile configurare RDMA nelle schede di rete associate a un gruppo NIC o a un Commuter virtuale Hyper-V. In Windows Server 2016 è possibile abilitare RDMA nelle schede di rete associate a un Commuter virtuale Hyper-V con o senza SET.

In Windows Server 2016 è possibile usare un numero inferiore di schede di rete quando si usa RDMA con o senza SET.

Nell'immagine seguente vengono illustrate le modifiche apportate all'architettura software tra Windows Server 2012 R2 e Windows Server 2016.

![Modifiche dell'architettura](../media/RDMA-and-SET/rdma_over.jpg)

Le sezioni seguenti forniscono istruzioni su come usare i comandi di Windows PowerShell per abilitare Data Center Bridging (DCB), creare un Commuter virtuale Hyper-V con una scheda di interfaccia di rete virtuale RDMA \(vNIC @ no__t-1 e creare un Commuti virtuale Hyper-V con SET e RDMA schede.

### <a name="enable-data-center-bridging-dcb"></a>Abilitare Data Center Bridging \(DCB @ no__t-1

Prima di usare una versione di RDMA Ethernet \(roce\) convergente RDMA, è necessario abilitare DCB.  Sebbene non sia necessario per il protocollo Internet wide area RDMA \(iWARP @ no__t-1 Networks, il test ha determinato che tutte le tecnologie RDMA basate su Ethernet funzionano meglio con DCB. Per questo motivo, è consigliabile usare DCB anche per le distribuzioni RDMA iWARP.

I comandi di esempio di Windows PowerShell seguenti illustrano come abilitare e configurare DCB per SMB diretto.

Attiva DCB

    Install-WindowsFeature Data-Center-Bridging

Impostare un criterio per SMB-Direct:

    New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3

Attivare il controllo di flusso per SMB:

    Enable-NetQosFlowControl  -Priority 3

Verificare che il controllo di flusso sia disattivato per altro traffico:

    Disable-NetQosFlowControl  -Priority 0,1,2,4,5,6,7

Applicare i criteri agli adattatori di destinazione:

    Enable-NetAdapterQos  -Name "SLOT 2"

Assegnare a SMB diretto il 30% della larghezza di banda minima:

`New-NetQosTrafficClass "SMB"  -Priority 3  -BandwidthPercentage 30  -Algorithm ETS`  

Se nel sistema è installato un debugger del kernel, è necessario configurare il debugger per consentire l'impostazione di QoS eseguendo il comando seguente.

Eseguire l'override del debugger. per impostazione predefinita, il debugger blocca NetQos:
 
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force

### <a name="create-a-hyper-v-virtual-switch-with-an-rdma-vnic"></a>Creare un Commuter virtuale Hyper-V con un vNIC RDMA

Se il valore impostato non è necessario per la distribuzione, è possibile usare i comandi di Windows PowerShell seguenti per creare un Commuter virtuale Hyper-V con un vNIC RDMA.

> [!NOTE]
> L'uso di SET team con schede di rete fisiche con supporto per RDMA offre più risorse RDMA per il schede.

    New-VMSwitch -Name RDMAswitch -NetAdapterName "SLOT 2"

Aggiungere schede host e renderli RDMA in grado di supportare:

    Add-VMNetworkAdapter -SwitchName RDMAswitch -Name SMB_1
    Enable-NetAdapterRDMA "vEthernet (SMB_1)" "SLOT 2"

Verificare le funzionalità di RDMA:

    Get-NetAdapterRdma

###  <a name="bkmk_set-rdma"></a>Creazione di un Commuter virtuale Hyper-V con SET e RDMA schede

Per usare RDMA funzionalità sulle schede di rete virtuali dell'host Hyper-V \(vNICs @ no__t-1 in un Commuter virtuale Hyper-V che supporta il gruppo di RDMA, è possibile usare questi comandi di Windows PowerShell di esempio.

    New-VMSwitch -Name SETswitch -NetAdapterName "SLOT 2","SLOT 3" -EnableEmbeddedTeaming $true

Aggiungere schede host:

    Add-VMNetworkAdapter -SwitchName SETswitch -Name SMB_1 -managementOS
    Add-VMNetworkAdapter -SwitchName SETswitch -Name SMB_2 -managementOS

Molti commutatori non passano informazioni sulle classi di traffico sul traffico VLAN senza tag, quindi assicurarsi che le schede host per RDMA si trovino sulle VLAN. Questo esempio assegna le due schede virtuali host SMB_ * alla VLAN 42.
    
    Set-VMNetworkAdapterIsolation -ManagementOS -VMNetworkAdapterName SMB_1  -IsolationMode VLAN -DefaultIsolationID 42
    Set-VMNetworkAdapterIsolation -ManagementOS -VMNetworkAdapterName SMB_2  -IsolationMode VLAN -DefaultIsolationID 42
    

Abilitare RDMA nell'host schede:

    Enable-NetAdapterRDMA "vEthernet (SMB_1)","vEthernet (SMB_2)" "SLOT 2", "SLOT 3"

Verificare le funzionalità di RDMA; Verificare che le funzionalità siano diverse da zero:

    Get-NetAdapterRdma | fl *


## <a name="switch-embedded-teaming-set"></a>Switch Embedded Teaming (SET)  

In questa sezione viene fornita una panoramica di switch Embedded Teaming (SET) in Windows Server 2016 e sono incluse le sezioni seguenti.

- [Panoramica SET](#bkmk_over)

- [Imposta disponibilità](#bkmk_avail)

- [NIC supportate e non supportate per il SET](#bkmk_nics)

- [CONFIGURARE la compatibilità con le tecnologie di rete di Windows Server](#bkmk_compat)

- [IMPOSTARE le modalità e le impostazioni](#bkmk_modes)

- [SET e code di macchine virtuali (code macchine virtuali)](#bkmk_vmq)

- [SET e virtualizzazione rete Hyper-V (HNV)](#bkmk_hnv)

- [IMPOSTA e Live Migration](#bkmk_live)

- [Indirizzo MAC usato nei pacchetti trasmessi](#bkmk_mac)

- [Gestione di un team di SET](#bkmk_manage)

## <a name="bkmk_over"></a>Panoramica SET

SET è una soluzione di gruppo NIC alternativa che è possibile usare in ambienti che includono Hyper-V e lo stack software defined networking \(SDN @ no__t-1 in Windows Server 2016. SET integra alcune funzionalità di gruppo NIC nel Commuter virtuale Hyper-V.

SET consente di raggruppare una o otto schede di rete Ethernet fisiche in una o più schede di rete virtuali basate su software. Queste schede di rete virtuali garantiscono prestazioni elevate e tolleranza di errore in caso di errore delle schede di rete.

IMPOSTARE le schede di rete del membro devono essere tutte installate nello stesso host Hyper-V fisico da inserire in un team.

> [!NOTE]
> L'uso di SET è supportato solo nel commutire virtuale Hyper-V in Windows Server 2016. Non è possibile distribuire SET in Windows Server 2012 R2.

È possibile connettere le NIC raggruppate allo stesso commutatore fisico o a diversi commutatori fisici. Se si connettono le schede NIC a diversi commutatori, entrambi i commutatori devono trovarsi nella stessa subnet.

Nella figura seguente viene illustrata l'architettura del SET.

![IMPOSTA architettura](../media/RDMA-and-SET/set_architecture.jpg)

Poiché il SET è integrato nel Commuter virtuale Hyper-V, non è possibile usare SET all'interno di una macchina virtuale (VM). È tuttavia possibile usare il gruppo NIC all'interno delle macchine virtuali.

Per altre informazioni, vedere [Gruppo NIC in macchine virtuali (VM)](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nict-vms).

Inoltre, l'architettura SET non espone le interfacce del team. Al contrario, è necessario configurare le porte del Commuter virtuale Hyper-V.

## <a name="bkmk_avail"></a>Imposta disponibilità

Il SET è disponibile in tutte le versioni di Windows Server 2016 che includono Hyper-V e lo stack SDN. Inoltre, è possibile utilizzare i comandi di Windows PowerShell e le connessioni Desktop remoto per gestire i SET da computer remoti che eseguono un sistema operativo client su cui sono supportati gli strumenti.

## <a name="bkmk_nics"></a>NIC supportate per SET

È possibile usare qualsiasi scheda di interfaccia di rete Ethernet che ha superato la qualifica hardware di Windows e il logo \(WHQL @ no__t-1 test in un gruppo SET in Windows Server 2016. Per impostare è necessario che tutte le schede di rete che sono membri di un team di SET siano identiche \(i. e., lo stesso produttore, lo stesso modello, lo stesso firmware e il driver @ no__t-1. SET supporta tra una e otto schede di rete in un team.
  
## <a name="bkmk_compat"></a>CONFIGURARE la compatibilità con le tecnologie di rete di Windows Server

Il SET è compatibile con le tecnologie di rete seguenti in Windows Server 2016.

- Data Center Bridging \(DCB @ no__t-1
  
- Virtualizzazione rete Hyper-V: NV-GRE e VxLAN sono entrambi supportati in Windows Server 2016.  
- Offload del checksum sul lato ricezione \(IPv4, IPv6, TCP @ no__t-1-sono supportati se uno dei membri del team SET li supporta.

- Accesso diretto a memoria remota \(RDMA @ no__t-1

- Single Root I/O Virtualization \(SR-IOV @ no__t-1

- Offload di checksum sul lato trasmissione \(IPv4, IPv6, TCP @ no__t-1-sono supportati se tutti i membri del team impostati li supportano.

- Code di macchine virtuali \(VMQ @ no__t-1

- Receive-Side Scaling virtuale \(RSS @ no__t-1

Il SET non è compatibile con le tecnologie di rete seguenti in Windows Server 2016.

- autenticazione 802.1 x. i pacchetti 802.1 x Extensible Authentication Protocol \(EAP @ no__t-1 vengono eliminati automaticamente dal Commuter virtuale Hyper @ no__t-2V in scenari di impostazione.
 
- Offload attività IPsec \(IPsecTO @ no__t-1. Si tratta di una tecnologia legacy che non è supportata dalla maggior parte delle schede di rete e da dove esiste, è disabilitata per impostazione predefinita.

- Uso di QoS @no__t -0pacer. exe @ no__t-1 in host o nei sistemi operativi nativi. Questi scenari QoS non sono scenari Hyper @ no__t-0V, quindi le tecnologie non si intersecano. Inoltre, QoS è disponibile ma non è abilitato per impostazione predefinita. è necessario abilitare intenzionalmente QoS.

- Ricezione Unione lato \(RSC @ no__t-1. RSC viene disabilitato automaticamente da Hyper @ no__t-0V Virtual Switch.

- Receive-Side Scaling \(RSS @ no__t-1. Poiché Hyper-V usa le code per VMQ e VMMQ, RSS viene sempre disabilitato quando si crea un commutire virtuale.

- TCP Chimney Offload. Questa tecnologia è disabilitata per impostazione predefinita.

- QoS della macchina virtuale \(VM-QoS @ no__t-1. La funzionalità QoS della macchina virtuale è disponibile ma è disabilitata per impostazione predefinita. Se si configura la funzionalità QoS della macchina virtuale in un ambiente SET, le impostazioni QoS provocheranno risultati imprevedibili.

## <a name="bkmk_modes"></a>IMPOSTARE le modalità e le impostazioni

A differenza del gruppo NIC, quando si crea un gruppo di SET, non è possibile configurare un nome Team. Inoltre, l'utilizzo di una scheda standby è supportato nel gruppo NIC, ma non è supportato nel SET. Quando si distribuisce il SET, tutte le schede di rete sono attive e nessuna è in modalità standby.

Un'altra differenza principale tra gruppo NIC e SET è che gruppo NIC consente di scegliere tra tre diverse modalità di raggruppamento, mentre SET supporta solo la modalità **Switch Independent** . Con la modalità indipendente dal commutatore, il commutatore o i commutatori a cui i membri del team SET sono connessi non sono consapevoli della presenza del team di SET e non determinano come distribuire il traffico di rete per impostare i membri del team. in alternativa, il team SET distribuisce la rete in ingresso traffico tra i membri del team SET.

Quando si crea un nuovo team di SET, è necessario configurare le seguenti proprietà del team.

- Adattatori membro

- Modalità di bilanciamento del carico

### <a name="member-adapters"></a>Adattatori membro

Quando si crea un gruppo di SET, è necessario specificare fino a otto schede di rete identiche associate al commessore virtuale Hyper-V come adattatori membri del team.

### <a name="load-balancing-mode"></a>Modalità di bilanciamento del carico

Le opzioni per impostare la modalità di distribuzione del bilanciamento del carico del team sono **porta Hyper-V** e **dinamica**.

**Porta Hyper-V**

Le macchine virtuali sono connesse a una porta nel Commuter virtuale Hyper-V. Quando si usa la modalità porta Hyper-V per i team impostati, la porta del Commuter virtuale Hyper-V e l'indirizzo MAC associato vengono usati per dividere il traffico di rete tra i membri del team SET.

> [!NOTE]
> Quando si usa SET insieme a Packet Direct, la modalità gruppo è **indipendente** e la **porta Hyper-V** in modalità di bilanciamento del carico è obbligatoria.

Poiché l'opzione adiacente Visualizza sempre un particolare indirizzo MAC su una determinata porta, il Commuter distribuisce il carico in ingresso (il traffico dal passaggio all'host) alla porta in cui si trova l'indirizzo MAC. Questa operazione è particolarmente utile quando si usano le code di macchine virtuali (code macchine virtuali), perché una coda può essere inserita nella scheda di interfaccia di rete specifica in cui è previsto il traffico.

Tuttavia, se l'host dispone solo di poche VM, questa modalità potrebbe non essere sufficientemente granulare per ottenere una distribuzione ben bilanciata. Questa modalità consente inoltre di limitare sempre una singola macchina virtuale, ovvero il traffico da una singola porta di commutazione, alla larghezza di banda disponibile su una singola interfaccia.

**Dinamico**

Questa modalità di bilanciamento del carico offre i vantaggi seguenti.

- I caricamenti in uscita vengono distribuiti in base a un hash delle porte e degli indirizzi IP TCP.  La modalità dinamica ribilancia inoltre i caricamenti in tempo reale, in modo che un determinato flusso in uscita possa spostarsi tra i membri del team SET.

- I caricamenti in ingresso vengono distribuiti allo stesso modo della modalità della porta Hyper-V.

I carichi in uscita in questa modalità sono bilanciati in modo dinamico in base al concetto di flowlets. Proprio come la sintesi vocale umana ha interruzioni naturali alle estremità di parole e frasi, i flussi TCP (flussi di comunicazione TCP) hanno anche interruzioni naturalmente. La parte di un flusso TCP tra due interruzioni di questo tipo è denominata flowlet.

Quando l'algoritmo in modalità dinamica rileva che è stato rilevato un limite flowlet, ad esempio quando si è verificata un'interrotta di lunghezza sufficiente nel flusso TCP, l'algoritmo ribilancia automaticamente il flusso a un altro membro del team, se appropriato.  In alcune circostanze non comuni, anche l'algoritmo potrebbe ribilanciare periodicamente i flussi che non contengono alcun flowlets. Per questo motivo, l'affinità tra il flusso TCP e il membro del team può cambiare in qualsiasi momento, poiché l'algoritmo di bilanciamento dinamico funziona per bilanciare il carico di lavoro dei membri del team.

## <a name="bkmk_vmq"></a>SET e code di macchine virtuali (code macchine virtuali)

VMQ e SET funzionano bene insieme ed è necessario abilitare VMQ quando si usa Hyper-V e impostare.

> [!NOTE]
> SET Visualizza sempre il numero totale di code disponibili in tutti i membri del team impostati. Nel gruppo NIC questa operazione viene definita modalità Sum-of-queues.

Per la maggior parte delle schede di rete sono disponibili code che possono essere utilizzate per Receive-Side Scaling \(RSS @ no__t-1 o VMQ, ma non entrambi nello stesso momento.
  
Alcune impostazioni di VMQ sembrano essere impostazioni per le code RSS ma sono in realtà impostazioni sulle code generiche usate da RSS e da VMQ a seconda della funzionalità attualmente in uso. Ogni scheda di interfaccia di rete ha, nelle proprietà avanzate, i valori per `*RssBaseProcNumber` e `*MaxRssProcessors`.

Di seguito sono riportate alcune impostazioni VMQ che garantiscono prestazioni di sistema migliori.

- Idealmente, ogni scheda di interfaccia di rete deve avere il `*RssBaseProcNumber` impostato su un numero pari maggiore o uguale a due (2). Ciò è dovuto al fatto che il primo processore fisico, Core 0 \(logical 0 e 1 @ no__t-1, in genere esegue la maggior parte dell'elaborazione del sistema, in modo che l'elaborazione della rete venga eliminata dal processore fisico. 

>[!NOTE]
>Alcune architetture di computer non hanno due processori logici per processore fisico, quindi per tali macchine il processore di base deve essere maggiore o uguale a 1. In caso di dubbi, si supponga che l'host usi un processore logico 2 per ogni architettura del processore fisico.

- I processori dei membri del team devono essere, nel senso che è pratico, non sovrapposto. Ad esempio, in un host a 4 core \(8 processori logici @ no__t-1 con un team di due schede di rete 10Gbps, è possibile impostare la prima per l'uso del processore di base 2 e per l'uso di 4 core. il secondo verrebbe impostato in modo da usare il processore di base 6 e usare 2 Core.

## <a name="bkmk_hnv"></a>SET e virtualizzazione rete Hyper-V \(HNV @ no__t-2

Il SET è completamente compatibile con la virtualizzazione rete Hyper-V in Windows Server 2016. Il sistema di gestione di HNV fornisce informazioni al driver di SET che consente a di distribuire il carico del traffico di rete in modo che sia ottimizzato per il traffico HNV.
  
## <a name="bkmk_live"></a>IMPOSTA e Live Migration

Live Migration è supportato in Windows Server 2016.

## <a name="bkmk_mac"></a>Indirizzo MAC usato nei pacchetti trasmessi

Quando si configura un team SET con la distribuzione dinamica del carico, i pacchetti da un'unica origine \(such come singola VM @ no__t-1 vengono distribuiti simultaneamente tra più membri del team. 

Per impedire che i commutatori siano confusi e per evitare la visualizzazione di avvisi MAC, impostare sostituisce l'indirizzo MAC di origine con un indirizzo MAC diverso nei frame trasmessi ai membri del team diversi dal membro del team creata un'affinità. Per questo motivo, ogni membro del team usa un indirizzo MAC diverso e i conflitti di indirizzi MAC vengono evitati, a meno che non si verifichino errori.

Quando viene rilevato un errore nella scheda di interfaccia di rete primaria, il software SET Teaming inizia a usare l'indirizzo MAC della macchina virtuale nel membro del team scelto per fungere da membro del team creata un'affinità temporaneo \(i. e., quello che ora verrà visualizzato come interfaccia della VM @ No_ _T-1.

Questa modifica si applica solo al traffico che verrà inviato al membro del team creata un'affinità della macchina virtuale con l'indirizzo MAC di origine della macchina virtuale. Il traffico continua a essere inviato con qualsiasi indirizzo MAC di origine usato prima dell'errore.

Di seguito sono elencati gli elenchi che descrivono il comportamento di sostituzione degli indirizzi MAC in base alla configurazione del team:

- In modalità indipendente dal cambio con la distribuzione della porta Hyper-V

    - Ogni porta vmSwitch è creata un'affinità a un membro del team
  
    - Ogni pacchetto viene inviato al membro del team a cui è creata un'affinità la porta  
  
    - Non è stata eseguita alcuna sostituzione del MAC di origine  
  
- In modalità indipendente dal cambio con distribuzione dinamica
  
    - Ogni porta vmSwitch è creata un'affinità a un membro del team  
  
    - Tutti i pacchetti ARP/NS vengono inviati al membro del team a cui è creata un'affinità la porta  
  
    - I pacchetti inviati al membro del team che corrisponde al membro del team creata un'affinità non hanno completato la sostituzione degli indirizzi MAC di origine  
  
    - I pacchetti inviati a un membro del team diverso dal membro del team creata un'affinità avranno completato la sostituzione degli indirizzi MAC di origine  
  
## <a name="bkmk_manage"></a>Gestione di un team di SET

È consigliabile usare System Center Virtual Machine Manager \(VMM @ no__t-1 per gestire i team del SET. Tuttavia, è anche possibile usare Windows PowerShell per gestire il SET. Le sezioni seguenti forniscono i comandi di Windows PowerShell che è possibile usare per gestire i SET.

Per informazioni su come creare un gruppo di SET usando VMM, vedere la sezione "configurare un commutatore logico" nell'argomento della libreria VMM di System Center [creare commutatori logici](https://docs.microsoft.com/system-center/vmm/network-switch).
  
### <a name="create-a-set-team"></a>Creare un gruppo di SET

È necessario creare un team di SET nello stesso momento in cui si crea il Commuter virtuale Hyper-V usando il comando **New-VMSwitch** di Windows PowerShell.

Quando si crea il Commuter virtuale Hyper-V, è necessario includere il nuovo parametro **EnableEmbeddedTeaming** nella sintassi del comando. Nell'esempio seguente viene creato un commutire Hyper-V denominato **TeamedvSwitch** con gruppo incorporato e due membri del team iniziali.
  
```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2" -EnableEmbeddedTeaming $true  
```  
  
Il parametro **EnableEmbeddedTeaming** viene utilizzato da Windows PowerShell quando l'argomento di **NetAdapterName** è una matrice di NIC anziché una singola scheda di interfaccia di rete. Di conseguenza, è possibile modificare il comando precedente nel modo seguente.

```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2"  
```  

Se si vuole creare un commute con supporto per SET con un singolo membro del team in modo che sia possibile aggiungere un membro del team in un secondo momento, è necessario usare il parametro EnableEmbeddedTeaming.

```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1" -EnableEmbeddedTeaming $true  
```  

### <a name="adding-or-removing-a-set-team-member"></a>Aggiunta o rimozione di un membro del team SET

Il comando **set-VMSwitchTeam** include l'opzione **NetAdapterName** . Per modificare i membri del team in un gruppo di SET, immettere l'elenco di membri del team desiderato dopo l'opzione **NetAdapterName** . Se **TeamedvSwitch** è stato originariamente creato con NIC 1 e NIC 2, il comando di esempio seguente elimina il set di membri del team "NIC 2" e aggiunge nuovo set del membro del team "NIC 3".
  
```  
Set-VMSwitchTeam -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 3"  
```  

### <a name="removing-a-set-team"></a>Rimozione di un team SET

È possibile rimuovere un gruppo di SET solo rimuovendo il Commuter virtuale Hyper-V che contiene il team impostato.  Usare l'argomento [Remove-VMSwitch](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/remove-vmswitch) per informazioni su come rimuovere il Commuter virtuale Hyper-V. Nell'esempio seguente viene rimosso un commutire virtuale denominato **SETvSwitch**.

```  
Remove-VMSwitch "SETvSwitch"  
```  

### <a name="changing-the-load-distribution-algorithm-for-a-set-team"></a>Modifica dell'algoritmo di distribuzione del carico per un team SET

Il cmdlet **set-VMSwitchTeam** ha un'opzione **LoadBalancingAlgorithm** . Questa opzione accetta uno dei due valori possibili: **HyperVPort** o **Dynamic**. Per impostare o modificare l'algoritmo di distribuzione del carico per un team con commutatore incorporato, usare questa opzione. 

Nell'esempio seguente, il VMSwitchTeam denominato **TeamedvSwitch** usa l'algoritmo di bilanciamento del carico **dinamico** .  
```  
Set-VMSwitchTeam -Name TeamedvSwitch -LoadBalancingAlgorithm Dynamic  
```  
### <a name="affinitizing-virtual-interfaces-to-physical-team-members"></a>Interfacce virtuali affinità fra per i membri del team fisico

SET consente di creare un'affinità tra un'interfaccia virtuale \(i. e., la porta del Commuter virtuale Hyper-V @ no__t-1 e una delle schede di interfaccia di rete fisiche del team. 

Se, ad esempio, si creano due host schede per SMB @ no__t-0Direct, come nella sezione [creare un Commutire virtuale Hyper-V con set e RDMA schede](#bkmk_set-rdma), è possibile assicurarsi che i due schede usino membri del team diversi. 

Aggiungendo allo script in questa sezione, è possibile usare i comandi di Windows PowerShell seguenti.

    Set-VMNetworkAdapterTeamMapping -VMNetworkAdapterName SMB_1 –ManagementOS –PhysicalNetAdapterName “SLOT 2”
    Set-VMNetworkAdapterTeamMapping -VMNetworkAdapterName SMB_2 –ManagementOS –PhysicalNetAdapterName “SLOT 3”

Questo argomento viene esaminato in modo più approfondito nella sezione 4.2.5 della scheda di interfaccia di rete di [Windows Server 2016 e nella Guida per l'utente del team di switch Embedded](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0).
