---
ms.assetid: 53ee93e2-09ea-4f8b-adb7-c24c59f055ea
title: Come usare gli URI in AD FS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 305bf0cece742c961604dacda7e27b8eac8065e5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
>Si applica a: Windows Server 2016, Windows Server 2012 R2

# <a name="how-uris-are-used-in-ad-fs"></a>Come usare gli URI in AD FS
Un \(URI\) Uniform Resource Identifier è una stringa di caratteri che viene utilizzata come un identificatore univoco.  In ADFS, gli URI vengono utilizzati per identificare sia gli indirizzi di rete partner e gli oggetti di configurazione.  Quando utilizzato per identificare gli indirizzi di rete di partner, l'URI è sempre un URL.  Quando utilizzato per identificare gli oggetti di configurazione, l'URI può essere un URN o un URL.  Per ulteriori informazioni generali sugli URI, vedere [RFC 2396](https://go.microsoft.com/fwlink/?LinkId=48289) e [RFC 3986](https://go.microsoft.com/fwlink/?LinkId=90453).  
  
## <a name="uris-as-partner-network-addresses"></a>URI come indirizzi di rete partner  
Di seguito sono indicati gli URL di indirizzo di rete che sono gestiti più spesso dagli amministratori in ADFS.  
  
-   Gli URL del servizio federativo, tra cui WS-Federation, SAML, WS-Trust, metadati di federazione, WS-MetadataExchange, Privacy e gli URL dell'organizzazione  
  
-   URL di un trust della relying party, tra cui WS-Federation, SAML e gli URL dei metadati di federazione  
  
-   URL di un trust di provider di attestazioni, tra cui WS-Federation, SAML e gli URL dei metadati di federazione  
  
## <a name="uris-as-object-identifiers"></a>URI come identificatori di oggetto  
Nella tabella seguente illustra gli identificatori gestiti più spesso dagli amministratori in ADFS.  
  
|Nome identificatore|Descrizione|Confronti|  
|-------------------|---------------|---------------|  
|Identificatore del servizio federativo|Questo identificatore viene usato per identificare il servizio federativo.  Viene usato dalle relying party che usano le attestazioni da questo servizio federativo, come provider di attestazioni che rilasciano le attestazioni al servizio federativo.|Quando un utente richiede le attestazioni da un provider di attestazioni per il servizio federativo, l'identificatore del servizio federativo verrà utilizzato per identificare la destinazione delle attestazioni.<br /><br />Quando il servizio federativo riceve le attestazioni da un provider di attestazioni, controllerà per garantire che le attestazioni ambite cercando il relativo identificatore del servizio federativo.<br /><br />Quando una relying party riceve le attestazioni dal servizio federativo, la relying party verifica che l'autorità emittente delle attestazioni corrisponda all'identificatore del servizio federativo.|  
|Identificatore della relying party|Questo identificatore viene usato per identificare la relying party nel servizio federativo.  Viene usato quando il rilascio delle attestazioni alla relying party.|Quando un utente richiede le attestazioni dal servizio federativo per la relying party, l'identificatore della relying party da utilizzare per identificare la relying party per cui devono essere destinate le attestazioni.  Questo confronto viene eseguito con prefisso \(see below\) corrispondente.<br /><br />Quando la relying party riceve le attestazioni, controlla l'identificatore nel token di sicurezza per verificare che le attestazioni siano ad essa associate.|  
|Identificatore del provider di attestazioni|Questo identificatore viene usato per identificare il provider di attestazioni al servizio federativo.  Viene usato quando riceve le attestazioni dal provider di attestazioni.|Quando il servizio federativo riceve le attestazioni dal provider di attestazioni, il servizio federativo verifica che l'autorità emittente delle attestazioni corrisponda all'identificatore del provider di attestazioni.|  
|Tipo di attestazione|Questo identificatore viene usato per definire il tipo di attestazione.  Viene utilizzato da questo servizio federativo, i provider di attestazioni e relying party per l'invio e ricezione delle attestazioni.|Quando il servizio federativo riceve le attestazioni da un provider di attestazioni, le regole attestazioni associate all'attendibilità del provider di attestazioni corrispondente consentono all'amministratore di confrontare i tipi di attestazione ed elaborare le attestazioni.  Inoltre, le regole attestazioni associate a un trust della relying party consentono all'amministratore di confrontare i tipi di attestazione tra le attestazioni provenienti dalle regole di attendibilità provider di attestazioni e decidere quali attestazioni rilasciare.|  
  
## <a name="uri-prefix-matching-for-relying-party-identifiers"></a>Per gli identificatori delle relying party corrispondenza dei prefissi URI  
La sintassi del percorso di un URI è organizzata gerarchicamente ed è delimitata da tutti "\ /"caratteri o tutti":" caratteri.  Di conseguenza, il percorso può essere suddiviso in sezioni di percorso in base al carattere di delimitazione.  Corrispondenza dei prefissi, ogni sezione deve essere una corrispondenza completa in base alle regole di corrispondenza \ (queste regole determinano le maiuscole e minuscole di matches\). Per ulteriori informazioni sulle regole di corrispondenza, vedere le RFC indicate in precedenza.  
  
Quando una relying party viene identificata in una richiesta al servizio federativo, ADFS Usa la logica di corrispondenza dei prefissi per determinare se esiste un corrispondenza trust della relying party nel database di configurazione di ADFS.  
  
Ad esempio, se l'identificatore della relying party nel database di configurazione di ADFS \(URI1\) è un prefisso per l'identificatore della relying party in ingresso richiedere \(URI2\), quindi devono essere soddisfatti i seguenti:  
  
-   I delimitatori finali \(slashes and colons\) del percorso sezioni o delle autorità devono essere ignorate  
  
-   Le parti di schema e autorità di URI1 e URI2 devono essere una corrispondenza esatta tra maiuscole e minuscole  
  
-   Ogni sezione di percorso di URI1 deve essere una corrispondenza esatta \ (in base il chosen\ tra maiuscole e minuscole) alla sezione di percorso corrispondente di URI2  
  
-   URI2 può avere più sezioni di percorso di URI1 ma URI1 non deve avere più sezioni di percorso di URI2  
  
-   URI1 non può avere più sezioni di percorso di URI2  
  
-   Se URI1 ha una stringa di query, deve corrispondere esattamente a una stringa di query di URI2  
  
-   Se URI1 ha un frammento, deve corrispondere esattamente a un frammento di URI2  
  
Nella tabella seguente vengono forniti esempi aggiuntivi.  
  
|Identificatore della relying party nel database di configurazione di ADFS|Identificatore della relying party nel messaggio di richiesta|Identificatore della richiesta corrisponde all'identificatore di configurazione?|Motivo|  
|------------------------------------------------------------|-----------------------------------------------|------------------------------------------------------------|----------|  
|http:///\/contoso.com|http:///\/contoso.com|TRUE|Corrispondenza esatta|  
|http:///\/contoso.com\/|http:///\/contoso.com|TRUE|Le barre finali vengono ignorate|  
|http:///\/contoso.com|http:///\/contoso.com\/|TRUE|Le barre finali vengono ignorate|  
|http:///\/contoso.com|http:///\/contoso.com\/hr|TRUE|URI1 non ha alcun percorso e corrisponde allo schema e autorità di URI2|  
|http:///\/contoso.com\/hr|http:///\/contoso.com\/hr\/Web|TRUE|Le prime sezioni di percorso corrispondono, URI1 non ha nessun seconda sezione di percorso|  
|http:///\/contoso.com\/hr|http:///\/contoso.com\/hr\/web\/?m\=t|TRUE|Gli stessi motivi indicati sopra, stringa di query non apporta alcuna modifica|  
|http:///\/contoso.com\/hr\/|http:///\/contoso.com\/hrw\/Main|FALSE|La sezione 1 di percorso di URI1 non corrisponde la sezione di percorso di URI2 1|  
|http:///\/contoso.com\/hr|http:///\/contoso.com|FALSE|URI1 ha più sezioni di percorso di URI2|  
|http:///\/contoso.com\/hr|http:///\/contoso.com\/hrweb|FALSE|Le prime sezioni di percorso non corrispondono|  
|http:///\/contoso.com\/?m\=t|http:///\/contoso.com\/?m\=f|FALSE|Parti di stringa di query non corrispondono|  
|https:\/\/contoso.com|http:///\/contoso.com|FALSE|Parti di schema non corrispondono|  
|http:///\/STS.contoso.com|http:///\/contoso.com|FALSE|Parti di autorità non corrispondono|  
|http:///\/contoso.com|http:///\/STS.contoso.com|FALSE|Parti di autorità non corrispondono|  
  

