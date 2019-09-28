---
ms.assetid: 173b72c1-ac83-4f42-abab-cf58f43769f0
title: Determinazione del numero di foreste necessarie
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 21bece55ef64a552ddc641befd94d3ce19e78db6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408882"
---
# <a name="determining-the-number-of-forests-required"></a>Determinazione del numero di foreste necessarie

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Per determinare il numero di foreste che è necessario distribuire, è necessario identificare e valutare con attenzione i requisiti di isolamento e autonomia per ogni gruppo dell'organizzazione ed eseguire il mapping di tali requisiti ai modelli di progettazione di foresta appropriati.  
  
Quando si determina il numero di foreste da distribuire per l'organizzazione, tenere presente quanto segue:  
  
-   I requisiti di isolamento limitano le scelte di progettazione. Pertanto, se si identificano i requisiti di isolamento, assicurarsi che i gruppi richiedano effettivamente l'isolamento dei dati e che l'autonomia dei dati non sia sufficiente per le proprie esigenze. Assicurarsi che i vari gruppi dell'organizzazione conoscano chiaramente i concetti di isolamento e autonomia.  
  
-   La negoziazione della progettazione può essere un processo lungo. Può essere difficile per i gruppi passare a un accordo sulla proprietà e USA per le risorse disponibili. Assicurarsi di disporre di tempo sufficiente per consentire ai gruppi dell'organizzazione di eseguire ricerche adeguate per identificare le proprie esigenze. Impostare scadenze aziendali per le decisioni di progettazione e ottenere consenso da tutte le parti alle scadenze stabilite.  
  
-   Determinare il numero di foreste da distribuire comporta un bilanciamento dei costi rispetto ai vantaggi. Un modello a foresta singola è l'opzione più conveniente e richiede la minima quantità di overhead amministrativo. Sebbene un gruppo dell'organizzazione preferisca operazioni di servizio autonome, potrebbe essere più conveniente per l'organizzazione sottoscrivere la distribuzione dei servizi da un gruppo IT centralizzato e attendibile. Ciò consente al gruppo di gestire la gestione dei dati senza creare i costi aggiuntivi per la gestione dei servizi. Il bilanciamento dei costi rispetto ai vantaggi potrebbe richiedere l'input dello sponsor Executive.  
  
    Una singola foresta è la configurazione più semplice da gestire. Consente la massima collaborazione all'interno dell'ambiente, perché:  
  
    -   Tutti gli oggetti in una singola foresta sono elencati nel catalogo globale. Pertanto, non è necessaria alcuna sincronizzazione tra foreste.  
  
    -   La gestione di un'infrastruttura duplicata non è obbligatoria.  
  
-   Non è consigliabile usare la co-proprietà di una singola foresta per due organizzazioni IT separate e autonome. In futuro, gli obiettivi dei due gruppi IT potrebbero cambiare, in modo che non possano più accettare il controllo condiviso.  
  
-   Non è consigliabile esternalizzare l'amministrazione del servizio a più di un partner esterno. Le organizzazioni multinazionali con gruppi in paesi o aree geografiche diverse possono scegliere di esternalizzare l'amministrazione del servizio a un partner esterno diverso per ogni paese o area geografica. Poiché più partner esterni non possono essere isolati gli uni dagli altri, le azioni di un partner possono influire sul servizio dell'altro, rendendo difficile la contabilità dei partner per i contratti di servizio.  
  
-   Deve esistere una sola istanza di un dominio di Active Directory in qualsiasi momento. Microsoft non supporta la clonazione, la suddivisione o la copia di controller di dominio da un dominio nel tentativo di stabilire una seconda istanza dello stesso dominio. Per ulteriori informazioni su questa limitazione, vedere la sezione seguente.  
  
## <a name="restructuring-limitations"></a>Limitazioni di ristrutturazione  
Quando un'azienda acquisisce un'altra società, una business unit o una linea di prodotti, è possibile che l'azienda acquisti desideri acquisire anche le risorse IT corrispondenti dal venditore. In particolare, l'acquirente potrebbe voler acquisire alcuni o tutti i controller di dominio che ospitano gli account utente, gli account computer e i gruppi di sicurezza che corrispondono agli asset aziendali da acquisire. Gli unici metodi supportati per l'acquirente per acquisire gli asset IT archiviati nella foresta Active Directory del venditore sono i seguenti:  
  
1.  Acquisire l'unica istanza della foresta, inclusi tutti i controller di dominio e i dati di directory nell'intera foresta del venditore.  
  
2.  Eseguire la migrazione dei dati di directory necessari dalla foresta o dai domini del venditore a uno o più domini dell'acquirente. La destinazione per una migrazione di questo tipo potrebbe essere una foresta completamente nuova o uno o più domini esistenti già distribuiti nella foresta dell'acquirente.  
  
Questa limitazione del supporto esiste perché:  
  
-   A ogni dominio in una foresta Active Directory viene assegnata un'identità univoca durante la creazione della foresta. La copia di controller di dominio da un dominio originale a un dominio clonato compromette la sicurezza dei domini e della foresta. Di seguito sono riportate le minacce per il dominio originale e per il dominio clonato:  
  
    -   Condivisione delle password che possono essere usate per ottenere l'accesso alle risorse  
  
    -   Informazioni dettagliate sugli account utente e i gruppi con privilegi  
  
    -   Mapping degli indirizzi IP ai nomi dei computer  
  
    -   Aggiunte, eliminazioni e modifiche delle informazioni di directory se i controller di dominio in un dominio clonato stabiliscono la connettività di rete con i controller di dominio del dominio originale  
  
-   I domini clonati condividono un'identità di sicurezza comune; non è pertanto possibile stabilire relazioni di trust tra di essi, anche se uno o entrambi i domini sono stati rinominati.  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Modelli di progettazione della foresta](https://technet.microsoft.com/library/cc770439.aspx)  
  
-   [Mapping dei requisiti di progettazione ai modelli di progettazione della foresta](Forest-Design-Models.md)  
  
-   [Uso del modello di foresta di domini aziendali](../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md)  
  


