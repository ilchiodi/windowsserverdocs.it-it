---
title: Gruppo NIC
description: In questo argomento viene fornita è una panoramica del gruppo di schede di interfaccia di rete (NIC) in Windows Server 2016. Gruppo NIC consente di raggruppare tra 1 e 32 schede di rete Ethernet fisiche in una o più schede di rete virtuale basata su software. Queste schede di rete virtuali garantiscono prestazioni elevate e tolleranza di errore in caso di errore delle schede di rete.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: abded6f3-5708-4e35-9a9e-890e81924fec
ms.author: pashort
author: shortpatti
ms.date: 09/10/2018
ms.openlocfilehash: 7e7ae609d41fc41770c43283033b623881b2d48e
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283786"
---
# <a name="nic-teaming"></a>Gruppo NIC

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

In questo argomento viene fornita è una panoramica del gruppo di schede di interfaccia di rete (NIC) in Windows Server 2016. Gruppo NIC consente di raggruppare tra 1 e 32 schede di rete Ethernet fisiche in una o più schede di rete virtuale basata su software. Queste schede di rete virtuali garantiscono prestazioni elevate e tolleranza di errore in caso di errore delle schede di rete.  
  
> [!IMPORTANT]
> È necessario installare schede di rete di gruppo NIC membro nello stesso computer host fisico. 

> [!TIP]  
> Un gruppo NIC che contiene una sola scheda di rete non può fornire il bilanciamento del carico e failover. Tuttavia, con una scheda di rete, è possibile utilizzare gruppo NIC per la separazione del traffico di rete quando vengono specificate anche le reti locali virtuali (VLAN).  
  
Quando si configurano le schede di rete in un gruppo NIC, si connettono nel NIC teaming soluzione core comune, che presenta quindi uno o più schede di rete virtuali (denominati anche interfacce team o team NIC [tNICs]) al sistema operativo. 

Poiché Windows Server 2016 supporta un massimo di 32 interfacce team per ogni team, sono disponibili un'ampia gamma di algoritmi che distribuisce il traffico in uscita (caricamento) tra le schede di rete.  La figura seguente illustra un gruppo NIC con tNICs più.  
  
![Gruppo NIC con più tNICs](../../media/NIC-Teaming/nict_overview.jpg)  
  
Inoltre, è possibile connettersi i gruppi NIC per il commutatore stesso o diversi commutatori. Se ci si connette le schede NIC a diversi commutatori, entrambe le opzioni devono essere nella stessa subnet.  
  
## <a name="availability"></a>Disponibilità  
Gruppo NIC è disponibile in tutte le versioni di Windows Server 2016. È possibile usare un'ampia gamma di strumenti per la gestione di gruppo NIC dai computer che eseguono un sistema operativo client, ad esempio: • • i cmdlet di PowerShell di Windows Desktop remoto • strumenti di amministrazione remota del Server  
  
## <a name="supported-and-unsupported-nics"></a>Interfacce di rete supportati e non supportati   
È possibile utilizzare qualsiasi interfaccia di rete Ethernet che ha superato il test di qualifica di Hardware di Windows e Logo (test WHQL) in un gruppo NIC in Windows Server 2016.  
  
Non è possibile immettere le seguenti schede di rete in un gruppo NIC:
  
-   Schede di rete virtuale Hyper-V presenti porte di commutatori virtuali Hyper-V esposte come schede di rete nella partizione host.  
  
    > [!IMPORTANT]  
    > Non inserire NIC virtuali Hyper-V esposta nella partizione host (Vnic) di un team. Raggruppamento di schede all'interno della partizione host non è supportato in qualsiasi configurazione. I tentativi di Vnic team potrebbero causare una perdita completa di comunicazione, se si verificano errori di rete.  
  
-   Scheda di rete di debug del kernel (KDNIC).  
  
-   Interfacce di rete utilizzati per l'avvio di rete.  
  
-   Interfacce di rete che usano tecnologie diverse da Ethernet, ad esempio nella rete WWAN WLAN/Wi-Fi, Bluetooth e Infiniband, incluso Internet Protocol su schede di rete Infiniband (IPoIB).  
  
## <a name="compatibility"></a>Compatibilità  
Gruppo NIC è compatibile con tutte le tecnologie di rete in Windows Server 2016 con le eccezioni seguenti.  
  
-   **Single-root i/o virtualization (SR-IOV)** . Per SR-IOV, i dati vengono recapitati direttamente alla scheda di rete senza passare attraverso lo stack di rete (nel sistema operativo host, nel caso di virtualizzazione). Non è pertanto possibile che il gruppo NIC controllare o reindirizzare i dati a un altro percorso nel gruppo.  
  
-   **Qualità del servizio (QoS) di host nativo**. Quando si impostano i criteri QoS alle nativo o del sistema host e i criteri di richiamano le limitazioni di larghezza di banda minima, la velocità effettiva complessiva per un gruppo NIC è minore di quanto sarebbe possibile senza i criteri di larghezza di banda in uso.  
  
-   **TCP Chimney**. TCP Chimney non è supportato con gruppo NIC perché TCP Chimney Offload l'intero stack di rete direttamente all'interfaccia di rete.  
  
-   **802.1 l'autenticazione x**. Non utilizzare l'autenticazione 802.1x con gruppo NIC perché alcune opzioni non consentono la configurazione dell'autenticazione 802.1X e gruppo NIC sulla stessa porta.  
  
Per altre informazioni sull'utilizzo di gruppo NIC all'interno di macchine virtuali (VM) in esecuzione su un host Hyper-V, vedere [creare un nuovo gruppo NIC in una VM o computer host](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md).
  
## <a name="virtual-machine-queues-vmqs"></a>Code di macchine virtuali (Code)  

Code è una funzionalità di interfaccia di rete che consente di allocare una coda per ogni macchina virtuale.  Ogni volta che si dispone di Hyper-V abilitato. è anche necessario abilitare coda macchine Virtuali. In Windows Server 2016, code usano VPort commutatore di interfaccia di rete con una singola coda assegnata al vPort per fornire la stessa funzionalità. 

A seconda della modalità di configurazione di commutatore e l'algoritmo di distribuzione del carico, NIC teaming presenta il minor numero di code disponibile e supportate da alcun adattatore del team (modalità Min-code) o il numero totale delle code disponibili in tutti i team membri (modalità Sum-di-code).  

Se il team è in modalità gruppo indipendenti dal commutatore e la distribuzione del carico è impostato su modalità di porta Hyper-V o dinamico, il numero di code segnalato è la somma di tutte le code disponibili dai membri del team (modalità Sum-di-code). In caso contrario, il numero di code segnalato è il più piccolo numero di code supportati da qualsiasi membro del team (modalità Min-code).

Ecco perché:  
  
-   Quando il team indipendenti dal commutatore è in modalità di porta Hyper-V o dinamico il traffico in ingresso per una porta del commutatore Hyper-V (VM) sempre arriva nella stesso membro del team. L'host può prevedere/controllo quale membro riceve il traffico per una determinata macchina virtuale in modo che gruppo NIC può essere più precise sulle code che coda macchine Virtuali da allocare in un membro del team specifico. Gruppo NIC, lavorare con il commutatore Hyper-V, imposta la funzionalità coda macchine Virtuali per una macchina virtuale in modo preciso un membro del team e sapere che il traffico in ingresso raggiunge tale coda.  
  
-   Quando il team è in qualsiasi modalità dipendenti switch (Creazione gruppi statica o LACP gruppo NIC), il commutatore che il team è connesso a consente di controllare la distribuzione del traffico in ingresso. Software gruppo NIC dell'host non è possibile prevedere quale team member Ottiene il traffico in ingresso per una macchina virtuale e potrebbe essere che l'opzione distribuisce il traffico per una macchina virtuale tra tutti i membri del team. Come risultato il software gruppo NIC, lavorare con il commutatore Hyper-V, i programmi una coda per la macchina virtuale in ogni membro del team, non solo un membro del team.  
  
-   Quando il team si trova in modalità indipendenti dal commutatore e hash indirizzo Usa il bilanciamento del carico, il traffico in ingresso sempre proviene da un'interfaccia di rete (membro del team primario) - tutto su un solo membro del team. Poiché altri membri del team non gestiscono il traffico in ingresso, in modo che se il membro primario non riesce, può essere utilizzato qualsiasi altro membro del team per prelevare il traffico in ingresso e le code sono già presenti ottenere programmati con le code stesso come membro primario.  

- La maggior parte delle interfacce di rete presenti code utilizzate per Receive-Side Scaling (RSS) o per coda macchine Virtuali, ma non contemporaneamente. Alcune impostazioni di coda macchine Virtuali vengono visualizzate sia le impostazioni per le code RSS ma sono le impostazioni nelle code generiche che usa RSS e coda macchine Virtuali a seconda di quale funzionalità è attualmente in uso. Ogni interfaccia di rete ha, nelle relative proprietà avanzate, i valori per * RssBaseProcNumber e \*MaxRssProcessors. Di seguito sono alcune impostazioni di coda macchine Virtuali che offrono prestazioni migliori di sistema.  
  
-   Idealmente, ogni interfaccia di rete deve avere il * RssBaseProcNumber impostata su un numero maggiore o uguale a due (2). Il primo processore fisico, Core 0 (processori logici 0 e 1), in genere la maggior parte dell'elaborazione di sistema in modo che l'elaborazione di rete debba guidare lontano da questo processore fisico. Alcune architetture di computer non dispongono di due processori logici per processore fisico, pertanto per tali macchine, l'elaboratore di base deve essere maggiore o uguale a 1. Se in dubbio presuppongono l'host sta utilizzando un processore logico 2 per ogni architettura del processore fisico.  
  
-   Se il team è in modalità di somma-di-code processori dei membri del team devono essere non sovrapposti. In un host di 4 core (8 processori logici) con un team di 2 schede di rete 10 Gbps, ad esempio, è possibile impostare il primo da usare il processore di base di 2 e 4 core, usare il secondo verrebbe impostato per usare il processore di base 6 e utilizzare 2 core.  
  
-   Se il team è in modalità Min-code i gruppi di processori usati dai membri del team devono essere identici.  

  
## <a name="hyper-v-network-virtualization-hnv"></a>Virtualizzazione rete Hyper-V (HNV)  
Gruppo NIC è completamente compatibile con Hyper-V rete virtualizzazione.  Il sistema di gestione di virtualizzazione rete fornisce informazioni per il driver NIC Teaming che consente di distribuire il carico in modo che consente di ottimizzare il traffico di virtualizzazione rete gruppo NIC.  
  
## <a name="live-migration"></a>Migrazione in tempo reale  
Gruppo NIC nelle macchine virtuali non influiscono sulla migrazione in tempo reale. Le stesse regole esistano per la migrazione in tempo reale indipendentemente dalla configurazione di gruppo NIC nella macchina virtuale.  


## <a name="virtual-local-area-networks-vlans"></a>Reti locali virtuali (VLAN)
Quando si Usa gruppo NIC, creazione di più interfacce di team consente a un host per la connessione a VLAN diverse nello stesso momento. Configurare l'ambiente usando le linee guida seguenti:
  
- Prima di abilitare gruppo NIC, configurare le porte del commutatore fisico connesse all'host del gruppo NIC per utilizzare la modalità trunk (promiscue). Il commutatore fisico deve passare tutto il traffico all'host per il filtro senza modificare il traffico.  

- Non configurare i filtri VLAN nelle schede di rete usando l'interfaccia di rete delle proprietà impostazioni avanzata. Lasciare che il software gruppo NIC o il commutatore virtuale Hyper-V (se presente) filtrare le VLAN.  
  
### <a name="use-vlans-with-nic-teaming-in-a-vm"></a>Usare alcuna VLAN con gruppo NIC in una macchina virtuale  
Quando un team si connette a un commutatore virtuale Hyper-V, la separazione di tutte le VLAN deve essere eseguita nel commutatore virtuale Hyper-V anziché nel gruppo NIC.  

Prevede di usare alcuna VLAN in una macchina virtuale configurata con un gruppo NIC usando le linee guida seguenti:
  
-   Il metodo preferito di supportare più VLAN in una macchina virtuale consiste nel configurare la macchina virtuale con più porte del commutatore virtuale Hyper-V e associare ogni porta a una VLAN. Non team mai queste porte nella macchina virtuale perché in caso contrario, i problemi di comunicazione di rete.  

-   Se la VM ha più funzioni virtuali SR-IOV (VFs), assicurarsi che siano nella stessa VLAN prima li gruppo NIC nella macchina virtuale. È facilmente possibile configurare i diversi VFs come presente su VLAN diverse e in caso contrario, i problemi di comunicazione di rete.  
 
  
### <a name="manage-network-interfaces-and-vlans"></a>Gestire le interfacce di rete e le VLAN 
Se è necessario avere più di una VLAN esposta in un sistema operativo guest, è consigliabile rinominare le interfacce Ethernet per chiarire VLAN assegnato all'interfaccia. Ad esempio, se si associa **Ethernet** interfacciarsi con 12 VLAN e la **Ethernet 2** interfacciarsi con 48 VLAN, rinominare l'interfaccia Ethernet per **EthernetVLAN12** e il per altri **EthernetVLAN48**. 

Rinominare le interfacce usando il comando di Windows PowerShell **Rename-NetAdapter** o eseguendo la procedura seguente:
  
1.  In Server Manager in **proprietà** della scheda di rete che si desidera rinominare, fare clic sul collegamento a destra del nome della scheda di rete. 
  
2.  Fare doppio clic su scheda di rete che si desidera rinominare e selezionare **Rinomina**.  
  
3.  Digitare il nuovo nome per la scheda di rete e premere INVIO.  


## <a name="virtual-machines-vms"></a>Macchine virtuali (VM)

Se si desidera utilizzare gruppo NIC in una macchina virtuale, è necessario connettersi le schede di rete virtuale nella macchina virtuale per Hyper-V commutatori virtuali esterni solo. In questo modo la macchina virtuale a sostenere la connettività di rete anche nei casi quando una delle schede di rete fisica connessa a un commutatore virtuale non riesce o viene disconnesso. Schede di rete virtuale connesse a commutatori virtuali Hyper-V interno o privato non sono in grado di connettersi al commutatore quando si trovano in un team e si verifica un errore di rete per la macchina virtuale.  
  
Gruppo NIC in Windows Server 2016 supporta i team con due membri nelle macchine virtuali. È possibile creare team più grandi, ma non è supportata per i team più grandi. Ogni membro del team deve connettersi a un commutatore virtuale del Hyper-V esterno diverso, e le interfacce di rete della macchina virtuale devono essere configurate per consentire di gruppo NIC.

  
Se si configura un gruppo NIC in una macchina virtuale, è necessario selezionare una **modalità gruppo** dei _commutatore indipendente_ e un **modalità di bilanciamento del carico** di _Hash indirizzo_.   
  
  
## <a name="sr-iov-capable-network-adapters"></a>Schede di rete SR-IOV-per  
Un gruppo NIC in o in host Hyper-V non è possibile proteggere il traffico SR-IOV perché qualcosa non va attraverso il commutatore Hyper-V.  Con l'opzione gruppo NIC di macchina virtuale, è possibile configurare due commutatori virtuali esterni di Hyper-V, ciascuna connessa a un proprio SR-IOV-per interfaccia di rete.  
  
![Gruppo NIC con schede di rete SR-IOV-per](../../media/NIC-Teaming-in-Virtual-Machines--VMs-/nict_in_vm.jpg)  
  
Ogni macchina virtuale può avere una funzione virtuale (VF) da una o entrambe le schede di rete SR-IOV e, in caso di disconnessione di una scheda di rete, il failover da primario VF nell'adapter di backup (VF). In alternativa, la macchina virtuale abbia un VF da una scheda di rete e a sua volta non VF connessi a un altro commutatore virtuale. Se l'interfaccia di rete associato VF è disconnessa, il traffico possibile eseguire il failover su altro commutatore senza perdere la connettività.  
  
Poiché il failover tra schede di rete in una macchina virtuale potrebbe comportare il traffico inviato con l'indirizzo MAC delle altre vmNIC, ogni porta del commutatore virtuale Hyper-V associata a una macchina virtuale utilizza gruppo NIC deve essere impostato su Consenti gruppo NIC. 


## <a name="related-topics"></a>Argomenti correlati

- [Gestione e uso di indirizzi MAC di NIC Teaming](NIC-Teaming-MAC-Address-Use-and-Management.md): Quando si configura un gruppo NIC con hash indirizzo o la distribuzione del carico dinamico e Cambia modalità indipendente, l'accesso ai supporti il team utilizza controllo indirizzo (MAC) del membro del Team di interfaccia di rete primario sul traffico in uscita. Membro del Team di interfaccia di rete primario è una scheda di rete selezionata per il sistema operativo del set iniziale di membri del team.

- [Le impostazioni di gruppo NIC](nic-teaming-settings.md): In questo argomento è fornire una panoramica delle proprietà del Team di interfaccia di rete, ad esempio gruppo NIC e modalità di bilanciamento del carico. È inoltre offrono informazioni dettagliate sull'impostazione della scheda di Standby e la proprietà dell'interfaccia gruppo primaria. Se si dispone di almeno due schede di rete in un gruppo NIC, non devi designare una scheda di Standby per la tolleranza di errore.
  
- [Creare un nuovo gruppo NIC in una VM o computer host](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md): In questo argomento, creare un nuovo gruppo NIC in un computer host o in una macchina virtuale di Hyper-V (VM) che esegue Windows Server 2016.

- [Risoluzione dei problemi di gruppo NIC](Troubleshooting-NIC-Teaming.md): In questo argomento vengono illustrati modi per risolvere i problemi gruppo NIC, ad esempio hardware, titoli commutatore fisico e l'abilitazione o disabilitazione schede di rete tramite Windows PowerShell. 
 
