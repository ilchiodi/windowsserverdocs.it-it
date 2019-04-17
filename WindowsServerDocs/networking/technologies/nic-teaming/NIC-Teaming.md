---
title: Gruppo NIC
description: In questo argomento viene fornita una panoramica di gruppo di schede di interfaccia di rete (NIC) in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: abded6f3-5708-4e35-9a9e-890e81924fec
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 142f56153187368effdb802c0c1b50359fffc36a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="nic-teaming"></a>Gruppo NIC

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

In questo argomento viene fornita una panoramica di gruppo di schede di interfaccia di rete (NIC) in Windows Server 2016.

> [!NOTE]  
> Oltre a questo argomento, il contenuto seguente gruppo NIC è disponibile.  
>   
> - [Gruppo NIC nelle macchine virtuali & #40; Le macchine virtuali & #41;](nict-vms.md)
> - [Gruppo NIC e reti locali virtuali & #40; VLAN & #41;](nict-and-vlans.md)
> - [Gruppo di gestione e l'utilizzo di indirizzi MAC NIC](NIC-Teaming-MAC-Address-Use-and-Management.md)
> - [Risoluzione dei problemi di gruppo NIC](Troubleshooting-NIC-Teaming.md) 
> - [Creare un nuovo gruppo NIC in un Computer Host o macchina virtuale](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md)
> - [Cmdlet (NetLBFO) in Windows PowerShell gruppo NIC](https://technet.microsoft.com/library/jj130849.aspx)
> - Download di raccolta TechNet: [Windows Server 2016 NIC e Switch Embedded Teaming manuale dell'utente](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0)
  
## <a name="bkmk_over"></a>Panoramica di gruppo NIC  
Gruppo NIC consente di raggruppare tra uno e trenta due fisiche schede di rete Ethernet in uno o più schede di rete virtuali basate su software. Queste schede di rete virtuali garantiscono prestazioni elevate e tolleranza di errore in caso di errore di scheda di rete.  
  
Tutte le schede di rete membro gruppo NIC devono essere installate nello stesso computer host fisico da inserire in un gruppo.  
  
> [!NOTE]  
> Un gruppo NIC che contiene una sola scheda di rete in grado di fornire il bilanciamento del carico e failover. Tuttavia con una scheda di rete, è possibile utilizzare gruppo NIC per la separazione del traffico di rete quando si utilizza anche reti locali virtuali (VLAN).  
  
Quando si configurano le schede di rete in un gruppo NIC, che sono connessi nelle schede NIC teaming soluzione comune principali, che presenta quindi una o più schede di rete virtuali (denominati anche le interfacce di team o team NIC [tNICs]) al sistema operativo. Windows Server 2016 supporta fino a 32 interfacce di team per ogni gruppo. Esistono diversi algoritmi che distribuisce il traffico in uscita (load) tra schede di rete.  
  
Nella figura seguente viene illustrato un gruppo NIC con più tNICs.  
  
![Gruppo NIC con più tNICs](../../media/NIC-Teaming/nict_overview.jpg)  
  
Inoltre, è possibile collegare le NIC combinate allo stesso commutatore o a diversi commutatori. Se ci si connettono a NIC a diversi commutatori, entrambe le opzioni devono essere sulla stessa subnet.  
  
## <a name="bkmk_avail"></a>Disponibilità gruppo NIC  
Gruppo NIC è disponibile in tutte le versioni di Windows Server 2016. Inoltre, è possibile utilizzare i comandi di Windows PowerShell, Desktop remoto e strumenti di amministrazione remota del Server per gestire gruppo NIC da computer che eseguono un sistema operativo client su cui sono supportati gli strumenti.  
  
## <a name="bkmk_nics"></a>Schede NIC supportate e non supportate per gruppo NIC  
È possibile utilizzare qualsiasi scheda di rete Ethernet che hanno superato il test di qualificazione Hardware di Windows e il Logo (test WHQL) in un gruppo NIC in Windows Server 2016.  
  
Le schede NIC seguenti non possono trovarsi in un gruppo NIC.  
  
-   Schede di rete virtuale Hyper-V sono porte del commutatore virtuale Hyper-V esposte come schede di rete nella partizione host.  
  
    > [!IMPORTANT]  
    > NIC virtuale Hyper-V che vengono esposte nella partizione host (Vnic) non deve essere inserita in un gruppo. Raggruppamento di vNICs all'interno di partizione host non è supportato in qualsiasi configurazione o una combinazione. I tentativi di vNICs team potrebbero causare la perdita completa di comunicazione se si verificano errori di rete.  
  
-   Il kernel debug scheda di rete (KDNIC).  
  
-   Schede di rete che vengono utilizzati per l'avvio di rete.  
  
-   Schede di rete che utilizzano tecnologie diverse da Ethernet, ad esempio WWAN, WLAN/Wi-Fi, Bluetooth e Infiniband, compreso il protocollo Internet tramite schede di rete Infiniband (IPoIB).  
  
## <a name="bkmk_compat"></a>NIC Teaming compatibilità  
Gruppo NIC è compatibile con tutte le tecnologie di rete in Windows Server 2016 con le eccezioni seguenti.  
  
-   **Single-root i/o virtualization (SR-IOV)**. Per SR-IOV, i dati vengono recapitati direttamente alla scheda NIC senza passare attraverso lo stack di rete (nel sistema operativo host, nel caso di virtualizzazione). Pertanto, non è possibile che il gruppo NIC controllare o reindirizzare i dati a un altro percorso nel gruppo.  
  
-   **Host nativo Quality of Service (QoS)**. Quando i criteri QoS vengono impostati su uno nativo o sistema host e tali criteri richiamano limitazioni della larghezza di banda minima, la velocità effettiva complessiva per un gruppo NIC sarà inferiore rispetto a come sarebbe senza i criteri di larghezza di banda in posizione.  
  
-   **TCP Chimney**. TCP Chimney non è supportato con gruppo NIC perché TCP Chimney Offload offload l'intero stack di rete direttamente alla scheda di rete.  
  
-   **802.1 x autenticazione**. 802.1 Non utilizzare l'autenticazione X con gruppo NIC. Alcune opzioni che consentono la configurazione dell'autenticazione 802.1 X e gruppo NIC sulla stessa porta.  
  
Per ulteriori informazioni sull'utilizzo di gruppo NIC all'interno di macchine virtuali (VM) in esecuzione in un host Hyper-V, vedere [gruppo NIC in macchine virtuali & #40; Le macchine virtuali & #41;](../../technologies/nic-teaming/../../technologies/nic-teaming/NIC-Teaming-in-Virtual-Machines--VMs-.md).  
  
## <a name="bkmk_vmq"></a>Gruppo NIC e code di macchina virtuale (Code)  
Coda macchine Virtuali e gruppo NIC funzionino in modo ottimale; Coda macchine Virtuali devono essere attivata ogni volta che è abilitato Hyper-V. A seconda la modalità di configurazione del commutatore e l'algoritmo di distribuzione del carico, gruppo NIC sia presenta funzionalità coda macchine Virtuali al commutatore Hyper-V che mostra il numero di code disponibili per essere il minor numero di code supportati da qualsiasi scheda nel team (modalità Min code) o il numero totale di code disponibili in tutti i membri del team (modalità somma di code).  
  
In particolare, se il team è indipendenti dal commutatore teaming modalità e la distribuzione del carico è impostato per la modalità di porta Hyper-V o dinamico, quindi il numero di code segnalato è la somma di tutte le code disponibili dai membri del team (modalità somma di code); in caso contrario, il numero di code segnalato è il minor numero di code supportati da qualsiasi membro del team (modalità Min code).  
  
Ecco perché:  
  
-   Quando il team indipendenti commutatore è in modalità di porta Hyper-V o dinamico, il traffico in entrata per una porta del commutatore Hyper-V (VM) arriveranno sempre nel membro del team stesso. L'host può prevedere al controllo il membro verrà visualizzato il traffico per una determinata macchina virtuale in modo gruppo NIC può essere più ponderato sulle quali code di coda macchine Virtuali da allocare in un membro del gruppo specifico. Gruppo NIC, funziona con il commutatore Hyper-V, verrà impostata la coda di macchine Virtuali per una macchina virtuale in un membro del team e sapere che il traffico in entrata verrà raggiunto coda.  
  
-   Quando il team è in qualsiasi modalità dipendenti commutatore (Creazione gruppi statica o LACP teaming), il commutatore che il team è connesso a controlla la distribuzione del traffico in entrata. Software gruppo NIC dell'host non è in grado di prevedere quali team membro verrà visualizzato il traffico in entrata per una macchina virtuale, che potrebbe essere che il commutatore distribuisce il traffico per una macchina virtuale tra tutti i membri del team. Come risultato di un software gruppo NIC, funziona con il commutatore Hyper-V, i programmi una coda per la macchina virtuale in ogni membro del team, non solo un membro del team.  
  
-   Se il team è in modalità indipendenti dal commutatore e utilizza un algoritmo di distribuzione carico hash indirizzo, il traffico in entrata verrà sempre si accende una scheda di rete (il membro del gruppo primario) - interamente in un solo membro del team. Dato che altri membri del team non gestiscono il traffico in entrata che ottenere programmati con lo stesso code come membro primario in modo che se il membro primario non riesce a qualsiasi altro membro del team può essere usato per sollevare il traffico in entrata e le code sono già presenti.  
  
La maggior parte delle schede di rete sono code che possono essere utilizzate per Receive Side Scaling (RSS) o coda macchine Virtuali, ma non entrambi allo stesso tempo. Alcune impostazioni di coda macchine Virtuali risultano impostazioni per le code RSS, ma sono realmente le impostazioni nelle code generiche che utilizza RSS e coda macchine Virtuali a seconda di quale funzionalità è attualmente in uso. Ogni scheda di rete è, in essa ha le proprietà avanzate, i valori del parametro * RssBaseProcNumber e \*MaxRssProcessors. Di seguito sono alcune impostazioni di coda macchine Virtuali che offrono migliori prestazioni del sistema.  
  
-   Idealmente deve disporre di schede di rete di * RssBaseProcNumber impostato su un numero maggiore o uguale a due (2). Questo avviene perché il primo processore fisico, Core 0 (processori logici 0 e 1), in genere la maggior parte dell'elaborazione di sistema in modo che l'elaborazione di rete deve essere sterzanti fuori il processore fisico. (Alcuni architetture non hanno due processori logici per processore fisico quindi per tali macchine il processore di base deve essere maggiore o uguale a 1. Se in dubbio presuppongono l'host è con un processore logico 2 per ogni architettura di processore fisico.)  
  
-   Se il team è in modalità di somma-code devono essere processori dei membri del team per l'estensione pratico, non sovrapposte. In un host 4 core (8 processori logici) con un gruppo di schede NIC 10 Gbps 2, ad esempio, è possibile impostare il primo da utilizzare base processore pari a 2 e 4 core, usare il secondo verrebbe impostato per usare base processore 6 e 2 core.  
  
-   Se il team è in modalità Min code i set di processore per i membri del team devono essere identici.  
  
## <a name="bkmk_hnv"></a>Gruppo NIC e Hyper-V rete virtualizzazione (virtualizzazione)  
Gruppo NIC è compatibile con Hyper-V rete virtualizzazione.  Il sistema di gestione di virtualizzazione rete fornisce informazioni per il driver gruppo NIC che consente di distribuire il carico in modo che è ottimizzato per il traffico di virtualizzazione rete funzionalità gruppo NIC.  
  
## <a name="bkmk_live"></a>Gruppo NIC e migrazione in tempo reale  
Gruppo NIC nelle macchine virtuali non influiscono sulla migrazione in tempo reale. Le stesse regole esistano per la migrazione in tempo reale o meno gruppo NIC è configurato nella macchina virtuale.  
  
## <a name="see-also"></a>Vedere anche  
[Gruppo NIC nelle macchine virtuali & #40; Le macchine virtuali & #41;](../../technologies/nic-teaming/../../technologies/nic-teaming/NIC-Teaming-in-Virtual-Machines--VMs-.md)  
  


