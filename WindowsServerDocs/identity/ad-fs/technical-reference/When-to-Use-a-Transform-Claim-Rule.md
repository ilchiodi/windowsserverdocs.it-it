---
ms.assetid: 77aa61bf-9c04-4889-a5d2-6f45bc1b8bd2
title: Quando utilizzare una regola attestazione di trasformazione
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c7b7ea2c8d9a08a4cbf6c89c2de2482043efe25b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="when-to-use-a-transform-claim-rule"></a>Quando utilizzare una regola attestazione di trasformazione
È possibile utilizzare questa regola in Active Directory Federation Services \(AD FS\) quando è necessario eseguire il mapping di un tipo di attestazione in ingresso a un tipo di attestazione in uscita e quindi applicare un'azione che determina il tipo di output deve essere eseguita in base ai valori che ha originato nell'attestazione in ingresso. Quando si usa questa regola, si pass-through o si trasformano le attestazioni corrispondenti alla logica della regola seguente, in base a una delle opzioni che è configurata nella regola, come descritto nella tabella seguente.  
  
|Opzione della regola|Logica della regola|  
|---------------|--------------|  
|Pass-through tutte le attestazioni in ingresso|Se il tipo di attestazione in ingresso è uguale a *tipo di attestazione specificato* e il valore è uguale a *qualsiasi valore*, quindi passare l'attestazione tramite con uguale a tipo di attestazione in uscita *tipo di attestazione specificato*|  
|Sostituire un valore attestazione in ingresso con un diverso valore attestazione in uscita|Se il tipo di attestazione in ingresso è uguale a *tipo di attestazione specificato* e il valore è uguale a *valore attestazione specificato*, l'attestazione con un nuovo valore attestazione in uscita viene trasformata *valore attestazione specificato* e tipo di attestazione con in uscita *tipo di attestazione specificato*|  
|Sostituzione di attestazioni in ingresso suffisso di posta elettronica e\ con un nuovo suffisso di posta elettronica e\|Se il tipo di attestazione in ingresso è uguale a *tipo di attestazione specificato* e il valore è uguale a *qualsiasi suffisso*, l'attestazione con un nuovo valore attestazione in uscita viene trasformata *valore suffisso specificato* e tipo di attestazione con in uscita *tipo di attestazione specificato*|  
  
Le sezioni seguenti forniscono un'introduzione di base per le regole di attestazione e fornire ulteriori dettagli su quando usare questa regola.  
  
## <a name="about-claim-rules"></a>Informazioni sulle regole attestazioni  
Una regola attestazioni rappresenta un'istanza della logica di business che recupera un'attestazione in ingresso, applica una condizione \ (se x quindi y\) e genera un'attestazione in uscita in base ai parametri di condizione. L'elenco seguente fornisce importanti suggerimenti che è necessario conoscere sulle regole attestazione prima di procedere con la lettura di questo argomento:  
  
-   Nella gestione di ADFS snap-in, attestazione regole possono essere create solo utilizzando modelli di regola attestazione  
  
-   Processo di regole attestazione in ingresso di attestazioni direttamente da un provider di attestazioni \ (ad esempio Active Directory o un altro Service \ federativo) oppure dall'output dell'accettazione regole in un trust di provider di attestazioni di trasformazione.  
  
-   Le regole attestazioni vengono elaborate dal motore di rilascio delle attestazioni in ordine cronologico entro un determinato set di regole. Impostando la precedenza sulle regole, è possibile perfezionare ulteriormente o filtrare le attestazioni generate dalle regole precedenti all'interno di un determinato set di regole.  
  
-   Modelli di regola attestazione verranno sempre necessario specificare un tipo di attestazione in ingresso. Tuttavia, è possibile elaborare più valori di attestazione con lo stesso tipo di attestazione usando una singola regola.  
  
Per ulteriori informazioni sulle regole attestazione e set di regole attestazione, vedere [ruolo delle attestazioni regole](The-Role-of-Claim-Rules.md). Per ulteriori informazioni su come vengono elaborate le regole, vedere [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md). Per ulteriori informazioni, modalità di elaborazione dei set di regole attestazione, vedere [ruolo delle Pipeline delle attestazioni](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="pass-through-all-claim-values"></a>Pass-through di tutti i valori attestazione  
Quando si utilizza questa operazione, tutti i valori di attestazione in ingresso che corrispondono a un tipo di attestazione in ingresso specificato vengono eseguito il mapping a un tipo di attestazione in uscita specificato prima che vengano inviati come attestazioni in uscita in token firmati dal servizio federativo.  
  
Ad esempio, quando è impostata una regola con il **Pass-through di tutti i valori di attestazione** opzione logica e in ingresso attestazione di tipo di gruppo e viene specificato il tipo di attestazione in uscita del ruolo, vengono copiati tutti i valori di attestazione in ingresso che inseriti dall'emittente singolarmente in nuove attestazioni in uscita con tipo di attestazione del ruolo.  
  
## <a name="transforming-a-claim"></a>Trasformazione di un'attestazione  
In ADFS, il termine *trasformazione di attestazioni* significa sostituire un fax in ingresso attestazione valore con un diverso valore attestazione in uscita. È la trasformazione di una regola di attestazione in ingresso che rende possibile questa funzione. All'interno delle proprietà di questa regola, è possibile impostare le condizioni per trasformare i valori in ingresso con un diverso valore attestazione in uscita in base al tipo di attestazione in ingresso specificato.  
  
Ad esempio, come illustrato nella figura seguente, quando è impostata una regola con la condizione per sostituire un valore in ingresso con un diverso valore attestazione in uscita, tutti i tipi di attestazione in ingresso del gruppo vengono mappati a nuovi tipi di attestazione in uscita del ruolo. In questo caso, il valore attestazione in ingresso dell'acquirente viene sostituito con il nuovo valore attestazione in uscita dell'amministratore.  
  
![Quando utilizzare una trasformazione](media/adfs2_transform.gif)  
  
È inoltre possibile utilizzare questa regola per applicare una condizione che sostituirà tutte le attestazioni in ingresso con un valore suffisso di posta elettronica e\ specificato con un nuovo valore. Ad esempio, è possibile impostare una condizione nella regola per modificare tutti i valori attestazione con il suffisso di sales.corp.fabrikam.com a fabrikam.com.  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configurazione della regola in un trust del provider di attestazioni  
Quando si usa un trust del provider di attestazioni, è possibile configurare questa regola per trasformare le attestazioni dal provider di attestazioni in ingresso in equivalenti attendibili. Tipi di attestazione o i valori possono avere un significato diverso all'interno dell'organizzazione rispetto nelle organizzazioni di provider di attestazioni di attestazione. È possibile utilizzare questa regola per normalizzare i tipi di attestazione e i valori provenienti dal provider di attestazioni in modo che i relativi equivalenti delle attestazione in uscita possono essere riconosciute dalla relying party.  
  
## <a name="configuring-this-rule-on-a-relying-party-trust"></a>Configurazione della regola in un trust della relying party  
Quando si usa un trust della relying party, è possibile configurare questa regola per trasformare le attestazioni per la relying party specifica. Tipi di attestazione o i valori potrebbero avere un significato diverso per una relying party specifica e questa regola consente di modificare i tipi di attestazione in uscita e i valori per una singola relying party di attestazione.  
  
## <a name="how-to-create-this-rule"></a>Come creare questa regola  
Creare questa regola utilizzando il linguaggio di regola attestazione o il **trasformare un'attestazione in ingresso** modello di regola nello snap-in di gestione di ADFS. Questo modello di regola offre le opzioni di configurazione seguenti:  
  
-   Specificare un nome di regola attestazione  
  
-   Trasformare un tipo di attestazione in ingresso specifico in un tipo di attestazione in uscita specificato  
  
-   Pass-through di tutti i valori attestazione  
  
-   Sostituire un valore attestazione in ingresso con un diverso valore attestazione in uscita  
  
-   Sostituire attestazioni suffisso e\ di posta elettronica in ingresso con un nuovo suffisso di posta elettronica e\  
  
Per ulteriori istruzioni su come creare questo modello, vedere [creare una regola per trasformare un'attestazione in ingresso](https://technet.microsoft.com/library/dd807068.aspx) nella Guida alla distribuzione di ADFS.  
  
## <a name="using-the-claim-rule-language"></a>Utilizzando il linguaggio di regola attestazione  
Se l'attestazione in uscita deve essere costruito dal contenuto di più attestazioni in ingresso, è necessario usare invece una regola personalizzata. Se il valore dell'attestazione dell'attestazione in uscita deve essere basato sul valore dell'attestazione in ingresso, ma con contenuto aggiuntivo, è necessario utilizzare una regola personalizzata anche in tale contesto. Per ulteriori informazioni, vedere [When to Use a Custom Claim Rule](When-to-Use-a-Custom-Claim-Rule.md).  
  
### <a name="examples-of-how-to-construct-a-transform-rule-syntax"></a>Esempi di come costruire una sintassi regola di trasformazione  
Quando si usa la sintassi di linguaggio delle regole attestazioni per trasformare le attestazioni, è possibile impostare una proprietà dell'attestazione trasformata in un nuovo valore letterale. Ad esempio, la regola seguente modifica il valore attestazione del ruolo da "Administrators" in "root" mantenendo lo stesso tipo di attestazione:  
  
```  
c:[type == “https://schemas.microsoft.com/ws/2008/06/identity/claims/role”, value == “Administrators”]  => issue(type = c.type, value = “root”);  
```  
  
Espressioni regolari possono essere utilizzate anche per le trasformazioni di attestazioni. Ad esempio, la regola seguente imposterà il dominio in attestazioni basate su nome utente windows nel formato dominio\\Nome utente a FABRIKAM:  
  
```  
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"] => issue(type = c.type, value = regexreplace(c.value, "(?<domain>[^\\]+)\\(?<user>.+)", "FABRIKAM\${user}"));  
```  
  
### <a name="best-practices-for-creating-custom-rules"></a>Procedure consigliate per la creazione di regole personalizzate  
Le trasformazioni di attestazioni possono essere applicate selettivamente alle attestazioni selezionate con funzionalità di base di filtro. Ogni proprietà di attestazione usata per il filtro è possibile assegnare valori, con le avvertenze seguenti:  
  
|Proprietà attestazione|Descrizione|  
|------------------|---------------|  
|Tipo di valore, ValueType|Queste proprietà consentirà più di frequente per le assegnazioni. Nel tipo e il valore minimo deve essere specificati per l'attestazione di trasformazione risultante.|  
|Autorità di certificazione|Mentre il linguaggio delle regole attestazioni consente di impostare l'autorità emittente di un'attestazione, non si tratta in genere consigliabile. L'autorità emittente di un'attestazione non è serializzata nel token. Quando si riceve un token la proprietà Issuer di tutte le attestazioni è impostata sull'identificatore del server federativo che ha firmato il token. In questo modo, impostare l'autorità emittente di un'attestazione nelle regole non avrà effetto sul contenuto del token e l'impostazione andrà persa dopo l'inclusione dell'attestazione in un token. Il solo scenario in cui impostare l'autorità emittente di un'attestazione ha senso è quando questa è impostata su un valore specifico nel set di regole di provider di attestazioni e regole della relying party set viene creato con regole che fanno riferimento a questo valore specifico. Se la proprietà Issuer non è impostata in modo esplicito su un valore in una regola attestazioni che il motore di rilascio delle attestazioni imposta su "LOCAL AUTHORITY".|  
|OriginalIssuer|In modo analogo a Issuer, OriginalIssuer in genere non deve essere in modo esplicito assegnato un valore. A differenza di Issuer, la proprietà OriginalIssuer viene serializzata nel token, ma l'aspettativa di consumer di token è che, se impostata, conterrà l'identificatore del server federativo che ha emesso originariamente un'attestazione.|  
|Proprietà|Come descritto nella sezione precedente, il contenitore delle proprietà di un'attestazione non è persistente nel token, in modo che le assegnazioni alle proprietà devono essere rimosse solo se i criteri locali successivi intende faranno riferimento le informazioni archiviate nella proprietà.|  
  
Per ulteriori informazioni su come usare il linguaggio di regola attestazione, vedere [il ruolo del linguaggio di regola attestazione di](The-Role-of-the-Claim-Rule-Language.md).  
  

