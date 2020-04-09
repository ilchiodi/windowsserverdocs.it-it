---
ms.assetid: 53ee93e2-09ea-4f8b-adb7-c24c59f055ea
title: Modalità d'uso degli URI in AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 3d875d510b0f8311d1d3012255214f2ea0841faf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860204"
---
# <a name="how-uris-are-used-in-ad-fs"></a>Modalità d'uso degli URI in AD FS
Un Uniform Resource Identifier \(URI\) è una stringa di caratteri utilizzata come identificatore univoco.  In ADFS, gli URI vengono usati per identificare sia gli indirizzi di rete dei partner che gli oggetti di configurazione.  Quando vengono usati per identificare gli indirizzi di rete dei partner, l'URI è sempre un URL.  Quando vengono usati per identificare gli oggetti di configurazione, l'URI può essere un URN o un URL.  Per altre informazioni generali sugli URI, vedere [RFC 2396](https://go.microsoft.com/fwlink/?LinkId=48289) e [RFC 3986](https://go.microsoft.com/fwlink/?LinkId=90453).  
  
## <a name="uris-as-partner-network-addresses"></a>URI come indirizzi di rete dei partner  
Di seguito sono indicati gli URL degli indirizzi di rete gestiti più spesso dagli amministratori in ADFS.  
  
-   URL del Servizio federativo, inclusi WS\-Federation, SAML, WS\-Trust, metadati federativi, WS\-MetadataExchange, URL privacy e organizzazione  
  
-   URL di un trust relying party, inclusi gli URL di WS\-Federation, SAML e metadati della Federazione  
  
-   URL di un trust del provider di attestazioni, inclusi WS\-Federation, SAML e URL dei metadati della Federazione  
  
## <a name="uris-as-object-identifiers"></a>URI come identificatori di oggetto  
La tabella seguente illustra gli identificatori gestiti più spesso dagli amministratori in ADFS.  
  
|Nome identificatore|Descrizione|Confronti|  
|-------------------|---------------|---------------|  
|Identificatore del servizio federativo|Questo identificatore viene usato per identificare il servizio federativo.  Viene usato dalle relying party che usano le attestazioni del servizio federativo nonché dai provider di attestazioni che rilasciano le attestazioni al servizio federativo.|Quando un utente richiede le attestazioni a un provider di attestazioni per il servizio federativo, l'identificatore del servizio federativo viene usato per identificare la destinazione delle attestazioni.<p>Quando il servizio federativo riceve le attestazioni da un provider di attestazioni, ne verifica l'ambito cercando il relativo identificatore del servizio federativo.<p>Quando una relying party riceve le attestazioni dal servizio federativo, verifica che l'autorità emittente delle attestazioni corrisponda all'identificatore del servizio federativo.|  
|Identificatore della relying party|Questo identificatore viene usato per identificare la relying party nel servizio federativo.  Viene usato durante il rilascio delle attestazioni alla relying party.|Quando un utente richiede le attestazioni al servizio federativo per la relying party, l'identificatore della relying party viene usato per identificare la relying party a cui sono associate le attestazioni.  Questo confronto viene eseguito usando la corrispondenza del prefisso \(vedere di seguito\).<p>Quando la relying party riceve le attestazioni, controlla l'identificatore nel token di sicurezza per verificare che le attestazioni siano ad essa associate.|  
|Identificatore del provider di attestazioni|Questo identificatore viene usato per identificare il provider di attestazioni nel servizio federativo.  Viene usato durante la ricezione delle attestazioni dal provider di attestazioni.|Quando il servizio federativo riceve le attestazioni dal provider di attestazioni, controlla che l'autorità emittente delle attestazioni corrisponda all'identificatore del provider di attestazioni.|  
|Tipo di attestazione.|Questo identificatore viene usato per definire il tipo di attestazione.  Viene usato dal servizio federativo, dai provider di attestazioni e dalle relying party durante l'invio e ricezione delle attestazioni.|Quando il servizio federativo riceve le attestazioni da un provider di attestazioni, le regole attestazioni associate al trust del provider di attestazioni corrispondente consentono all'amministratore di confrontare i tipi di attestazione ed elaborare le attestazioni.  Le regole attestazioni associate a un trust della relying party consentono inoltre all'amministratore di confrontare i tipi di attestazione tra le attestazioni provenienti dalle regole del trust del provider di attestazioni e decidere le attestazioni da rilasciare.|  
  
## <a name="uri-prefix-matching-for-relying-party-identifiers"></a>Corrispondenza dei prefissi URI per gli identificatori delle relying party  
La sintassi del percorso di un URI è organizzata gerarchicamente ed è delimitata da tutti i caratteri "\/" o da tutti i caratteri ":".  Pertanto, il percorso può essere suddiviso in sezioni di percorso in base al carattere di delimitazione.  Quando il prefisso corrisponde, ogni sezione deve essere una corrispondenza completa in base alle regole di corrispondenza \(queste regole regolano la combinazione di maiuscole e minuscole delle corrispondenze\). Per ulteriori informazioni sulle regole di corrispondenza, vedere le RFC indicate in precedenza.  
  
Quando una relying party viene identificata in una richiesta al servizio federativo, ADFS usa la logica di corrispondenza dei prefissi per determinare se esiste un trust della relying party corrispondente nel database di configurazione di ADFS.  
  
Se, ad esempio, l'identificatore del relying party nel database di configurazione AD FS \(URI1\) è un prefisso per l'identificatore relying party nella richiesta in ingresso \(URI2\), è necessario che siano soddisfatte le condizioni seguenti:  
  
-   I delimitatori finali \(le barre e i due punti\) delle sezioni di percorso o delle autorità devono essere ignorati  
  
-   Le parti di schema e autorità di URI1 e URI2 devono essere una corrispondenza esatta senza distinzione tra maiuscole e minuscole  
  
-   Ogni sezione di percorso di URI1 deve essere una corrispondenza esatta \(in base alla distinzione tra maiuscole e minuscole scelta\) alla sezione di percorso corrispondente di URI2  
  
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
|http:\/\/contoso.com|http:\/\/contoso.com\/HR|TRUE|URI1 non ha alcun percorso e corrisponde allo schema e all'autorità di URI2|  
|http:\/\/contoso.com\/HR|http:\/\/contoso.com\/HR\/Web|TRUE|Le prime sezioni di percorso corrispondono, URI1 non ha una seconda sezione di percorso|  
|http:\/\/contoso.com\/HR|http:\/\/contoso.com\/HR\/Web\/? m\=t|TRUE|Gli stessi motivi indicati sopra, la stringa di query non modifica nulla|  
|http:\/\/contoso.com\/HR\/|http:\/\/contoso.com\/lavoro ibrido\/Main|FALSE|La sezione di percorso 1 di URI1 non corrisponde alla sezione di percorso 1 di URI2|  
|http:\/\/contoso.com\/HR|http:\/\/contoso.com|FALSE|URI1 ha più sezioni di percorso di URI2|  
|http:\/\/contoso.com\/HR|http:\/\/contoso.com\/HRweb|FALSE|Le prime sezioni di percorso non corrispondono|  
|http:\/\/contoso.com\/? m\=t|http:\/\/contoso.com\/? m\=f|FALSE|Le parti di stringa di query non corrispondono|  
|https:\/\/contoso.com|http:\/\/contoso.com|FALSE|Le parti di schema non corrispondono|  
|http:\/\/sts.contoso.com|http:\/\/contoso.com|FALSE|Le parti di autorità non corrispondono|  
|http:\/\/contoso.com|http:\/\/sts.contoso.com|FALSE|Le parti di autorità non corrispondono|  
  

