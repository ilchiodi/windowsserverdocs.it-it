---
ms.assetid: 606df285-259c-4c6b-8583-9aca1d614c43
title: Quando usare una regola di pass-through o di filtro delle attestazioni
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 803d939aeaa640e2a8411da156f74f505a2f89d4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444046"
---
# <a name="when-to-use-a-pass-through-or-filter-claim-rule"></a>Quando usare una regola di pass-through o di filtro delle attestazioni
È possibile usare questa regola in Active Directory Federation Services \(ADFS\) quando è necessario richiedere un tipo di attestazione in ingresso specifico e quindi applicare un'azione che determina il tipo di output deve essere eseguita in base ai valori nell'attestazione in ingresso. Quando si usa questa regola, si consente l'ingresso o si filtrano le eventuali attestazioni corrispondenti alla logica della regola nella tabella seguente, in base all'opzione configurata nella regola.  
  
|Opzione della regola|Logica della regola|  
|---------------|--------------|  
|Pass-through di tutti i valori attestazione|Se il tipo di attestazione in ingresso è uguale a *specificato tipo di attestazione* e valore è uguale a *qualsiasi valore*, quindi passare l'attestazione tramite|  
|Pass-through solo di un valore attestazione specifico|Se il tipo di attestazione in ingresso è uguale a *specificato tipo di attestazione* e valore è uguale a *valore di attestazione specificato*, quindi passare l'attestazione tramite|  
|Pass-through solo i valori di attestazione che corrispondono a uno specifico e\-valore suffisso di posta elettronica|Se il tipo di attestazione in ingresso è uguale al *tipo di attestazione specificato* e il valore è uguale al *valore del suffisso specificato*, consente l'ingresso dell'attestazione|  
|Pass-through solo dei valori attestazione che iniziano con un valore specifico|Se il tipo di attestazione in ingresso è uguale al *tipo di attestazione specificato* e il valore inizia con il *valore attestazione specificato*, consente l'ingresso dell'attestazione|  
  
Le sezioni seguenti forniscono un'introduzione di base alle regole attestazioni e indicano i casi in cui è necessario usare questa regola.  
  
## <a name="about-claim-rules"></a>Informazioni sulle regole attestazioni  
Una regola attestazione rappresenta un'istanza di logica di business che recupera un'attestazione in ingresso, applicare una condizione \(Se x y quindi\) e produrre un'attestazione in uscita in base ai parametri di condizione. L'elenco seguente fornisce suggerimenti importanti sulle regole attestazioni da tenere presenti prima di procedere con la lettura di questo argomento:  
  
-   Nello snap di gestione di ADFS\-attestazione regole possono essere create solo utilizzando modelli di regola attestazione  
  
-   Processo di regole attestazione in ingresso di attestazioni direttamente da un provider di attestazioni \(ad esempio Active Directory o un altro servizio federativo\) o dall'output dell'accettazione di trasformare le regole in un trust di provider di attestazioni.  
  
-   Le regole attestazioni sono elaborate dal motore di rilascio delle attestazioni in ordine cronologico entro un determinato set di regole. Impostando la precedenza sulle regole, è possibile perfezionare o filtrare ulteriormente le attestazioni generate dalle regole precedenti all'interno di un set di regole specificato.  
  
-   Per i modelli di regola attestazioni è sempre necessario specificare un tipo di attestazione in ingresso. Tuttavia, è possibile elaborare più valori di attestazione con lo stesso tipo di attestazione usando una singola regola.  
  
Per ulteriori informazioni sulle regole attestazione e i set di regole attestazione, vedere [ruolo delle attestazioni regole](The-Role-of-Claim-Rules.md). Per ulteriori informazioni sulle modalità di elaborazione delle regole, vedere [il ruolo del motore di attestazioni](The-Role-of-the-Claims-Engine.md). Per ulteriori informazioni, modalità di elaborazione dei set di regole attestazione, vedere [ruolo delle Pipeline delle attestazioni](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="pass-through-all-claim-values"></a>Pass-through di tutti i valori attestazione  
Quando si usa questa regola, viene consentito l'ingresso di tutti i valori attestazione in ingresso per il tipo di attestazione specificato come attestazioni in uscita. Ad esempio quando il tipo di attestazione in ingresso è specificato come tipo di attestazione Ruolo, tutti i valori attestazione in ingresso vengono copiati singolarmente in nuove attestazioni in uscita con il tipo di attestazione in uscita Ruolo.  
  
## <a name="filtering-a-claim"></a>Filtrare un'attestazione  
In ADFS, il termine *attestazioni filtro* consentono di filtrare o limitare in ingresso in modo che solo determinati valori vengono passati o inviati come attestazioni in uscita tramite i valori di attestazione. È il **Pass Through or Filter an Incoming Claim** il modello di regola che rende possibile questa funzione. All'interno delle proprietà di questa regola, è possibile impostare le condizioni per filtrare i valori in ingresso per consentire l'ingresso solo dei valori che soddisfano i criteri specificati.  
  
Ad esempio, è possibile usare questa regola per consentire l'ingresso solo delle attestazioni che corrispondono al valore attestazione di Purchaser quando il tipo di attestazione in ingresso corrisponde al tipo di attestazione Ruolo o quando si vuole rilasciare solo attestazioni relative al nome dell'utente ma non quelle contenenti il codice fiscale dell'utente.  
  
Quando si usa una condizione di filtro con questa regola, vengono esaminate tutte le attestazioni in ingresso per determinare quelle che corrispondono ai criteri impostati dalla regola. Tutte le altre attestazioni vengono ignorate, in modo da consentire l'ingresso solo ai valori attestazione specificati che corrispondono al tipo di attestazione selezionato.  
  
Ad esempio, come illustrato nella figura seguente, quando una regola è impostata con la condizione per filtrare le attestazioni solo in ingresso che corrispondono all'UPN tipo di attestazione e inoltre terminare con @fabrikam.com, tutte le altre attestazioni in ingresso vengono ignorate a meno che non soddisfino questi criteri. L'attestazione in ingresso con il tipo di attestazione E sono inclusi\-indirizzo di posta elettronica anche se il valore dell'attestazione termina con @fabrikam.com. In questo caso, solo l'attestazione contenente il valore di Nick@fabrikam.com viene inviato alla relying party.  
  
![Quando utilizzare passata tramite](media/adfs2_filter.gif)  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configurazione della regola in un trust del provider di attestazioni  
Quando si usa un trust del provider di attestazioni, è possibile configurare questa regola in modo che consenta solo le attestazioni in ingresso dal provider di attestazioni che corrispondono a determinati vincoli. È ad esempio accettare solo e\-posta attestazioni dai provider di attestazioni; pertanto, si utilizza questo modello di regola per accettare e\-posta tipi di attestazione che terminano con Domain Name System del provider di attestazioni \(DNS\) nome.  
  
## <a name="configuring-this-rule-on-a-relying-party-trust"></a>Configurazione di questa regola in un trust della relying party  
Quando si usa un trust della relying party, è possibile configurare questa regola in modo che consenta l'ingresso o filtri le attestazioni in uscita che verranno inviate alla relying party. Alcune relying party potrebbero non riconoscere determinati tipi di attestazioni e determinate attestazioni potrebbero contenere informazioni riservate che non devono essere inviate a determinate relying party. Questo modello di regola semplifica l'imposizione di tali criteri per un trust della relying party specifico.  
  
## <a name="how-to-create-this-rule"></a>Come creare la regola  
Creare questa regola utilizzando il linguaggio di regola attestazione o il Pass-Through o filtro a un modello di regola attestazione in ingresso nello snap di gestione di ADFS\-in. Questo modello di regola offre le opzioni di configurazione seguenti:  
  
-   Specificare un nome della regola attestazione  
  
-   Specificare un tipo di attestazione in ingresso  
  
-   Pass-through di tutti i valori attestazione  
  
-   Pass-through solo di un valore attestazione specifico  
  
-   Pass-through solo i valori di attestazione che corrispondono a uno specifico e\-valore suffisso di posta elettronica  
  
-   Pass-through solo dei valori attestazione che iniziano con un valore specifico  
  
Per ulteriori istruzioni su come creare questo modello, vedere [creare una regola di Pass-Through o filtrare un'attestazione in ingresso](https://technet.microsoft.com/library/dd807060.aspx) nella Guida alla distribuzione di ADFS.  
  
## <a name="using-the-claim-rule-language"></a>Uso del linguaggio delle regole attestazioni  
Se un'attestazione deve essere inviata solo quando il relativo valore corrisponde a un modello personalizzato, è necessario usare una regola personalizzata. Per altre informazioni, vedere Quando usare una regola personalizzata.  
  
### <a name="examples-of-how-to-construct-a-pass-through-or-filter-rule-syntax"></a>Esempi di come costruire la sintassi di una regola pass-through o di filtro  
Una semplice regola di filtro filtrerà le attestazioni in base a una delle proprietà descritte in precedenza. Ad esempio, la regola seguente passerà tutti e\-le attestazioni di posta elettronica:  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”]  => issue(claim  = c);  
```  
  
I filtri possono essere logicamente e\-ed insieme. Ad esempio, la regola seguente accetterà tutti e\-attestazioni con valore johndoe@fabrikam.com:  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”, value == “johndoe@fabrikam.com “]  => issue(claim  = c);  
```  
  
Negli esempi precedenti per i filtri è sempre stato usato un operatore di uguaglianza. Il linguaggio delle regole attestazioni supporta gli operatori seguenti:  
  
-   \=\= \- è uguale a \(caso\-sensibili\)  
  
-   \!\= \- non è uguale a \(caso\-sensibili\)  
  
-   \=~\- corrispondenza di espressione regolare  
  
-   \!~ \- espressione regolare non\-corrispondono  
  
Ad esempio, la regola seguente accetterà tutti e\-le attestazioni non emessi dal server federativo locale con un suffisso di boeing.com di posta elettronica:  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”, value =~ “^.*@boeing\.com$” , issuer != “LOCAL AUTHORITY”]  => issue(claim  = c);  
```  
  
### <a name="best-practices-for-creating-custom-rules"></a>Procedure consigliate per la creazione di regole personalizzate  
È possibile applicare un filtro a una o più proprietà di ogni attestazione, come descritto nella tabella seguente-  
  

| Proprietà attestazione |                                                                                                                                                                                                                                                                                                                                                  Descrizione                                                                                                                                                                                                                                                                                                                                                  |
|----------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      Type      |                                                                                                                                                                                        Il tipo di attestazione \(generalmente rappresentato come un Uri\) riflette un accordo tra partner in una federazione su quale tipo di informazioni viene comunicato nell'attestazione implicito. Ad esempio, le attestazioni di tipo http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identità\/attestazioni\/emailaddress conterrà l'espulsione\-l'indirizzo dell'utente di posta elettronica.                                                                                                                                                                                         |
|     Value      |                                                                                                                                                                                                                                                                   Valore dell'attestazione. Ad esempio, un'attestazione di tipo http:\/\/schemas.xmlsoap.org\/ws\/2005\/05\/identità\/attestazioni\/emailaddress può avere un valore di johndoe@fabrikam.com                                                                                                                                                                                                                                                                    |
|   ValueType    |                                                                                                                                                                                                  ValueType rappresenta il modo in cui le informazioni contenute nel valore dell'attestazione devono essere interpretate. In genere ValueType verrà impostato su http:\/\/www.w3.org\/2001\/XMLSchema\#stringa, ma il valore dell'attestazione potrebbe contenere dati Base64Binary codificato \(, ad esempio, un'immagine \) o una data, Boolean e così via.                                                                                                                                                                                                  |
|     Issuer     | L'emittente rappresenta la parte che ha rilasciato per ultima le attestazioni relative all'utente. Se le attestazioni sono state ricevute da un server federativo del provider di attestazioni, l'autorità emittente di tutte le attestazioni verrà impostata su "LOCAL AUTHORITY". Se le attestazioni sono state ricevute da un server federativo di un provider federativo, l'autorità emittente delle attestazioni verrà impostata sull'identificatore del provider di attestazioni del provider di attestazioni che ha firmato il token. Di conseguenza, durante l'elaborazione delle regole sulle attestazioni ricevute da un provider di attestazioni, l'autorità emittente di tutte le attestazioni verrà impostata sullo stesso valore. Durante la creazione delle regole per una relying party, la proprietà Issuer può essere usata per distinguere le attestazioni originate da diversi provider di attestazioni. |
| OriginalIssuer |                                                                                                   Questa proprietà dell'attestazione indica il server federativo che ha originariamente rilasciato l'attestazione. Poiché la proprietà issuer delle attestazioni è impostata e l'ultimo server federativo che ha firmato il token, l'autorità emittente originale è utile negli scenari in cui un'attestazione è transitata attraverso più di un server federativo \(, ad esempio, una relying party che riceve un token da un provider di federazione server federativo potrà risultare utili quali particolare attestazioni server federativo provider autenticato l'utente\)                                                                                                   |
|   Proprietà   |                                                                                                                             Oltre alle cinque proprietà descritte in precedenza, ogni attestazione ha un contenitore delle proprietà in cui è possibile archiviare proprietà denominate. Queste proprietà no sono serializzate nel token e servono esclusivamente per passare le informazioni tra i componenti della pipeline di rilascio delle attestazioni all'interno dell'ambito di un singolo server federativo. Ad esempio, quando viene impostata una proprietà durante l'elaborazione delle regole del provider di attestazioni e quindi vi si fa riferimento nelle regole della relying party.                                                                                                                              |

