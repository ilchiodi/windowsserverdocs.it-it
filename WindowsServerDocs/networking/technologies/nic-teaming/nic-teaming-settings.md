---
title: Impostazioni di gruppo NIC
description: In questo argomento è fornire una panoramica delle proprietà del Team di interfaccia di rete, ad esempio gruppo NIC e modalità di bilanciamento del carico. È inoltre offrono informazioni dettagliate sull'impostazione della scheda di Standby e la proprietà dell'interfaccia gruppo primaria. Se si dispone di almeno due schede di rete in un gruppo NIC, non devi designare una scheda di Standby per la tolleranza di errore.
manager: dougkim
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
ms.date: 09/13/2018
ms.openlocfilehash: 57957e88ff4c398be23355534d5cc0ad7f920bb1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877932"
---
# <a name="nic-teaming-settings"></a>Impostazioni di gruppo NIC
In questo argomento è fornire una panoramica delle proprietà del Team di interfaccia di rete, ad esempio gruppo NIC e modalità di bilanciamento del carico. È inoltre offrono informazioni dettagliate sull'impostazione della scheda di Standby e la proprietà dell'interfaccia gruppo primaria. Se si dispone di almeno due schede di rete in un gruppo NIC, non devi designare una scheda di Standby per la tolleranza di errore.


  
![Proprietà di gruppo NIC](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_06_properties.jpg)  

## <a name="teaming-modes"></a>Modalità di raggruppamento 
Le opzioni per la modalità gruppo vengono **commutatore indipendente** e **commutatore dipendenti**. Include la modalità dipendenti commutatore **Creazione gruppi statica** e **protocollo LACP (Link Aggregation controllo Protocol)**. 

>[!TIP]
>Per ottimizzare le prestazioni di gruppo NIC, è consigliabile usare una modalità di bilanciamento del carico della distribuzione dinamica.  
  
### <a name="switch-independent"></a>Commutatore indipendente
  
[!INCLUDE [switch-independent-shortdesc-include](../../includes/switch-independent-shortdesc-include.md)] 
  
Quando si usa la modalità commutatore indipendente con la distribuzione dinamica, il carico del traffico di rete viene distribuito in base all'hash indirizzo porte TCP come modificata dall'algoritmo di bilanciamento del carico dinamico. Algoritmo di bilanciamento del carico dinamico vengono ridistribuiti i flussi per ottimizzare l'utilizzo della larghezza di banda del membro del team in modo che le trasmissioni di flow singoli possono passare da un membro del team di active a altra. L'algoritmo prende in considerazione la possibilità di piccole dimensioni che ridistribuzione del traffico potrebbe causare ordinati di recapito dei pacchetti, in modo da questa procedura per ridurre al minimo tale possibilità.  
  
### <a name="switch-dependent"></a>Dipendenti dal commutatore  

[!INCLUDE [switch-dependent-shortdesc-include](../../includes/switch-dependent-shortdesc-include.md)]  
  
> [!IMPORTANT]  
> Opzione gruppo NIC dipendenti richiede che tutti i membri del team sono connessi allo stesso commutatore fisico o uno multi-chassis switch che condivide un ID di passare tra più chassis.


- **Creazione gruppi statica.** Creazione gruppi statica è necessario configurare manualmente il commutatore sia l'host per identificare quali collegamenti formano il gruppo. Poiché si tratta di una soluzione configurata staticamente, non vi è nessun protocollo aggiuntivo per facilitare l'opzione e l'host per identificare in modo non corretto collegato cavi o altri errori che potrebbero compromettere il team a non riuscire a eseguire. Questa modalità è in genere supportata dai commutatori di classe server.

- **Link Aggregation Control Protocol (LACP).** A differenza di creazione gruppi statica, la modalità di raggruppamento LACP identifica in modo dinamico i collegamenti connessi tra l'host e il commutatore. Questa connessione dinamica consente la creazione automatica di un team e, in teoria, ma raramente in pratica, l'espansione e riduzione di un gruppo semplicemente mediante la trasmissione o ricezione dei pacchetti di LACP dall'entità peer. Tutti i commutatori di classe server supportano LACP tutti richiedono l'operatore di rete da abilitare a livello amministrativo LACP sulla porta di commutazione. Quando si configura una modalità gruppo di LACP, NIC Teaming viene sempre eseguito in modalità attiva del LACP con un timer breve.  Opzione non è attualmente disponibile per la modifica del timer o modificare la modalità di LACP.


Quando si Usa modalità dipendenti commutatore con la distribuzione dinamica, il carico del traffico di rete viene distribuito in base all'hash indirizzo TransportPorts come modificata dall'algoritmo di bilanciamento del carico dinamico.  L'algoritmo di bilanciamento del carico dinamico vengono ridistribuiti i flussi per ottimizzare l'utilizzo della larghezza di banda del membro del team. Le trasmissioni di flussi possono spostare da un membro attivo a un altro come parte della distribuzione dinamica. L'algoritmo prende in considerazione la possibilità di piccole dimensioni che ridistribuzione del traffico potrebbe causare ordinati di recapito dei pacchetti, in modo da questa procedura per ridurre al minimo tale possibilità.  
  
Come con tutte le configurazioni dipendenti di commutatore, il commutatore determina come distribuire il traffico in ingresso tra i membri del team.  L'opzione è previsto per eseguire un processo di distribuzione del traffico tra i membri del team ragionevole, ma dispone di indipendenza completa per determinare come vengono eseguite.  


## <a name="load-balancing-modes"></a>Modalità di bilanciamento del carico  
Le opzioni per la modalità di distribuzione del bilanciamento del carico sono **Hash indirizzo**, **porta Hyper-V**, e **dinamica**.  
  
### <a name="address-hash"></a>Hash indirizzo
  
[!INCLUDE [address-hash-shortdesc-include](../../includes/address-hash-shortdesc-include.md)]
  
Usare Windows PowerShell per specificare i valori per i seguenti componenti di funzione di hashing.  
  
-   Indirizzi IP di origine e di destinazione e porte TCP di origine e destinazione. Questo è il valore predefinito quando si seleziona **Hash indirizzo** come modalità di bilanciamento del carico.  
  
-   Origine e destinazione solo indirizzi IP.  
  
-   Solo indirizzi di origine e destinazione MAC.  
  
L'hash di porte TCP crea la distribuzione più granulare dei flussi di traffico, determinando flussi di dimensioni inferiori che possono essere spostati in modo indipendente tra i membri del team NIC. Tuttavia, non è possibile utilizzare l'hash di porte TCP per il traffico non TCP o UDP, o in cui le porte TCP e UDP sono nascoste dallo stack, ad esempio con traffico protetto tramite IPsec. In questi casi, l'hash usa automaticamente l'hash di indirizzo IP o, se il traffico non è il traffico IP, viene usato l'hash di indirizzo MAC.  
  
### <a name="hyper-v-port"></a>Porta Hyper-V
  
[!INCLUDE [hyper-v-port-shortdesc-include](../../includes/hyper-v-port-shortdesc-include.md)]  
  
Poiché il commutatore adiacente rileva sempre un particolare indirizzo MAC su una porta, l'opzione distribuisce il carico di traffico in ingresso (il traffico dal commutatore all'host) su più collegamenti in base all'indirizzo MAC (della macchina virtuale MAC) di destinazione. Ciò è particolarmente utile quando si utilizzano le code di macchine virtuali (Code), perché una coda può essere inserita nella scheda di rete specifico in cui è previsto il traffico in arrivo.  
  
Tuttavia, se l'host ha solo alcune macchine virtuali, questa modalità potrebbe non essere sufficientemente granulare per ottenere una distribuzione ben bilanciata. Questa modalità limita inoltre sempre una singola macchina virtuale (ad esempio, il traffico da una porta di commutazione singolo) per la larghezza di banda disponibile su una singola interfaccia. Gruppo NIC utilizza la porta del commutatore virtuale Hyper-V come identificatore invece di usare l'indirizzo MAC di origine perché, in alcuni casi, potrebbe essere possibile configurare una VM con più di un indirizzo MAC su una porta di commutazione.  
  
### <a name="dynamic"></a>Dynamic
  
[!INCLUDE [dynamic-shortdesc-include](../../includes/dynamic-shortdesc-include.md)]
  
I caricamenti in uscita in questa modalità vengono bilanciati in modo dinamico basato sul concetto di flowlets. Esattamente come la voce umana è naturale interruzioni alla fine delle parole e frasi, i flussi TCP (flussi di comunicazione TCP) inoltre contenere interruzioni di naturali. La parte di un flusso TCP tra due interruzioni di questo tipo si intende un flowlet.  
  
Quando l'algoritmo in modalità dinamica consente di rilevare che è stato rilevato un limite flowlet - ad esempio quando è stata eseguita un'interruzione di lunghezza sufficiente nel flusso TCP - algoritmo ribilancia automaticamente il flusso a un altro membro del team, se appropriato.  In alcune circostanze l'algoritmo potrebbe anche periodicamente ribilanciare i flussi che non contengono alcun flowlets. Per questo motivo, l'affinità tra i membri di team e il flusso TCP può cambiare in qualsiasi momento come funziona l'algoritmo di bilanciamento del carico dinamico per bilanciare il carico di lavoro dei membri del team.  
  
Se il team è configurato con commutatore indipendente o una delle modalità di commutatore dipendente, è consigliabile utilizzare la modalità di distribuzione dinamica per ottenere prestazioni ottimali.  
  
Si verifica un'eccezione a questa regola quando il gruppo NIC ha solo due membri del team, è configurato in modalità commutatore indipendente ed è abilitata, con una scheda di rete attiva la modalità attiva/Standby e l'altro configurato per la modalità Standby. Con questa configurazione di gruppo NIC, la distribuzione Hash indirizzo offre prestazioni leggermente migliori rispetto alle dinamiche distribuzione.  


## <a name="standby-adapter-setting"></a>Impostazione della scheda in standby  
Le opzioni per Adapter di Standby **Nessuno (tutte le schede attive)** o la selezione di una scheda di rete specifici nel gruppo NIC che agisce come una scheda di Standby. Quando si configura una scheda di rete come scheda di Standby, tutti gli altri membri del team non selezionato sono attivi e nessun traffico di rete viene inviato o elaborato dall'adapter fino a quando non si verifica un errore di un'interfaccia di rete attiva. Dopo che un'interfaccia di rete attivi non riesce, l'interfaccia di rete di Standby diventa attiva e il traffico di rete dei processi. Quando vengono ripristinati tutti i membri del team al servizio, il membro del team standby viene ripristinato lo stato standby.  

Se si dispone di un team di due schede di rete e si sceglie di configurare una scheda di rete come scheda di Standby, la larghezza di banda si perdono i vantaggi di aggregazione che esistono in due schede di rete active.  Non è necessario designare una scheda di Standby per raggiungere tolleranza di errore; tolleranza di errore è sempre presente ogni volta che sono presenti almeno due schede di rete in un gruppo NIC.
 
  
## <a name="primary-team-interface-property"></a>Proprietà di interfaccia primaria Team  
Per accedere alla finestra di dialogo interfaccia gruppo primaria, è necessario fare clic sul collegamento evidenziato nell'immagine seguente.  
  
![Proprietà dell'interfaccia gruppo primaria](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_10_primaryteaminterface.jpg)  
  
Dopo aver fatto clic sul collegamento evidenziato, quanto segue **nuova interfaccia Team** verrà visualizzata la finestra di dialogo.  
  
![Finestra di dialogo Nuova interfaccia gruppo](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_newteaminterface.jpg)  
  
Se si usano VLAN, è possibile utilizzare questa finestra di dialogo per specificare un numero VLAN.  
  
Se si usano VLAN, è possibile specificare un nome tNIC per il gruppo NIC.  
  


---