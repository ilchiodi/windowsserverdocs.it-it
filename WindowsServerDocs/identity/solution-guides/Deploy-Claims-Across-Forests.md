---
ms.assetid: ceb9ce18-5a94-4166-9edd-2685b81fc15f
title: Distribuire attestazioni nelle foreste
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 99af1022870c891c75bb2008f57e8d8e171961ff
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861234"
---
# <a name="deploy-claims-across-forests"></a>Distribuire attestazioni nelle foreste

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In Windows Server 2012, un tipo di attestazione è un'asserzione sull'oggetto a cui è associato. In Active Directory i tipi di attestazione sono definiti per foresta. Ci sono molti scenari in cui un'entità di sicurezza può avere la necessità di attraversare un limite di trust per accedere alle risorse in una foresta trusted. La trasformazione delle attestazioni tra foreste in Windows Server 2012 consente di trasformare le attestazioni in uscita e in ingresso che attraversano le foreste, in modo che le attestazioni vengano riconosciute e accettate nelle foreste trusting e trusted. Ecco alcuni scenari reali per la trasformazione delle attestazioni:  
  
-   Le foreste trusting possono usare la trasformazione delle attestazioni come protezione contro l'elevazione dei privilegi filtrando le attestazioni in ingresso con valori specifici.  
  
    Le foreste trusting possono anche rilasciare attestazioni per le entità di arrivo attraverso un limite di trust, se la foresta trusted non supporta o non rilascia attestazioni.  
  
-   Le foreste trusted possono usare la trasformazione delle attestazioni per impedire a determinati tipi di attestazione e attestazioni con determinati valori di uscire dalla foresta trusting.  
  
-   È anche possibile usare la trasformazione delle attestazioni per mappare diversi tipi di attestazioni tra foreste trusted e trusting. Le trasformazioni possono essere usate per generalizzare il tipo o il valore dell'attestazione o entrambi. In caso contrario, è necessario standardizzare i dati tra le foreste prima di poter usare le attestazioni. La generalizzazione delle attestazioni tra foreste trusted e trusting riduce i costi dell'IT.  
  
## <a name="claim-transformation-rules"></a>Regole di trasformazione delle attestazioni  
La sintassi del linguaggio delle regole di trasformazione suddivide una singola regola in due parti principali: una serie di istruzioni condizionali e l'istruzione issue. Ogni istruzione condizionale ha due sottocomponenti: l'identificatore dell'attestazione e la condizione. L'istruzione issue include parole chiave, delimitatori e un'espressione issue. L'istruzione condizionale inizia facoltativamente con una variabile di identificatore di attestazione, che rappresenta l'attestazione di input corrispondente. La condizione controlla l'espressione. Se l'attestazione di input non corrisponde alla condizione, il motore di trasformazione ignora l'istruzione issue e valuta l'attestazione di input successiva rispetto alla regola di trasformazione. Se tutte le condizioni corrispondono all'attestazione di input, viene elaborata l'istruzione issue.  
  
Per informazioni dettagliate sul linguaggio delle regole di attestazione, vedere [Claims Transformation Rules Language](Claims-Transformation-Rules-Language.md).  
  
## <a name="linking-claim-transformation-policies-to-forests"></a>Collegamento dei criteri di trasformazione delle attestazioni alle foreste  
Esistono due componenti coinvolti nella configurazione dei criteri di trasformazione delle attestazioni: gli oggetti criteri di trasformazione delle attestazioni e il collegamento di trasformazione. Gli oggetti criteri risiedono nel contesto dei nomi di configurazione di una foresta e contengono informazioni di mapping per le attestazioni. Il collegamento specifica a quali foreste trusting e trusted viene applicato il mapping.  
  
È importante capire se la foresta è trusting o trusted, perché è la base per il collegamento degli oggetti criteri di trasformazione. Ad esempio, la foresta trusted è la foresta che contiene gli account utente che richiedono l'accesso. La foresta trusting è la foresta che contiene le risorse per cui si vuole concedere l'accesso agli utenti. Le attestazioni viaggiano nella stessa direzione dell'entità di sicurezza che richiede l'accesso. Ad esempio, nel caso di un trust unidirezionale dalla foresta contoso.com alla foresta adatum.com, le attestazioni passeranno da adatum.com a contoso.com, consentendo agli utenti di adatum.com di accedere alle risorse in contoso.com.  
  
Per impostazione predefinita, una foresta trusted consente il passaggio a tutte le attestazioni in uscita e una foresta trusting ignora tutte le attestazioni in ingresso ricevute.  
  
## <a name="in-this-scenario"></a>In questo scenario  
Per questo scenario è disponibile il materiale sussidiario seguente:  
  
-   [Distribuisci attestazioni &#40;tra foreste procedura dimostrativa&#41;](Deploy-Claims-Across-Forests--Demonstration-Steps-.md)  
  
-   [Linguaggio delle regole di trasformazione delle attestazioni](Claims-Transformation-Rules-Language.md)  
  
## <a name="roles-and-features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>Ruoli e funzionalità inclusi in questo scenario  
Nella tabella seguente sono elencati i ruoli e le funzionalità che interessano questo scenario e sono descritte le modalità di supporto.  
  
|Ruolo/funzionalità|Modalità di supporto dello scenario|  
|-----------------|---------------------------------|  
|Servizi di dominio Active Directory|In questo scenario è necessario impostare due foreste di Active Directory con un trust bidirezionale. Sono disponibili attestazioni per entrambe le foreste. Si possono anche impostare criteri di accesso centrale nella foresta trusting in cui risiedono le risorse.|  
|Ruolo Servizi file e archiviazione|In questo scenario la classificazione dei dati viene applicata alle risorse nei file server. I criteri di accesso centrale vengono applicati alla cartella a cui si vuole concedere l'accesso utente. Dopo la trasformazione, l'attestazione concede l'accesso utente alle risorse in base ai criteri di accesso centrale applicati alla cartella nel file server.|  
  


