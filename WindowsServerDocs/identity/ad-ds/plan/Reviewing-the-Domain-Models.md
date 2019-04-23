---
ms.assetid: e727a33d-133b-43c9-b6a4-7c00f9cb6000
title: Esaminare i modelli di dominio
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 23c1eb66eeecab8df63cbd7910a9398bc4e3c705
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870922"
---
# <a name="reviewing-the-domain-models"></a>Esaminare i modelli di dominio

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

I fattori seguenti influiscono sul modello di progettazione di dominio selezionati:  
  
- Quantità di capacità disponibile nella rete in cui è disposti ad allocare in Active Directory Domain Services (AD DS). L'obiettivo consiste nel selezionare un modello che fornisce la replica efficiente delle informazioni con un impatto minimo sulla larghezza di banda di rete disponibile.  

- Numero di utenti nell'organizzazione. Se l'organizzazione include un numero elevato di utenti, la distribuzione di più di un dominio consente di partizionare i dati e offre un maggiore controllo sulla quantità di traffico di replica che verrà passati attraverso una connessione di rete specificato. Questo rende possibile per controllare dove i dati vengono replicati e ridurre il carico creato per il traffico di replica su collegamenti lenti nella rete.  

La progettazione di dominio più semplice è un singolo dominio. In una progettazione con singolo dominio, tutte le informazioni vengono replicate a tutti i controller di dominio. Se necessario, tuttavia, è possibile distribuire altri domini regionali. Ciò può verificarsi se alcune parti dell'infrastruttura di rete connessi da collegamenti lenti e vuole che il proprietario della foresta essere certi che il traffico di replica non superi la capacità di cui è stata allocata da Active Directory Domain Services.  

È consigliabile per ridurre al minimo il numero di domini che si distribuisce la foresta. Ciò riduce la complessità della distribuzione globale e, di conseguenza, riduce il costo totale di proprietà. La tabella seguente elenca i costi amministrativi associati con l'aggiunta di domini regionali.  

|Costo|Implicazioni|  
|--------|----------------|  
|Gestione di più gruppi di amministratore del servizio|Ogni dominio ha un proprio gruppo di amministratori del servizio desidera essere gestiti in modo indipendente. L'appartenenza a questi gruppi di amministratore del servizio deve essere controllato attentamente.|  
|Mantenimento della coerenza tra le impostazioni di criteri di gruppo che sono comuni a più domini|Impostazioni di criteri di gruppo che devono essere applicati a livello di foresta devono essere applicate singolarmente a ogni singolo dominio nella foresta.|  
|Mantenere la coerenza tra il controllo di accesso e le impostazioni che sono comuni a più domini di controllo|Controllo degli accessi e le impostazioni che devono essere applicati nell'insieme di strutture di controllo deve essere applicate separatamente per ogni singolo dominio nella foresta.|  
|Una maggiore probabilità di oggetti lo spostamento tra domini|Maggiore è il numero di domini, maggiore è la probabilità che gli utenti dovranno passare da un dominio a altro. Questo passaggio può potenzialmente influire sugli utenti finali.|  

> [!NOTE]  
> Granulari per le password di Windows Server e il blocco degli account possono incidere il modello di progettazione di dominio selezionato. Prima di questa versione di Windows Server 2008, è possibile applicare un solo account e password criterio di blocco, che viene specificato nel dominio criterio dominio predefinito, a tutti gli utenti nel dominio. Di conseguenza, se si vuole ottenere le impostazioni di blocco degli account per diversi set di utenti e password diversi, era necessario creare un filtro delle password o distribuire più domini. È ora possibile usare i criteri granulari per le password per specificare più criteri password e applicare criteri di blocco account e password diverse restrizioni a set diversi di utenti all'interno di un singolo dominio. Per altre informazioni sulla fine-grained password e il blocco degli account, vedere l'articolo [Guida dettagliata alla configurazione dei criteri di blocco degli Account e Password specifici per le](https://go.microsoft.com/fwlink/?LinkID=91477).  

## <a name="single-domain-model"></a>Modello di dominio singolo

Un modello di dominio singolo è più semplice da amministrare e meno costoso da gestire. È costituito da una foresta che contiene un singolo dominio. Questo dominio è il dominio radice della foresta e contiene tutti gli account utente e gruppo nella foresta.  

Un modello di foresta dominio singolo riduce complessità amministrativa fornendo i vantaggi seguenti:  

- Qualsiasi controller di dominio possa autenticare qualsiasi utente nella foresta.  

- Tutti i controller di dominio possono essere cataloghi globali, in modo che non è necessario pianificare il posizionamento di server di catalogo globale.  
  
In una foresta a dominio singolo, tutti i dati di directory vengono replicati in tutte le aree geografiche che ospitano controller di dominio. Anche se questo modello è la più facile da gestire, crea anche il traffico di replica la maggior parte dei modelli di due dominio. Partizione della directory in più domini limita la replica degli oggetti per determinate aree geografiche, ma i risultati di ulteriori il carico amministrativo.  
  
## <a name="regional-domain-model"></a>Modello di dominio regionale

Tutti i dati dell'oggetto all'interno di un dominio viene replicato in tutti i controller di dominio nel dominio. Per questo motivo, se la foresta include un numero elevato di utenti distribuiti in località geografiche diverse connesse tramite una rete WAN (WAN), potrebbe essere necessario per la distribuzione di domini regionali per ridurre il traffico di replica sui collegamenti WAN. Domini regionali geografico possono essere organizzati in base alla connettività di rete WAN.  
  
Il modello di dominio a livello di area consente di gestire un ambiente stabile nel tempo. Le aree consente di definire domini nel modello per gli elementi stabili, ad esempio i limiti intracontinentale di base. I domini in base ad altri fattori, ad esempio i gruppi all'interno dell'organizzazione, possono cambiare di frequente e potrebbe essere necessario ristrutturare l'ambiente.  
  
Il modello di dominio internazionali è costituito da un dominio radice della foresta e uno o più domini regionali. Creazione di una progettazione di modelli di dominio regionale implica che identifica il dominio è il dominio radice della foresta e determinazione del numero di domini aggiuntivi che sono necessari per soddisfare i requisiti di replica. Se l'organizzazione include i gruppi che richiedono l'isolamento dei dati o l'isolamento dei servizi da altri gruppi dell'organizzazione, creare una foresta separata per questi gruppi. Domini non offrono dati isolamento o isolamento dei servizi.  
