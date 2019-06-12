---
ms.assetid: 28f4a518-1341-4a10-8a4e-5f84625b314b
title: Requisiti di AD FS 2016
description: Requisiti per l'installazione di Active Directory Federation Services.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/06/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e62032533b15ec3d93896d242273612faafdca58
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444099"
---
# <a name="ad-fs-requirements"></a>Requisiti per ADFS



Di seguito sono indicati i requisiti per la distribuzione di ADFS:  
  
-   [Requisiti del certificato](ad-fs-requirements.md#BKMK_1)  
  
-   [Requisiti hardware](ad-fs-requirements.md#BKMK_2)  
  
-   [Requisiti di proxy](ad-fs-requirements.md#BKMK_3)  
  
-   [Requisiti di Active Directory Domain Services](ad-fs-requirements.md#BKMK_4)  
  
-   [Requisiti di database di configurazione](ad-fs-requirements.md#BKMK_5)  
  
-   [Requisiti del browser](ad-fs-requirements.md#BKMK_6)  

-   [Requisiti di rete](ad-fs-requirements.md#BKMK_7)  
  
-   [Requisiti di autorizzazione](ad-fs-requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>Requisiti dei certificati  
  
### <a name="ssl-certificates"></a>Certificati SSL

Ogni server ADFS e Proxy applicazione Web dispone di un certificato SSL per soddisfare le richieste HTTPS per il servizio federativo.  Il Proxy dell'applicazione Web può avere altri certificati SSL per soddisfare le richieste alle applicazioni pubblicate.

**Raccomandazione:** Usare lo stesso certificato SSL per tutti i server federativi AD FS e proxy applicazione Web. 

**Requisiti:**

I certificati SSL nei server federativi devono soddisfare i requisiti seguenti
- Certificato è attendibile pubblicamente (per le distribuzioni di produzione)
- Certificato contiene il valore Server autenticazione chiavi avanzato (EKU)
- Certificato contiene il nome del servizio federativo, ad esempio "fs.contoso.com" nell'oggetto o nome alternativo soggetto (SAN)
- Per l'autenticazione del certificato utente sulla porta 443, certificato contiene "certauth.\<nome del servizio federativo\>", ad esempio"certauth.fs.contoso.com"nella rete SAN
- Per la registrazione del dispositivo o per l'autenticazione moderna per le risorse locali tramite i client precedenti a Windows 10, la SAN deve contenere "enterpriseregistration.\<suffisso UPN\>"per ogni suffisso UPN in uso nell'organizzazione.

I certificati SSL sul Proxy di applicazione Web devono soddisfare i requisiti seguenti
- Se viene utilizzato il proxy per le richieste proxy ADFS che utilizzano l'autenticazione integrata di Windows, il certificato SSL del proxy devono corrispondere il (utilizzare la stessa chiave) il certificato SSL del server federativo
- Se la proprietà di AD FS "ExtendedProtectionTokenCheck" è abilitata (l'impostazione predefinita in AD FS), il certificato SSL del proxy deve essere lo stesso (utilizzare la stessa chiave) come il certificato SSL del server federativo
- In caso contrario, i requisiti per il certificato SSL del proxy sono identici a quelli per il certificato SSL del server federativo

### <a name="service-communication-certificate"></a>Certificato di comunicazione del servizio
Questo certificato non è necessario per la maggior parte degli scenari di ADFS, tra cui Azure AD e Office 365. Per impostazione predefinita, ADFS consente di configurare il certificato SSL fornito subito dopo la configurazione iniziale come certificato di comunicazione del servizio.

**Raccomandazione:**
- Utilizzare lo stesso certificato quando si utilizza per SSL.  

### <a name="token-signing-certificate"></a>Certificato di firma di token
Questo certificato viene usato per firmare i token emessi per le relying party, quindi applicazioni relying party devono riconoscere il certificato è associata una chiave di conosciuto e attendibile. Quando il token modifiche certificato firma, ad esempio quando scade e si configurano un nuovo certificato, è necessario aggiornare tutte le relying party.

**Raccomandazione:** Usare il valore predefinito di ADFS, i certificati di firma di token autofirmati, generati internamente.  

**Requisiti:**
- Se l'organizzazione richiede l'utilizzo di certificati emessi da PKI dell'azienda per la firma di token, questa operazione può essere eseguita utilizzando il parametro SigningCertificateThumbprint del cmdlet Install-AdfsFarm.
- Se si desidera utilizza i certificati predefiniti generati internamente o esternamente registrati certificati, quando viene modificato il certificato di firma di token è necessario verificare che tutti i componenti vengono aggiornati con le nuove informazioni del certificato.  In caso contrario, gli accessi a qualsiasi relying party non aggiornato avrà esito negativo.

### <a name="token-encryptingdecrypting-certificate"></a>Certificato di crittografia/decrittografia dei token
Questo certificato viene utilizzato dai provider di attestazioni che crittografare i token emessi per ADFS.

**Raccomandazione:** Usare il valore predefinito di ADFS, i certificati di decrittografia di token autofirmati, generati internamente.  

**Requisiti:**
- Se l'organizzazione richiede l'utilizzo di certificati emessi da PKI dell'azienda per la firma di token, questa operazione può essere eseguita utilizzando il parametro DecryptingCertificateThumbprint del cmdlet Install-AdfsFarm.
- Se si desidera utilizza i certificati predefiniti generati internamente o esternamente registrati certificati, quando viene modificato il token di decrittografia di certificato è necessario verificare che tutti i provider di attestazioni vengono aggiornati con le nuove informazioni del certificato.  In caso contrario, gli accessi tramite qualsiasi provider non aggiornato di attestazioni avrà esito negativo.
  
> [!CAUTION]  
> I certificati utilizzati per token\-firma e il token\-decrittografia\/la crittografia sono fondamentali per la stabilità del servizio federativo. I clienti di gestire i propri token\-firma & token\-decrittografia\/crittografare i certificati devono assicurarsi che questi certificati vengono sottoposti a backup e sono disponibili in modo indipendente durante un evento di ripristino.  

### <a name="user-certificates"></a>Certificati utente
- Quando si utilizza l'autenticazione del certificato utente con AD FS, tutti i certificati utente deve essere concatenato a un'autorità di certificazione radice considerata attendibile dal server ADFS e Proxy applicazione Web x509.

## <a name="BKMK_2"></a>Requisiti hardware  
Requisiti hardware ADFS e Proxy applicazione Web (fisici o virtuali) sono gestiti su CPU, pertanto è necessario adattare le dimensioni della farm per la capacità di elaborazione.  
- Utilizzare il [foglio di calcolo pianificazione delle capacità di ADFS 2016](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx) per determinare il numero di server ADFS e Proxy applicazione Web, è necessario.

I requisiti di memoria e disco per ADFS sono abbastanza statici, vedere la tabella riportata di seguito:


|**Requisito hardware**|**Requisito minimo**|**Requisito consigliato**|
|----- | ----- |-----|
|RAM|2 GB|4 GB |
|Spazio su disco|32 GB|100 GB |

**Requisiti Hardware del Server SQL**

Se si utilizza SQL Server per il database di configurazione di ADFS, le dimensioni di SQL Server in base alle raccomandazioni di base di SQL Server.  Le dimensioni del database di ADFS sono molto piccola e ADFS non impone un notevole carico di elaborazione all'istanza del database.  AD FS, tuttavia, connettersi al database più volte durante l'autenticazione, pertanto la connessione di rete deve essere affidabile.  Sfortunatamente, SQL Azure non è supportato per il database di configurazione di ADFS.
  
## <a name="BKMK_3"></a>Requisiti di proxy  
  
-   Per l'accesso extranet, è necessario distribuire il servizio ruolo Proxy applicazione Web \- fa parte del ruolo del server di accesso remoto. 

-   Proxy di terze parti deve supportare il [protocollo MS-ADFSPIP](https://msdn.microsoft.com/en-us/library/dn392811.aspx) devono essere supportati come un proxy ADFS.  Per un elenco di parti 3rd i fornitori di vedere le [domande frequenti su](AD-FS-FAQ.md#what-third-party-proxies-are-available-for-ad-fs-that-support-ms-adfspip).

-   AD FS 2016 richiede il server Proxy applicazione Web in Windows Server 2016.  Un proxy di livello inferiore non può essere configurato per una farm di AD FS 2016 in esecuzione a livello di comportamento 2016 farm.
  
-   Impossibile installare un server federativo e il servizio ruolo Proxy applicazione Web nello stesso computer.  
  
## <a name="BKMK_4"></a>Requisiti di Active Directory Domain Services  
**Requisiti del controller di dominio**  
  
- ADFS richiede che i controller di dominio che esegue Windows Server 2008 o versione successiva.

- Almeno un controller di dominio di Windows Server 2016 è necessario per Microsoft Passport for Work.
  
> [!NOTE]  
> Tutto il supporto per gli ambienti con controller di dominio di Windows Server 2003 è stata terminata. Visitare [questa pagina](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO) Per ulteriori informazioni sulla disponibilità del supporto Microsoft.  
  
**Funzionalità del dominio\-requisiti di livello**  
  
 - Tutti i domini di account utente e il dominio a cui sono stati aggiunti i server ADFS deve funzionare a livello funzionale di dominio di Windows Server 2003 o versione successiva.  
  
 - Una funzionalità del dominio Windows Server 2008 livello o versione successiva è richiesto per l'autenticazione del certificato client se il certificato è mappato in modo esplicito a un account utente in Active Directory.  
  
**Requisiti dello schema**  
  
-   Le nuove installazioni di AD FS 2016 richiedono lo schema di Active Directory 2016 (versione minima 85).

-   Aumentare il livello di comportamento di farm di ADFS (FBL) a livello di 2016 richiede lo schema di Active Directory 2016 (versione minima 85).  

  
**Requisiti dell'account di servizio**  
  
-   Qualsiasi account di dominio standard può essere utilizzato come account del servizio per ADFS. Sono supportati anche gli account del servizio gestito di gruppo. Le autorizzazioni necessarie in fase di esecuzione verranno aggiunto automaticamente quando si configura ADFS.

-   L'assegnazione di diritti utente necessari per l'account del servizio Active Directory è 'Accedi come servizio'

-   L'assegnazione di diritti utente necessari per la 'NT Service\adfssrv' e 'NT Service\drs' sono 'Generazione controlli di sicurezza' e 'Accedi come servizio'.

-   Account del servizio gestito gruppo richiedono almeno un controller di dominio che esegue Windows Server 2012 o versione successiva.  GMSA deve risiedere sotto il valore predefinito ' CN = contenitore degli account del servizio gestito.  

-   Per l'autenticazione Kerberos, il nome dell'entità servizio '`HOST/<adfs\_service\_name>`' deve essere registrato nell'account del servizio AD FS. Per impostazione predefinita, ADFS configurerà questo quando si crea una nuova farm ADFS.  In caso di errore, ad esempio nel caso di conflitto o di autorizzazioni sufficienti, verrà visualizzato un avviso ed è necessario aggiungerla manualmente.  
   
**Requisiti di dominio**  
  
-   Tutti i server ADFS devono essere di un dominio di Active Directory.  
  
-   Tutti i server ADFS all'interno di una farm devono essere distribuiti nello stesso dominio.  
   
**Requisiti di più foreste**  
  
-   Ogni dominio o foresta che contiene gli utenti l'autenticazione per il servizio ADFS, deve considerare attendibile il dominio a cui sono stati aggiunti i server ADFS.  

-   La foresta, che l'account del servizio ADFS sia un membro, deve appurare di tutte le foreste di account di accesso utente. 
  
-   L'account del servizio ADFS deve disporre delle autorizzazioni per leggere gli attributi utente in ogni dominio che contiene gli utenti l'autenticazione per il servizio ADFS.  
  
## <a name="BKMK_5"></a>Requisiti di database di configurazione  
In questa sezione vengono descritti i requisiti e restrizioni per le farm di ADFS che utilizza rispettivamente la Database interno di Windows (WID) o SQL Server del database:  
  
**WID**  
  
-   Il profilo di risoluzione dell'elemento di SAML 2.0 non è supportato in una farm database interno di Windows.    

-   Rilevamento riproduzione token non è supportata una farm database interno di Windows. (Questa funzionalità viene utilizzata soltanto solo in scenari in cui ADFS è funge da provider di federazione e l'utilizzo di token di sicurezza da provider di attestazioni esterno).  
  
Nella tabella seguente viene fornito un riepilogo di quanti server ADFS sono supportati in Visual Studio un WID una farm SQL Server.    
  
  
|| 1 - 100 trust della relying party (RP) configurato in AD FS | Più di 100 attendibilità della relying Party configurato  |
| --- |--- | --- |
|1-30 server ADFS|Database interno di Windows supportati|Non supportato utilizzando WID: SQL Server necessarie |
|Più di 30 server ADFS|Non supportato utilizzando WID: SQL Server necessarie|Non supportato utilizzando WID: SQL Server necessarie  
  
**SQL Server**  
  
- Per ADFS in Windows Server 2016, SQL Server 2008 e versioni successive sono supportate.

- Risoluzione artefatto SAML sia il rilevamento riproduzione token sono supportati in una farm di SQL Server.  
  
## <a name="BKMK_6"></a>Requisiti del browser  
Quando viene eseguita l'autenticazione di ADFS tramite un browser o un controllo browser, il browser deve essere conforme ai requisiti seguenti:  
  
- È necessario abilitare JavaScript  
  
- Per single sign-on, il browser client deve essere configurato per consentire i cookie  
  
- Indicazione del nome del server \(SNI\) devono essere supportate  
  
- Per certificato & dispositivo autenticazione dei certificati utente, il browser deve supportare l'autenticazione del certificato client SSL  

- Per l'accesso trasparente utilizzando l'autenticazione integrata di Windows, il nome del servizio federativo (ad esempio https:\/\/fs.contoso.com) devono essere configurate nella zona intranet locale o siti attendibili.
  ## <a name="BKMK_7"></a>Requisiti di rete  
 
**Requisiti del firewall**  
  
Entrambi i firewall si trova tra il Proxy dell'applicazione Web e la server farm federativa e il firewall tra i client e il Proxy dell'applicazione Web deve avere la porta TCP 443 abilitato in ingresso.  
  
Inoltre, se l'autenticazione del certificato client utente \(autenticazione clientTLS utilizzando X509 i certificati utente\) è obbligatorio e l'endpoint certauth sulla porta 443 non è abilitato, AD FS 2016 richiede che sia attivato 49443 la porta TCP in ingresso nel firewall tra i client e il Proxy dell'applicazione Web. Questa operazione non è necessaria nel firewall tra il Proxy di applicazione Web e i server federativi\). 

Per altre informazioni sulla porta ibrida requisiti, vedere [protocolli e porte di identità ibrida](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports). 

Per altre informazioni vedere [procedure consigliate per la protezione di Active Directory Federation Services](../deployment/Best-Practices-Securing-AD-FS.md)
  
**Requisiti DNS**  
  
-   Per l'accesso intranet, tutti i client che accedono AD FS service all'interno della rete azienda interna \(intranet\) deve essere in grado di risolvere il nome del servizio ADFS al bilanciamento del carico per i server ADFS o il server ADFS.  
  
-   Per l'accesso extranet, tutti i client che accedono AD FS service dall'esterno della rete aziendale \(extranet\/internet\) deve essere in grado di risolvere il nome del servizio ADFS al bilanciamento del carico per il server Proxy applicazione Web o il server Proxy applicazione Web.  
  
-   Ogni server di Proxy applicazione Web nella rete Perimetrale deve essere in grado di risolvere il nome di servizio ADFS al bilanciamento del carico per i server ADFS o il server ADFS. Ciò può essere ottenuto tramite un server DNS alternativo nella rete Perimetrale o modificando la risoluzione del server locale utilizzando il file HOSTS.  
  
-   Per l'autenticazione integrata di Windows, è necessario utilizzare un record DNS A \(non CNAME\) per il nome del servizio federativo.  

-   Per l'autenticazione del certificato utente sulla porta 443, "certauth.\<nome del servizio federativo\>"deve essere configurato in DNS per risolvere al proxy di applicazione web o server federativo.

-   Per la registrazione del dispositivo o per l'autenticazione moderna per le risorse locali tramite i client precedenti a Windows 10, "enterpriseregistration.\<suffisso UPN\>", per ogni suffisso UPN in uso nell'organizzazione, deve essere configurato per risolvere il proxy di applicazione web o server federativo.

**Requisiti del bilanciamento del carico**
- Il bilanciamento del carico non deve interrompere la sessione SSL. ADFS supporta più casi di utilizzo con l'autenticazione del certificato che verrà interrotta quando terminazione SSL. Terminazione SSL al servizio di bilanciamento del carico non è supportata per qualsiasi caso d'uso. 
- È consigliabile usare un servizio di bilanciamento del carico che supporta SNI. Nell'evento non le utilizza, usando la 0.0.0.0 fallback associazione su AD FS o server Proxy applicazione Web deve fornire una soluzione alternativa.
- È consigliabile usare l'endpoint del probe di integrità HTTP (non HTTPS) per eseguire controlli di integrità di load balancer per il routing del traffico. Questo evita qualsiasi problema relativo a SNI. La risposta a questi endpoint di probe è un messaggio HTTP 200 OK e viene gestita in locale con alcuna dipendenza dalla servizi back-end. Il probe HTTP è accessibile su HTTP utilizzando il percorso probe/adfs /
    - http://&lt;nome del Proxy applicazione Web &gt; /adfs/probe
    - http://&lt;nome del server ad FS &gt; /adfs/probe
    - http://&lt;indirizzo IP del Proxy applicazione Web &gt; /adfs/probe
    - http://&lt;indirizzo IP di ADFS &gt; /adfs/probe
- NON è consigliabile usare DNS round robin che consente di bilanciare il carico. Utilizzo di questo tipo di bilanciamento del carico non fornisce un metodo automatico per rimuovere un nodo dal servizio di bilanciamento del carico con probe di integrità. 
- È consigliabile non usare l'affinità di sessione basata su IP o le sessioni permanenti per il traffico di autenticazione per AD FS all'interno di bilanciamento del carico. Ciò può provocare un overload di alcuni nodi quando si usa il protocollo di autenticazione legacy per i client di posta elettronica per la connessione ai servizi di posta elettronica di Office 365 (Exchange Online). 

## <a name="BKMK_13"></a>Requisiti di autorizzazione  
L'amministratore che esegue l'installazione e la configurazione iniziale di ADFS deve disporre delle autorizzazioni di amministratore locale sul server ADFS.  Se l'amministratore locale non dispone delle autorizzazioni per creare oggetti in Active Directory, è necessario innanzitutto disporre di un amministratore di dominio creare gli oggetti di Active Directory necessari, quindi configurare la farm di ADFS tramite il parametro AdminConfiguration.  
  
  

