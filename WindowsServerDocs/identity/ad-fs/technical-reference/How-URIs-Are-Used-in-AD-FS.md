---
ms.assetid: 53ee93e2-09ea-4f8b-adb7-c24c59f055ea
title: Modalità d'uso degli URI in AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 305bf0cece742c961604dacda7e27b8eac8065e5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812222"
---
>Si applica a: Windows Server 2016, Windows Server 2012 R2

# <a name="how-uris-are-used-in-ad-fs"></a>Modalità d'uso degli URI in AD FS
Un Uniform Resource Identifier \(URI\) è una stringa di caratteri che viene usata come identificatore univoco.  In ADFS, gli URI vengono usati per identificare sia gli indirizzi di rete dei partner che gli oggetti di configurazione.  Quando vengono usati per identificare gli indirizzi di rete dei partner, l'URI è sempre un URL.  Quando vengono usati per identificare gli oggetti di configurazione, l'URI può essere un URN o un URL.  Per altre informazioni generali sugli URI, vedere [RFC 2396](https://go.microsoft.com/fwlink/?LinkId=48289) e [RFC 3986](https://go.microsoft.com/fwlink/?LinkId=90453).  
  
## <a name="uris-as-partner-network-addresses"></a>URI come indirizzi di rete dei partner  
Di seguito sono indicati gli URL degli indirizzi di rete gestiti più spesso dagli amministratori in ADFS.  
  
-   Gli URL del servizio federativo, tra cui WS\-Federation, SAML, WS\-Trust, metadati della federazione, WS\-MetadataExchange, Privacy e gli URL dell'organizzazione  
  
-   Gli URL di un trust della relying party, tra cui WS\-Federation, SAML e URL dei metadati di federazione  
  
-   Gli URL di una relazione di trust del provider di attestazioni, tra cui WS\-Federation, SAML e URL dei metadati di federazione  
  
## <a name="uris-as-object-identifiers"></a>URI come identificatori di oggetto  
La tabella seguente illustra gli identificatori gestiti più spesso dagli amministratori in ADFS.  
  
|Nome identificatore|Descrizione|Confronti|  
|-------------------|---------------|---------------|  
|Identificatore del servizio federativo|Questo identificatore viene usato per identificare il servizio federativo.  Viene usato dalle relying party che usano le attestazioni del servizio federativo nonché dai provider di attestazioni che rilasciano le attestazioni al servizio federativo.|Quando un utente richiede le attestazioni a un provider di attestazioni per il servizio federativo, l'identificatore del servizio federativo viene usato per identificare la destinazione delle attestazioni.<br /><br />Quando il servizio federativo riceve le attestazioni da un provider di attestazioni, ne verifica l'ambito cercando il relativo identificatore del servizio federativo.<br /><br />Quando una relying party riceve le attestazioni dal servizio federativo, verifica che l'autorità emittente delle attestazioni corrisponda all'identificatore del servizio federativo.|  
|Identificatore della relying party|Questo identificatore viene usato per identificare la relying party nel servizio federativo.  Viene usato durante il rilascio delle attestazioni alla relying party.|Quando un utente richiede le attestazioni al servizio federativo per la relying party, l'identificatore della relying party viene usato per identificare la relying party a cui sono associate le attestazioni.  Questo confronto viene eseguito usando la corrispondenza di prefisso \(vedere di seguito\).<br /><br />Quando la relying party riceve le attestazioni, controlla l'identificatore nel token di sicurezza per verificare che le attestazioni siano ad essa associate.|  
|Identificatore del provider di attestazioni|Questo identificatore viene usato per identificare il provider di attestazioni nel servizio federativo.  Viene usato durante la ricezione delle attestazioni dal provider di attestazioni.|Quando il servizio federativo riceve le attestazioni dal provider di attestazioni, controlla che l'autorità emittente delle attestazioni corrisponda all'identificatore del provider di attestazioni.|  
|Tipo di attestazione|Questo identificatore viene usato per definire il tipo di attestazione.  Viene usato dal servizio federativo, dai provider di attestazioni e dalle relying party durante l'invio e ricezione delle attestazioni.|Quando il servizio federativo riceve le attestazioni da un provider di attestazioni, le regole attestazioni associate al trust del provider di attestazioni corrispondente consentono all'amministratore di confrontare i tipi di attestazione ed elaborare le attestazioni.  Le regole attestazioni associate a un trust della relying party consentono inoltre all'amministratore di confrontare i tipi di attestazione tra le attestazioni provenienti dalle regole del trust del provider di attestazioni e decidere le attestazioni da rilasciare.|  
  
## <a name="uri-prefix-matching-for-relying-party-identifiers"></a>Corrispondenza dei prefissi URI per gli identificatori delle relying party  
La sintassi del percorso di un URI è organizzata gerarchicamente ed è delimitata da tutti "\/"caratteri o tutto":" caratteri.  Pertanto, il percorso può essere suddiviso in sezioni di percorso in base al carattere di delimitazione.  Corrispondenza dei prefissi, ogni sezione deve essere una corrispondenza completa in base alle regole di corrispondenza \(queste regole determinano le maiuscole e minuscole delle corrispondenze\). Per altre informazioni sulle regole di corrispondenza, vedere le RFC indicate in precedenza.  
  
Quando una relying party viene identificata in una richiesta al servizio federativo, ADFS usa la logica di corrispondenza dei prefissi per determinare se esiste un trust della relying party corrispondente nel database di configurazione di ADFS.  
  
Ad esempio, se l'identificatore della relying party nel database di configurazione di AD FS \(URI1\) è un prefisso per l'identificatore della relying party nella richiesta in ingresso \(URI2\), quindi deve essere true quanto segue:  
  
-   I delimitatori finali \(barre e due punti\) del percorso di sezioni o delle autorità devono essere ignorate  
  
-   Le parti di schema e autorità di URI1 e URI2 devono essere una corrispondenza esatta senza distinzione tra maiuscole e minuscole  
  
-   Ogni sezione di percorso di URI1 deve essere una corrispondenza esatta \(base la distinzione maiuscole/minuscole scelto\) alla sezione di percorso corrispondente di URI2  
  
-   URI2 può avere più sezioni di percorso di URI1 ma URI1 non deve avere più sezioni di percorso di URI2  
  
-   URI1 non può avere più sezioni di percorso di URI2  
  
-   Se URI1 ha una stringa di query, questa deve corrispondere esattamente a una stringa di query di URI2  
  
-   Se URI1 ha un frammento, questo deve corrispondere esattamente a un frammento di URI2  
  
La tabella seguente fornisce altri esempi.  
  
|Identificatore della relying party nel database di configurazione di ADFS|Identificatore della relying party nel messaggio di richiesta|L'identificatore della richiesta corrisponde all'identificatore di configurazione?|Motivo|  
|------------------------------------------------------------|-----------------------------------------------|------------------------------------------------------------|----------|  
|http:\/\/contoso.com|http:\/\/contoso.com|TRUE|Corrispondenza esatta|  
|http:\/\/contoso.com\/|http:\/\/contoso.com|TRUE|Le barre finali vengono ignorate|  
|http:\/\/contoso.com|http:\/\/contoso.com\/|TRUE|Le barre finali vengono ignorate|  
|http:\/\/contoso.com|http:\/\/contoso.com\/hr|TRUE|URI1 non ha alcun percorso e corrisponde allo schema e all'autorità di URI2|  
|http:\/\/contoso.com\/hr|http:\/\/contoso.com\/hr\/web|TRUE|Le prime sezioni di percorso corrispondono, URI1 non ha una seconda sezione di percorso|  
|http:\/\/contoso.com\/hr|http:\/\/contoso.com\/hr\/web\/? m\=t|TRUE|Gli stessi motivi indicati sopra, la stringa di query non apporta alcuna modifica|  
|http:\/\/contoso.com\/hr\/|http:\/\/contoso.com\/ibrido\/principale|FALSE|La sezione di percorso 1 di URI1 non corrisponde alla sezione di percorso 1 di URI2|  
|http:\/\/contoso.com\/hr|http:\/\/contoso.com|FALSE|URI1 ha più sezioni di percorso di URI2|  
|http:\/\/contoso.com\/hr|http:\/\/contoso.com\/hrweb|FALSE|Le prime sezioni di percorso non corrispondono|  
|http:\/\/contoso.com\/?m\=t|http:\/\/contoso.com\/?m\=f|FALSE|Le parti di stringa di query non corrispondono|  
|https:\/\/contoso.com|http:\/\/contoso.com|FALSE|Le parti di schema non corrispondono|  
|http:\/\/sts.contoso.com|http:\/\/contoso.com|FALSE|Le parti di autorità non corrispondono|  
|http:\/\/contoso.com|http:\/\/sts.contoso.com|FALSE|Le parti di autorità non corrispondono|  
  

