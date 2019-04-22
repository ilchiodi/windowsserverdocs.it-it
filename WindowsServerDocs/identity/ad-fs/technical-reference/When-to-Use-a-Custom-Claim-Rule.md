---
ms.assetid: 20d183f0-ef94-44bb-9dfc-ed93799dd1a6
title: Quando usare una regola attestazioni personalizzata
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b08622cd9eefa153e2fe8403fbe85077644182f4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813112"
---
>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="when-to-use-a-custom-claim-rule"></a>Quando usare una regola attestazioni personalizzata
Scrivere una regola attestazioni personalizzata in Active Directory Federation Services \(ADFS\) usando il linguaggio delle regole attestazioni, ovvero un framework che utilizza il motore di rilascio delle attestazioni per generare a livello di codice, trasformare, pass-through e filtrare crediti. Con una regola personalizzata è possibile creare regole con una logica più complessa rispetto a un modello di regola standard. È consigliabile usare una regola personalizzata per:  
  
-   Inviare attestazioni in base ai valori estratti da un linguaggio di Query strutturate \(SQL\) archivio di attributi.  
  
-   Inviare attestazioni in base ai valori estratti da un Lightweight Directory Access Protocol \(LDAP\) archivio di attributi con un filtro LDAP personalizzato.  
  
-   Inviare attestazioni in base a valori estratti da un archivio di attributi personalizzato.  
  
-   Inviare attestazioni solo quando sono presenti due o più attestazioni in ingresso.  
  
-   Inviare attestazioni solo quando viene rilevata la corrispondenza di un valore attestazione in ingresso con un criterio complesso.  
  
-   Inviare attestazioni con modifiche complesse a un valore attestazione in ingresso.  
  
-   Creare attestazioni da usare solo in regole successive, senza inviare effettivamente le attestazioni.  
  
-   Creare un'attestazione in uscita dal contenuto di più attestazioni in ingresso.  
  
È anche possibile creare una regola personalizzata quando il valore attestazione dell'attestazione in uscita deve essere basato sul valore dell'attestazione in ingresso e includere contenuto aggiuntivo.  
  
Il linguaggio delle regole attestazioni è basato su regole. Presenta una parte di condizione e una parte di esecuzione. È possibile usare la sintassi del linguaggio delle regole attestazioni per enumerare, aggiungere o modificare attestazioni in base alle esigenze dell'organizzazione. Per altre informazioni sul modo in cui ognuna di queste parti funziona, vedere [ruolo del linguaggio di regola attestazione](The-Role-of-the-Claim-Rule-Language.md).  
  
Le sezioni seguenti forniscono un'introduzione di base alle regole attestazioni, oltre a fornire informazioni dettagliate sui casi in cui è opportuno usare una regola attestazioni personalizzata.  
  
## <a name="about-claim-rules"></a>Informazioni sulle regole attestazioni  
Una regola attestazioni rappresenta un'istanza della logica di business che accetta un'attestazione in ingresso, applica una condizione \(se x quindi y\) e genera un'attestazione in uscita in base ai parametri di condizione.  
  
> [!IMPORTANT]  
> -   Nello snap di gestione di AD FS\-attestazione regole possono essere create solo utilizzando modelli di regola attestazione  
> -   Processo delle regole di attestazione in ingresso attestazioni direttamente da un provider di attestazioni \(, ad esempio Active Directory o un altro servizio federativo\) o dall'output dell'accettazione regole di trasformazione in un trust del provider di attestazioni.  
> -   Le regole attestazioni sono elaborate dal motore di rilascio delle attestazioni in ordine cronologico entro un determinato set di regole. Impostando la precedenza sulle regole, è possibile perfezionare o filtrare ulteriormente le attestazioni generate dalle regole precedenti all'interno di un set di regole specificato.  
> -   Per i modelli di regola attestazioni è sempre necessario specificare un tipo di attestazione in ingresso. Tuttavia, è possibile elaborare più valori di attestazione con lo stesso tipo di attestazione usando una singola regola.  
  
Per ulteriori informazioni sulle regole attestazione e i set di regole attestazione, vedere [ruolo delle attestazioni regole](The-Role-of-Claim-Rules.md). Per ulteriori informazioni sulle modalità di elaborazione delle regole, vedere [il ruolo del motore di attestazioni](The-Role-of-the-Claims-Engine.md). Per ulteriori informazioni, modalità di elaborazione dei set di regole attestazione, vedere [ruolo delle Pipeline delle attestazioni](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="how-to-create-this-rule"></a>Come creare la regola  
Creare questa regola, è sufficiente creare prima la sintassi che è necessario per l'operazione usando il linguaggio delle regole attestazioni e quindi incollare il risultato nel testo della finestra che è fornito nella invia attestazioni mediante un modello di regola personalizzata nelle proprietà di un provider di attestazioni tr USt o una relying party trust nello snap di gestione di AD FS\-in.  
  
Questo modello di regola offre le opzioni seguenti:  
  
-   Specificare un nome della regola attestazione  
  
-   Digitare uno o più condizioni opzionali e un'istruzione di rilascio usando AD FS linguaggio delle regole attestazioni  
  
Per altre istruzioni per la creazione di una regola personalizzata usando questo modello, vedere [creare una regola per inviare attestazioni mediante una regola personalizzata](https://technet.microsoft.com/library/dd807049.aspx) nella Guida alla distribuzione di AD FS.  
  
Per una migliore comprensione del funzionamento del linguaggio di regola attestazione, visualizzare la sintassi del linguaggio di regola attestazione di altre regole già esistenti nello snap\-in facendo il **Visualizza lingua delle regole** scheda nelle proprietà per la regola. Le informazioni fornite in questa sezione e la sintassi disponibile in questa scheda forniscono indicazioni su come creare regole personalizzate.  
  
Per ulteriori informazioni su come utilizzare il linguaggio di regola attestazione, vedere [il ruolo del linguaggio di regola attestazione](The-Role-of-the-Claim-Rule-Language.md).  
  
## <a name="using-the-claim-rule-language"></a>Uso del linguaggio delle regole attestazioni  
  
### <a name="example-how-to-combine-first-and-last-names-based-on-a-users-name-attribute-values"></a>Esempio: Come combinare nomi e cognomi in base ai valori di attributo del nome dell'utente  
La sintassi della regola seguente combina nomi e cognomi dei valori di attributi in un archivio di attributi specificato. Il motore dei criteri genera un prodotto cartesiano delle corrispondenze per ogni condizione. Ad esempio, l'output per i nomi {"Frank", "Alan"} e i cognomi {"Miller", "Shen"} è {"Frank Miller", "Frank Shen", "Alan Miller", "Alan Shen"}:  
  
```  
c1:[type == "http://exampleschema/firstname" ]  
&&  c2:[type == "http://exampleschema/lastname",]   
=> issue(type = "http://exampleschema/name", value = c1.value + “  “ + c2.value);  
```  
  
### <a name="example-how-to-issue-a-manager-claim-based-on-whether-users-have-direct-reports"></a>Esempio: Come rilasciare un'attestazione responsabile in base alla presenza o meno di dipendenti diretti per gli utenti  
La regola seguente rilascia un'attestazione responsabile solo se l'utente ha dipendenti diretti:  
  
```  
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"] => add(store = "SQL Store", types = ("http://schemas.xmlsoap.org/claims/Reports"), query = "SELECT Reports FROM dbo.DirectReports WHERE UserName = {0}", param = c.value );  
count([type == “http://schemas.xmlsoap.org/claims/Reports“] ) > 0 => issue(= "http://schemas.xmlsoap.org/claims/ismanager", value = "true");  
```  
  
### <a name="example-how-to-issue-a-ppid-claim-based-on-an-ldap-attribute"></a>Esempio: Come rilasciare un'attestazione PPID basata su un attributo LDAP  
La regola seguente rilascia un identificatore personale privato \(PPID\) attestazione basata sulle **windowsaccountname** e **originalissuer** attributi degli utenti in un attributo LDAP archivio:  
  
```  
c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
 => issue(store = "_OpaqueIdStore", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier"), query = "{0};{1};{2}", param = "ppid", param = c.Value, param = c.OriginalIssuer);  
```  
  
Tra gli attributi comuni che possono essere usati per identificare in modo univoco l'utente per questa query sono inclusi:  
  
-   **SID dell'utente**  
  
-   **windowsaccountname**  
  
-   **samaccountname**  
  

