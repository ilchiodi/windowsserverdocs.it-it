---
title: Creare un nuovo gruppo NIC in un Computer Host o macchina virtuale
description: Questo argomento fornisce informazioni sulla configurazione del gruppo NIC in modo da comprendere le selezioni che quando si configura un nuovo gruppo NIC in Windows Server 2016, è necessario apportare.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4caaa86-5799-4580-8775-03ee213784a3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6a9866d1f4e72b3c77c3233b5e5582d250cfe6a2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-new-nic-team-on-a-host-computer-or-vm"></a>Creare un nuovo gruppo NIC in un Computer Host o macchina virtuale

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Questo argomento fornisce informazioni sulla configurazione del gruppo NIC in modo da comprendere le selezioni che quando si configura un nuovo gruppo NIC, è necessario apportare. In questo argomento contiene le sezioni seguenti.  
  
-   [Scelta di una modalità di gruppo](#bkmk_teaming)  
  
-   [Scelta di una modalità di bilanciamento del carico](#bkmk_lb)  
  
-   [Scelta di un'impostazione scheda Standby](#bkmk_standby)  
  
-   [Utilizzando la proprietà dell'interfaccia gruppo primaria](#bkmk_primary)  
  
> [!NOTE]  
> Se si conoscono già questi elementi di configurazione, è possibile utilizzare le procedure seguenti per configurare gruppo NIC.  
>   
> -   [Creare un nuovo gruppo NIC in una macchina virtuale](../../technologies/nic-teaming/Create-a-New-NIC-Team-in-a-VM.md)  
> -   [Creare un nuovo gruppo NIC](../../technologies/nic-teaming/Create-a-New-NIC-Team.md)  
  
Quando si crea un nuovo gruppo NIC, è necessario configurare le seguenti proprietà di gruppo NIC.  
  
-   Nome gruppo  
  
-   Schede membro  
  
-   Modalità gruppo  
  
-   Modalità di bilanciamento del carico  
  
-   Scheda standby  
  
È anche possibile configurare l'interfaccia gruppo primaria e configurare un numero di LAN (VLAN) virtuale.  
  
Queste proprietà gruppo NIC vengono visualizzate nella figura seguente, che contiene i valori di esempio per alcune proprietà di gruppo NIC.  
  
![Proprietà di gruppo NIC](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_06_properties.jpg)  
  
## <a name="bkmk_teaming"></a>Scelta di una modalità di gruppo  
Le opzioni per la modalità di gruppo sono **commutatore indipendenti**, **Creazione gruppi statica**, e **controllo protocollo LACP (Link Aggregation)**. Creazione gruppi statica e LACP sono modalità dipendenti commutatore. Per ottimizzare le prestazioni gruppo NIC con tutte le tre modalità di gruppo, è consigliabile utilizzare una modalità di bilanciamento del carico di distribuzione dinamica.  
  
**Indipendenti dal commutatore**  
  
Con modalità passare indipendente, il commutatore o commutatori a cui sono connessi i membri del gruppo NIC non sono a conoscenza della presenza del gruppo NIC e non determinare come distribuire il traffico di rete per i membri del gruppo NIC - al contrario, il gruppo NIC distribuisce il traffico di rete in entrata tra i membri del gruppo NIC.  
  
Quando si Usa modalità indipendente commutatore con la distribuzione dinamica, il carico del traffico di rete viene distribuito in base l'hash indirizzo porte TCP come modificato dall'algoritmo di bilanciamento del carico dinamico. Algoritmo di bilanciamento del carico dinamico ridistribuisce flussi per ottimizzare l'utilizzo della larghezza di banda membro del team in modo che le trasmissioni di flusso singoli possono spostarsi da un membro del team di active a un altro. L'algoritmo prende in considerazione la possibilità di piccole dimensioni che ridistribuzione traffico potrebbe causare in ordine recapito dei pacchetti, in modo da passaggi per ridurre al minimo questa possibilità.  
  
**Dipendenti dal commutatore**  
  
Con modalità passare dipendente, il commutatore che sono connessi i membri del Team NIC determina come distribuire il traffico di rete in ingresso tra i membri del gruppo NIC. Il commutatore ha indipendenza completo per determinare come distribuire il traffico di rete tra i membri del gruppo NIC.  
  
> [!IMPORTANT]  
> Commutatore dipendenti gruppo richiede che tutti i membri del team sono connessi allo stesso commutatore fisico o uno multi-chassis di passa che condivide un ID di opzione tra più chassis.  
  
Gruppo statico è necessario configurare manualmente l'opzione sia l'host per identificare i collegamenti dal team. Poiché si tratta di una soluzione configurata staticamente, non esiste nessun protocollo aggiuntivo per facilitare il commutatore e collegato alla rete all'host di identificare correttamente i cavi o altri errori che potrebbero causare il team per non eseguire. In genere, questa modalità è supportata da commutatori di classe server.  
  
A differenza di creazione gruppi statica, modalità gruppo LACP identifica in modo dinamico i collegamenti che sono connessi tra l'host e il commutatore. Questa connessione dinamica consente la creazione automatica di un gruppo e, in teoria, ma raramente in pratica, l'espansione e riduzione di un gruppo semplicemente per la trasmissione o la ricezione dei pacchetti di LACP dall'entità peer. Tutti i commutatori di classe server supportano LACP tutti richiedono l'operatore di rete dall'amministratore abiliti manualmente LACP sulla porta del commutatore. Quando si configura una modalità gruppo di LACP, gruppo NIC funziona sempre in modalità attiva di LACP con un timer breve.  Nessuna opzione è attualmente disponibile per modificare il timer o modificare la modalità di LACP.  
  
Quando si Usa modalità dipendenti commutatore con la distribuzione dinamica, il carico del traffico di rete viene distribuito in base l'hash indirizzo TransportPorts come modificato dall'algoritmo di bilanciamento del carico dinamico.  L'algoritmo di bilanciamento del carico dinamico ridistribuisce flussi per ottimizzare l'utilizzo della larghezza di banda di membro del team. Le trasmissioni di flusso singoli possono spostare dal membro di un gruppo active a un altro come parte della distribuzione dinamica. L'algoritmo prende in considerazione la possibilità di piccole dimensioni che ridistribuzione traffico potrebbe causare in ordine recapito dei pacchetti, in modo da passaggi per ridurre al minimo questa possibilità.  
  
Come con tutte le configurazioni dipendenti di commutatore, il commutatore determina come distribuire il traffico in ingresso tra i membri del team.  Il commutatore deve eseguire un processo di distribuzione del traffico tra i membri del team ragionevole, ma ha indipendenza completo per determinare come ciò avviene.  
  
## <a name="bkmk_lb"></a>Scelta di una modalità di bilanciamento del carico  
Le opzioni per la modalità di distribuzione del bilanciamento del carico sono **Hash indirizzo**, **porta Hyper-V**, e **dinamico**.  
  
**Hash indirizzo**  
  
Questa modalità di bilanciamento del carico crea un hash basato su componenti degli indirizzi del pacchetto. Assegna i pacchetti che contengono tale valore hash a una delle schede disponibili. In genere questo singolo meccanismo è sufficiente per creare un bilanciamento ragionevole tra le schede disponibili.  
  
È possibile utilizzare Windows PowerShell per specificare valori per i seguenti componenti funzione hash.  
  
-   Indirizzi IP di origine e di destinazione e porte TCP di origine e di destinazione. Questa è l'impostazione predefinita quando si seleziona **Hash indirizzo** come la modalità di bilanciamento del carico.  
  
-   Origine e destinazione solo indirizzi IP.  
  
-   Solo indirizzi di origine e destinazione MAC.  
  
L'hash di porte TCP crea la distribuzione più granulare dei flussi di traffico, determinando flussi di dimensioni inferiori che possono essere spostati in modo indipendente tra i membri del team NIC. Tuttavia, è possibile utilizzare l'hash di porte TCP per il traffico non TCP o UDP su, o in cui le porte TCP e UDP sono nascoste dallo stack, ad esempio con traffico protetto da IPsec. In questi casi, l'hash viene utilizzato automaticamente l'hash indirizzo IP o, se il traffico non è il traffico IP, viene utilizzato l'hash indirizzo MAC.  
  
**Porta Hyper-V**  
  
Esiste un vantaggio in modalità di porta Hyper-V per i gruppi NIC configurati in host Hyper-V. Poiché le macchine virtuali dispongono di indirizzi MAC indipendenti, l'indirizzo MAC della macchina virtuale - o la porta che la macchina virtuale è collegata il commutatore virtuale Hyper-V - può essere la base su cui dividere il traffico di rete tra i membri del Team NIC.  
  
> [!IMPORTANT]  
> I gruppi NIC creati all'interno di macchine virtuali non può essere configurati con la modalità di bilanciamento del carico porta Hyper-V. Utilizzare l'Hash indirizzo caricare invece modalità bilanciamento del carico.  
  
Poiché il commutatore adiacente rileva sempre un determinato indirizzo MAC su una porta, il commutatore distribuisce il carico di ingresso (il traffico dal commutatore all'host) su più collegamenti in base all'indirizzo MAC (MAC di macchina virtuale) di destinazione. Ciò è particolarmente utile quando si utilizzano le code di macchina virtuale (Code), perché una coda può essere posizionata nella scheda di rete specifico in cui il traffico è previsto l'arrivo.  
  
Tuttavia, se l'host ha solo poche macchine virtuali, questa modalità potrebbe non essere abbastanza dettagliata per ottenere una distribuzione ben bilanciata. Questa modalità limita sempre una singola macchina virtuale (ad esempio, il traffico da una singola porta di commutazione) per la larghezza di banda disponibile su una singola interfaccia. Gruppo NIC utilizza la porta del commutatore virtuale Hyper-V come identificatore invece di usare l'indirizzo MAC di origine, perché, in alcuni casi, una macchina virtuale potrebbe essere configurata con più di un indirizzo MAC su una porta di commutazione.  
  
**Dinamico**  
  
Questa modalità di bilanciamento carico utilizza gli aspetti migliori di ognuna delle due modalità e li combina in una singola modalità:  
  
-   Carica in uscita viene distribuite in base a un hash degli indirizzi IP e le porte TCP.  In modalità dinamica eseguito anche il ribilanciamento carichi in tempo reale in modo che un determinato flusso in uscita, può spostarsi avanti e indietro tra i membri del team.  
  
-   Carica in ingresso viene distribuiti nello stesso modo come la modalità di porta Hyper-V.  
  
I carichi in uscita in questa modalità sono bilanciati in modo dinamico in base al concetto flowlets. Come voce umana è naturale interruzioni alla fine delle parole e frasi, flussi TCP (flussi di comunicazioni TCP) hanno anche interruzioni naturali. La parte di un flusso TCP tra due tali interruzioni viene definita come un flowlet.  
  
Quando l'algoritmo in modalità dinamica rileva che è stato rilevato un limite flowlet -, ad esempio quando è stata eseguita un'interruzione di lunghezza sufficiente nel flusso TCP - l'algoritmo eseguito automaticamente il ribilanciamento il flusso verso un altro membro del gruppo se opportuno.  In alcuni casi l'algoritmo potrebbe inoltre periodicamente ribilanciare i flussi che non contengono alcun flowlets. Per questo motivo, l'affinità tra membri del team e di flusso TCP cambiare in qualsiasi momento per bilanciare il carico di lavoro dei membri del team di funzionamento dell'algoritmo di bilanciamento del carico dinamico.  
  
Se il team è configurato con indipendenti commutatore o una delle modalità dipendenti commutatore, è consigliabile utilizzare la modalità di distribuzione dinamica per ottenere prestazioni ottimali.  
  
Quando il gruppo NIC ha solo due membri del team, è configurato in modalità indipendenti commutatore ed è abilitata, con una scheda di rete attiva la modalità attivo/Standby e l'altro configurato per la modalità Standby, esiste un'eccezione a questa regola. Con questa configurazione gruppo NIC, distribuzione di Hash indirizzo garantisce prestazioni migliori leggermente dinamico distribuzione.  
  
## <a name="bkmk_standby"></a>Scelta di un'impostazione scheda Standby  
Le opzioni per la scheda di Standby sono None (tutte le schede attivo) o la selezione di una scheda di rete specifico del team NIC che fungerà da una scheda di Standby, mentre altri membri del team non selezionati sono attivi. Quando si configura una scheda di rete come una scheda di Standby, alcun traffico di rete viene inviato o elaborato dalla scheda, a meno che e fino al momento quando una scheda di rete Active ha esito negativo. Dopo una scheda di rete Active non riesce, la scheda NIC di Standby diventa attiva e i processi il traffico di rete. Se e quando vengono ripristinati tutti i membri del team, viene restituito il membro del team di standby per lo stato standby.  
  
Se si dispone di un gruppo di due schede di rete e si sceglie di configurare una scheda di rete come una scheda di Standby, si perderanno la larghezza di banda vantaggi di aggregazione esistenti con due schede di rete attive.  
  
> [!IMPORTANT]  
> Non è necessario designare una scheda di Standby per ottenere la tolleranza di errore; la tolleranza di errore è sempre presente ogni volta che sono disponibili almeno due schede di rete in un gruppo NIC.  
  
## <a name="bkmk_primary"></a>Utilizzando la proprietà dell'interfaccia gruppo primaria  
Per accedere a nella finestra di dialogo interfaccia Team primario, è necessario fare clic sul collegamento evidenziato nell'immagine seguente.  
  
![Proprietà interfaccia gruppo primaria](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_10_primaryteaminterface.jpg)  
  
Dopo aver selezionato il collegamento evidenziato, il comando seguente **nuova interfaccia Team** apre la finestra di dialogo.  
  
![Finestra di dialogo nuova interfaccia Team](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_newteaminterface.jpg)  
  
Se si utilizzano VLAN, è possibile utilizzare questa finestra di dialogo per specificare un numero VLAN.  
  
Se si utilizzano VLAN, è possibile specificare un nome tNIC per il gruppo NIC.  
  
## <a name="see-also"></a>Vedere anche  
[Creare un nuovo gruppo NIC in una macchina virtuale](../../technologies/nic-teaming/Create-a-New-NIC-Team-in-a-VM.md)  
[Gruppo NIC](NIC-Teaming.md)  
  


