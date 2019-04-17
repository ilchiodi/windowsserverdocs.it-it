---
ms.assetid: 173b72c1-ac83-4f42-abab-cf58f43769f0
title: Determinazione del numero di foreste necessario
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: a461dbb2b5bf9d2ca1bb6a336cb11ace775fb1cd
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="determining-the-number-of-forests-required"></a>Determinazione del numero di foreste necessario

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Per determinare il numero di insiemi di strutture che è necessario distribuire, è necessario identificare e valutare i requisiti di isolamento e autonomia per ogni gruppo all'interno dell'organizzazione e il mapping di tali requisiti per i modelli di progettazione di foresta appropriato con attenzione.  
  
Quando si determina il numero di foreste per la distribuzione dell'organizzazione, considerare quanto segue:  
  
-   Requisiti di isolamento limitano le scelte di progettazione. Pertanto, se identificare i requisiti di isolamento, assicurarsi che i gruppi effettivamente richiedono l'isolamento di dati e che autonomia dei dati non sono sufficiente per le proprie esigenze. Assicurarsi che i gruppi diversi all'interno dell'organizzazione chiaramente conoscano i concetti di autonomia e isolamento.  
  
-   Negoziazione di progettazione può essere un processo lungo. Può essere difficile per i gruppi per raggiungere un accordo sulla proprietà e viene utilizzato per le risorse disponibili. Assicurarsi che consentire tempo sufficiente per i gruppi nella tua organizzazione per eseguire ricerche adeguate per identificare le loro esigenze. Impostare scadenze piana e stabile per scelte di progettazione e ottenere il consenso di tutte le parti sulle scadenze stabilite.  
  
-   Determinazione del numero di foreste per la distribuzione consente di bilanciare i costi con vantaggi. Un modello di foresta singola è l'opzione più conveniente e richiede il minimo carico amministrativo. Anche se un gruppo nell'organizzazione preferibile operazioni del servizio autonomo, potrebbe essere più conveniente per l'organizzazione sottoscrivere fornitura di servizi da un gruppo di reparti IT informazioni centralizzata e attendibile. In questo modo il gruppo di gestione dei propri dati senza la creazione di costi e della gestione dei servizi. Bilanciamento del carico costi vantaggi potrebbe richiedere l'input dallo sponsor executive.  
  
    Una singola foresta è la configurazione più semplice da gestire. Consente di collaborazione massima all'interno dell'ambiente perché:  
  
    -   Tutti gli oggetti in una singola foresta sono elencati nel catalogo globale. Pertanto, è richiesta alcuna sincronizzazione tra foreste.  
  
    -   Non è necessaria la gestione di un'infrastruttura duplicata.  
  
-   Non è consigliabile comproprietà di una singola foresta da due organizzazioni IT separate e autonomi. In futuro, gli obiettivi dei due gruppi IT potrebbero cambiare, in modo che non accettino non è più controllo condiviso.  
  
-   Si consiglia di non amministrazione dei servizi a più di un partner di fuori outsourcing. Le organizzazioni internazionali che dispongono di gruppi in diversi paesi o aree geografiche potrebbero scegliere di delegare l'amministrazione dei servizi a un altro partner esterno per ogni paese o area geografica. Poiché più partner esterni non può essere isolate una da altra, le azioni di un partner possono influenzare il servizio di altra parte, che rende difficile contenere i partner responsabili per i contratti di servizio.  
  
-   Solo un'istanza di un dominio Active Directory devono essere presenti in qualsiasi momento. Microsoft non supporta la clonazione, divisione di copia dei controller di dominio o da un dominio in un tentativo di stabilire una seconda istanza dello stesso dominio. Per ulteriori informazioni su questa limitazione, vedere la sezione seguente.  
  
## <a name="restructuring-limitations"></a>Ristrutturazione dei limiti  
Quando una società acquisisce un'altra azienda, business unit o linea di prodotti, la società acquirente potrebbe inoltre desidera acquisire risorse IT corrispondente al venditore. In particolare, l'acquirente possibile acquisire alcuni o tutti i controller di dominio che ospitano gli account utente, account computer e gruppi di sicurezza che corrispondono alle risorse aziendali che devono essere acquisiti. I metodi supportati soli per l'acquirente acquisire le risorse IT che vengono archiviate nella foresta di Active Directory del venditore sono i seguenti:  
  
1.  Acquisire una sola istanza dell'insieme di strutture, inclusi tutti i controller di dominio e i dati di directory nell'intera foresta del venditore.  
  
2.  Eseguire la migrazione di dati di directory necessarie dall'insieme di strutture o domini del venditore a uno o più domini dell'acquirente. Un completamente nuovo insieme di strutture o uno o più domini esistenti che sono già stati distribuiti nella foresta dell'acquirente, potrebbe essere la destinazione di questo tipo di migrazione.  
  
Questa limitazione del supporto è disponibile perché:  
  
-   Ogni dominio in una foresta di Active Directory viene assegnata un'identità univoca durante la creazione della foresta. Copia i controller di dominio da un dominio originale per compromettere un dominio clonato la sicurezza di entrambi i domini e della foresta. Minacce per il dominio originale e di dominio clonato includono quanto segue:  
  
    -   Condivisione delle password che può essere utilizzata per accedere alle risorse  
  
    -   Informazioni su account utente con privilegi e gruppi  
  
    -   Mapping di indirizzi IP ai nomi di computer  
  
    -   Le aggiunte, eliminazioni e modifiche delle informazioni di directory se i controller di dominio in un dominio clonato mai di stabilire la connettività di rete con i controller di dominio dal dominio originale  
  
-   Domini clonati condividono un'identità di sicurezza comuni; Pertanto, non possono stabilire relazioni di trust tra loro, anche se uno o entrambi i domini sono stati rinominati.  
  
## <a name="in-this-section"></a>In questa sezione  
  
-   [Modelli di progettazione di foresta](https://technet.microsoft.com/library/cc770439.aspx)  
  
-   [Mapping dei requisiti di progettazione ai modelli di progettazione di foresta](Forest-Design-Models.md)  
  
-   [Utilizzando il modello di foresta di domini organizzativi](../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md)  
  


