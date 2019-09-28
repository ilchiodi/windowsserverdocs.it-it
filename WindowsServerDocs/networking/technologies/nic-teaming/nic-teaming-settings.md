---
title: Impostazioni gruppo NIC
description: In questo argomento viene illustrata una panoramica delle proprietà del gruppo NIC, ad esempio le modalità gruppo e bilanciamento del carico. Vengono inoltre illustrati i dettagli relativi all'impostazione della scheda standby e alla proprietà principale dell'interfaccia del team. Se si dispone di almeno due schede di rete in un gruppo NIC, non è necessario designare una scheda standby per la tolleranza di errore.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4caaa86-5799-4580-8775-03ee213784a3
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: ab9a8e309c8031108d58c73d82357e913d5ce398
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396478"
---
# <a name="nic-teaming-settings"></a>Impostazioni gruppo NIC
In questo argomento viene illustrata una panoramica delle proprietà del gruppo NIC, ad esempio le modalità gruppo e bilanciamento del carico. Vengono inoltre illustrati i dettagli relativi all'impostazione della scheda standby e alla proprietà principale dell'interfaccia del team. Se si dispone di almeno due schede di rete in un gruppo NIC, non è necessario designare una scheda standby per la tolleranza di errore.


  
![Proprietà del gruppo NIC](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_06_properties.jpg)  

## <a name="teaming-modes"></a>Modalità gruppo 
Le opzioni per la modalità gruppo sono **indipendenti dall'opzione** e **variano a seconda**del parametro. La modalità dipendente dal Commuter include il **gruppo statico** e il **protocollo LACP (Link Aggregation Control Protocol)** . 

>[!TIP]
>Per ottimizzare le prestazioni del team NIC, è consigliabile usare una modalità di bilanciamento del carico di distribuzione dinamica.  
  
### <a name="switch-independent"></a>Cambia indipendente
  
[!INCLUDE [switch-independent-shortdesc-include](../../includes/switch-independent-shortdesc-include.md)] 
  
Quando si usa la modalità switch Independent con la distribuzione dinamica, il carico del traffico di rete viene distribuito in base all'hash dell'indirizzo delle porte TCP modificato dall'algoritmo di bilanciamento del carico dinamico. L'algoritmo di bilanciamento del carico dinamico ridistribuisce i flussi per ottimizzare l'utilizzo della larghezza di banda dei membri del team in modo che le trasmissioni di flussi individuali possano passare da un membro del team attivo a un altro. L'algoritmo prende in considerazione la piccola possibilità che la ridistribuzione del traffico potrebbe causare il recapito non ordinato dei pacchetti, pertanto viene eseguita una procedura per ridurre al minimo tale possibilità.  
  
### <a name="switch-dependent"></a>Dipendente dal commutire  

[!INCLUDE [switch-dependent-shortdesc-include](../../includes/switch-dependent-shortdesc-include.md)]  
  
> [!IMPORTANT]  
> Il cambio gruppo dipendente richiede che tutti i membri del team siano connessi allo stesso compartitore fisico o a uno switch a più chassis che condivide un ID switch tra più chassis.


- **Gruppo statico.** Il gruppo statico richiede di configurare manualmente sia l'opzione che l'host per identificare i collegamenti che formano il team. Poiché si tratta di una soluzione configurata in modo statico, non esiste alcun protocollo aggiuntivo per assistere il Commuter e l'host per identificare i cavi collegati in modo errato o altri errori che potrebbero causare un errore del team. Questa modalità è in genere supportata dai commutatori di classe server.

- **Protocollo LACP (Link Aggregation Control Protocol).** A differenza del gruppo statico, la modalità gruppo LACP identifica in modo dinamico i collegamenti connessi tra l'host e il Commuter. Questa connessione dinamica consente la creazione automatica di un team e, in teoria, ma raramente in pratica, l'espansione e la riduzione di un team semplicemente tramite la trasmissione o la ricezione di pacchetti LACP dall'entità peer. Tutte le opzioni di classe server supportano LACP e richiedono che l'operatore di rete abiliti in modo amministrativo LACP sulla porta del commutatore. Quando si configura una modalità gruppo di LACP, gruppo NIC opera sempre in modalità attiva di LACP con un breve timer.  Attualmente non è disponibile alcuna opzione per modificare il timer o modificare la modalità LACP.


Quando si usano le modalità di commutazione dipendente con la distribuzione dinamica, il carico del traffico di rete viene distribuito in base all'hash dell'indirizzo TransportPorts modificato dall'algoritmo di bilanciamento del carico dinamico.  L'algoritmo di bilanciamento del carico dinamico ridistribuisce i flussi per ottimizzare l'utilizzo della larghezza di banda dei membri del team. Le trasmissioni di singoli flussi possono passare da un membro del team attivo a un altro come parte della distribuzione dinamica. L'algoritmo prende in considerazione la piccola possibilità che la ridistribuzione del traffico potrebbe causare il recapito non ordinato dei pacchetti, pertanto viene eseguita una procedura per ridurre al minimo tale possibilità.  
  
Come per tutte le configurazioni dipendenti dal cambio, l'opzione determina come distribuire il traffico in ingresso tra i membri del team.  È previsto che il compartitore esegua un processo ragionevole per la distribuzione del traffico tra i membri del team, ma ha un'indipendenza completa per determinare la modalità di esecuzione.  


## <a name="load-balancing-modes"></a>Modalità di bilanciamento del carico  
Le opzioni per la modalità di distribuzione del bilanciamento del carico sono **hash indirizzo**, **porta Hyper-V**e **dinamica**.  
  
### <a name="address-hash"></a>Hash indirizzo
  
[!INCLUDE [address-hash-shortdesc-include](../../includes/address-hash-shortdesc-include.md)]
  
Utilizzare Windows PowerShell per specificare i valori per i seguenti componenti della funzione hash.  
  
-   Porte TCP di origine e di destinazione e indirizzi IP di origine e di destinazione. Si tratta dell'impostazione predefinita quando si seleziona **hash indirizzo** come modalità di bilanciamento del carico.  
  
-   Solo indirizzi IP di origine e di destinazione.  
  
-   Solo indirizzi MAC di origine e di destinazione.  
  
L'hash delle porte TCP crea la distribuzione più granulare dei flussi di traffico, ottenendo flussi di dimensioni inferiori che possono essere spostati in modo indipendente tra i membri del gruppo NIC. Tuttavia, non è possibile utilizzare l'hash delle porte TCP per il traffico non basato su TCP o UDP oppure quando le porte TCP e UDP sono nascoste dallo stack, ad esempio con traffico protetto da IPsec. In questi casi, l'hash usa automaticamente l'hash dell'indirizzo IP o, se il traffico non è il traffico IP, viene usato l'hash dell'indirizzo MAC.  
  
### <a name="hyper-v-port"></a>Porta Hyper-V
  
[!INCLUDE [hyper-v-port-shortdesc-include](../../includes/hyper-v-port-shortdesc-include.md)]  
  
Poiché l'opzione adiacente Visualizza sempre un particolare indirizzo MAC su una porta, l'opzione distribuisce il carico in ingresso (il traffico dal passaggio all'host) su più collegamenti in base all'indirizzo MAC di destinazione (MAC VM). Questa operazione è particolarmente utile quando si usano le code di macchine virtuali (code macchine virtuali), perché una coda può essere inserita nella scheda di interfaccia di rete specifica in cui è previsto il traffico.  
  
Tuttavia, se l'host dispone solo di poche VM, questa modalità potrebbe non essere sufficientemente granulare per ottenere una distribuzione ben bilanciata. Questa modalità consente inoltre di limitare sempre una singola macchina virtuale, ovvero il traffico da una singola porta di commutazione, alla larghezza di banda disponibile su una singola interfaccia. Gruppo NIC usa la porta del Commuter virtuale Hyper-V come identificatore anziché usare l'indirizzo MAC di origine perché, in alcuni casi, una macchina virtuale può essere configurata con più di un indirizzo MAC su una porta di commutazione.  
  
### <a name="dynamic"></a>Dynamic
  
[!INCLUDE [dynamic-shortdesc-include](../../includes/dynamic-shortdesc-include.md)]
  
I carichi in uscita in questa modalità sono bilanciati in modo dinamico in base al concetto di flowlets. Proprio come la sintesi vocale umana ha interruzioni naturali alle estremità di parole e frasi, i flussi TCP (flussi di comunicazione TCP) hanno anche interruzioni naturalmente. La parte di un flusso TCP tra due interruzioni di questo tipo è denominata flowlet.  
  
Quando l'algoritmo in modalità dinamica rileva che è stato rilevato un limite flowlet, ad esempio quando si è verificata un'interrotta di lunghezza sufficiente nel flusso TCP, l'algoritmo ribilancia automaticamente il flusso a un altro membro del team, se appropriato.  In alcune circostanze l'algoritmo potrebbe anche ribilanciare periodicamente i flussi che non contengono alcun flowlets. Per questo motivo, l'affinità tra il flusso TCP e il membro del team può cambiare in qualsiasi momento, poiché l'algoritmo di bilanciamento dinamico funziona per bilanciare il carico di lavoro dei membri del team.  
  
Se il team è configurato con switch Independent o una delle modalità dipendente dall'opzione, è consigliabile usare la modalità di distribuzione dinamica per ottenere prestazioni ottimali.  
  
Si verifica un'eccezione a questa regola quando il gruppo NIC dispone solo di due membri del team, viene configurato in modalità indipendente dal commutatore ed è abilitata la modalità attivo/standby, con una scheda di interfaccia di rete attiva e l'altra configurata per la modalità standby. Con questa configurazione del team NIC, la distribuzione degli hash degli indirizzi offre prestazioni leggermente migliori rispetto alla distribuzione dinamica.  


## <a name="standby-adapter-setting"></a>Impostazione della scheda standby  
Le opzioni per la scheda standby sono **None (tutti gli adapter attivi)** o la selezione di una scheda di rete specifica nel gruppo NIC che funge da scheda standby. Quando si configura una scheda di interfaccia di rete come adattatore standby, tutti gli altri membri del team non selezionati sono attivi e nessun traffico di rete viene inviato o elaborato dall'adapter finché una scheda di interfaccia di rete attiva non riesce. Quando una scheda di interfaccia di rete attiva non riesce, la scheda di interfaccia di rete standby diventa attiva ed elabora il traffico Quando tutti i membri del team vengono ripristinati al servizio, il membro del team standby torna allo stato standby.  

Se si dispone di un gruppo NIC e si sceglie di configurare una scheda di interfaccia di rete come adattatore standby, si perderanno i vantaggi dell'aggregazione della larghezza di banda esistenti con due schede di interfaccia di rete attive.  Non è necessario designare una scheda standby per ottenere la tolleranza di errore. la tolleranza di errore è sempre presente ogni volta che sono presenti almeno due schede di rete in un gruppo NIC.
 
  
## <a name="primary-team-interface-property"></a>Proprietà di interfaccia del team primario  
Per accedere alla finestra di dialogo interfaccia del team principale, è necessario fare clic sul collegamento evidenziato nella figura seguente.  
  
![Proprietà di interfaccia del team primario](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_10_primaryteaminterface.jpg)  
  
Dopo aver fatto clic sul collegamento evidenziato, viene visualizzata la finestra di dialogo **nuova interfaccia team** seguente.  
  
![Finestra di dialogo Nuova interfaccia gruppo](../../media/Create-a-New-NIC-Team-on-a-Host-Computer-or-VM/nict_newteaminterface.jpg)  
  
Se si usano le VLAN, è possibile usare questa finestra di dialogo per specificare un numero VLAN.  
  
Indipendentemente dal fatto che si utilizzino le VLAN, è possibile specificare un nome tNIC per il gruppo NIC.  
  


---