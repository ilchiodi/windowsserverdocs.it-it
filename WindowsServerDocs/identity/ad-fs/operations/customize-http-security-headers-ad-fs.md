---
title: Personalizzare intestazioni della risposta HTTP protezione con AD FS
description: In questo documento descirbes come personalizzare le intestazioni di sicurezza contro le vulnerabilità di sicurezza.
author: billmath
ms.author: billmath
manager: daveba
ms.reviewer: akgoel23
ms.date: 02/19/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cd3ad4e6547194a971d8a51ecb95ee56f5e4e8c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822722"
---
# <a name="customize-http-security-response-headers-with-ad-fs-2019"></a>Personalizzare intestazioni della risposta HTTP protezione con AD FS 2019 
Si applica a: Windows Server 2019 
 
Per proteggersi da vulnerabilità della sicurezza comuni e offrono agli amministratori la possibilità di sfruttare i miglioramenti più recenti di meccanismi di protezione basata su browser, AD FS 2019 aggiunto la funzionalità per personalizzare le intestazioni di risposta di sicurezza HTTP inviato da ADFS. Questa operazione viene eseguita con l'introduzione di due nuovi cmdlet: `Get-AdfsResponseHeaders` e `Set-AdfsResponseHeaders`.  
 
In questo documento che verranno illustrati comunemente usato le intestazioni di risposta di sicurezza per illustrare come personalizzare le intestazioni inviate da AD FS 2019.   
 
>[!NOTE]
>Il documento si presuppone che sia stato installato AD FS 2019.  

 
Prima di illustrare le intestazioni, è possibile esaminare alcuni scenari, creando così l'esigenza per gli amministratori di personalizzare le intestazioni di sicurezza 
 
## <a name="scenarios"></a>Scenari 
1. Amministratore ha abilitato [ **HTTP Strict-Transport-Security (HSTS)** ](#http-strict-transport-security-hsts) (forza tutte le connessioni tramite crittografia HTTPS) per proteggere gli utenti che potrebbero accedere all'app web tramite HTTP da un accesso Wi-Fi pubblico punto che potrebbe essere soggetta ad attacchi. Anna vuole rafforzare ulteriormente la sicurezza abilitando HSTS per sottodomini.  
2. L'amministratore ha configurato il [ **X-Frame-Options** ](#x-frame-options) intestazione risposta (impedisce il rendering di qualsiasi pagina web in un iFrame) per proteggere le pagine web venga clickjacked. Tuttavia, Anna dovrà personalizzare il valore dell'intestazione a causa di un nuovo requisito di business per visualizzare i dati (in iFrame) da un'applicazione con un'origine diversa (dominio).
3. Amministratore ha abilitato [ **X-XSS-Protection** ](#x-xss-protection) (impedisce tra gli attacchi di scripting) per purificare e bloccare la pagina se viene rilevato dal browser tra gli attacchi di scripting. Tuttavia, necessarie personalizzare l'intestazione per consentire il caricamento di pagina una volta ripulito.  
4. Amministratore deve abilitare [ **Origin Resource Sharing CORS (Cross)** ](#cross-origin-resource-sharing-cors-headers) e impostare l'origine (dominio) per AD FS per consentire un'applicazione a pagina singola accedere a un'API web con un altro dominio.  
5. Amministratore ha abilitato [ **Content Security Policy (CSP)** ](#content-security-policy-csp) intestazione per impedire cross site scripting e dati injection attacchi disabilitando le richieste tra domini. Tuttavia, a causa di un nuovo requisito di business Anna dovrà personalizzare l'intestazione per consentire la pagina web caricare le immagini da qualsiasi origine e limitare supporti a un provider attendibile.  

 
## <a name="http-security-response-headers"></a>Intestazioni di risposta di sicurezza HTTP 
Le intestazioni di risposta sono inclusi nella risposta HTTP in uscita inviata da AD FS in un web browser. Le intestazioni possono essere elencate usando la `Get-AdfsResponseHeaders` cmdlet come illustrato di seguito.  

![Risposta di intestazione](media\customize-http-security-headers-ad-fs\header1.png)

Il `ResponseHeaders` nello screenshot precedente che identifica le intestazioni di sicurezza che verranno incluso da AD FS in ogni risposta HTTP. Le intestazioni di risposta verranno inviate solo se `ResponseHeadersEnabled` è impostata su `True` (valore predefinito). Il valore può essere impostato su `False` per impedire qualsiasi delle intestazioni di sicurezza incluso nella risposta HTTP di AD FS. Tuttavia questa operazione è sconsigliata.  A scopo, utilizzare la seguente:

```PowerShell
Set-AdfsResponseHeaders -EnableResponseHeaders $false
```
 
### <a name="http-strict-transport-security-hsts"></a>HTTP Strict-Transport-Security (HSTS) 
HSTS è un meccanismo di criteri di sicurezza web che consente di attenuare gli attacchi di protocollo downgrade e Hijack del cookie per i servizi che hanno endpoint HTTP e HTTPS. Consente i server web dichiarare che i browser web (o altri agenti utente a soddisfare) devono solo interagire con questa usando HTTPS e mai tramite il protocollo HTTP.  
 
Tutti gli endpoint AD FS per il traffico di autenticazione web aperti esclusivamente tramite HTTPS. Di conseguenza, AD FS in modo efficace consente di ridurre le minacce che fornisce il meccanismo di criterio HTTP Strict Transport Security (per impostazione predefinita non vi è alcun effettua il downgrade a HTTP poiché non sono presenti listener HTTP). L'intestazione può essere personalizzato impostando i parametri seguenti 
 
- **max-age =&lt;fase scadono&gt;**  : specifica l'ora di scadenza, in secondi, quanto tempo il sito debba essere accessibili solo tramite HTTPS. Valore predefinito e consigliato è 31536000 secondi (1 anno).  
- **includeSubDomains** – si tratta di un parametro facoltativo. Se specificato, la regola HSTS si applica a tutti i sottodomini.  
 
#### <a name="hsts-customization"></a>Personalizzazione HSTS 
Per impostazione predefinita, l'intestazione è attivata e `max-age` impostato su 1 anno; tuttavia, gli amministratori possono modificare il `max-age` (riduzione valore max-age viene scelta non consigliata) o abilitare HSTS per sottodomini attraverso il `Set-AdfsResponseHeaders` cmdlet.  
 
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Strict-Transport-Security" -SetHeaderValue "max-age=<seconds>; includeSubDomains" 
``` 

Esempio: 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Strict-Transport-Security" -SetHeaderValue "max-age=31536000; includeSubDomains" 
 ```

Per impostazione predefinita, l'intestazione è incluso nel `ResponseHeaders` dell'attributo; tuttavia, gli amministratori possono rimuovere l'intestazione tramite il `Set-AdfsResponseHeaders` cmdlet.  
 
```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "Strict-Transport-Security" 
```

### <a name="x-frame-options"></a>X-Frame-Options 
AD FS per impostazione predefinita non consente le applicazioni esterne usare gli IFRAME durante l'esecuzione di account di accesso interattivo. Questa operazione viene eseguita per impedire che uno stile di attacchi di phishing. Si noti che gli account di accesso interattivo può essere eseguita tramite iFrame a causa di sicurezza a livello di sessione precedente è stata stabilita.  
 
Tuttavia, in alcuni casi rari si potrebbe considera attendibile una specifica applicazione che richiede la pagina di accesso di iFrame in grado di supportare interattivi AD FS. L'intestazione 'X-Frame-Options' viene usato per questo scopo.  
 
Questa intestazione di risposta di sicurezza HTTP viene utilizzata per comunicare al browser se è possibile eseguire il rendering di una pagina in un &lt;frame&gt;/&lt;iframe&gt;. L'intestazione può essere impostata su uno dei valori seguenti: 
 
- **negare** – non verrà visualizzata la pagina in un frame. Questo è il valore predefinito e impostazione consigliata.  
- **sameorigin** : la pagina verrà visualizzata solo nel frame se l'origine è quello utilizzato per l'origine della pagina web. L'opzione non è molto utile, a meno che non tutti i predecessori sono riportate anche nella stessa origine.  
- **Consenti da <specified origin>**  -la pagina verrà visualizzata solo nel frame se l'origine (ad esempio, https://www. ". COM) trova la corrispondenza di origine specifici nell'intestazione. 

#### <a name="x-frame-options-customization"></a>Personalizzazione di X-Frame-Options  
Per impostazione predefinita, imposterà intestazione negare; Tuttavia, gli amministratori possono modificare il valore tramite il `Set-AdfsResponseHeaders` cmdlet.  
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-Frame-Options" -SetHeaderValue "<deny/sameorigin/allow-from<specified origin>>" 
 ```

Esempio: 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-Frame-Options" -SetHeaderValue "allow-from https://www.example.com" 
 ```

Per impostazione predefinita, l'intestazione è incluso nel `ResponseHeaders` dell'attributo; tuttavia, gli amministratori possono rimuovere l'intestazione tramite il `Set-AdfsResponseHeaders` cmdlet.  

```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "X-Frame-Options" 
```

### <a name="x-xss-protection"></a>X-XSS-Protection 
Questa intestazione di risposta di sicurezza HTTP viene utilizzata per interrompere le pagine web venga caricato quando vengono rilevati attacchi cross-site scripting (XSS) dal browser. Ciò viene definito il filtro XSS. L'intestazione può essere impostata su uno dei valori seguenti 
 
- **0** : il filtro XSS disabilita. Non è consigliata.  
- **1** : il filtro XSS consente. Se viene rilevato attacco XSS, browser purifica la pagina.   
- **1. modalità = blocco** : il filtro XSS consente. Se viene rilevato attacco XSS, browser impedisce il rendering della pagina. Questo è il valore predefinito e impostazione consigliata.  

#### <a name="x-xss-protection-customization"></a>Personalizzazione di X-XSS-Protection 
Per impostazione predefinita, l'intestazione verrà impostata su 1. modalità = blocco; Tuttavia, gli amministratori possono modificare il valore tramite il `Set-AdfsResponseHeaders` cmdlet.  

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-XSS-Protection" -SetHeaderValue "<0/1/1; mode=block/1; report=<reporting-uri>>" 
``` 

Esempio: 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-XSS-Protection" -SetHeaderValue "1" 
 ```

Per impostazione predefinita, l'intestazione è incluso nel `ResponseHeaders` dell'attributo; tuttavia, gli amministratori possono rimuovere l'intestazione tramite il `Set-AdfsResponseHeaders` cmdlet. 

```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "X-XSS-Protection" 
```

### <a name="cross-origin-resource-sharing-cors-headers"></a>Intestazioni di cross-Origin Resource Sharing (CORS) 
Protezione del browser Web impedisce l'esecuzione di richieste multiorigine avviate da script una pagina web. Tuttavia, in alcuni casi è possibile accedere alle risorse di altre origini (domini). CORS è un standard W3C che consente a un server di ridurre i criteri di corrispondenza dell'origine. Con CORS un server può consentire in modo esplicito alcune richieste multiorigine e rifiutarne altre.  
 
Per comprendere meglio richiesta CORS, è possibile walkthrough uno scenario in cui una singola pagina (SPA) dell'applicazione deve chiamare un'API web con un dominio diverso. Inoltre, si consideri che SPA e API sono entrambi configurati in ad FS 2019 e AD FS è abilitato per CORS, ad esempio AD FS possono identificare le intestazioni CORS nella richiesta HTTP, convalidare i valori di intestazione e includere intestazioni CORS appropriate nella risposta (informazioni dettagliate su come abilitare e configurare CORS in AD FS 2019 nella sezione CORS personalizzazione più avanti). Flusso di esempio: 

1. Utente accede SPA tramite browser client e viene reindirizzato all'endpoint di autenticazione di AD FS per l'autenticazione. Poiché l'applicazione a singola pagina è configurato per il flusso di concessione implicita, richiedere token restituisce un accesso + ID al browser al termine dell'autenticazione.  
2. Dopo l'autenticazione utente, il front-end JavaScript inclusi in SPA effettua una richiesta per accedere all'API web. La richiesta viene reindirizzata ad ADFS con le intestazioni seguenti
    - Le opzioni – descrive le opzioni di comunicazione per la risorsa di destinazione 
    - Origine: include l'origine dell'API web
    - Access-Control-Request-Method: identifica il metodo HTTP (ad esempio, eliminare) da utilizzare quando viene effettuata la richiesta effettiva 
    - Access-Control-Request-Headers - identifica le intestazioni HTTP da utilizzare quando viene effettuata la richiesta effettiva 
    
   >[!NOTE]
   >Richiesta CORS è simile a una richiesta HTTP standard, tuttavia, la presenza di un'intestazione di origine segnala la richiesta in ingresso correlate CORS. 
3. AD FS verifica che l'origine di API incluso nell'intestazione web sia elencato nelle origini attendibili configurate in AD FS (informazioni dettagliate su come modificare le origini attendibili nella sezione CORS personalizzazione più avanti). ADFS risponde quindi con intestazioni seguenti.  
    - Access-Control-Allow-Origin: stesso valore come intestazione di origine 
    - Access-Control-Consenti-Method-stesso valore come intestazione Access-Control-Request-Method 
    - Access-Control-Consenti-Headers - stesso valore come intestazione Access-Control-Request-Headers 
4. Browser invia la richiesta effettiva includendo le intestazioni seguenti 
    - Metodo HTTP (ad esempio, l'eliminazione) 
    - Origine: include l'origine dell'API web 
    - Tutte le intestazioni incluse nell'intestazione della risposta Access-Control-Consenti-Headers 
5. Al termine della verifica, AD FS approva la richiesta, includendo il dominio di API web (origine) nell'intestazione della risposta Access-Control-Allow-Origin.  
6. L'inclusione dell'intestazione Access-Control-Allow-Origin consentirà il browser andare avanti con la chiamata dell'API richiesto.

#### <a name="cors-customization"></a>Personalizzazione di CORS 
Per impostazione predefinita, non sarà possibile abilitare funzionalità CORS; Tuttavia, gli amministratori possono abilitare la funzionalità tramite il cmdlet Set-AdfsResponseHeaders.  

```PowerShell 
Set-AdfsResponseHeaders -EnableCORS $true 
 ```

Uno è abilitata, gli amministratori saranno in grado di enumerare un elenco delle origini attendibili usando lo stesso cmdlet. Il comando seguente consente ad esempio, le richieste CORS da origini **https&#58;//example1.com** e **https&#58;//example1.com**. 
 
```PowerShell
Set-AdfsResponseHeaders -CORSTrustedOrigins https://example1.com,https://example2.com 
 ```

> [!NOTE]
> Gli amministratori possono consentire le richieste CORS da qualsiasi origine, includendo "*" nell'elenco delle origini attendibili, sebbene questo approccio non è consigliato a causa di vulnerabilità della sicurezza e un messaggio di avviso viene fornito se scelgono di. 

### <a name="content-security-policy-csp"></a>Criteri di protezione del contenuto (CSP) 
Questa intestazione di risposta di sicurezza HTTP viene utilizzata per evitare il cross-site scripting, clickjacking e altri attacchi intrusivi nel codice dei dati impedendo browser inavvertitamente l'esecuzione di contenuto dannoso. I browser che non supportano CSP ignora semplicemente le intestazioni di risposta CSP.  
 
#### <a name="csp-customization"></a>Personalizzazione di CSP 
Personalizzazione dell'intestazione CSP comporta la modifica i criteri di sicurezza che definisce il browser alle risorse sono consentito il caricamento della pagina web. I criteri di sicurezza predefiniti  
 
`Content-Security-Policy: default-src ‘self’ ‘unsafe-inline’ ‘’unsafe-eval’; img-src ‘self’ data:;` 
 
Il **predefinito-src** direttiva viene usata per modificare [src - direttive](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/default-src) senza che vengano elencati in modo esplicito ogni direttiva. Nell'esempio seguente il criterio, ad esempio, 1 è uguale a 2 il criterio.  

Criteri di 1 
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "default-src 'self'" 
```
 
Policy 2
```PowerShell 
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "script-src ‘self’; img-src ‘self’; font-src 'self';  
frame-src 'self'; manifest-src 'self'; media-src 'self';" 
```

Se una direttiva è elencata in modo esplicito, il valore specificato sostituisce il valore specificato per impostazione predefinita-src. Nell'esempio seguente, l'elemento img-src avrà il valore come ' *' (consentendo le immagini devono essere caricati da qualsiasi origine) mentre altre direttive - src richiederà il valore di 'self' (che limita alla stessa origine della pagina web).  

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "default-src ‘self’; img-src *" 
```
Le seguenti origini possono essere definite per il criterio predefinito-src 
 
- 'self': se si specifica questo limita l'origine del contenuto da caricare all'origine della pagina web 
- 'unsafe inline': se si specifica questo nel criterio consente l'uso di codice inline JavaScript e CSS 
- 'unsafe eval': se si specifica questo nel criterio consente l'uso di testo a JavaScript meccanismi come eval 
- 'none': se si specifica questo limita il contenuto da qualsiasi origine da caricare 
- dati:-specificare i dati: Gli URI consente ai creatori di contenuti incorporare i file di piccole dimensioni inline nei documenti. Utilizzo non consigliato.  
 
>[!NOTE]
>AD FS Usa JavaScript nel processo di autenticazione e di conseguenza JavaScript includendo 'unsafe inline' e 'unsafe eval' le origini predefinite dei criteri.  

### <a name="custom-headers"></a>Intestazioni personalizzate 
Oltre a quanto sopra elencate le intestazioni di risposta di sicurezza (HSTS, CSP, X-Frame-Options, X-XSS-Protection e la condivisione CORS), AD FS 2019 offre la possibilità di impostare nuove intestazioni.  
 
Esempio: Per impostare una nuova intestazione "TestHeader" con valore come "TestHeaderValue" 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "TestHeader" -SetHeaderValue "TestHeaderValue" 
 ```

Una volta impostato, la nuova intestazione viene inviata in risposta AD FS (fiddler frammento di codice riportato di seguito).  
 
![Fiddler](media\customize-http-security-headers-ad-fs\header2.png)

## <a name="web-browswer-compatibility"></a>Compatibilità browswer Web
Usare la tabella e i collegamenti seguenti per determinare quali browser web sono compatibili con ognuna delle intestazioni di risposta di sicurezza.

|Intestazioni di risposta di sicurezza HTTP|Compatibilità browser|
|-----|-----|
|HTTP Strict-Transport-Security (HSTS)|[Compatibilità del browser HSTS](https://developer.mozilla.org/docs/Web/HTTP/Headers/Strict-Transport-Security#Browser_compatibility)|
|X-Frame-Options|[Compatibilità tra browser X-Frame-Options](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options#Browser_compatibility)| 
|X-XSS-Protection|[Compatibilità tra browser X-XSS-Protection](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-XSS-Protection#Browser_compatibility)| 
|Cross Origin Resource Sharing (CORS)|[Compatibilità del browser CORS](https://developer.mozilla.org/docs/Web/HTTP/CORS#Browser_compatibility) 
|Criteri di protezione del contenuto (CSP)|[Compatibilità del browser CSP](https://developer.mozilla.org/docs/Web/HTTP/CSP#Browser_compatibility) 

## <a name="next"></a>Avanti

- [Usare le guide troublehshooting Guida di ADFS](https://aka.ms/adfshelp/troubleshooting )
- [Risoluzione dei problemi di AD FS](../../ad-fs/troubleshooting/ad-fs-tshoot-overview.md)
