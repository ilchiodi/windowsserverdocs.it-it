---
ms.assetid: 173b72c1-ac83-4f42-abab-cf58f43769f0
title: Determinazione del numero di foreste necessarie
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1721190bf592b6f7a1274d60d47bbc755eeff1c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820652"
---
# <a name="determining-the-number-of-forests-required"></a>Determinazione del numero di foreste necessarie

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Per determinare il numero di insiemi di strutture che è necessario distribuire, è necessario identificare e valutare i requisiti di isolamento e autonomia per ogni gruppo di nell'organizzazione ed eseguire il mapping di tali requisiti per i modelli di progettazione foreste appropriate con attenzione.  
  
Quando si determina il numero di foreste da distribuire nell'organizzazione, considerare quanto segue:  
  
-   I requisiti di isolamento limitano le scelte di progettazione. Di conseguenza, dopo aver identificato i requisiti di isolamento, assicurarsi che i gruppi richiedono effettivamente l'isolamento dei dati e che autonomia dei dati non è sufficiente per le proprie esigenze. Assicurarsi che i vari gruppi all'interno dell'organizzazione comprendano chiaramente i concetti di isolamento e autonomia.  
  
-   La negoziazione della progettazione può essere un processo lungo. Può essere difficile per i gruppi per raggiungere un accordo sulle proprietà e viene utilizzato per le risorse disponibili. Assicurarsi di consentire tempo sufficiente per i gruppi nell'organizzazione per condurre ricerche di adeguati per identificare le proprie esigenze. Impostare scadenze ferma per prendere decisioni di progettazione e ottenere il consenso a tutte le parti su ai termini stabiliti.  
  
-   Determinazione del numero di insiemi di strutture per la distribuzione consente di bilanciare i costi con i vantaggi. Un modello di foresta singola è l'opzione più economica e richiede il minimo carico amministrativo. Anche se un gruppo di nell'organizzazione potrebbe preferire le operazioni di servizio autonomo, potrebbe essere più conveniente per l'organizzazione per la sottoscrizione per una distribuzione di servizio da un gruppo di reparti IT centralizzata e attendibili le informazioni. In questo modo il gruppo di gestione dei propri dati senza creare costi e della gestione dei servizi. I costi con i vantaggi di bilanciamento del carico può richiedere indicazioni dallo sponsor direttivo.  
  
    Una singola foresta è la configurazione più semplice da gestire. Poiché consente massima collaborazione all'interno dell'ambiente:  
  
    -   Tutti gli oggetti in una singola foresta sono elencati nel catalogo globale. Pertanto, è richiesta alcuna sincronizzazione tra foreste.  
  
    -   Gestione di un'infrastruttura duplicata non è obbligatorio.  
  
-   Non è consigliabile comproprietà di una singola foresta da due organizzazioni IT separate e autonome. In futuro, potrebbero cambiare gli obiettivi dei due gruppi IT, in modo che non accettino non è più controllo condiviso.  
  
-   Non è consigliabile amministrazione dei servizi di outsourcing a più all'esterno di partner. Le organizzazioni multinazionali con i gruppi in aree o paesi diversi potrebbero scegliere per l'outsourcing dell'amministrazione del servizio a un partner esterno diverso per ogni paese o area geografica. Poiché più i partner esterni non possono essere isolati una da altra, le azioni di un partner possono influenzare il servizio di altra parte, cosa che rende difficile contenere i partner responsabili per i contratti di servizio.  
  
-   Solo un'istanza di un dominio Active Directory deve esistere in qualsiasi momento. Microsoft non supporta la clonazione, divisione o copia i controller di dominio da un dominio nel tentativo di stabilire una seconda istanza dello stesso dominio. Per altre informazioni su questa limitazione, vedere la sezione seguente.  
  
## <a name="restructuring-limitations"></a>Ristrutturazione dei limiti  
Quando una società acquisisce un'altra società, business unit, ovvero o linea di prodotti, l'acquisto aziendale potrebbe essere necessario anche acquisire risorse IT corrispondente dal venditore. In particolare, l'aggregazione buyer possibile acquisire alcuni o tutti i controller di dominio che ospitano gli account utente, gli account computer e i gruppi di sicurezza che corrispondono alle risorse aziendali che devono essere acquisiti. Gli unici metodi supportati per l'acquirente acquisire gli asset IT che sono archiviati nella foresta di Active Directory del venditore sono i seguenti:  
  
1.  Acquisire l'unica istanza dell'insieme di strutture, inclusi tutti i controller di dominio e i dati della directory nell'intera foresta del venditore.  
  
2.  Eseguire la migrazione di dati di directory necessari provenienti da foreste o domini del venditore a uno o più domini dell'acquirente. La destinazione per questo tipo di migrazione potrebbe essere una completamente nuova foresta o uno o più domini esistenti già distribuite nella foresta dell'acquirente.  
  
Perché esiste una limitazione supporto:  
  
-   Ogni dominio in una foresta di Active Directory viene assegnata un'identità univoca durante la creazione della foresta. Copia i controller di dominio da un dominio originale a una compromissione di dominio clonato la sicurezza sia i domini della foresta. Le minacce per il dominio originale e quello clonato includono quanto segue:  
  
    -   Condivisione di password che può essere utilizzata per accedere alle risorse  
  
    -   Informazioni dettagliate relative a gruppi e account utente con privilegi  
  
    -   Mapping di indirizzi IP per i nomi dei computer  
  
    -   Aggiunte, eliminazioni e modifiche di informazioni sulla directory se i controller di dominio in un dominio clonato mai stabilire la connettività di rete con i controller di dominio dal dominio originale  
  
-   Domini clonati condividono un'identità di sicurezza comuni. Pertanto, non è possibile stabilire relazioni di trust tra di essi, anche se uno o entrambi i domini sono stati rinominati.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Modelli di progettazione di foresta](https://technet.microsoft.com/library/cc770439.aspx)  
  
-   [Requisiti di progettazione di mapping ai modelli di progettazione di foresta](Forest-Design-Models.md)  
  
-   [Usando il modello di foresta di domini organizzativi](../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md)  
  


