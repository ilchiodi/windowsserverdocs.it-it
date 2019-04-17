---
ms.assetid: 20d183f0-ef94-44bb-9dfc-ed93799dd1a6
title: Quando utilizzare una regola attestazioni personalizzata
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f5398f0b10d9e62548145fdde0354a3d047eb0ae
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="when-to-use-a-custom-claim-rule"></a>Quando utilizzare una regola attestazioni personalizzata
Puoi scrivere una regola attestazioni personalizzata in Active Directory Federation Services \(AD FS\) usando il linguaggio di regola attestazione, ovvero un framework che usa il motore di rilascio delle attestazioni per generare, trasformare, pass-through a livello di codice e filtrare le attestazioni. Utilizzando una regola personalizzata, è possibile creare regole con una logica più complessa rispetto a un modello di regola standard. È consigliabile usare una regola personalizzata quando vuoi:  
  
-   Inviare attestazioni in base a valori estratti da un archivio di attributi Structured Query Language \(SQL\).  
  
-   Inviare attestazioni in base a valori estratti da un archivio di attributi \(LDAP\) Lightweight Directory Access Protocol usando un filtro LDAP personalizzato.  
  
-   Inviare attestazioni in base a valori estratti da un archivio attributi personalizzato.  
  
-   Inviare attestazioni solo quando sono presenti due o più attestazioni in ingresso.  
  
-   Inviare attestazioni solo quando un'attestazione in ingresso valore corrisponde un criterio complesso.  
  
-   Inviare attestazioni con modifiche complesse a un fax in ingresso attestazione valore.  
  
-   Creare attestazioni da usare solo in regole successive, senza inviare effettivamente le attestazioni.  
  
-   Creare un'attestazione in uscita dal contenuto di più attestazioni in ingresso.  
  
È inoltre possibile utilizzare una regola personalizzata quando il valore dell'attestazione dell'attestazione in uscita deve essere basato sul valore dell'attestazione in ingresso, ma deve inoltre includere contenuto aggiuntivo.  
  
Il linguaggio delle regole attestazioni è basato su regole. Include una parte di condizione e una parte di esecuzione. È possibile utilizzare la sintassi di linguaggio delle regole attestazioni per enumerare, aggiungere, eliminare o modificare attestazioni in base alle esigenze dell'organizzazione. Per ulteriori informazioni su come ognuno di questi componenti funziona, vedere [il ruolo del linguaggio di regola attestazione di](The-Role-of-the-Claim-Rule-Language.md).  
  
Le sezioni seguenti forniscono un'introduzione di base per le regole di attestazione. Oltre a informazioni dettagliate su quando usare una regola attestazioni personalizzata.  
  
## <a name="about-claim-rules"></a>Informazioni sulle regole attestazioni  
Una regola attestazioni rappresenta un'istanza della logica di business che accetta un'attestazione in ingresso, applica una condizione \ (se x, quindi y\) e genera un'attestazione in uscita in base ai parametri di condizione.  
  
> [!IMPORTANT]  
> -   Nella gestione di ADFS snap-in, attestazione regole possono essere create solo utilizzando modelli di regola attestazione  
> -   Processo di regole attestazione in ingresso di attestazioni direttamente da un provider di attestazioni \ (ad esempio Active Directory o un altro Service \ federativo) oppure dall'output dell'accettazione regole in un trust di provider di attestazioni di trasformazione.  
> -   Le regole attestazioni vengono elaborate dal motore di rilascio delle attestazioni in ordine cronologico entro un determinato set di regole. Impostando la precedenza sulle regole, è possibile perfezionare ulteriormente o filtrare le attestazioni generate dalle regole precedenti all'interno di un determinato set di regole.  
> -   Modelli di regola attestazioni è sempre necessario specificare un tipo di attestazione in ingresso. Tuttavia, è possibile elaborare più valori di attestazione con lo stesso tipo di attestazione usando una singola regola.  
  
Per ulteriori informazioni sulle regole attestazione e set di regole attestazione, vedere [ruolo delle attestazioni regole](The-Role-of-Claim-Rules.md). Per ulteriori informazioni su come vengono elaborate le regole, vedere [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md). Per ulteriori informazioni, modalità di elaborazione dei set di regole attestazione, vedere [ruolo delle Pipeline delle attestazioni](The-Role-of-the-Claims-Pipeline.md).  
  
## <a name="how-to-create-this-rule"></a>Come creare questa regola  
Per creare questa regola, prima di creazione la sintassi che è necessario per l'operazione utilizzando il linguaggio delle regole attestazioni e quindi incollando il risultato nella casella di testo che viene fornita nell'inviare attestazioni mediante un modello di regola personalizzata nelle proprietà di un provider di attestazioni attendibile o una relying party trust nello snap-in di gestione di ADFS.  
  
Questo modello di regola offre le opzioni seguenti:  
  
-   Specificare un nome di regola attestazione  
  
-   Digitare uno o più condizioni facoltative e un'istruzione di rilascio con AD FS linguaggio delle regole attestazioni  
  
Per ulteriori istruzioni per la creazione di una regola personalizzata con questo modello, vedere [creare una regola per inviare attestazioni mediante una regola personalizzata](https://technet.microsoft.com/library/dd807049.aspx) nella Guida alla distribuzione di ADFS.  
  
Per una migliore comprensione del funzionamento del linguaggio di regola attestazione, visualizzare la sintassi di linguaggio di regola attestazione di altre regole già esistenti nello snap-in facendo il **Visualizza lingua delle regole** scheda delle proprietà per tale regola. Utilizzando le informazioni contenute in questa sezione e le informazioni sulla sintassi in questa scheda può fornire informazioni su come costruire regole personalizzate.  
  
Per ulteriori informazioni su come usare il linguaggio di regola attestazione, vedere [il ruolo del linguaggio di regola attestazione di](The-Role-of-the-Claim-Rule-Language.md).  
  
## <a name="using-the-claim-rule-language"></a>Utilizzando il linguaggio di regola attestazione  
  
### <a name="example-how-to-combine-first-and-last-names-based-on-a-users-name-attribute-values"></a>Esempio: Come combinare nomi e cognomi in base ai valori attributo del nome dell'utente  
La sintassi della regola seguente combina nomi e cognomi valori di attributi in un archivio di attributi specificato. Il motore dei criteri costituisce un prodotto cartesiano delle corrispondenze per ogni condizione. Ad esempio, l'output per nome {"Frank", "Alan"} e i cognomi {"Miller", "Shen"} è {"Frank Miller", "Frank Shen", "Alan Miller", "Alan Shen"}:  
  
```  
c1:[type == "http://exampleschema/firstname" ]  
&&  c2:[type == "http://exampleschema/lastname",]   
=> issue(type = "http://exampleschema/name", value = c1.value + “  “ + c2.value);  
```  
  
### <a name="example-how-to-issue-a-manager-claim-based-on-whether-users-have-direct-reports"></a>Esempio: Come rilasciare un'attestazione responsabile in base indica se gli utenti dipendenti diretti  
La regola seguente rilascia un'attestazione responsabile solo se l'utente ha dipendenti diretti:  
  
```  
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"] => add(store = "SQL Store", types = ("http://schemas.xmlsoap.org/claims/Reports"), query = "SELECT Reports FROM dbo.DirectReports WHERE UserName = {0}", param = c.value );  
count([type == “http://schemas.xmlsoap.org/claims/Reports“] ) > 0 => issue(= "http://schemas.xmlsoap.org/claims/ismanager", value = "true");  
```  
  
### <a name="example-how-to-issue-a-ppid-claim-based-on-an-ldap-attribute"></a>Esempio: Come rilasciare un'attestazione PPID basata su un attributo LDAP  
La regola seguente rilascia un'attestazione ID personale privato \(PPID\) in base il **windowsaccountname** e **originalissuer** gli attributi di utenti in un archivio di attributi LDAP:  
  
```  
c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]  
 => issue(store = "_OpaqueIdStore", types = ("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier"), query = "{0};{1};{2}", param = "ppid", param = c.Value, param = c.OriginalIssuer);  
```  
  
Gli attributi comuni che possono essere usati per identificare l'utente per questa query includono quanto segue:  
  
-   **SID utente**  
  
-   **windowsaccountname**  
  
-   **sAMAccountName**  
  

