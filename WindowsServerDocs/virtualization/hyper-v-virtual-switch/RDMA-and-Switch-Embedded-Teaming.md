---
title: Accesso diretto a memoria remota (RDMA) e Switch Embedded Teaming (SET)
description: In questo argomento vengono fornite informazioni sulla configurazione di interfacce di Direct accesso memoria remota (RDMA) con Hyper-V in Windows Server 2016, oltre alle informazioni su Switch Embedded Teaming (SET).
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 68c35b64-4d24-42be-90c9-184f2b5f19be
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 485da451eb092336ec93eddfadc6ffa0e677452b
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222755"
---
# <a name="remote-direct-memory-access-rdma-and-switch-embedded-teaming-set"></a>Accesso diretto a memoria remota \(RDMA\) e Switch Embedded Teaming \(impostata\)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento vengono fornite informazioni sulla configurazione di Remote Direct Memory Access \(RDMA\) interfacce con Hyper-V in Windows Server 2016, anche per informazioni su Switch Embedded Teaming \(impostare\).  

> [!NOTE]
> Oltre a questo argomento, il contenuto seguente Switch Embedded Teaming è disponibile. 
> - Download TechNet Gallery: [Interfaccia di rete di Windows Server 2016 e Switch Embedded Teaming manuale dell'utente](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0)

## <a name="bkmk_rdma"></a>Configurare le interfacce RDMA con Hyper-V  

In Windows Server 2012 R2, con RDMA e Hyper-V nello stesso computer in cui le schede di rete che forniscono servizi RDMA non possono essere associati a un commutatore virtuale Hyper-V. In questo modo si aumenta il numero di schede di rete fisica che devono essere installati in host Hyper-V.

>[!TIP]
>Nelle edizioni di Windows Server precedenti a Windows Server 2016, non è possibile configurare RDMA sulle schede di rete che sono associate a un gruppo NIC o a un commutatore virtuale Hyper-V. In Windows Server 2016, è possibile abilitare RDMA sulle schede di rete che sono associate a un commutatore virtuale Hyper-V con o senza SET.

In Windows Server 2016, è possibile usare un minor numero di schede di rete durante l'uso di RDMA con o senza SET.

L'immagine seguente illustra le modifiche di architettura software tra Windows Server 2012 R2 e Windows Server 2016.

![Modifiche all'architettura](../media/RDMA-and-SET/rdma_over.jpg)

Le sezioni seguenti forniscono istruzioni su come usare i comandi di Windows PowerShell per abilitare il Data Center Bridging (DCB), creare un commutatore virtuale Hyper-V con una scheda di rete virtuale RDMA \(vNIC\)e creare un commutatore virtuale Hyper-V con SET e Vnic RDMA.

### <a name="enable-data-center-bridging-dcb"></a>Abilitare Data Center Bridging \(DCB\)

Prima di usare qualsiasi RDMA over Converged Ethernet \(RoCE\) versione RDMA, è necessario abilitare DCB.  Sebbene non sia necessario per il protocollo Internet ampia Area RDMA \(iWARP\) reti, il test ha rilevato che tutte le tecnologie RDMA basata su Ethernet funzionano meglio con DCB. Per questo motivo, è consigliabile usare DCB anche per le distribuzioni di RDMA iWARP.

I comandi di esempio di Windows PowerShell seguenti illustrano come abilitare e configurare Data Center Bridging per SMB diretto.

Abilitare Data Center Bridging

    Install-WindowsFeature Data-Center-Bridging

Impostare un criterio per SMB diretto:

    New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3

Abilitare il controllo di flusso per SMB:

    Enable-NetQosFlowControl  -Priority 3

Verificare che il controllo di flusso è disattivato per il resto del traffico:

    Disable-NetQosFlowControl  -Priority 0,1,2,4,5,6,7

Applica i criteri per le schede di destinazione:

    Enable-NetAdapterQos  -Name "SLOT 2"

Assegnare a SMB diretto 30% della larghezza di banda minima:

`New-NetQosTrafficClass "SMB"  -Priority 3  -BandwidthPercentage 30  -Algorithm ETS`  

Se si dispone di un debugger del kernel installato nel sistema, è necessario configurare il debugger per consentire QoS da impostare eseguendo il comando seguente.

Eseguire l'override del Debugger, per impostazione predefinita il debugger Blocca NetQos:
 
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force

### <a name="create-a-hyper-v-virtual-switch-with-an-rdma-vnic"></a>Creare un commutatore virtuale Hyper-V con una scheda vNIC RDMA

Se SET non è necessaria per la distribuzione, è possibile usare i comandi di Windows PowerShell seguenti per creare un commutatore virtuale Hyper-V con una scheda di rete virtuale RDMA.

> [!NOTE]
> Utilizzando Team SET con schede di rete fisiche con supporto per RDMA fornisce altre risorse RDMA per le schede da utilizzare.

    New-VMSwitch -Name RDMAswitch -NetAdapterName "SLOT 2"

Aggiungere Vnic dell'host e li rendono RDMA in grado di supportare:

    Add-VMNetworkAdapter -SwitchName RDMAswitch -Name SMB_1
    Enable-NetAdapterRDMA "vEthernet (SMB_1)" "SLOT 2"

Verificare le funzionalità RDMA:

    Get-NetAdapterRdma

###  <a name="bkmk_set-rdma"></a>Creare un commutatore virtuale Hyper-V con SET e RDMA Vnic

Per usare RDMA consolidata disponibile in Hyper-V host le schede di rete virtuale \(Vnic\) in un commutatore virtuale Hyper-V che supporta RDMA gruppo NIC, è possibile usare questi comandi di Windows PowerShell di esempio.

    New-VMSwitch -Name SETswitch -NetAdapterName "SLOT 2","SLOT 3" -EnableEmbeddedTeaming $true

Aggiungere Vnic dell'host:

    Add-VMNetworkAdapter -SwitchName SETswitch -Name SMB_1 -managementOS
    Add-VMNetworkAdapter -SwitchName SETswitch -Name SMB_2 -managementOS

Numerosi cambi di non passare informazioni sulle classi di traffico il traffico non contrassegnato VLAN, assicurarsi che le schede di host per RDMA sono su VLAN. Questo esempio assegna i due SMB_ * host schede di rete virtuali da 42 VLAN.
    
    Set-VMNetworkAdapterIsolation -ManagementOS -VMNetworkAdapterName SMB_1  -IsolationMode VLAN -DefaultIsolationID 42
    Set-VMNetworkAdapterIsolation -ManagementOS -VMNetworkAdapterName SMB_2  -IsolationMode VLAN -DefaultIsolationID 42
    

Abilitare RDMA sulle Vnic dell'Host:

    Enable-NetAdapterRDMA "vEthernet (SMB_1)","vEthernet (SMB_2)" "SLOT 2", "SLOT 3"

Verificare le funzionalità RDMA; Assicurarsi che le funzionalità sono diverse da zero:

    Get-NetAdapterRdma | fl *


## <a name="switch-embedded-teaming-set"></a>Switch Embedded Teaming (SET)  

In questa sezione viene fornita una panoramica di Switch Embedded Teaming (SET) in Windows Server 2016 e include le sezioni seguenti.

- [Informazioni generali sui SET](#bkmk_over)

- [SET di disponibilità](#bkmk_avail)

- [Interfacce di rete supportati e non supportati per SET](#bkmk_nics)

- [IMPOSTARE la compatibilità con le tecnologie di rete di Windows Server](#bkmk_compat)

- [Modalità di SET e le impostazioni](#bkmk_modes)

- [SET e le code di macchine virtuali (Code)](#bkmk_vmq)

- [SET e la virtualizzazione rete Hyper-V (virtualizzazione rete)](#bkmk_hnv)

- [Migrazione in tempo reale e imposta](#bkmk_live)

- [Uso di indirizzi MAC nel pacchetti trasmessi](#bkmk_mac)

- [Gestione di un SET di team](#bkmk_manage)

## <a name="bkmk_over"></a>Informazioni generali sui SET

SET è una soluzione alternativa gruppo NIC che è possibile usare in ambienti che includono Hyper-V e Software Defined Networking \(SDN\) stack in Windows Server 2016. SET integra alcune funzionalità gruppo NIC nel commutatore virtuale Hyper-V.

SET consente di raggruppare tra uno e otto Ethernet schede di rete fisiche in una o più schede di rete virtuale basata su software. Queste schede di rete virtuali garantiscono prestazioni elevate e tolleranza di errore in caso di errore delle schede di rete.

Tutte le schede di rete membro SET devono essere installate nello stesso host fisico Hyper-V da inserire in un team.

> [!NOTE]
> L'uso di SET è supportata solo nel commutatore virtuale Hyper-V in Windows Server 2016. Non è possibile distribuire SET in Windows Server 2012 R2.

È possibile connettere i gruppi NIC allo stesso commutatore fisico o a diversi commutatori fisici. Se ci si connette le schede NIC a diversi commutatori, entrambe le opzioni devono essere nella stessa subnet.

La figura seguente illustra l'architettura SET.

![Architettura SET](../media/RDMA-and-SET/set_architecture.jpg)

Poiché SET è integrato nel commutatore virtuale Hyper-V, è possibile utilizzare SET all'interno di una macchina virtuale (VM). È tuttavia possibile utilizzare gruppo NIC nelle macchine virtuali.

Per altre informazioni, vedere [gruppo NIC nelle macchine virtuali (VM)](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nict-vms).

Inoltre, architettura SET non espone le interfacce di team. In alternativa, è necessario configurare porte di commutatori virtuali Hyper-V.

## <a name="bkmk_avail"></a>SET di disponibilità

SET è disponibile in tutte le versioni di Windows Server 2016 che includono Hyper-V e lo stack SDN. Inoltre, è possibile utilizzare i comandi di Windows PowerShell e le connessioni Desktop remoto per la gestione di SET da computer remoti che eseguono un sistema operativo client in cui gli strumenti sono supportati.

## <a name="bkmk_nics"></a>Interfacce di rete supportati per SET

È possibile utilizzare qualsiasi interfaccia di rete Ethernet che ha superato la qualifica di Hardware di Windows e il Logo \(WHQL\) test nel team SET in Windows Server 2016. SET richiede che tutte le schede di rete che sono membri del team SET devono essere identiche \(ad esempio, stesso produttore, stesso modello, stessa firmware e driver\). SET di supporti tra uno e otto schede di rete in un team.
  
## <a name="bkmk_compat"></a>IMPOSTARE la compatibilità con le tecnologie di rete di Windows Server

SET è compatibile con le seguenti tecnologie di rete in Windows Server 2016.

- Data Center bridging \(DCB\)
  
- Virtualizzazione rete Hyper-V-NV GRE e VxLAN sono entrambe supportate in Windows Server 2016.  
- Offload Checksum lato di ricezione \(IPv4, IPv6, TCP\) -sono supportati se uno qualsiasi dei membri del team SET supportarli.

- Accesso diretto a memoria remota \(RDMA\)

- Virtualizzazione Single root i/o \(SR-IOV\)

- Offload Checksum trasmettere lato \(IPv4, IPv6, TCP\) -sono supportate se tutti i SET di membri del team supportarli.

- Le code di macchine virtuali \(coda macchine Virtuali\)

- Virtual Receive-Side Scaling \(RSS\)

SET non è compatibile con le seguenti tecnologie di rete in Windows Server 2016.

- Autenticazione 802.1x. 802.1x Extensible Authentication Protocol \(EAP\) i pacchetti vengono rimossi automaticamente da Hyper\-V commutatore virtuale nel SET di scenari.
 
- Offload attività IPsec \(IPsecTO\). Si tratta di una tecnologia legacy che non è supportata per la maggior parte delle schede di rete e in cui esiste, è disabilitato per impostazione predefinita.

- Uso di QoS \(pacer.exe\) nell'host o i sistemi operativi nativi. Questi scenari QoS non sono Hyper\-scenari V, in modo che le tecnologie non si intersecano. Inoltre, QoS è disponibile, ma non abilitato per impostazione predefinita, è necessario abilitare intenzionalmente QoS.

- Ricezione side coalescing \(RSC\). RSC viene disabilitata automaticamente da Hyper\-V Virtual Switch.

- Receive-side scaling \(RSS\). Dato che Hyper-V Usa le code per coda macchine Virtuali e VMMQ, RSS è sempre disabilitato quando si crea un commutatore virtuale.

- TCP Chimney Offload. Questa tecnologia è disabilitata per impostazione predefinita.

- Macchina virtuale QoS \(VM-QoS\). QoS di macchina virtuale è disponibile ma disabilitato per impostazione predefinita. Se si configura QoS macchina virtuale in un ambiente di gruppo, le impostazioni QoS causerà risultati imprevisti.

## <a name="bkmk_modes"></a>Modalità di SET e le impostazioni

A differenza di gruppo NIC, quando si crea un gruppo SET, è possibile configurare un nome del team. Inoltre, l'uso di una scheda in standby è supportato nel gruppo NIC, ma non è supportato nel SET. Quando si distribuisce SET, tutte le schede di rete sono attive e nessuna sia in modalità standby.

Un'altra differenza chiave tra SET e gruppo NIC è che gruppo NIC fornisce la scelta di tre diverse modalità di raggruppamento, mentre SET supporta solo **commutatore indipendente** modalità. Con la modalità di commutatore indipendente, il commutatore o switch in cui sono collegati i membri del Team di impostare sono consapevoli della presenza del team di SET e non determinano come distribuire il traffico di rete per impostare i membri del team: al contrario, il team SET distribuisce rete in ingresso traffico tra i membri del team SET.

Quando si crea un nuovo SET di team, è necessario configurare le seguenti proprietà di team.

- Schede membro

- Modalità di bilanciamento del carico

### <a name="member-adapters"></a>Schede membro

Quando si crea un SET di team, è necessario specificare fino a otto schede di rete identico che vengono associate al commutatore virtuale Hyper-V come SET di schede di membro di gruppo.

### <a name="load-balancing-mode"></a>Modalità di bilanciamento del carico

Le opzioni per SET di bilanciamento del carico sono modalità di distribuzione del team **porta Hyper-V** e **dinamica**.

**Porta Hyper-V**

Le macchine virtuali sono connesse a una porta del commutatore virtuale Hyper-V. Quando si usa la modalità di porta Hyper-V per i team SET, la porta del commutatore virtuale Hyper-V e l'indirizzo MAC associato vengono utilizzati per dividere il traffico di rete tra i membri del team SET.

> [!NOTE]
> Quando si utilizza SET in combinazione con Packet Direct, la modalità gruppo NIC **commutatore indipendente** e la modalità di bilanciamento del carico **porta Hyper-V** sono necessari.

Poiché il commutatore adiacente rileva sempre un particolare indirizzo MAC su una porta specifica, l'opzione distribuisce il carico di traffico in ingresso (il traffico dal commutatore all'host) alla porta in cui si trova l'indirizzo MAC. Ciò è particolarmente utile quando si utilizzano le code di macchine virtuali (Code), perché una coda può essere inserita nella scheda di rete specifico in cui è previsto il traffico in arrivo.

Tuttavia, se l'host ha solo alcune macchine virtuali, questa modalità potrebbe non essere sufficientemente granulare per ottenere una distribuzione ben bilanciata. Questa modalità limita inoltre sempre una singola macchina virtuale (ad esempio, il traffico da una porta di commutazione singolo) per la larghezza di banda disponibile su una singola interfaccia.

**Dynamic**

Questa modalità di bilanciamento del carico offre i vantaggi seguenti.

- Carica in uscita venga distribuiti in base a un hash degli indirizzi IP e porte TCP.  In modalità dinamica anche nuovamente carico viene bilanciato in tempo reale in modo che un determinato flusso in uscita possa spostarsi avanti e indietro tra i membri del team SET.

- Carica in ingresso viene distribuite in modo analogo la modalità della porta Hyper-V.

I caricamenti in uscita in questa modalità vengono bilanciati in modo dinamico basato sul concetto di flowlets. Esattamente come la voce umana è naturale interruzioni alla fine delle parole e frasi, i flussi TCP (flussi di comunicazione TCP) inoltre contenere interruzioni di naturali. La parte di un flusso TCP tra due interruzioni di questo tipo si intende un flowlet.

Quando l'algoritmo in modalità dinamica consente di rilevare che è stato rilevato un limite flowlet - ad esempio quando un'interruzione di lunghezza sufficiente si è verificato nel flusso TCP - algoritmo torna a bilanciare automaticamente il flusso a un altro membro del team se appropriato.  In alcune circostanze non comuni, l'algoritmo potrebbe anche periodicamente ribilanciare i flussi che non contengono alcun flowlets. Per questo motivo, l'affinità tra i membri di team e il flusso TCP può cambiare in qualsiasi momento come funziona l'algoritmo di bilanciamento del carico dinamico per bilanciare il carico di lavoro dei membri del team.

## <a name="bkmk_vmq"></a>SET e le code di macchine virtuali (Code)

Coda macchine Virtuali e SET funzionano bene insieme e si consiglia di abilitare VMQ ogni volta che si usa Hyper-V e impostare.

> [!NOTE]
> Impostare sempre presenta il numero totale di code che sono disponibili nei SET di tutti i membri del team. Nel gruppo NIC, questo viene definito modalità Sum-di-code.

La maggior parte delle schede di rete sia le code che possono essere usate per entrambi Receive-Side Scaling \(RSS\) o coda macchine Virtuali, ma non entrambi allo stesso tempo.
  
Alcune impostazioni di coda macchine Virtuali vengono visualizzati sia le impostazioni per le code RSS ma sono effettivamente le impostazioni nelle code generiche che usa RSS e coda macchine Virtuali a seconda di quale funzionalità è attualmente in uso. Ogni interfaccia di rete ha, nelle relative proprietà avanzate, i valori per `*RssBaseProcNumber` e `*MaxRssProcessors`.

Di seguito sono alcune impostazioni di coda macchine Virtuali che offrono prestazioni migliori di sistema.

- In teoria ogni interfaccia di rete deve avere il `*RssBaseProcNumber` impostata su un numero maggiore o uguale a due (2). Infatti, il primo processore fisico, Core 0 \(processori logici 0 e 1\), in genere la maggior parte del sistema elaborando in modo che l'elaborazione di rete debba essere deviato lontano da questo processore fisico. 

>[!NOTE]
>Alcune architetture di computer non dispongono di due processori logici per processore fisico, in modo che per tali macchine, l'elaboratore di base deve essere maggiore o uguale a 1. In caso di dubbio, si presuppone che l'host sta utilizzando un processore logico 2 per ogni architettura del processore fisico.

- Devono essere processori dei membri del team, nella misura in cui è pratica e non sovrapposti. Ad esempio, in un host di 4 core \(8 processori logici\) con un team di 2 schede di rete 10 Gbps, è possibile impostare il primo usare base processore 2 e 4 core usare; il secondo verrebbe impostato per usare il processore di base 6 e utilizzare 2 core.

## <a name="bkmk_hnv"></a>SET e Hyper-V rete virtualizzazione \(HNV\)

SET è completamente compatibile con virtualizzazione rete Hyper-V in Windows Server 2016. Il sistema di gestione di virtualizzazione rete fornisce informazioni al SET di driver che consente di insieme distribuire il carico del traffico di rete in modo che è ottimizzato per il traffico di virtualizzazione rete.
  
## <a name="bkmk_live"></a>Migrazione in tempo reale e imposta

Migrazione in tempo reale è supportata in Windows Server 2016.

## <a name="bkmk_mac"></a>Uso di indirizzi MAC nel pacchetti trasmessi

Quando si configura un SET di team con distribuzione del carico dinamico, i pacchetti provenienti da una singola fonte \(, ad esempio una singola VM\) contemporaneamente vengono distribuite tra più membri del team. 

Per impedire che i commutatori confusione e per evitare che gli allarmi instabile MAC, impostare sostituisce l'indirizzo MAC di origine con un indirizzo MAC diverso sul frame trasmessi su membri del team diverso dal membro del team di affinità. Per questo motivo, ogni membro del team Usa un indirizzo MAC diverso e conflitti di indirizzi MAC non è consentiti solo se e fino a quando non si verifica un errore.

Quando viene rilevato un errore nella scheda di rete primaria, il SET di software del gruppo viene avviato usando l'indirizzo MAC della macchina virtuale nel membro del team che viene scelto come membro del team di affinità temporaneo \(, ovvero quella che verrà visualizzati come la macchina virtuale al commutatore interfaccia\).

Questa modifica si applica solo al traffico che avrebbe dovuto essere inviato sul membro del team di affinità della macchina virtuale con l'indirizzo MAC della macchina virtuale come relativo indirizzo MAC di origine. Il resto del traffico continua a essere inviato con qualsiasi origine, indirizzo MAC sarebbe utilizzato prima dell'errore.

Di seguito sono elenchi di descrivono SET teaming comportamento di sostituzione indirizzo MAC, basato sulla configurazione del team:

- In modalità Independente commutatore alla distribuzione di porta Hyper-V

    - Ogni porta del commutatore virtuale viene creata un'affinità a un membro del team
  
    - Tutti i pacchetti vengono inviati per il membro del team a cui viene creata un'affinità della porta  
  
    - Non viene eseguita alcuna sostituzione MAC di origine  
  
- Nel commutatore indipendente dalla modalità con la distribuzione dinamica
  
    - Ogni porta del commutatore virtuale viene creata un'affinità a un membro del team  
  
    - Tutti i pacchetti ARP/NS vengono inviati sul membro del team a cui viene creata un'affinità della porta  
  
    - I pacchetti inviati nel membro del team che è membro del team creata non dispone di alcuna origine sostituzione di indirizzo MAC eseguita  
  
    - I pacchetti inviati in un membro del team diverso dal membro del team creata disporrà di sostituzione di indirizzo MAC di origine eseguita  
  
## <a name="bkmk_manage"></a>Gestione di un SET di team

Si consiglia di usare System Center Virtual Machine Manager \(VMM\) per gestire SET di team, tuttavia è possibile utilizzare Windows PowerShell per gestire i SET. Le sezioni seguenti riportano che i comandi di Windows PowerShell che è possibile usare per gestire i SET.

Per informazioni su come creare un SET di team con VMM, vedere la sezione "Configurare un commutatore logico" nell'argomento della libreria di System Center VMM [creare i commutatori logici](https://docs.microsoft.com/system-center/vmm/network-switch).
  
### <a name="create-a-set-team"></a>Creare un SET di team

È necessario creare un SET di team allo stesso tempo che si crea il commutatore virtuale Hyper-V usando il **New-VMSwitch** comando Windows PowerShell.

Quando si crea il commutatore virtuale Hyper-V, è necessario includere le nuove **EnableEmbeddedTeaming** parametri nella sintassi del comando. Nell'esempio seguente, denominato un commutatore Hyper-V **TeamedvSwitch** embedded teaming e due team iniziale viene creata membri.
  
```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2" -EnableEmbeddedTeaming $true  
```  
  
Il **EnableEmbeddedTeaming** parametro viene considerato da Windows PowerShell quando l'argomento **NetAdapterName** è una matrice di schede di rete anziché una singola scheda. Di conseguenza, è possibile rivedere il comando precedente nel modo seguente.

```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2"  
```  

Se si desidera creare un SET in grado di supportare switch con un singolo membro del team in modo che sia possibile aggiungere un membro del team in un secondo momento, sarà necessario usare il parametro EnableEmbeddedTeaming.

```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1" -EnableEmbeddedTeaming $true  
```  

### <a name="adding-or-removing-a-set-team-member"></a>Aggiunta o rimozione di un membro del team SET

Il **Set-VMSwitchTeam** comando include il **NetAdapterName** opzione. Per modificare i membri del team in team un SET, immettere l'elenco desiderato di membri del team dopo il **NetAdapterName** opzione. Se **TeamedvSwitch** è stato originariamente creato con l'interfaccia di rete 1 e 2 di interfaccia di rete, quindi il comando seguente Elimina membro del team SET "Scheda di rete 2" e aggiunge nuovo membro del team SET "scheda di rete 3".
  
```  
Set-VMSwitchTeam -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 3"  
```  

### <a name="removing-a-set-team"></a>Rimozione di un SET di team

È possibile rimuovere un gruppo SET solo rimuovendo il commutatore virtuale Hyper-V che contiene il SET di team.  Usare l'argomento [Remove-VMSwitch](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/remove-vmswitch) per informazioni su come rimuovere il commutatore virtuale Hyper-V. L'esempio seguente rimuove un commutatore virtuale denominato **SETvSwitch**.

```  
Remove-VMSwitch "SETvSwitch"  
```  

### <a name="changing-the-load-distribution-algorithm-for-a-set-team"></a>Modificare l'algoritmo di distribuzione del carico per un SET di team

Il **Set-VMSwitchTeam** cmdlet dispone di un **LoadBalancingAlgorithm** opzione. Questa opzione accetta uno dei due valori possibili: **HyperVPort** oppure **dinamica**. Per impostare o modificare l'algoritmo di distribuzione del carico per un team opzione incorporato, usare questa opzione. 

Nell'esempio seguente, denominato il VMSwitchTeam **TeamedvSwitch** Usa le **dinamico** algoritmo di bilanciamento del carico.  
```  
Set-VMSwitchTeam -Name TeamedvSwitch -LoadBalancingAlgorithm Dynamic  
```  
### <a name="affinitizing-virtual-interfaces-to-physical-team-members"></a>Interfacce virtuali consentendo ai membri del team fisico

SET consente di creare un'affinità tra un'interfaccia virtuale \(ad esempio, porta del commutatore virtuale Hyper-V\) e una delle schede NIC fisiche del team. 

Ad esempio, se si creano due host Vnic per SMB\-dirette, come nella sezione [creare un commutatore virtuale Hyper-V con SET e RDMA Vnic](#bkmk_set-rdma), è possibile assicurarsi che le due schede diversi membri del team. 

Aggiunta allo script in tale sezione, è possibile usare i comandi di Windows PowerShell seguenti.

    Set-VMNetworkAdapterTeamMapping -VMNetworkAdapterName SMB_1 –ManagementOS –PhysicalNetAdapterName “SLOT 2”
    Set-VMNetworkAdapterTeamMapping -VMNetworkAdapterName SMB_2 –ManagementOS –PhysicalNetAdapterName “SLOT 3”

In questo argomento viene esaminato in modo più approfondito nella sezione 4.2.5 il [interfaccia di rete di Windows Server 2016 e Switch Embedded Teaming manuale dell'utente](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0).
