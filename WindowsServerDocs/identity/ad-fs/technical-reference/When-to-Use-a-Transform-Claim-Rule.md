---
ms.assetid: 77aa61bf-9c04-4889-a5d2-6f45bc1b8bd2
title: Quando usare una regola attestazioni di trasformazione
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b7cdf68783db1b6b775209e4e42dc6b6ccf0e1b8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385424"
---
# <a name="when-to-use-a-transform-claim-rule"></a>Quando usare una regola attestazioni di trasformazione
È possibile usare questa regola in Active Directory Federation Services \(AD FS @ no__t-1 quando è necessario eseguire il mapping di un tipo di attestazione in ingresso a un tipo di attestazione in uscita e quindi applicare un'azione che determini quale output deve essere eseguito in base ai valori originati nel attestazione in ingresso. Quando si usa questa regola, si consente l'ingresso o si trasformano le attestazioni corrispondenti alla logica della regola seguente, in base all'opzione configurata nella regola, come descritto nella tabella seguente.  
  
|Opzione della regola|Logica della regola|  
|---------------|--------------|  
|Passare tutte le attestazioni in ingresso|Se il tipo di attestazione in ingresso è uguale al *tipo di attestazione specificato* e il valore è uguale a *qualsiasi valore*, consente l'ingresso dell'attestazione senza che il tipo di attestazione in uscita sia uguale al *tipo di attestazione specificato*.|  
|Sostituire un valore attestazione in ingresso con un valore attestazione in uscita diverso|Se il tipo di attestazione in ingresso è uguale al *tipo di attestazione specificato* e il valore è uguale al *valore attestazione specificato*, l'attestazione viene trasformata con un nuovo valore attestazione in uscita corrispondente al *valore attestazione specificato* e con il tipo di attestazione in uscita corrispondente al *tipo di attestazione specificato*|  
|La sostituzione in ingresso e\-suffisso attestazioni con una nuova e di posta elettronica\-suffisso di posta elettronica|Se il tipo di attestazione in ingresso è uguale al *tipo di attestazione specificato* e il valore è uguale a *qualsiasi valore suffisso*, l'attestazione viene trasformata con un nuovo valore attestazione in uscita corrispondente al *valore suffisso specificato* e con il tipo di attestazione in uscita corrispondente al *tipo di attestazione specificato*|  
  
Le sezioni seguenti forniscono un'introduzione di base alle regole attestazioni e indicano i casi in cui è necessario usare questa regola.  
  
## <a name="about-claim-rules"></a>Informazioni sulle regole attestazioni  
Una regola attestazione rappresenta un'istanza di logica di business che recupera un'attestazione in ingresso, applicare una condizione \(Se x y quindi\) e produrre un'attestazione in uscita in base ai parametri di condizione. L'elenco seguente fornisce suggerimenti importanti sulle regole attestazioni da tenere presenti prima di procedere con la lettura di questo argomento:  
  
-   Nello snap di gestione di ADFS\-attestazione regole possono essere create solo utilizzando modelli di regola attestazione  
  
-   Processo di regole attestazione in ingresso di attestazioni direttamente da un provider di attestazioni \(ad esempio Active Directory o un altro servizio federativo\) o dall'output dell'accettazione di trasformare le regole in un trust di provider di attestazioni.  
  
-   Le regole attestazioni sono elaborate dal motore di rilascio delle attestazioni in ordine cronologico entro un determinato set di regole. Impostando la precedenza sulle regole, è possibile perfezionare o filtrare ulteriormente le attestazioni generate dalle regole precedenti all'interno di un set di regole specificato.  
  
-   Per i modelli di regola attestazioni è sempre necessario specificare un tipo di attestazione in ingresso. Tuttavia, è possibile elaborare più valori di attestazione con lo stesso tipo di attestazione usando una singola regola.  
  
Per ulteriori informazioni sulle regole attestazione e i set di regole attestazione, vedere [ruolo delle attestazioni regole](The-Role-of-Claim-Rules.md). Per ulteriori informazioni sulle modalità di elaborazione delle regole, vedere [il ruolo del motore di attestazioni](The-Role-of-the-Claims-Engine.md). Per ulteriori informazioni, modalità di elaborazione dei set di regole attestazione, vedere [ruolo delle Pipeline delle attestazioni](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="pass-through-all-claim-values"></a>Pass-through di tutti i valori attestazione  
Quando si usa questa azione, per tutti i valori attestazione in ingresso che corrispondono a un tipo di attestazione in ingresso specificato viene eseguito il mapping a un tipo di attestazione in uscita specificato prima che di essere inviati come attestazioni in uscita in token firmati dal servizio federativo.  
  
Ad esempio, quando una regola è impostata con il **Pass-through di tutti i valori di attestazione** opzione logica e in ingresso attestazione di tipo di gruppo e viene specificato il tipo di attestazione in uscita del ruolo, vengono copiati tutti i valori di attestazione in ingresso che vengono inseriti dall'emittente singolarmente in nuove attestazioni in uscita con il tipo di attestazione del ruolo.  
  
## <a name="transforming-a-claim"></a>Trasformazione di un'attestazione  
In ADFS, il termine *trasformazione di attestazioni* mezzi per sostituire un fax in ingresso attestazione valore con un diverso valore attestazione in uscita. È la regola Trasformazione di un'attestazione in ingresso che rende possibile questa funzione. Nelle proprietà di questa regola è possibile impostare le condizioni per trasformare i valori in ingresso con un valore attestazione in uscita diverso in base al tipo di attestazione in ingresso specificato.  
  
Ad esempio, come illustrato nella figura seguente, quando è impostata una regola con la condizione per sostituire un valore in ingresso con un valore attestazione in uscita diverso, viene eseguito il mapping di tutti i tipi di attestazione in ingresso del gruppo ai nuovi tipi di attestazione in uscita del ruolo. In questo caso, il valore attestazione in ingresso dell'acquirente, viene sostituito con il nuovo valore attestazione in uscita dell'amministrazione.  
  
![Quando utilizzare una trasformazione](media/adfs2_transform.gif)  
  
È inoltre possibile utilizzare questa regola per applicare una condizione che sostituirà tutte le attestazioni in ingresso con una specificata e\-valore suffisso con un nuovo valore di posta elettronica. Ad esempio, è possibile impostare una condizione nella regola per modificare tutti i valori attestazione con il suffisso sales.corp.fabrikam.com in fabrikam.com.  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configurazione della regola in un trust del provider di attestazioni  
Quando si usa un trust del provider di attestazioni, è possibile configurare questa regola perché trasformi le attestazioni in ingresso in equivalenti attendibili. I tipi di attestazione o i valori attestazione possono avere un significato diverso all'interno dell'organizzazione rispetto alle organizzazioni di provider di attestazioni. È possibile usare questa regola per normalizzare i tipi di attestazione e i valori attestazione provenienti dal provider di attestazioni, in modo che i rispettivi equivalenti delle attestazione in uscita possano essere riconosciuti dalla relying party.  
  
## <a name="configuring-this-rule-on-a-relying-party-trust"></a>Configurazione di questa regola in un trust della relying party  
Quando si usa un trust della relying party, questa regola può essere configurata per trasformare le attestazioni per la relying party specifica. I tipi di attestazione o i valori attestazione possono avere un significato diverso per una relying party specifica e questa regola consente di modificare i tipi di attestazione e i valori attestazione in uscita per una singola relying party.  
  
## <a name="how-to-create-this-rule"></a>Come creare la regola  
Creare questa regola utilizzando il linguaggio di regola attestazione o il **trasformare un'attestazione in ingresso** il modello di regola nello snap di gestione di ADFS\-in. Questo modello di regola offre le opzioni di configurazione seguenti:  
  
-   Specificare un nome della regola attestazione  
  
-   Trasformare un tipo di attestazione in ingresso specifico in un tipo di attestazione in uscita specificato  
  
-   Pass-through di tutti i valori attestazione  
  
-   Sostituire un valore attestazione in ingresso con un valore attestazione in uscita diverso  
  
-   Sostituire in ingresso e\-suffisso attestazioni con una nuova e di posta elettronica\-suffisso di posta elettronica  
  
Per ulteriori istruzioni su come creare questo modello, vedere [creare una regola per trasformare un'attestazione in ingresso](https://technet.microsoft.com/library/dd807068.aspx) nella Guida alla distribuzione di ADFS.  
  
## <a name="using-the-claim-rule-language"></a>Uso del linguaggio delle regole attestazioni  
Se l'attestazione in uscita deve essere creata dal contenuto di più di un'attestazione in ingresso, è necessario usare invece una regola personalizzata. Se il valore attestazione dell'attestazione in uscita deve essere basato sul valore dell'attestazione in ingresso, ma con contenuto aggiuntivo, è necessario includere una regola personalizzata anche in quel contesto. Per altre informazioni, vedere [quando usare una regola attestazioni personalizzata](When-to-Use-a-Custom-Claim-Rule.md).  
  
### <a name="examples-of-how-to-construct-a-transform-rule-syntax"></a>Esempi di creazione della sintassi di una regola di trasformazione  
Quando si usa la sintassi del linguaggio delle regole attestazioni per trasformare le attestazioni, è possibile impostare una proprietà dell'attestazione trasformata in un nuovo valore letterale. Ad esempio, la regola seguente modifica il valore attestazione del ruolo da "Administrators" in "root" mantenendo lo stesso tipo di attestazione:  
  
```  
c:[type == “https://schemas.microsoft.com/ws/2008/06/identity/claims/role”, value == “Administrators”]  => issue(type = c.type, value = “root”);  
```  
  
Per le trasformazioni delle attestazioni è anche possibile usare espressioni regolari. Ad esempio, la seguente regola verrà impostato il dominio in attestazioni basate su nome utente di windows nel DOMINIO\\formato UTENTE a FABRIKAM:  
  
```  
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"] => issue(type = c.type, value = regexreplace(c.value, "(?<domain>[^\\]+)\\(?<user>.+)", "FABRIKAM\${user}"));  
```  
  
### <a name="best-practices-for-creating-custom-rules"></a>Procedure consigliate per la creazione di regole personalizzate  
Le trasformazioni di attestazioni possono essere applicate selettivamente alle attestazioni selezionate con funzionalità di filtro di base. A ogni proprietà di attestazione usata per il filtro è possibile assegnare valori, con le avvertenze seguenti:  
  
|Proprietà attestazione|Descrizione|  
|------------------|---------------|  
|Type, Value, ValueType|Queste proprietà vengono usate più di frequente per le assegnazioni. Come minimo, è necessario specificare il tipo e il valore per l'attestazione di trasformazione risultante.|  
|Issuer|Anche se il linguaggio delle regole attestazioni consente di impostare l'autorità emittente di un'attestazione, questa operazione non è in genere consigliabile. L'autorità emittente dell'attestazione non è serializzata nel token. Quando si riceve un token, la proprietà Issuer di tutte le attestazioni è impostata sull'identificatore del server federativo che ha firmato il token. Di conseguenza, l'impostazione dell'autorità emittente di un'attestazione nelle regole non avrà effetto sul contenuto del token e l'impostazione andrà persa dopo l'inclusione dell'attestazione in un token. Il solo scenario in cui può risultare utile impostare l'autorità emittente di un'attestazione è quando questa è impostata su un valore specifico nel set di regole del provider di attestazioni e il set di regole della relying party viene creato con regole che fanno riferimento a questo valore specifico. Se la proprietà Issuer non è impostata esplicitamente su un valore in una regola attestazioni, il motore di rilascio delle attestazioni la imposta su "LOCAL AUTHORITY".|  
|OriginalIssuer|In modo analogo a Issuer, a OriginalIssuer non deve in genere essere assegnato esplicitamente un valore. A differenza di Issuer, la proprietà OriginalIssuer viene serializzata nel token, ma l'aspettativa di consumer di token è che, se impostata, conterrà l'identificatore del server federativo che ha emesso originariamente un'attestazione.|  
|Proprietà|Come descritto nella sezione precedente, il contenitore delle proprietà di un'attestazione non è persistente nel token, quindi le assegnazioni alle proprietà devono essere eseguite solo se i criteri locali successivi intende faranno riferimento le informazioni archiviate nella proprietà.|  
  
Per ulteriori informazioni su come utilizzare il linguaggio di regola attestazione, vedere [il ruolo del linguaggio di regola attestazione](The-Role-of-the-Claim-Rule-Language.md).  
  

