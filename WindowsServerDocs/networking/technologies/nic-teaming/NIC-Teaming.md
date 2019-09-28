---
title: Gruppo NIC
description: In questo argomento viene illustrata una panoramica del gruppo NIC (Network Interface Card) in Windows Server 2016. Gruppo NIC consente di raggruppare una o più schede di rete fisiche Ethernet da una a 32 in una o più schede di rete virtuali basate su software. Queste schede di rete virtuali garantiscono prestazioni elevate e tolleranza di errore in caso di errore delle schede di rete.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: abded6f3-5708-4e35-9a9e-890e81924fec
ms.author: pashort
author: shortpatti
ms.date: 09/10/2018
ms.openlocfilehash: 2356de674bfc6e57c9444136b1244934464a2d02
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396496"
---
# <a name="nic-teaming"></a>Gruppo NIC

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento viene illustrata una panoramica del gruppo NIC (Network Interface Card) in Windows Server 2016. Gruppo NIC consente di raggruppare una o più schede di rete fisiche Ethernet da una a 32 in una o più schede di rete virtuali basate su software. Queste schede di rete virtuali garantiscono prestazioni elevate e tolleranza di errore in caso di errore delle schede di rete.  
  
> [!IMPORTANT]
> È necessario installare schede di rete membro del gruppo NIC nello stesso computer host fisico. 

> [!TIP]  
> Un gruppo NIC che contiene una sola scheda di rete non può fornire il bilanciamento del carico e il failover. Tuttavia, con una scheda di rete, è possibile usare il gruppo NIC per la separazione del traffico di rete quando si usano anche reti locali virtuali (VLAN).  
  
Quando si configurano le schede di rete in un gruppo NIC, si connettono alla soluzione di gruppo NIC Common Core, che quindi presenta al sistema operativo una o più schede virtuali, denominate anche NIC del team [tNICs] o interfacce del team. 

Poiché Windows Server 2016 supporta fino a 32 interfacce del team per ogni team, esistono diversi algoritmi che distribuiscono il traffico in uscita (carico) tra le schede di interfaccia di rete.  La figura seguente illustra un gruppo NIC con più tNICs.  
  
![Gruppo NIC con più tNICs](../../media/NIC-Teaming/nict_overview.jpg)  
  
Inoltre, è possibile connettere le NIC raggruppate allo stesso commutatore o a diversi commutatori. Se si connettono le schede NIC a diversi commutatori, entrambi i commutatori devono trovarsi nella stessa subnet.  
  
## <a name="availability"></a>Disponibilità  
Gruppo NIC è disponibile in tutte le versioni di Windows Server 2016. È possibile utilizzare un'ampia gamma di strumenti per gestire il gruppo NIC da computer che eseguono un sistema operativo client, ad esempio: • cmdlet di Windows PowerShell • Desktop remoto • Strumenti di amministrazione remota del server  
  
## <a name="supported-and-unsupported-nics"></a>NIC supportate e non supportate   
È possibile usare qualsiasi scheda di interfaccia di rete Ethernet che ha superato il test del logo e la qualifica hardware Windows (test WHQL) in un gruppo NIC in Windows Server 2016.  
  
Non è possibile inserire le schede di interfaccia di rete seguenti in un gruppo NIC:
  
-   Schede di rete virtuali Hyper-V che sono porte del Commuter virtuale Hyper-V esposte come NIC nella partizione host.  
  
    > [!IMPORTANT]  
    > Non posizionare le schede di rete virtuali Hyper-V esposte nella partizione host (schede) in un team. Il gruppo di schede all'interno della partizione host non è supportato in alcuna configurazione. Se si verificano errori di rete, i tentativi di schede del team potrebbero causare una perdita completa della comunicazione.  
  
-   Scheda di rete di debug del kernel (KDNIC).  
  
-   Schede di rete utilizzate per l'avvio di rete.  
  
-   NIC che usano tecnologie diverse da Ethernet, ad esempio WWAN, WLAN/Wi-Fi, Bluetooth e InfiniBand, incluse schede NIC Internet Protocol over InfiniBand (IPoIB).  
  
## <a name="compatibility"></a>Compatibilità  
Gruppo NIC è compatibile con tutte le tecnologie di rete in Windows Server 2016 con le eccezioni seguenti.  
  
-   **Single-Root I/O Virtualization (SR-IOV)** . Per SR-IOV, i dati vengono recapitati direttamente alla scheda di interfaccia di rete senza passarli attraverso lo stack di rete (nel sistema operativo host, nel caso della virtualizzazione). Pertanto, il team NIC non può ispezionare o reindirizzare i dati a un altro percorso del team.  
  
-   **QoS (Quality of Service) host nativo**. Quando si impostano i criteri QoS in un sistema nativo o host e tali criteri richiamano limitazioni minime per la larghezza di banda, la velocità effettiva complessiva per un gruppo NIC è inferiore a quella dei criteri di larghezza di banda.  
  
-   **TCP Chimney**. TCP Chimney non è supportato con gruppo NIC perché TCP Chimney trasferisce l'intero stack di rete direttamente alla scheda di interfaccia di rete.  
  
-   **autenticazione 802.1 x**. Non è consigliabile usare l'autenticazione 802.1 X con gruppo NIC perché alcuni commutatori non consentono la configurazione dell'autenticazione 802.1 X e del gruppo NIC sulla stessa porta.  
  
Per informazioni sull'uso del gruppo NIC all'interno delle macchine virtuali (VM) in esecuzione in un host Hyper-V, vedere [creare un nuovo gruppo NIC in un computer host o una macchina virtuale](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md).
  
## <a name="virtual-machine-queues-vmqs"></a>Code di macchine virtuali (code macchine virtuali)  

Code macchine virtuali è una funzionalità NIC che alloca una coda per ogni macchina virtuale.  Quando Hyper-V è abilitato, è anche necessario abilitare VMQ. In Windows Server 2016, code macchine virtuali usa l'opzione NIC finestre con una singola coda assegnata al vPort per fornire la stessa funzionalità. 

A seconda della modalità di configurazione del compartitore e dell'algoritmo di distribuzione del carico, gruppo NIC presenta il minor numero di code disponibili e supportate da qualsiasi adapter del team (modalità min-Queues) o il numero totale di code disponibili in tutti i team membri (modalità Sum-of-queues).  

Se il team è in modalità gruppo indipendente dall'opzione e si imposta la distribuzione del carico sulla modalità della porta Hyper-V o dinamica, il numero di code restituite è la somma di tutte le code disponibili dai membri del team (modalità Sum-of-queues). In caso contrario, il numero di code restituite è il minor numero di code supportate da qualsiasi membro del team (modalità min-Queues).

Ecco perché:  
  
-   Quando il team indipendente dal commutire si trova in modalità porta Hyper-V o in modalità dinamica, il traffico in ingresso per una porta di commutazione Hyper-V (VM) arriva sempre allo stesso membro del team. L'host può stimare/controllare il membro che riceve il traffico per una determinata macchina virtuale, in modo che gruppo NIC possa essere più attento alle code VMQ da allocare in un particolare membro del team. Gruppo NIC, uso del compilatore Hyper-V, imposta VMQ per una macchina virtuale su un solo membro del team e sa che il traffico in ingresso raggiunge la coda.  
  
-   Quando il team è in qualsiasi modalità dipendente dal Commuter (gruppo statico o gruppo LACP), l'opzione a cui il team è connesso controlla la distribuzione del traffico in ingresso. Il software di gruppo NIC dell'host non è in grado di stimare il membro del team che ottiene il traffico in ingresso per una macchina virtuale e potrebbe essere il fatto che il Commuter distribuisce il traffico per una macchina virtuale in tutti i membri del team. In seguito al software gruppo NIC, l'uso del Commuter Hyper-V, programma una coda per la macchina virtuale in ogni membro del team, non solo un membro del team.  
  
-   Quando il team è in modalità indipendente dal cambio e usa il bilanciamento del carico hash degli indirizzi, il traffico in ingresso viene sempre eseguito in una scheda di interfaccia di rete (il membro del team primario), tutto in un solo membro del team. Poiché gli altri membri del team non gestiscono il traffico in ingresso, vengono programmati con le stesse code del membro primario, in modo che se il membro primario ha esito negativo, qualsiasi altro membro del team può essere usato per prelevare il traffico in ingresso e le code sono già presenti.  

- Per la maggior parte delle schede di rete sono disponibili code utilizzate per Receive-Side Scaling (RSS) o VMQ, ma non nello stesso momento. Alcune impostazioni di VMQ sembrano essere impostazioni per le code RSS, ma sono impostazioni sulle code generiche usate da RSS e da VMQ a seconda della funzionalità attualmente in uso. Ogni scheda di interfaccia di rete ha, nelle proprietà avanzate, i valori \*per * RssBaseProcNumber e MaxRssProcessors. Di seguito sono riportate alcune impostazioni VMQ che garantiscono prestazioni di sistema migliori.  
  
-   Idealmente, ogni scheda di interfaccia di rete deve avere la * RssBaseProcNumber impostata su un numero pari maggiore o uguale a due (2). Il primo processore fisico, Core 0 (processori logici 0 e 1), in genere esegue la maggior parte dell'elaborazione del sistema, in modo da evitare che l'elaborazione della rete venga ricavata dal processore fisico. Alcune architetture di computer non dispongono di due processori logici per processore fisico. per tali computer, quindi, il processore di base deve essere maggiore o uguale a 1. Se in dubbio si presuppone che l'host usi un processore logico 2 per ogni architettura del processore fisico.  
  
-   Se il team è in modalità di somma coda, i processori dei membri del team non devono sovrapporsi. Ad esempio, in un host con 4 core (8 processori logici) con un team di due schede di rete 10Gbps, è possibile impostare la prima per l'uso del processore di base 2 e per l'uso di 4 core. il secondo verrebbe impostato in modo da usare il processore di base 6 e usare 2 Core.  
  
-   Se il team è in modalità min-queues, i set di processori usati dai membri del team devono essere identici.  

  
## <a name="hyper-v-network-virtualization-hnv"></a>Virtualizzazione rete Hyper-V (HNV)  
Gruppo NIC è completamente compatibile con la virtualizzazione rete Hyper-V (HNV).  Il sistema di gestione di HNV fornisce informazioni al driver gruppo NIC che consente al gruppo NIC di distribuire il carico in modo da ottimizzare il traffico di HNV.  
  
## <a name="live-migration"></a>Migrazione in tempo reale  
Gruppo NIC nelle macchine virtuali non influisce Live Migration. Esistono le stesse regole per Live Migration se si configura il gruppo NIC nella macchina virtuale.  


## <a name="virtual-local-area-networks-vlans"></a>Reti locali virtuali (VLAN)
Quando si usa il gruppo NIC, la creazione di più interfacce del team consente a un host di connettersi a VLAN diverse nello stesso momento. Configurare l'ambiente usando le linee guida seguenti:
  
- Prima di abilitare il gruppo NIC, configurare le porte di commutazione fisiche connesse all'host di raggruppamento per l'utilizzo della modalità trunk (promiscua). Il Commuter fisico deve passare tutto il traffico all'host per filtrare senza modificare il traffico.  

- Non configurare i filtri VLAN nelle schede di interfaccia di rete usando le impostazioni avanzate delle proprietà della scheda di interfaccia di rete. Consentire al software gruppo NIC o al Commuter virtuale Hyper-V (se presente) di eseguire il filtro VLAN.  
  
### <a name="use-vlans-with-nic-teaming-in-a-vm"></a>Usare VLAN con gruppo NIC in una macchina virtuale  
Quando un team si connette a un Commuter virtuale Hyper-V, è necessario eseguire tutte le operazioni di separazione VLAN nel Commuter virtuale Hyper-V anziché nel gruppo NIC.  

Pianificare l'uso delle VLAN in una macchina virtuale configurata con un gruppo NIC usando le linee guida seguenti:
  
-   Il metodo preferito per supportare più VLAN in una macchina virtuale consiste nel configurare la macchina virtuale con più porte nel Commuter virtuale Hyper-V e associare ogni porta a una VLAN. Non eseguire mai il team di queste porte nella macchina virtuale perché causa problemi di comunicazione di rete.  

-   Se la macchina virtuale ha più funzioni virtuali SR-IOV (VFs), assicurarsi che si trovino nella stessa VLAN prima di raggrupparle nella macchina virtuale. È possibile configurare facilmente i diversi VFs in modo che si trovino in VLAN diverse e questo provochi problemi di comunicazione di rete.  
 
  
### <a name="manage-network-interfaces-and-vlans"></a>Gestire le interfacce di rete e le VLAN 
Se è necessario disporre di più VLAN esposte in un sistema operativo guest, prendere in considerazione la possibilità di rinominare le interfacce Ethernet per chiarire la VLAN assegnata all'interfaccia. Ad esempio, se si associa l'interfaccia **Ethernet** con VLAN 12 e l'interfaccia **Ethernet 2** con VLAN 48, rinominare l'interfaccia Ethernet in **EthernetVLAN12** e l'altra in **EthernetVLAN48**. 

Rinominare le interfacce usando il comando di Windows PowerShell **Rename-NetAdapter** o eseguendo la procedura seguente:
  
1.  In Server Manager, in **Proprietà** della scheda di rete che si desidera rinominare, fare clic sul collegamento a destra del nome della scheda di rete. 
  
2.  Fare clic con il pulsante destro del mouse sulla scheda di rete che si desidera rinominare, quindi scegliere **Rinomina**.  
  
3.  Digitare il nuovo nome della scheda di rete e premere INVIO.  


## <a name="virtual-machines-vms"></a>Macchine virtuali (VM)

Se si vuole usare il gruppo NIC in una macchina virtuale, è necessario connettere le schede di rete virtuali nella macchina virtuale solo ai commutatori virtuali Hyper-V esterni. Questa operazione consente alla macchina virtuale di sostenere la connettività di rete anche quando una delle schede di rete fisiche connesse a un Commuter virtuale ha esito negativo o viene scollegata. Le schede di rete virtuali connesse a commutatori virtuali Hyper-V interni o privati non sono in grado di connettersi al commutatore quando si trovano in un team e la rete non riesce per la macchina virtuale.  
  
Gruppo NIC in Windows Server 2016 supporta i team con due membri nelle macchine virtuali. È possibile creare team di grandi dimensioni, ma non è disponibile alcun supporto per i team di grandi dimensioni. Ogni membro del team deve connettersi a un Commuter virtuale Hyper-V esterno diverso e le interfacce di rete della macchina virtuale devono essere configurate per consentire il raggruppamento.

  
Se si sta configurando un gruppo NIC in una macchina virtuale, è necessario selezionare una **modalità gruppo** di _commutazione indipendente_ e una **modalità di bilanciamento del carico** per _hash Indirizzo_.   
  
  
## <a name="sr-iov-capable-network-adapters"></a>Schede di rete che supportano SR-IOV  
Un gruppo NIC in o nell'host Hyper-V non può proteggere il traffico SR-IOV perché non passa attraverso il Commuter Hyper-V.  Con l'opzione Gruppo NIC VM è possibile configurare due commutatori virtuali Hyper-V esterni, ciascuno connesso alla propria NIC con supporto SR-IOV.  
  
![Gruppo NIC con schede di rete compatibili con SR-IOV](../../media/NIC-Teaming-in-Virtual-Machines--VMs-/nict_in_vm.jpg)  
  
Ogni VM può avere una funzione virtuale (VF) di una o entrambe le schede di interfaccia di rete SR-IOV e, in caso di disconnessione di una scheda di interfaccia di rete, eseguire il failover dal dispositivo VF primario a quello di backup (VF). In alternativa, la macchina virtuale può avere un VF da una scheda di interfaccia di rete e una scheda non VF connessa a un altro Commuter virtuale. Se la scheda di interfaccia di rete associata al VF viene disconnessa, il traffico può eseguire il failover all'altro switch senza perdita di connettività.  
  
Poiché il failover tra schede di interfaccia di rete in una macchina virtuale può causare il traffico inviato con l'indirizzo MAC degli altri scheda, ogni porta del Commuter virtuale Hyper-V associata a una macchina virtuale che usa gruppo NIC deve essere impostata in modo da consentire il raggruppamento. 


## <a name="related-topics"></a>Argomenti correlati

- [Gestione e utilizzo degli indirizzi MAC del gruppo NIC](NIC-Teaming-MAC-Address-Use-and-Management.md): Quando si configura un gruppo NIC con modalità indipendente dal commutine e una distribuzione con hash di indirizzi o di carico dinamico, il team USA l'indirizzo Media Access Control (MAC) del membro del gruppo NIC primario nel traffico in uscita. Il membro del gruppo NIC primario è una scheda di rete selezionata dal sistema operativo dal set iniziale di membri del team.

- [Impostazioni gruppo NIC](nic-teaming-settings.md): In questo argomento viene illustrata una panoramica delle proprietà del gruppo NIC, ad esempio le modalità gruppo e bilanciamento del carico. Vengono inoltre illustrati i dettagli relativi all'impostazione della scheda standby e alla proprietà principale dell'interfaccia del team. Se si dispone di almeno due schede di rete in un gruppo NIC, non è necessario designare una scheda standby per la tolleranza di errore.
  
- [Creare un nuovo gruppo NIC in un computer host o una macchina virtuale](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md): In questo argomento viene creato un nuovo gruppo NIC in un computer host o in una macchina virtuale (VM) Hyper-V che esegue Windows Server 2016.

- [Risoluzione dei problemi relativi al gruppo NIC](Troubleshooting-NIC-Teaming.md): In questo argomento vengono illustrati i metodi per risolvere i problemi relativi al gruppo NIC, ad esempio l'hardware, i titoli dei commutatori fisici e la disabilitazione o l'abilitazione di schede di rete con Windows PowerShell. 
 
