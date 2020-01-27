---
title: Personalizzare le intestazioni di risposta di sicurezza HTTP con AD FS
description: Questo documento descirbes come personalizzare le intestazioni di sicurezza per la protezione da vulnerabilità di sicurezza.
author: billmath
ms.author: billmath
manager: daveba
ms.reviewer: akgoel23
ms.date: 02/19/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b81d498c6e601fcce0a0760cb4877fcc98c8beb9
ms.sourcegitcommit: ff0db5ca093a31034ccc5e9156f5e9b45b69bae5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725796"
---
# <a name="customize-http-security-response-headers-with-ad-fs-2019"></a>Personalizzare le intestazioni di risposta di sicurezza HTTP con AD FS 2019 
 
Per proteggersi da vulnerabilità di sicurezza comuni e fornire agli amministratori la possibilità di sfruttare i miglioramenti più recenti dei meccanismi di protezione basati su browser, AD FS 2019 ha aggiunto la funzionalità per personalizzare le intestazioni di risposta di sicurezza HTTP inviato dal AD FS. Questa operazione viene eseguita tramite l'introduzione di due nuovi cmdlet: `Get-AdfsResponseHeaders` e `Set-AdfsResponseHeaders`.  

>[!NOTE]
>Funzionalità per personalizzare le intestazioni di risposta di sicurezza HTTP (eccetto le intestazioni CORS) tramite i cmdlet: `Get-AdfsResponseHeaders` e `Set-AdfsResponseHeaders` sono state sottoposte a backporting AD FS 2016. È possibile aggiungere la funzionalità alla AD FS 2016 installando [KB4493473](https://support.microsoft.com/help/4493473/windows-10-update-kb4493473) e [KB4507459](https://support.microsoft.com/help/4507459/windows-10-update-kb4507459). 

In questo documento vengono illustrate le intestazioni di risposta di sicurezza usate di frequente per dimostrare come personalizzare le intestazioni inviate da AD FS 2019.   
 
>[!NOTE]
>Nel documento si presuppone che sia stato installato AD FS 2019.  

 
Prima di illustrare le intestazioni, verranno esaminati alcuni scenari in cui viene creata la necessità di amministratori di personalizzare le intestazioni di sicurezza 
 
## <a name="scenarios"></a>Scenari 
1. L'amministratore ha abilitato [**http Strict-Transport-Security (HSTS)** ](#http-strict-transport-security-hsts) (forza tutte le connessioni sulla crittografia HTTPS) per proteggere gli utenti che possono accedere all'app Web tramite HTTP da un punto di accesso Wi-Fi pubblico che può essere hackerato. Si vuole rafforzare ulteriormente la sicurezza abilitando HSTS per i sottodomini.  
2. L'amministratore ha configurato l'intestazione della risposta [**X-frame-options**](#x-frame-options) (impedisce il rendering di qualsiasi pagina Web in un iframe) per proteggere le pagine Web da clickjacked. Tuttavia, devono personalizzare il valore dell'intestazione a causa di un nuovo requisito aziendale per visualizzare i dati (in iFrame) da un'applicazione con un'origine (dominio) diversa.
3. L'amministratore ha abilitato [**X-XSS-Protection**](#x-xss-protection) (impedisce gli attacchi tra script) per purificare e bloccare la pagina se il browser rileva attacchi tra script. Tuttavia, devono personalizzare l'intestazione per consentire il caricamento della pagina una volta purificata.  
4. L'amministratore deve abilitare la [**condivisione di risorse tra le origini (CORS)** ](#cross-origin-resource-sharing-cors-headers) e impostare l'origine (dominio) in ad FS per consentire a un'applicazione a pagina singola di accedere a un'API Web con un altro dominio.  
5. L'amministratore ha abilitato l'intestazione del [**criterio di sicurezza del contenuto (CSP)** ](#content-security-policy-csp) per evitare attacchi di scripting tra siti e attacchi di dati, non consentendo richieste tra domini. Tuttavia, a causa di un nuovo requisito aziendale, è necessario personalizzare l'intestazione per consentire alla pagina Web di caricare le immagini da qualsiasi origine e limitare i supporti ai provider attendibili.  

 
## <a name="http-security-response-headers"></a>Intestazioni di risposta di sicurezza HTTP 
Le intestazioni di risposta sono incluse nella risposta HTTP in uscita inviata da AD FS a un Web browser. Le intestazioni possono essere elencate utilizzando il cmdlet `Get-AdfsResponseHeaders` come illustrato di seguito.  

![Risposta intestazione](media/customize-http-security-headers-ad-fs/header1.png)

L'attributo `ResponseHeaders` nella schermata precedente identifica le intestazioni di sicurezza che verranno incluse AD FS in ogni risposta HTTP. Le intestazioni di risposta verranno inviate solo se `ResponseHeadersEnabled` è impostato su `True` (valore predefinito). Il valore può essere impostato su `False` per impedire AD FS incluse le intestazioni di sicurezza nella risposta HTTP. Questa operazione non è tuttavia consigliata.  A tale scopo, utilizzare quanto segue:

```PowerShell
Set-AdfsResponseHeaders -EnableResponseHeaders $false
```
 
### <a name="http-strict-transport-security-hsts"></a>HTTP Strict-Transport-Security (HSTS) 
HSTS è un meccanismo di criteri di sicurezza Web che consente di attenuare gli attacchi di downgrade del protocollo e il Hijack dei cookie per i servizi con endpoint HTTP e HTTPS. Consente ai server Web di dichiarare che i Web browser (o altri agenti utente conformi) devono interagire con esso solo usando HTTPS e mai tramite il protocollo HTTP.  
 
Tutti gli endpoint AD FS per il traffico di autenticazione Web vengono aperti esclusivamente tramite HTTPS. Di conseguenza, AD FS Attenua efficacemente le minacce fornite dal meccanismo del criterio di sicurezza del trasporto HTTP Strict (per impostazione predefinita, non esiste alcun downgrade a HTTP poiché non sono presenti listener in HTTP). L'intestazione può essere personalizzata impostando i parametri seguenti:
 
- **Max-Age =&lt;scadenza-ora&gt;** : la scadenza (in secondi) specifica per quanto tempo il sito deve essere accessibile solo tramite HTTPS. Il valore predefinito e consigliato è 31536000 secondi (1 anno).  
- **includeSubDomains** : parametro facoltativo. Se specificato, la regola HSTS si applica anche a tutti i sottodomini.  
 
#### <a name="hsts-customization"></a>Personalizzazione di HSTS 
Per impostazione predefinita, l'intestazione è abilitata e `max-age` impostata su 1 anno; Tuttavia, gli amministratori possono modificare la `max-age` (non è consigliabile abbassare il valore max-age) o abilitare HSTS per i sottodomini tramite il cmdlet `Set-AdfsResponseHeaders`.  
 
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Strict-Transport-Security" -SetHeaderValue "max-age=<seconds>; includeSubDomains" 
``` 

Esempio: 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Strict-Transport-Security" -SetHeaderValue "max-age=31536000; includeSubDomains" 
 ```

Per impostazione predefinita, l'intestazione è inclusa nell'attributo `ResponseHeaders`; Tuttavia, gli amministratori possono rimuovere l'intestazione tramite il cmdlet `Set-AdfsResponseHeaders`.  
 
```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "Strict-Transport-Security" 
```

### <a name="x-frame-options"></a>X-frame-options 
AD FS per impostazione predefinita non consente alle applicazioni esterne di utilizzare iFrame quando si eseguono accessi interattivi. Questa operazione viene eseguita per evitare determinati stili di attacchi di phishing. Si noti che gli account di accesso non interattivi possono essere eseguiti tramite iFrame a causa della sicurezza a livello di sessione precedente stabilita.  
 
Tuttavia, in alcuni casi rari è possibile considerare attendibile un'applicazione specifica che richiede la pagina di accesso interattiva abilitata per iFrame AD FS. A questo scopo viene utilizzata l'intestazione ' X-frame-options '.  
 
Questa intestazione della risposta di sicurezza HTTP viene usata per comunicare con il browser se può eseguire il rendering di una pagina in un frame &lt;&gt;/&lt;iframe&gt;. L'intestazione può essere impostata su uno dei valori seguenti: 
 
- **Deny** : la pagina in un frame non verrà visualizzata. Si tratta dell'impostazione predefinita e consigliata.  
- **SAMEORIGIN** : la pagina verrà visualizzata nel frame solo se l'origine è la stessa dell'origine della pagina Web. L'opzione non è molto utile a meno che tutti i predecessori si trovino anche nella stessa origine.  
- **Consenti-da <specified origin>** : la pagina verrà visualizzata solo nel frame se l'origine, ad esempio https://www. ". com) corrisponde all'origine specifica nell'intestazione. 

#### <a name="x-frame-options-customization"></a>Personalizzazione X-frame-options  
Per impostazione predefinita, l'intestazione verrà impostata su Nega; Tuttavia, gli amministratori possono modificare il valore tramite il cmdlet `Set-AdfsResponseHeaders`.  
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-Frame-Options" -SetHeaderValue "<deny/sameorigin/allow-from<specified origin>>" 
 ```

Esempio: 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-Frame-Options" -SetHeaderValue "allow-from https://www.example.com" 
 ```

Per impostazione predefinita, l'intestazione è inclusa nell'attributo `ResponseHeaders`; Tuttavia, gli amministratori possono rimuovere l'intestazione tramite il cmdlet `Set-AdfsResponseHeaders`.  

```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "X-Frame-Options" 
```

### <a name="x-xss-protection"></a>X-XSS-Protection 
Questa intestazione della risposta di sicurezza HTTP viene utilizzata per arrestare il caricamento delle pagine Web quando vengono rilevati attacchi di scripting tra siti (XSS) dai browser. Questa operazione viene definita filtro XSS. L'intestazione può essere impostata su uno dei valori seguenti:
 
- **0** : Disabilita il filtro XSS. Non consigliato.  
- **1** : Abilita il filtro XSS. Se viene rilevato un attacco XSS, la pagina verrà purificata dal browser.   
- **1; mode = block** : Abilita il filtro XSS. Se viene rilevato un attacco XSS, il browser eviterà il rendering della pagina. Si tratta dell'impostazione predefinita e consigliata.  

#### <a name="x-xss-protection-customization"></a>Personalizzazione X-XSS-Protection 
Per impostazione predefinita, l'intestazione verrà impostata su 1; Mode = blocco; Tuttavia, gli amministratori possono modificare il valore tramite il cmdlet `Set-AdfsResponseHeaders`.  

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-XSS-Protection" -SetHeaderValue "<0/1/1; mode=block/1; report=<reporting-uri>>" 
``` 

Esempio: 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-XSS-Protection" -SetHeaderValue "1" 
 ```

Per impostazione predefinita, l'intestazione è inclusa nell'attributo `ResponseHeaders`; Tuttavia, gli amministratori possono rimuovere l'intestazione tramite il cmdlet `Set-AdfsResponseHeaders`. 

```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "X-XSS-Protection" 
```

### <a name="cross-origin-resource-sharing-cors-headers"></a>Intestazioni di condivisione risorse tra le origini (CORS) 
Web browser sicurezza impedisce a una pagina Web di effettuare richieste tra le origini dall'interno degli script. Tuttavia, a volte potrebbe essere necessario accedere alle risorse in altre origini (domini). CORS è uno standard W3C che consente a un server di ridurre i criteri della stessa origine. Usando CORS, un server può consentire in modo esplicito alcune richieste tra origini e rifiutare altre.  
 
Per comprendere meglio la richiesta CORS, viene illustrato uno scenario in cui un'applicazione a pagina singola (SPA) deve chiamare un'API Web con un dominio diverso. Inoltre, si consideri che l'API e la SPA sono configurate in ADFS 2019 e AD FS è abilitato CORS, ad esempio AD FS possibile identificare le intestazioni CORS nella richiesta HTTP, convalidare i valori di intestazione e includere le intestazioni CORS appropriate nella risposta (informazioni dettagliate su come abilitare e configurare CORS in AD FS 2019 nella sezione relativa alla personalizzazione di CORS riportata di seguito). Flusso di esempio: 

1. L'utente accede a SPA tramite il browser client e viene reindirizzato a AD FS endpoint auth per l'autenticazione. Poiché SPA è configurato per il flusso di concessione implicito, Request restituisce un token di accesso + ID al browser dopo la corretta autenticazione.  
2. Dopo l'autenticazione dell'utente, il codice JavaScript front-end incluso in SPA effettua una richiesta di accesso all'API Web. La richiesta viene reindirizzata a AD FS con le intestazioni seguenti:
    - Opzioni: descrive le opzioni di comunicazione per la risorsa di destinazione 
    - Origin: include l'origine dell'API Web
    - Access-Control-request-method: identifica il metodo HTTP (ad esempio, DELETE) da usare quando viene effettuata la richiesta effettiva 
    - Access-Control-Request-Headers: identifica le intestazioni HTTP da usare quando viene effettuata la richiesta effettiva 
    
   >[!NOTE]
   >La richiesta CORS è simile a una richiesta HTTP standard, tuttavia la presenza di un'intestazione di origine segnala che la richiesta in ingresso è correlata a CORS. 
3. AD FS verifica che l'origine dell'API Web inclusa nell'intestazione sia elencata nelle origini attendibili configurate in AD FS (informazioni dettagliate su come modificare le origini attendibili nella sezione relativa alla personalizzazione di CORS di seguito). AD FS risponde quindi con le intestazioni seguenti:  
    - Access-Control-Allow-Origin: valore uguale a quello dell'intestazione Origin 
    - Access-Control-Allow-Method: valore uguale a quello dell'intestazione Access-Control-request-method 
    - Access-Control-Allow-Headers-Value uguale a quello dell'intestazione Access-Control-Request-Headers 
4. Browser invia la richiesta effettiva, incluse le intestazioni seguenti:
    - Metodo HTTP (ad esempio, DELETE) 
    - Origin: include l'origine dell'API Web 
    - Tutte le intestazioni incluse nell'intestazione della risposta Access-Control-Allow-Headers 
5. Una volta verificata, AD FS approva la richiesta includendo il dominio dell'API Web (Origin) nell'intestazione della risposta Access-Control-Allow-Origin.  
6. L'inclusione dell'intestazione Access-Control-Allow-Origin consentirà al browser di proseguire con la chiamata all'API richiesta.

#### <a name="cors-customization"></a>Personalizzazione di CORS 
Per impostazione predefinita, la funzionalità CORS non verrà abilitata; Tuttavia, gli amministratori possono abilitare la funzionalità tramite il cmdlet Set-AdfsResponseHeaders.  

```PowerShell 
Set-AdfsResponseHeaders -EnableCORS $true 
 ```

Una abilitata, gli amministratori saranno in grado di enumerare un elenco di origini attendibili usando lo stesso cmdlet. Ad esempio, il comando seguente consente le richieste CORS dalle origini **https&#58;//Example1.com** e **https&#58;//Example1.com**. 
 
```PowerShell
Set-AdfsResponseHeaders -CORSTrustedOrigins https://example1.com,https://example2.com 
 ```

> [!NOTE]
> Gli amministratori possono consentire le richieste CORS da qualsiasi origine includendo "*" nell'elenco delle origini attendibili, anche se questo approccio non è consigliato a causa di vulnerabilità della sicurezza e viene visualizzato un messaggio di avviso se scelgono di. 

### <a name="content-security-policy-csp"></a>Criteri di sicurezza del contenuto (CSP) 
Questa intestazione della risposta di sicurezza HTTP viene usata per impedire l'esecuzione di script tra siti, Clickjacking e altri attacchi di data Injection impedendo ai browser di eseguire inavvertitamente contenuti dannosi. I browser che non supportano CSP ignorano semplicemente le intestazioni di risposta CSP.  
 
#### <a name="csp-customization"></a>Personalizzazione CSP 
La personalizzazione dell'intestazione CSP comporta la modifica dei criteri di sicurezza che definiscono il browser risorse che è autorizzato a caricare per la pagina Web. I criteri di sicurezza predefiniti sono  
 
`Content-Security-Policy: default-src ‘self' ‘unsafe-inline' ‘'unsafe-eval'; img-src ‘self' data:;` 
 
La direttiva **default-src** viene usata per modificare le [direttive-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/default-src) senza elencare in modo esplicito ogni direttiva. Nell'esempio seguente, ad esempio, il criterio 1 corrisponde al criterio 2.  

Criteri 1 
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "default-src 'self'" 
```
 
Criterio 2
```PowerShell 
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "script-src ‘self'; img-src ‘self'; font-src 'self';  
frame-src 'self'; manifest-src 'self'; media-src 'self';" 
```

Se una direttiva è elencata in modo esplicito, il valore specificato sostituisce il valore specificato per default-src. Nell'esempio seguente, img-src accetta il valore come ' *' (consentendo il caricamento delle immagini da qualsiasi origine) mentre altre direttive src accettano il valore come ' self ' (limitandosi alla stessa origine della pagina Web).  

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "default-src ‘self'; img-src *" 
```
È possibile definire le origini seguenti per i criteri src predefiniti:
 
- ' self ': la specifica di questa opzione consente di limitare l'origine del contenuto da caricare nell'origine della pagina Web 
- ' unsafe-inline ': se si specifica questo criterio, è possibile usare JavaScript e CSS inline 
- ' unsafe-eval ': se si specifica questo criterio, è possibile usare il testo per i meccanismi JavaScript come EVAL 
- ' none ': se si specifica questo valore, il contenuto di qualsiasi origine da caricare verrà limitato 
- Data:-specifica dei dati: gli URI consentono agli autori di contenuti di incorporare piccoli file inline nei documenti. Utilizzo non consigliato.  
 
>[!NOTE]
>AD FS usa JavaScript nel processo di autenticazione e pertanto Abilita JavaScript includendo le origini ' unsafe-inline ' è unsafe-eval ' nei criteri predefiniti.  

### <a name="custom-headers"></a>Intestazioni personalizzate 
Oltre alle intestazioni di risposta di sicurezza elencate in precedenza (HSTS, CSP, X-frame-options, X-XSS-Protection e CORS), AD FS 2019 fornisce la possibilità di impostare nuove intestazioni.  
 
Esempio: per impostare una nuova intestazione "TestHeader" con il valore "TestHeaderValue" 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "TestHeader" -SetHeaderValue "TestHeaderValue" 
 ```

Una volta impostato, la nuova intestazione viene inviata nella risposta AD FS (frammento di Fiddler riportato di seguito).  
 
![Fiddler](media/customize-http-security-headers-ad-fs/header2.png)

## <a name="web-browser-compatibility"></a>Compatibilità Web browser
Utilizzare la tabella e i collegamenti seguenti per determinare quali Web browser sono compatibili con ognuna delle intestazioni di risposta di sicurezza.

|Intestazioni di risposta di sicurezza HTTP|Compatibilità browser|
|-----|-----|
|HTTP Strict-Transport-Security (HSTS)|[Compatibilità del browser HSTS](https://developer.mozilla.org/docs/Web/HTTP/Headers/Strict-Transport-Security#Browser_compatibility)|
|X-frame-options|[Compatibilità del browser X-frame-options](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options#Browser_compatibility)| 
|X-XSS-Protection|[Compatibilità del browser X-XSS-Protection](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-XSS-Protection#Browser_compatibility)| 
|Condivisione risorse tra le origini (CORS)|[Compatibilità del browser CORS](https://developer.mozilla.org/docs/Web/HTTP/CORS#Browser_compatibility) 
|Criteri di sicurezza del contenuto (CSP)|[Compatibilità del browser CSP](https://developer.mozilla.org/docs/Web/HTTP/CSP#Browser_compatibility) 

## <a name="next"></a>Avanti

- [Usare AD FS Guida alla risoluzione dei problemi](https://aka.ms/adfshelp/troubleshooting )
- [Risoluzione dei problemi relativi ad AD FS](../../ad-fs/troubleshooting/ad-fs-tshoot-overview.md)
