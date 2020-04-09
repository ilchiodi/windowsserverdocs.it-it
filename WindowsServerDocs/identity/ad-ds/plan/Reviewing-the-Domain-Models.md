---
ms.assetid: e727a33d-133b-43c9-b6a4-7c00f9cb6000
title: Revisione dei modelli di dominio
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: b77f634d8548994d2f9e130faad9ca34aa226327
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821954"
---
# <a name="reviewing-the-domain-models"></a>Revisione dei modelli di dominio

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

I fattori seguenti influiscono sul modello di progettazione del dominio selezionato:  
  
- Quantità di capacità disponibile sulla rete che si desidera allocare per Active Directory Domain Services (AD DS). L'obiettivo consiste nel selezionare un modello che fornisca una replica efficiente delle informazioni con un effetto minimo sulla larghezza di banda di rete disponibile.  

- Numero di utenti dell'organizzazione. Se l'organizzazione include un numero elevato di utenti, la distribuzione di più di un dominio consente di partizionare i dati e offre un maggiore controllo sulla quantità di traffico di replica che passerà attraverso una connessione di rete specifica. In questo modo è possibile controllare la posizione in cui i dati vengono replicati e ridurre il carico creato dal traffico di replica sui collegamenti lenti nella rete.  

La progettazione del dominio più semplice è un singolo dominio. In una progettazione a singolo dominio, tutte le informazioni vengono replicate in tutti i controller di dominio. Se necessario, tuttavia, è possibile distribuire altri domini regionali. Questo problema può verificarsi se parti dell'infrastruttura di rete sono connesse da collegamenti lenti e il proprietario della foresta desidera assicurarsi che il traffico di replica non superi la capacità allocata ai servizi di dominio Active Directory.  

È consigliabile ridurre al minimo il numero di domini distribuiti nella foresta. In questo modo si riduce la complessità complessiva della distribuzione e, di conseguenza, viene ridotto il costo totale di proprietà. Nella tabella seguente sono elencati i costi amministrativi associati all'aggiunta di domini regionali.  

|Costo|Implicazioni|  
|--------|----------------|  
|Gestione di più gruppi di amministratori del servizio|Ogni dominio ha i propri gruppi di amministratori del servizio che devono essere gestiti in modo indipendente. L'appartenenza di questi gruppi di amministratori del servizio deve essere controllata attentamente.|  
|Gestione della coerenza tra le impostazioni di Criteri di gruppo comuni a più domini|Criteri di gruppo le impostazioni che devono essere applicate a livello di foresta devono essere applicate separatamente a ogni singolo dominio della foresta.|  
|Gestione della coerenza tra le impostazioni di controllo degli accessi e di controllo comuni a più domini|Le impostazioni di controllo degli accessi e di controllo che devono essere applicate nella foresta devono essere applicate separatamente a ogni singolo dominio della foresta.|  
|Aumento della probabilità che gli oggetti si muovano tra domini|Maggiore è il numero di domini, maggiore sarà la probabilità che gli utenti debbano spostarsi da un dominio all'altro. Questo spostamento può influire potenzialmente sugli utenti finali.|  

> [!NOTE]  
> I criteri granulari per le password e il blocco degli account di Windows Server possono anche avere un effetto sul modello di progettazione del dominio selezionato. Prima di questa versione di Windows Server 2008, era possibile applicare a tutti gli utenti del dominio solo un criterio di blocco di password e account, specificato nel criterio dominio predefinito dominio. Di conseguenza, se si desiderano impostazioni diverse per le password e il blocco degli account per set di utenti diversi, è necessario creare un filtro password o distribuire più domini. È ora possibile usare i criteri granulari per le password per specificare più criteri per le password e applicare restrizioni diverse per le password e criteri di blocco degli account a diversi insiemi di utenti all'interno di un singolo dominio. Per ulteriori informazioni sui criteri specifici per le password e il blocco degli account, vedere l'articolo [Guida dettagliata alla configurazione dei criteri specifici per le password e il blocco degli account](https://go.microsoft.com/fwlink/?LinkID=91477).  

## <a name="single-domain-model"></a>Modello a dominio singolo

Un modello a dominio singolo è il più semplice da amministrare ed è il meno costoso da gestire. È costituito da una foresta che contiene un singolo dominio. Questo dominio è il dominio radice della foresta e contiene tutti gli account utente e di gruppo nella foresta.  

Un modello di foresta a dominio singolo riduce la complessità amministrativa offrendo i vantaggi seguenti:  

- Qualsiasi controller di dominio può autenticare qualsiasi utente nella foresta.  

- Tutti i controller di dominio possono essere cataloghi globali, pertanto non è necessario pianificare il posizionamento del server di catalogo globale.  
  
In una foresta a dominio singolo, tutti i dati della directory vengono replicati in tutte le posizioni geografiche che ospitano i controller di dominio. Sebbene questo modello sia il più semplice da gestire, crea anche il maggior traffico di replica dei due modelli di dominio. Il partizionamento della directory in più domini limita la replica di oggetti in aree geografiche specifiche, ma comporta un sovraccarico amministrativo.  
  
## <a name="regional-domain-model"></a>Modello di dominio regionale

Tutti i dati dell'oggetto all'interno di un dominio vengono replicati in tutti i controller di dominio del dominio. Per questo motivo, se la foresta include un numero elevato di utenti distribuiti in diverse posizioni geografiche connesse da un Wide Area Network (WAN), potrebbe essere necessario distribuire i domini regionali per ridurre il traffico di replica sui collegamenti WAN. I domini regionali basati geograficamente possono essere organizzati in base alla connettività WAN di rete.  
  
Il modello di dominio regionale consente di mantenere un ambiente stabile nel tempo. Basare le aree usate per definire i domini nel modello su elementi stabili, ad esempio i limiti continentali. I domini basati su altri fattori, ad esempio i gruppi all'interno dell'organizzazione, possono essere modificati di frequente e potrebbe essere necessario ristrutturare l'ambiente.  
  
Il modello di dominio regionale è costituito da un dominio radice della foresta e da uno o più domini regionali. La creazione di una progettazione di un modello di dominio regionale implica l'identificazione del dominio radice della foresta e la determinazione del numero di domini aggiuntivi necessari per soddisfare i requisiti di replica. Se l'organizzazione include gruppi che richiedono isolamento dei dati o isolamento dei servizi da altri gruppi nell'organizzazione, creare una foresta separata per questi gruppi. I domini non forniscono isolamento dei dati o isolamento dei servizi.  
