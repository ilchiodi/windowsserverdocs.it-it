---
ms.assetid: 28f4a518-1341-4a10-8a4e-5f84625b314b
title: Requisiti per ADFS 2016
description: Requisiti per l'installazione di Active Directory Federation Services.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/06/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 81b51c751d5f54436b14450ef21bf49feb864290
ms.sourcegitcommit: 556361fe7c73c75d6cdc46a67dc88679fbe89c61
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/08/2018
---
# <a name="ad-fs-requirements"></a>Requisiti per ADFS

>Si applica a: Windows Server 2016

Di seguito sono indicati i requisiti per la distribuzione di ADFS:  
  
-   [Requisiti del certificato](ad-fs-requirements.md#BKMK_1)  
  
-   [Requisiti hardware](ad-fs-requirements.md#BKMK_2)  
  
-   [Requisiti del proxy](ad-fs-requirements.md#BKMK_3)  
  
-   [Requisiti di Active Directory](ad-fs-requirements.md#BKMK_4)  
  
-   [Requisiti del database di configurazione](ad-fs-requirements.md#BKMK_5)  
  
-   [Requisiti del browser](ad-fs-requirements.md#BKMK_6)  

-   [Requisiti di rete](ad-fs-requirements.md#BKMK_7)  
  
-   [Requisiti di autorizzazione](ad-fs-requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>Requisiti del certificato  
  
### <a name="ssl-certificates"></a>Certificati SSL

Ogni server ADFS e Proxy applicazione Web dispone di un certificato SSL per soddisfare le richieste HTTPS per il servizio federativo.  Il Proxy dell'applicazione Web può avere altri certificati SSL per soddisfare le richieste alle applicazioni pubblicate.

**Raccomandazione:** utilizzare lo stesso certificato SSL per tutti i server federativi ADFS e proxy applicazione Web. 

**Requisiti:**

I certificati SSL nei server federativi devono soddisfare i requisiti seguenti
- Certificato è attendibile pubblicamente (per le distribuzioni di produzione)
- Certificato contiene il valore Server autenticazione utilizzo chiavi avanzato (EKU)
- Certificato contiene il nome del servizio federativo, ad esempio "fs.contoso.com" nell'oggetto o nome alternativo soggetto (SAN)
- Per l'autenticazione del certificato utente sulla porta 443, certificato contiene "certauth. \ < name\ servizio federativo >", ad esempio "certauth.fs.contoso.com" nella rete SAN
- Per la registrazione dei dispositivi o per l'autenticazione moderna per le risorse locali tramite i client precedenti a Windows 10, la SAN deve contenere "enterpriseregistration. \ < upn suffix\ >" per ogni suffisso UPN in uso nell'organizzazione.

I certificati SSL sul Proxy applicazione Web devono soddisfare i requisiti seguenti
- Se il proxy viene utilizzato per le richieste proxy ADFS che utilizzano l'autenticazione integrata di Windows, il certificato SSL del proxy devono essere lo stesso (utilizzare la stessa chiave) come il certificato SSL del server federativo
- Se la proprietà di AD FS "ExtendedProtectionTokenCheck" è abilitata (l'impostazione predefinita in AD FS), il certificato SSL del proxy deve essere lo stesso (utilizzare la stessa chiave) come il certificato SSL del server federativo
- In caso contrario, i requisiti per il certificato SSL del proxy sono identici a quelli per il certificato SSL del server federativo

### <a name="service-communication-certificate"></a>Certificato di comunicazione del servizio
Questo certificato non è necessario per la maggior parte degli scenari di ADFS, tra cui Azure AD e Office 365. Per impostazione predefinita, ADFS consente di configurare il certificato SSL fornito subito dopo la configurazione iniziale come certificato di comunicazione del servizio.

**Raccomandazione:**
- Utilizzare lo stesso certificato per SSL.  

### <a name="token-signing-certificate"></a>Certificato di firma di token
Questo certificato è token usati segno rilasciato per le relying party, quindi applicazioni relying party devono riconoscere il certificato è associata una chiave di conosciuto e attendibile. Quando il token modifiche certificato firma, ad esempio quando scade e si configurano un nuovo certificato, è necessario aggiornare tutte le relying party.

**Raccomandazione:** usare il valore predefinito di ADFS, generati internamente, token autofirmato certificati di firma.  

**Requisiti:**
- Se l'organizzazione richiede che certificati emessi da PKI dell'azienda deve essere utilizzato per la firma di token, questa operazione può essere eseguita utilizzando il parametro SigningCertificateThumbprint del cmdlet Install-AdfsFarm.
- Se si desidera utilizza i certificati predefiniti generati internamente o esternamente registrati certificati, quando viene modificato il certificato di firma di token è necessario verificare che tutte le relying party vengono aggiornate con le nuove informazioni del certificato.  In caso contrario, gli accessi a qualsiasi relying party non aggiornato avrà esito negativo.

### <a name="token-encryptingdecrypting-certificate"></a>Certificato di crittografia/decrittografia di token
Questo certificato viene utilizzato dai provider di attestazioni che crittografare i token emessi per ADFS.

**Raccomandazione:** usare il valore predefinito di ADFS, generati internamente, token autofirmato certificati di decrittografia.  

**Requisiti:**
- Se l'organizzazione richiede che certificati emessi da PKI dell'azienda deve essere utilizzato per la firma di token, questa operazione può essere eseguita utilizzando il parametro DecryptingCertificateThumbprint del cmdlet Install-AdfsFarm.
- Se si desidera utilizza i certificati predefiniti generati internamente o esternamente registrati certificati, quando viene modificato il token di decrittografia di certificato è necessario verificare che tutti i provider di attestazioni vengono aggiornati con le nuove informazioni del certificato.  In caso contrario, gli accessi tramite qualsiasi provider non aggiornato di attestazioni avrà esito negativo.
  
> [!CAUTION]  
> I certificati utilizzati per la firma token\ e token\ decrypting\/crittografia sono fondamentali per la stabilità del servizio federativo. I clienti di gestire i propri certificati di firma token\ & token\ decrypting\/crittografia devono assicurarsi che questi certificati vengono sottoposti a backup e sono disponibili in modo indipendente durante un evento di ripristino.  

### <a name="user-certificates"></a>Certificati utente
- Quando si utilizza l'autenticazione del certificato utente con AD FS, tutti i certificati utente deve essere concatenato a un'autorità di certificazione radice considerata attendibile dal server di ADFS e Proxy applicazione Web x509.

## <a name="BKMK_2"></a>Requisiti hardware  
Requisiti hardware ADFS e Proxy applicazione Web (fisici o virtuali) sono gestiti su CPU, pertanto è necessario adattare le dimensioni della farm per la capacità di elaborazione.  
- Utilizzare il [foglio di calcolo pianificazione delle capacità di AD FS 2016](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx) per determinare il numero di server ADFS e Proxy applicazione Web, sarà necessario.

I requisiti di memoria e disco per ADFS sono abbastanza statici, vedere la tabella seguente:


|**Requisito hardware**|**Requisito minimo**|**Requisito consigliato**|
|----- | ----- |-----|
|RAM|2 GB|4 GB |
|Spazio su disco|32 GB|100 GB |

**Requisiti Hardware del Server SQL**

Se si utilizza SQL Server per il database di configurazione di ADFS, dimensioni di SQL Server in base alle raccomandazioni di base SQL Server.  Le dimensioni del database di ADFS sono molto piccola e ADFS non impone un notevole carico di elaborazione nell'istanza di database.  AD FS, tuttavia, connettersi al database più volte durante l'autenticazione, in modo che la connessione di rete deve essere affidabile.  Sfortunatamente, SQL Azure non è supportato per il database di configurazione di ADFS.
  
## <a name="BKMK_3"></a>Requisiti del proxy  
  
-   Per l'accesso extranet, è necessario distribuire il servizio ruolo Proxy applicazione Web \ - parte del ruolo del server di accesso remoto. 

-   Proxy di terze parti deve supportare il [protocollo MS-ADFSPIP](https://msdn.microsoft.com/en-us/library/dn392811.aspx) devono essere supportati come un proxy AD FS.  Per un elenco di 3 parti vedere fornitori di [domande frequenti](AD-FS-FAQ.md#what-third-party-proxies-are-available-for-ad-fs-that-support-ms-adfspip).

-   AD FS 2016 richiede il server Proxy applicazione Web in Windows Server 2016.  Un proxy di livello inferiore non può essere configurato per una farm AD FS 2016 in esecuzione a livello di comportamento 2016 farm.
  
-   Un server federativo e il servizio ruolo Proxy applicazione Web non può essere installati nello stesso computer.  
  
## <a name="BKMK_4"></a>Requisiti di Active Directory  
**Requisiti del controller di dominio**  
  
- ADFS richiede i controller di dominio che esegue Windows Server 2008 o versione successiva.

- Almeno un controller di dominio di Windows Server 2016 è necessario per Microsoft Passport for Work.
  
> [!NOTE]  
> Tutto il supporto per gli ambienti con controller di dominio di Windows Server 2003 è terminato. Visitare [questa pagina](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO) per ulteriori informazioni sul ciclo di vita di supporto Microsoft.  
  
**Requisiti a livello functional\ dominio**  
  
 - Tutti i domini di account utente e il dominio a cui sono stati aggiunti i server ADFS deve funzionare a livello funzionale di dominio di Windows Server 2003 o versione successiva.  
  
 - Una funzionalità del dominio Windows Server 2008 livello o versione successiva è richiesto per l'autenticazione del certificato client se il certificato è mappato in modo esplicito a un account utente in Active Directory.  
  
**Requisiti dello schema**  
  
-   Le nuove installazioni di AD FS 2016 richiedono lo schema di Active Directory 2016 (versione minima 85).

-   Generare il livello di comportamento farm di ADFS (FBL) a livello di 2016 richiede lo schema di Active Directory 2016 (versione minima 85).  

  
**Requisiti dell'account del servizio**  
  
-   Qualsiasi account di dominio standard può essere utilizzato come account del servizio per ADFS. Sono supportati anche gli account del servizio gestito del gruppo. Le autorizzazioni necessarie in fase di esecuzione verranno aggiunto automaticamente quando si configura ADFS.

-   Account del servizio gestito gruppo richiedono almeno un controller di dominio che esegue Windows Server 2012 o versione successiva.  Sotto il valore predefinito deve live GMSA ' CN = contenitore degli account del servizio gestito.  

-   Per l'autenticazione Kerberos, il nome principale servizio '`HOST/<adfs\_service\_name>`' deve essere registrato nell'account del servizio ADFS. Per impostazione predefinita, ADFS configurerà questo quando si crea una nuova farm ADFS.  In caso di errore, ad esempio nel caso di conflitto o di autorizzazioni sufficienti, verrà visualizzato un avviso ed è necessario aggiungerla manualmente.  
   
**Requisiti di dominio**  
  
-   Tutti i server ADFS devono essere di un dominio di Active Directory.  
  
-   Tutti i server ADFS all'interno di una farm devono essere distribuiti nello stesso dominio.  
   
**Requisiti di più foreste**  
  
-   Ogni dominio o foresta che contiene gli utenti l'autenticazione per il servizio ADFS, deve considerare attendibile il dominio a cui sono stati aggiunti i server ADFS.  

-   L'insieme di strutture, l'account del servizio ADFS è un membro di, deve considerare attendibile tutte le foreste di accesso utente. 
  
-   L'account del servizio ADFS deve disporre delle autorizzazioni per leggere gli attributi utente in ogni dominio che contiene gli utenti l'autenticazione per il servizio ADFS.  
  
## <a name="BKMK_5"></a>Requisiti del database di configurazione  
In questa sezione vengono descritti i requisiti e restrizioni per le farm di ADFS che utilizza rispettivamente la Database interno di Windows (WID) di SQL Server come database:  
  
**DATABASE INTERNO DI WINDOWS**  
  
-   Il profilo di risoluzione artefatto SAML 2.0 non è supportato in una farm database interno di Windows.    

-   Il rilevamento riproduzione token non è supportata una farm database interno di Windows. (Questa funzionalità viene solo utilizzata solo in scenari in cui ADFS è funge da provider di federazione e utilizzo di token di sicurezza da provider di attestazioni esterno).  
  
Nella tabella seguente fornisce un riepilogo di quanti server ADFS sono supportati in Visual Studio un WID una farm SQL Server.    
  
  
|| 1 - 100 trust della relying party (RP) configurato in ADFS | Più di 100 attendibilità della Relying party configurata  |
| --- |--- | --- |
|1-30 server ADFS|Database interno di Windows supportati|Non supportato utilizzando WID - SQL Server necessarie |
|Più di 30 server ADFS|Non supportato utilizzando WID - SQL Server necessarie|Non supportato utilizzando WID - SQL Server necessarie  
  
**SQL Server**  
  
- Per ADFS in Windows Server 2016, SQL Server 2008 e versioni successive sono supportate.

- Risoluzione artefatto SAML sia il rilevamento riproduzione token sono supportati in una farm SQL Server.  
  
## <a name="BKMK_6"></a>Requisiti del browser  
Quando viene eseguita l'autenticazione di ADFS tramite un browser o un controllo browser, il browser deve essere conforme ai requisiti seguenti:  
  
-   È necessario abilitare JavaScript  
  
-   Per single sign-on, il browser client deve essere configurato per consentire i cookie  
  
-   \(SNI\) indicazione nome server deve essere supportata.  
  
-   Per certificato & dispositivo autenticazione dei certificati utente, il browser deve supportare l'autenticazione del certificato client SSL  

-   Per l'accesso trasparente utilizzando l'autenticazione integrata di Windows, è necessario configurare il nome del servizio federativo (ad esempio https:\/\/fs.contoso.com) nell'area intranet locale o di area siti attendibili.
## <a name="BKMK_7"></a>Requisiti di rete  
 
**Requisiti firewall**  
  
Il firewall si trova tra il Proxy dell'applicazione Web e la server farm federativa e il firewall tra i client e il Proxy dell'applicazione Web devono avere la porta TCP 443 abilitato in ingresso.  
  
Inoltre, se l'autenticazione del certificato client utente \ (autenticazione clientTLS utilizzando X509 certificates\ utente) è obbligatorio e non è abilitato l'endpoint certauth sulla porta 443, AD FS 2016 richiede che sia attivato 49443 la porta TCP in ingresso nel firewall tra i client e il Proxy dell'applicazione Web. Questo non è richiesto nel firewall tra il Proxy dell'applicazione Web e il servers\ federativo). 

Per ulteriori informazioni sulla porta ibrida vedere Requisiti [protocolli e porte di identità ibrido](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports). 

Per ulteriori informazioni vedere [procedure consigliate per la protezione di Active Directory Federation Services](..\deployment\Best-Practices-Securing-AD-FS.md)
  
**Requisiti DNS**  
  
-   Per l'accesso intranet, tutti i client che accedono servizio ADFS all'interno di \(intranet\) la rete aziendale interna devono essere in grado di risolvere il nome del servizio ADFS al bilanciamento del carico per i server ADFS o il server AD FS.  
  
-   Per l'accesso extranet, tutti i client che accedono servizio ADFS dall'esterno \(extranet\/internet\) la rete aziendale devono essere in grado di risolvere il nome del servizio ADFS al bilanciamento del carico per il server Proxy applicazione Web o il server Proxy applicazione Web.  
  
-   Ogni server di Proxy applicazione Web nella rete Perimetrale deve essere in grado di risolvere il nome del servizio ADFS al bilanciamento del carico per i server ADFS o il server AD FS. Ciò può essere ottenuto tramite un server DNS alternativo nella rete Perimetrale o modificando la risoluzione di un server locale utilizzando il file HOSTS.  
  
-   Per l'autenticazione integrata di Windows, è necessario utilizzare un \(not CNAME\) record DNS A per il nome del servizio federativo.  

-   Per l'autenticazione del certificato utente sulla porta 443, "certauth. \ < name\ servizio federativo >" deve essere configurato in DNS per risolvere al server federativo o proxy applicazione web.

-   Per la registrazione dei dispositivi o per l'autenticazione moderna per le risorse locali tramite i client precedenti a Windows 10, "enterpriseregistration. \ < upn suffix\ >", per ogni suffisso UPN in uso nell'organizzazione, deve essere configurato per risolvere il server federativo o proxy applicazione web.

**Requisiti del servizio di bilanciamento del carico**
- Il bilanciamento del carico non deve terminare SSL. ADFS supporta più casi di utilizzo con l'autenticazione del certificato che si verifichino interruzioni quando terminazione SSL. Terminazione SSL al servizio di bilanciamento del carico non è supportato per qualsiasi caso di utilizzo. 
- Si consiglia di utilizzare un bilanciamento del carico che supporta SNI. Nell'evento non esiste, Usa il binding di fallback 0.0.0.0 in ADFS al server Proxy applicazione Web deve fornire una soluzione alternativa.
- È consigliabile utilizzare gli endpoint di probe di stato HTTP (non HTTPS) per eseguire controlli di integrità servizio di bilanciamento carico per il routing del traffico. Ciò consente di evitare eventuali problemi relativi alla SNI. La risposta per gli endpoint di tipo probe HTTP 200 OK e viene pubblicata in locale con nessuna dipendenza da servizi back-end. È possibile accedere il probe HTTP utilizzando il percorso di probe/adfs/HTTP
    - http://&lt;nome Proxy applicazione Web&gt;/adfs/probe
    - http://&lt;nome del server ADFS&gt;/adfs/probe
    - http://&lt;indirizzo IP di Proxy applicazione Web&gt;/adfs/probe
    - http://&lt;indirizzo IP di ADFS&gt;/adfs/probe
- Non è consigliabile utilizzare DNS round robin come metodo per il bilanciamento del carico. Questo tipo di bilanciamento del carico non fornisce un modo automatizzato per rimuovere un nodo dal bilanciamento del carico di utilizzo dei probe dello stato. 
- Non è consigliabile utilizzare l'affinità IP basati su sessione o le sessioni permanenti per il traffico di autenticazione ad ADFS all'interno di bilanciamento del carico. Ciò può provocare un overload di alcuni nodi quando si utilizza il protocollo di autenticazione legacy per i client di posta elettronica per connettersi a servizi di posta elettronica di Office 365 (Exchange Online). 

## <a name="BKMK_13"></a>Requisiti di autorizzazione  
L'amministratore che esegue l'installazione e la configurazione iniziale di ADFS deve disporre delle autorizzazioni di amministratore locale sul server ADFS.  Se l'amministratore locale non dispone delle autorizzazioni per creare oggetti in Active Directory, è necessario innanzitutto disporre un amministratore di dominio creare gli oggetti di Active Directory necessari, quindi configurare la farm di ADFS tramite il parametro AdminConfiguration.  
  
  

