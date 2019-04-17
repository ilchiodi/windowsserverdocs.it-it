---
ms.assetid: 606df285-259c-4c6b-8583-9aca1d614c43
title: Quando utilizzare un Pass-Through or Filter Claim Rule
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: aa205c46bf67dc25a55232b799bdd39fee4ac3c6
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="when-to-use-a-pass-through-or-filter-claim-rule"></a>Quando utilizzare un Pass-Through or Filter Claim Rule
È possibile utilizzare questa regola in Active Directory Federation Services \(AD FS\) quando è necessario richiedere un tipo di attestazione in ingresso specifico e quindi applicare un'azione che determina il tipo di output deve essere eseguita in base ai valori nell'attestazione in ingresso. Quando si usa questa regola, si pass-through o filtrano le attestazioni corrispondenti alla logica della regola nella tabella seguente, in base all'opzione configurata nella regola.  
  
|Opzione della regola|Logica della regola|  
|---------------|--------------|  
|Pass-through di tutti i valori attestazione|Se il tipo di attestazione in ingresso è uguale a *tipo di attestazione specificato* e il valore è uguale a *qualsiasi valore*, quindi passare l'attestazione tramite|  
|Pass-through solo un valore attestazione specifico|Se il tipo di attestazione in ingresso è uguale a *tipo di attestazione specificato* e il valore è uguale a *valore attestazione specificato*, quindi passare l'attestazione tramite|  
|Pass-through solo dei valori attestazione che corrispondono a un valore suffisso di posta elettronica e\ specifico|Se il tipo di attestazione in ingresso è uguale a *tipo di attestazione specificato* e il valore è uguale a *valore suffisso specificato*, quindi passare l'attestazione tramite|  
|Pass-through solo dei valori attestazione che iniziano con un valore specifico|Se il tipo di attestazione in ingresso è uguale a *tipo di attestazione specificato* e il valore inizia con *valore attestazione specificato*, quindi passare l'attestazione tramite|  
  
Le sezioni seguenti forniscono un'introduzione di base per le regole di attestazione e fornire ulteriori dettagli su quando usare questa regola.  
  
## <a name="about-claim-rules"></a>Informazioni sulle regole attestazioni  
Una regola attestazioni rappresenta un'istanza della logica di business che recupera un'attestazione in ingresso, applica una condizione \ (se x quindi y\) e genera un'attestazione in uscita in base ai parametri di condizione. L'elenco seguente fornisce importanti suggerimenti che è necessario conoscere sulle regole attestazione prima di procedere con la lettura di questo argomento:  
  
-   Nella gestione di ADFS snap-in, attestazione regole possono essere create solo utilizzando modelli di regola attestazione  
  
-   Processo di regole attestazione in ingresso di attestazioni direttamente da un provider di attestazioni \ (ad esempio Active Directory o un altro Service \ federativo) oppure dall'output dell'accettazione regole in un trust di provider di attestazioni di trasformazione.  
  
-   Le regole attestazioni vengono elaborate dal motore di rilascio delle attestazioni in ordine cronologico entro un determinato set di regole. Impostando la precedenza sulle regole, è possibile perfezionare ulteriormente o filtrare le attestazioni generate dalle regole precedenti all'interno di un determinato set di regole.  
  
-   Modelli di regola attestazione verranno sempre necessario specificare un tipo di attestazione in ingresso. Tuttavia, è possibile elaborare più valori di attestazione con lo stesso tipo di attestazione usando una singola regola.  
  
Per ulteriori informazioni sulle regole attestazione e set di regole attestazione, vedere [ruolo delle attestazioni regole](The-Role-of-Claim-Rules.md). Per ulteriori informazioni su come vengono elaborate le regole, vedere [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md). Per ulteriori informazioni, modalità di elaborazione dei set di regole attestazione, vedere [ruolo delle Pipeline delle attestazioni](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="pass-through-all-claim-values"></a>Pass-through di tutti i valori attestazione  
Utilizzando questa operazione, tutti i valori di attestazione in ingresso per il tipo di attestazione specificato vengono passati come attestazioni in uscita. Ad esempio, quando il tipo di attestazione in ingresso è specificato come tipo di attestazione ruolo, tutti i valori di attestazione in ingresso vengono copiati singolarmente in nuove attestazioni in uscita con il tipo di attestazione in uscita del ruolo.  
  
## <a name="filtering-a-claim"></a>Filtrare un'attestazione  
In ADFS, il termine *attestazioni filtro* significa filtrare o limitare in ingresso attestazione valori in modo che solo determinati valori vengono passati o inviati come attestazioni in uscita. È il **Pass-Through o filtro di un'attestazione in ingresso** modello di regola che rende possibile questa funzione. All'interno delle proprietà di questa regola, è possibile impostare le condizioni per filtrare i valori in ingresso in modo che vengono passati solo i valori che soddisfano i criteri specificati.  
  
Ad esempio, è possibile utilizzare questa regola in ingresso solo delle attestazioni che corrispondono al valore di attestazione dell'acquirente quando il tipo di attestazione del ruolo o si vuole rilasciare solo attestazioni relative al nome dell'utente corrispondenze di tipo di attestazione in ingresso, ma non quelle contenenti il codice fiscale dell'utente.  
  
Quando si usa una condizione di filtro con questa regola, vengono esaminate tutte le attestazioni in ingresso per determinare quali attestazioni corrispondano ai criteri impostati dalla regola. Tutte le altre attestazioni vengono ignorate, in modo che consentirà l'ingresso solo dei valori attestazione specificati che corrispondono a un tipo di attestazione selezionato.  
  
Ad esempio, come illustrato nella figura seguente, quando una regola è impostata con la condizione per filtrare le attestazioni solo in ingresso che corrispondono a UPN tipo di attestazione e inoltre terminare con @fabrikam.com, tutte le altre attestazioni vengono ignorate a meno che soddisfino questi criteri. Ciò include l'attestazione in ingresso con il tipo di attestazione dell'indirizzo di posta elettronica E\ anche se il valore dell'attestazione termina con @fabrikam.com. In questo caso, solo l'attestazione contenente il valore di Nick@fabrikam.com viene inviato alla relying party.  
  
![Quando utilizzare passata tramite](media/adfs2_filter.gif)  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>Configurazione della regola in un trust del provider di attestazioni  
Quando si usa un trust del provider di attestazioni, è possibile configurare questa regola per pass-through in ingresso solo le attestazioni dal provider di attestazioni che corrispondono a determinati vincoli. È ad esempio accettare solo le attestazioni di posta elettronica e\ dal provider di attestazioni; Pertanto, utilizzare questo modello di regola per accettare i tipi di attestazione e\ di posta elettronica che terminano con nome Domain Name System \(DNS\) del provider di attestazioni.  
  
## <a name="configuring-this-rule-on-a-relying-party-trust"></a>Configurazione della regola in un trust della relying party  
Quando si usa un trust della relying party, è possibile configurare questa regola per pass-through o filtrare le attestazioni in uscita che verranno inviate alla relying party. Alcune relying party potrebbero non riconoscere determinati tipi di attestazioni e determinate attestazioni potrebbero contenere informazioni riservate che non devono essere inviate a determinate relying party. Questo modello di regola consente di applicare tali criteri per un trust della relying party specifico.  
  
## <a name="how-to-create-this-rule"></a>Come creare questa regola  
Puoi creare questa regola utilizzando il linguaggio delle regole attestazione oppure utilizzando il Pass-Through o filtro a un modello di regola attestazione in ingresso in snap-in di gestione di ADFS. Questo modello di regola offre le opzioni di configurazione seguenti:  
  
-   Specificare un nome di regola attestazione  
  
-   Specificare un tipo di attestazione in ingresso  
  
-   Pass-through di tutti i valori attestazione  
  
-   Pass-through solo un valore attestazione specifico  
  
-   Pass-through solo dei valori attestazione che corrispondono a un valore suffisso di posta elettronica e\ specifico  
  
-   Pass-through solo dei valori attestazione che iniziano con un valore specifico  
  
Per ulteriori istruzioni su come creare questo modello, vedere [creare una regola di Pass-Through o filtrare un'attestazione in ingresso](https://technet.microsoft.com/library/dd807060.aspx) nella Guida alla distribuzione di ADFS.  
  
## <a name="using-the-claim-rule-language"></a>Utilizzando il linguaggio di regola attestazione  
Se un'attestazione deve essere inviata solo quando il valore dell'attestazione corrisponde a un modello personalizzato, è necessario utilizzare una regola personalizzata. Per ulteriori informazioni, vedere quando usare una regola personalizzata.  
  
### <a name="examples-of-how-to-construct-a-pass-through-or-filter-rule-syntax"></a>Esempi di come costruire un pass-through o filtrare sintassi regola  
Una semplice regola di filtro filtrerà le attestazioni in base a una delle proprietà descritte in precedenza. Ad esempio, la regola seguente passerà tutte le attestazioni di posta elettronica e\:  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”]  => issue(claim  = c);  
```  
  
I filtri possono essere logicamente AND\-ed insieme. Ad esempio, la regola seguente accetterà tutte le attestazioni di posta elettronica e\ con valorejohndoe@fabrikam.com:  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”, value == “johndoe@fabrikam.com “]  => issue(claim  = c);  
```  
  
Negli esempi precedenti i filtri è sempre usato un operatore di uguaglianza. Il linguaggio delle regole attestazioni supporta gli operatori seguenti:  
  
-   \ = \ = \-\(case\-sensitive\) è uguale a  
  
-   \! \ = \-non uguale a \(case\-sensitive\)  
  
-   \ = ~ \-corrispondenza dell'espressione regolare  
  
-   \! ~ \--corrispondenza dell'espressione regolare  
  
Ad esempio, la regola seguente accetterà tutte le attestazioni di posta elettronica e\ non emessi dal server federativo locale che hanno un suffisso di boeing.com:  
  
```  
c:[type == “http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress”, value =~ “^.*@boeing\.com$” , issuer != “LOCAL AUTHORITY”]  => issue(claim  = c);  
```  
  
### <a name="best-practices-for-creating-custom-rules"></a>Procedure consigliate per la creazione di regole personalizzate  
Un filtro può essere applicato a uno o più delle proprietà di ogni attestazione, come descritto nella tabella seguente.  
  
|Proprietà attestazione|Descrizione|  
|------------------|---------------|  
|Tipo|Il tipo di attestazione \ (generalmente rappresentato come un Uri\) riflette un contratto implicito tra i partner in una federazione su quale tipo di informazioni viene comunicato nell'attestazione. Ad esempio, le attestazioni di tipo http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/emailaddress conterrà l'indirizzo di posta elettronica e\ dell'utente.|  
|Valore|Il valore dell'attestazione. Ad esempio, un'attestazione di tipo http:///\/schemas.xmlsoap.org\/ws\/2005\/05\/identity\/claims\/emailaddress può avere un valore dijohndoe@fabrikam.com|  
|ValueType|ValueType rappresenta come le informazioni contenute nel valore dell'attestazione devono essere interpretate. In genere ValueType verrà impostato su http:///\/ www.w3.org \/2001\/XMLSchema\#string, ma il valore dell'attestazione potrebbe contenere dati Base64Binary codificato \ (ad esempio, un image\) o una data, Boolean e così via.|  
|Autorità di certificazione|L'autorità emittente rappresenta la parte che ha rilasciato per ultima le attestazioni relative all'utente. Se le attestazioni sono state ricevute da un server federativo del provider di attestazioni l'autorità emittente di tutte le attestazioni sta per essere impostato su "LOCAL AUTHORITY". Se le attestazioni sono state ricevute da un server federativo Provider federativo, l'autorità emittente delle attestazioni sta per essere impostato per l'identificatore del provider di attestazioni del provider di attestazioni che ha firmato il token. Di conseguenza, durante l'elaborazione delle regole sulle attestazioni ricevute da un provider di attestazioni è è l'autorità emittente di tutte le attestazioni verrà impostata sullo stesso valore. Durante la creazione di regole per una relying party, la proprietà issuer può essere utilizzata per distinguere le attestazioni provenienti da diversi provider di attestazioni.|  
|OriginalIssuer|Questa proprietà dell'attestazione è progettata per comunicare server federativo che originariamente rilasciato l'attestazione. Poiché la proprietà issuer delle attestazioni è impostata sull'ultimo server federativo che ha firmato il token, l'autorità emittente originale è utile negli scenari in cui un'attestazione è transitata attraverso più di un server federativo \ (ad esempio, una relying party che riceve un token da un server federativo del provider federativo potrebbe esserti utili quali particolare attestazioni server federativo provider autenticato dall'utente)|  
|Proprietà|Oltre alle cinque proprietà descritte in precedenza, ogni attestazione ha un contenitore delle proprietà in cui è possibile archiviare proprietà denominate. Queste proprietà No sono serializzate nel token e servono esclusivamente per passare le informazioni tra i componenti della pipeline di rilascio delle attestazioni all'interno dell'ambito di un server federativo singolo. Ad esempio, impostare una proprietà durante le attestazioni regole del provider di elaborazione e quindi fare riferimento a essa in regole della relying party.|  
  

