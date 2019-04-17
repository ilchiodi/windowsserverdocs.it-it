---
ms.assetid: e727a33d-133b-43c9-b6a4-7c00f9cb6000
title: Revisione dei modelli di dominio
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c57c23c0bd8694b66ab29b45bd9e47826ed1e01d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="reviewing-the-domain-models"></a>Revisione dei modelli di dominio

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

I seguenti fattori impatto il modello di progettazione di dominio selezionato:  
  
-   Quantità di capacità disponibile nella rete che si è disposti ad allocare a servizi di dominio Active Directory (AD DS). L'obiettivo consiste nel selezionare un modello che fornisce la replica efficiente delle informazioni con un impatto minimo sulla larghezza di banda di rete disponibile.  
  
-   Numero di utenti dell'organizzazione. Se l'organizzazione include un numero elevato di utenti, la distribuzione di più di un dominio consente di partizionare i dati e offre un maggiore controllo la quantità di traffico di replica che verrà passati attraverso una connessione di rete specificato. Questo consente di controllare in cui vengono replicati i dati e ridurre il carico creato dal traffico di replica sulla lente in rete.  
  
Il progetto di dominio più semplice è un singolo dominio. In un singolo dominio, tutte le informazioni viene replicate in tutti i controller di dominio. Se necessario, tuttavia, è possibile distribuire altri domini regionali. Ciò può verificarsi se alcune parti dell'infrastruttura di rete sono connessi tramite collegamenti lenti, e il proprietario della foresta desidera assicurarsi che il traffico di replica non superi la capacità che è stata allocata al dominio Active Directory.  
  
È consigliabile ridurre al minimo il numero di domini da distribuire nell'insieme di strutture. Ciò riduce la complessità della distribuzione globale e, di conseguenza, riduce il costo totale di proprietà. La tabella seguente elenca i costi amministrativi associati con l'aggiunta di domini regionali.  
  
|Costo|Implicazioni|  
|--------|----------------|  
|Gestione di più gruppi di amministratore del servizio|Ogni dominio ha propri gruppi di amministratori del servizio che devono essere gestiti in modo indipendente. L'appartenenza di questi gruppi di amministratori del servizio deve essere controllato attentamente.|  
|Mantenere la coerenza tra le impostazioni di criteri di gruppo che sono comuni a più domini|Le impostazioni di criteri di gruppo che devono essere applicate a livello di foresta devono essere applicate separatamente per ogni singolo dominio nella foresta.|  
|Mantenere la coerenza tra il controllo degli accessi e le impostazioni comuni a più domini di controllo|Il controllo degli accessi e le impostazioni che devono essere applicate nell'insieme di strutture di controllo devono essere applicati separatamente per ogni singolo dominio nella foresta.|  
|Maggiore probabilità di oggetti, lo spostamento tra domini|Maggiore è il numero di domini, maggiore è la probabilità che gli utenti dovranno passare da un dominio a altro. Questo spostamento potrebbe potenzialmente influire gli utenti finali.|  
  
> [!NOTE]  
>  Granulari per le password di Windows Server 2008 e il blocco degli account possono influire anche il modello di progettazione di dominio selezionato. Prima di questa versione di Windows Server 2008, è possibile applicare un solo account e password criterio di blocco, specificato nel dominio criterio dominio predefinito, a tutti gli utenti nel dominio. Di conseguenza, se si desidera utilizzare password diverse e impostazioni di blocco degli account per diversi set di utenti, è necessario creare un filtro password o distribuire più domini. È ora possibile utilizzare criteri granulari per le password per specificare più criteri password e per applicare restrizioni diverse password e criteri di blocco account a diversi gruppi di utenti in un singolo dominio. Per ulteriori informazioni su specifici per le password e il blocco degli account, vedere il Step-by-Step Guida per specifici per le Password e configurazione dei criteri di blocco Account ([https://go.microsoft.com/fwlink/?LinkID=91477](https://go.microsoft.com/fwlink/?LinkID=91477)).  
  
## <a name="single-domain-model"></a>Modello a dominio singolo  
Un modello di dominio singolo è più semplice da amministrare e meno costoso da mantenere. È costituito da un insieme di strutture che contiene un singolo dominio. Dominio radice della foresta è il dominio e contiene tutti gli account utente e gruppo nella foresta.  
  
Modello di foresta un unico dominio riduce complessità amministrativa fornendo i vantaggi seguenti:  
  
-   Qualsiasi controller di dominio può autenticare qualsiasi utente nella foresta.  
  
-   Tutti i controller di dominio possono essere cataloghi globali, pertanto non è necessario pianificare di posizionamento dei server di catalogo globale.  
  
In una foresta con dominio singolo, tutti i dati di directory viene replicata in tutte le aree geografiche che ospitano controller di dominio. Mentre questo modello è la più facile da gestire, viene creato anche il traffico di replica la maggior parte dei modelli di due domini. Partizione della directory in più domini limita la replica di oggetti a determinate aree geografiche ma risultati più il carico amministrativo.  
  
## <a name="regional-domain-model"></a>Modello di dominio regionale  
Tutti i dati oggetto all'interno di un dominio viene replicato in tutti i controller di dominio nel dominio. Per questo motivo, se la foresta include un numero elevato di utenti che sono distribuiti in diverse posizioni geografiche connesse tramite una rete di wide area network (WAN), potrebbe essere necessario per la distribuzione di domini regionali per ridurre il traffico di replica su collegamenti WAN. Domini regionali geografico possono essere organizzati in base alla connettività di rete WAN.  
  
Il modello di dominio regionale consente di gestire un ambiente stabile nel tempo. Le aree usate per definire i domini nel modello su elementi stabili, ad esempio continentali limiti di base. Domini basati su altri fattori, quali gruppi all'interno dell'organizzazione, possono cambiare di frequente e potrebbero essere necessario ristrutturare l'ambiente.  
  
Il modello di dominio regionale è costituito da un dominio radice della foresta e uno o più domini regionali. Creazione di una progettazione di modello di dominio regionale implica che identifica il dominio è il dominio radice della foresta e determinare il numero di domini aggiuntivi necessari per soddisfare i requisiti di replica. Se nell'organizzazione sono gruppi che richiedono dati isolamento o isolamento dei servizi da altri gruppi all'interno dell'organizzazione, creare una foresta separata per questi gruppi. Domini non forniscono dati isolamento o isolamento dei servizi.  
  


