---
ms.assetid: ceb9ce18-5a94-4166-9edd-2685b81fc15f
title: Distribuire attestazioni nelle foreste
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7d78258d8f1db9889b6d2db8c497780940ed35a1
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="deploy-claims-across-forests"></a>Distribuire attestazioni nelle foreste

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In Windows Server 2012, un tipo di attestazione è un'asserzione relativa all'oggetto con cui è associata. Tipi di attestazione sono definiti per foresta in Active Directory. Esistono molti scenari in cui un'entità di sicurezza potrebbe essere necessario attraversare un limite di trust per accedere alle risorse in una foresta trusted. Trasformazione di attestazioni tra foreste in Windows Server 2012 consente di trasformare le attestazioni di ingresso e in uscita che attraversano le foreste in modo che le attestazioni vengono riconosciute e accettazione nelle foreste trusted e trusting. Alcuni degli scenari reali per la trasformazione delle attestazioni sono:  
  
-   Foreste trusting possono usare trasformazione delle attestazioni come protezione contro l'elevazione dei privilegi, semplicemente filtrando le attestazioni in ingresso con valori specifici.  
  
    Le foreste trusting possono anche rilasciare attestazioni per l'entità di arrivo attraverso un limite di trust, se la foresta trusted non supporta o rilasciare le attestazioni.  
  
-   Foreste trusted possono usare trasformazione delle attestazioni per impedire a determinati tipi di attestazione e attestazioni con determinati valori di uscire alla foresta trusting.  
  
-   È inoltre possibile utilizzare l'attestazione di trasformazione per mappare diversi tipi di attestazioni tra foreste trusted e trusting. Può essere utilizzato per generalizzare il tipo di attestazione, il valore dell'attestazione o entrambi. In caso contrario, è necessario standardizzare i dati tra le foreste prima di poter utilizzare le attestazioni. La generalizzazione delle attestazioni tra foreste trusted e trusting riduce i costi IT.  
  
## <a name="claim-transformation-rules"></a>Regole di trasformazione delle attestazioni  
Sintassi del linguaggio di regola di trasformazione suddivide una singola regola in due parti principali: una serie di istruzioni condizionali e l'istruzione issue. Ogni istruzione condizionale ha due sottocomponenti: l'identificatore dell'attestazione e la condizione. L'istruzione issue include parole chiave, delimitatori e un'espressione issue. L'istruzione condizionale inizia facoltativamente con una variabile di identificatore di attestazione, che rappresenta l'attestazione di input corrispondente. La condizione controlla l'espressione. Se l'attestazione di input non corrisponde alla condizione, il motore di trasformazione ignora l'istruzione issue e valuta l'attestazione di input successiva rispetto alla regola di trasformazione. Se tutte le condizioni corrispondono di attestazioni di input, elaborata l'istruzione issue.  
  
Per informazioni dettagliate sul linguaggio delle regole attestazione, vedere [Claims Transformation Rules Language](Claims-Transformation-Rules-Language.md).  
  
## <a name="linking-claim-transformation-policies-to-forests"></a>Il collegamento di criteri di trasformazione delle attestazioni alle foreste  
Esistono due componenti coinvolti nell'impostazione di criteri di trasformazione delle attestazioni: gli oggetti Criteri di trasformazione e il collegamento di trasformazione di attestazioni. Gli oggetti Criteri risiedono nel contesto dei nomi di configurazione in una foresta e contengono informazioni di mapping per le attestazioni. Il collegamento specifica quali trusting e foreste trusted applica il mapping.  
  
È importante comprendere se la foresta è la foresta trusting o trusted, perché è la base per collegare gli oggetti Criteri di trasformazione. Ad esempio, la foresta trusted è la foresta che contiene gli account utente che richiedono l'accesso. La foresta trusting è la foresta che contiene le risorse che si desidera concedere agli utenti di accedere a. Le attestazioni viaggiano nella stessa direzione dell'entità che richiede l'accesso di sicurezza. Ad esempio, se è presente un trust unidirezionale dalla foresta contoso.com all'insieme di strutture adatum.com, le attestazioni passeranno da adatum.com a contoso.com, consentendo agli utenti di adatum.com di accedere alle risorse contoso.com.  
  
Per impostazione predefinita, una foresta trusted consente a tutte le attestazioni in uscita passare e una foresta trusting Ignora tutte le attestazioni in ingresso che riceve.  
  
## <a name="in-this-scenario"></a>In questo scenario  
Sono disponibile per questo scenario le indicazioni seguenti:  
  
-   [Distribuire attestazioni nelle foreste & #40; passaggi & #41;](Deploy-Claims-Across-Forests--Demonstration-Steps-.md)  
  
-   [Linguaggio delle regole di trasformazione delle attestazioni](Claims-Transformation-Rules-Language.md)  
  
## <a name="BKMK_NEW"></a>Ruoli e funzionalità incluse in questo scenario  
Nella tabella seguente elenca i ruoli e funzionalità che fanno parte di questo scenario e descrive la modalità di supporto.  
  
|Ruolo o funzionalità|Modalità di supporto in questo scenario|  
|-----------------|---------------------------------|  
|Servizi di dominio Active Directory|In questo scenario, è necessario impostare due foreste di Active Directory con un trust bidirezionale. Sono disponibili attestazioni per entrambe le foreste. È inoltre possibile impostare criteri di accesso centrale nella foresta trusting in cui risiedono le risorse.|  
|Ruolo Servizi file e archiviazione|In questo scenario, viene applicata la classificazione dei dati per le risorse nei file server. Viene applicato il criterio di accesso centrale alla cartella in cui si desidera concedere l'accesso utente. Dopo la trasformazione, l'attestazione concede l'accesso utente alle risorse in base ai criteri di accesso centrale applicati alla cartella nel file server.|  
  


