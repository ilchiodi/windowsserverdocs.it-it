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
ms.openlocfilehash: c330b5f65b2862628fd23e288c95e81653da5c5b
ms.sourcegitcommit: 4fa147d552481d8279a5390f458a9f7788061977
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/23/2019
ms.locfileid: "70009074"
---
# <a name="ad-fs-requirements"></a>Requisiti per ADFS



Di seguito sono indicati i requisiti per la distribuzione di ADFS:  
  
-   [Requisiti del certificato](ad-fs-requirements.md#BKMK_1)  
  
-   [Requisiti hardware](ad-fs-requirements.md#BKMK_2)  
  
-   [Requisiti del proxy](ad-fs-requirements.md#BKMK_3)  
  
-   [Requisiti di servizi di dominio Active Directory](ad-fs-requirements.md#BKMK_4)  
  
-   [Requisiti del database di configurazione](ad-fs-requirements.md#BKMK_5)  
  
-   [Requisiti del browser](ad-fs-requirements.md#BKMK_6)  

-   [Requisiti di rete](ad-fs-requirements.md#BKMK_7)  
  
-   [Requisiti per le autorizzazioni](ad-fs-requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>Requisiti del certificato  
  
### <a name="ssl-certificates"></a>Certificati SSL

Ogni server ADFS e Proxy applicazione Web dispone di un certificato SSL per soddisfare le richieste HTTPS per il servizio federativo.  Il Proxy dell'applicazione Web può avere altri certificati SSL per soddisfare le richieste alle applicazioni pubblicate.

**Raccomandazione** Utilizzare lo stesso certificato SSL per tutti i server federativi AD FS e proxy applicazione Web. 

**Requisiti**

I certificati SSL nei server federativi devono soddisfare i requisiti seguenti
- Certificato è attendibile pubblicamente (per le distribuzioni di produzione)
- Il certificato contiene il valore di utilizzo chiavi avanzato (EKU) di autenticazione server
- Certificato contiene il nome del servizio federativo, ad esempio "fs.contoso.com" nell'oggetto o nome alternativo soggetto (SAN)
- Per l'autenticazione del certificato utente sulla porta 443, certificato contiene "certauth.\<nome del servizio federativo\>", ad esempio"certauth.fs.contoso.com"nella rete SAN
- Per la registrazione del dispositivo o per l'autenticazione moderna per le risorse locali tramite i client precedenti a Windows 10, la SAN deve contenere "enterpriseregistration.\<suffisso UPN\>"per ogni suffisso UPN in uso nell'organizzazione.

I certificati SSL sul Proxy di applicazione Web devono soddisfare i requisiti seguenti
- Se viene utilizzato il proxy per le richieste proxy ADFS che utilizzano l'autenticazione integrata di Windows, il certificato SSL del proxy devono corrispondere il (utilizzare la stessa chiave) il certificato SSL del server federativo
- Se la proprietà di AD FS "ExtendedProtectionTokenCheck" è abilitata (l'impostazione predefinita in AD FS), il certificato SSL del proxy deve essere lo stesso (utilizzare la stessa chiave) come il certificato SSL del server federativo
- In caso contrario, i requisiti per il certificato SSL del proxy sono identici a quelli per il certificato SSL del server federativo

### <a name="service-communication-certificate"></a>Certificato di comunicazione del servizio
Questo certificato non è necessario per la maggior parte degli scenari di ADFS, tra cui Azure AD e Office 365. Per impostazione predefinita, ADFS consente di configurare il certificato SSL fornito subito dopo la configurazione iniziale come certificato di comunicazione del servizio.

**Raccomandazione**
- Utilizzare lo stesso certificato quando si utilizza per SSL.  

### <a name="token-signing-certificate"></a>Certificato di firma di token
Questo certificato viene usato per firmare i token emessi alle relying party, quindi relying party applicazioni devono riconoscere il certificato e la chiave associata è nota e attendibile. Quando il token modifiche certificato firma, ad esempio quando scade e si configurano un nuovo certificato, è necessario aggiornare tutte le relying party.

**Raccomandazione** Usare i certificati di firma di token autofirmati AD FS predefiniti, generati internamente.  

**Requisiti**
- Se l'organizzazione richiede l'utilizzo di certificati emessi da PKI dell'azienda per la firma di token, questa operazione può essere eseguita utilizzando il parametro SigningCertificateThumbprint del cmdlet Install-AdfsFarm.
- Se si desidera utilizza i certificati predefiniti generati internamente o esternamente registrati certificati, quando viene modificato il certificato di firma di token è necessario verificare che tutti i componenti vengono aggiornati con le nuove informazioni del certificato.  In caso contrario, gli accessi a qualsiasi relying party non aggiornato avrà esito negativo.

### <a name="token-encryptingdecrypting-certificate"></a>Certificato di crittografia/decrittografia dei token
Questo certificato viene utilizzato dai provider di attestazioni che crittografare i token emessi per ADFS.

**Raccomandazione** Usare i certificati di decrittografia dei token autofirmati AD FS predefiniti generati internamente.  

**Requisiti**
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
|RAM|2 GB|4 GB |
|Spazio su disco|32 GB|100 GB |

**Requisiti hardware SQL Server**

Se si utilizza SQL Server per il database di configurazione di ADFS, le dimensioni di SQL Server in base alle raccomandazioni di base di SQL Server.  Le dimensioni del database di ADFS sono molto piccola e ADFS non impone un notevole carico di elaborazione all'istanza del database.  AD FS, tuttavia, connettersi al database più volte durante l'autenticazione, pertanto la connessione di rete deve essere affidabile.  Sfortunatamente, SQL Azure non è supportato per il database di configurazione di ADFS.
  
## <a name="BKMK_3"></a>Requisiti del proxy  
  
-   Per l'accesso extranet, è necessario distribuire il servizio ruolo Proxy applicazione Web \- fa parte del ruolo del server di accesso remoto. 

-   Proxy di terze parti deve supportare il [protocollo MS-ADFSPIP](https://msdn.microsoft.com/library/dn392811.aspx) devono essere supportati come un proxy ADFS.  Per un elenco di fornitori di terze parti, vedere le [domande frequenti](AD-FS-FAQ.md).

-   AD FS 2016 richiede il server Proxy applicazione Web in Windows Server 2016.  Un proxy di livello inferiore non può essere configurato per una farm di AD FS 2016 in esecuzione a livello di comportamento 2016 farm.
  
-   Impossibile installare un server federativo e il servizio ruolo Proxy applicazione Web nello stesso computer.  
  
## <a name="BKMK_4"></a>Requisiti di servizi di dominio Active Directory  
**Requisiti del controller di dominio**  
  
- ADFS richiede che i controller di dominio che esegue Windows Server 2008 o versione successiva.

- Almeno un controller di dominio di Windows Server 2016 è necessario per Microsoft Passport for Work.
  
> [!NOTE]  
> Tutto il supporto per gli ambienti con controller di dominio di Windows Server 2003 è stata terminata. Visitare [questa pagina](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO) Per ulteriori informazioni sulla disponibilità del supporto Microsoft.  
  
**Requisiti del\-livello di funzionalità del dominio**  
  
 - Tutti i domini di account utente e il dominio a cui sono stati aggiunti i server ADFS deve funzionare a livello funzionale di dominio di Windows Server 2003 o versione successiva.  
  
 - Una funzionalità del dominio Windows Server 2008 livello o versione successiva è richiesto per l'autenticazione del certificato client se il certificato è mappato in modo esplicito a un account utente in Active Directory.  
  
**Requisiti dello schema**  
  
-   Le nuove installazioni di AD FS 2016 richiedono lo schema di Active Directory 2016 (versione minima 85).

-   Aumentare il livello di comportamento di farm di ADFS (FBL) a livello di 2016 richiede lo schema di Active Directory 2016 (versione minima 85).  

  
**Requisiti dell'account del servizio**  
  
-   Qualsiasi account di dominio standard può essere utilizzato come account del servizio per ADFS. Sono supportati anche gli account del servizio gestito di gruppo. Le autorizzazioni necessarie in fase di esecuzione verranno aggiunto automaticamente quando si configura ADFS.

-   L'assegnazione dei diritti utente necessaria per l'account del servizio Active Directory è' accedi come servizio '

-   Le assegnazioni dei diritti utente necessarie per "NT Service\adfssrv" e "NT Service\drs" sono "genera controlli di sicurezza" e "Accedi come servizio".

-   Account del servizio gestito gruppo richiedono almeno un controller di dominio che esegue Windows Server 2012 o versione successiva.  Il GMSA deve risiedere nel contenitore predefinito ' CN = Managed Service Accounts '.  

-   Per l'autenticazione Kerberos, il nome dell'entità`HOST/<adfs\_service\_name>`servizio '' deve essere registrato nell'account del servizio ad FS. Per impostazione predefinita, ADFS configurerà questo quando si crea una nuova farm ADFS.  In caso di errore, ad esempio nel caso di conflitto o di autorizzazioni sufficienti, verrà visualizzato un avviso ed è necessario aggiungerla manualmente.  
   
**Requisiti di dominio**  
  
-   Tutti i server ADFS devono essere di un dominio di Active Directory.  
  
-   Tutti i server ADFS all'interno di una farm devono essere distribuiti nello stesso dominio.  
   
**Requisiti per più foreste**  
  
-   Ogni dominio o foresta che contiene gli utenti l'autenticazione per il servizio ADFS, deve considerare attendibile il dominio a cui sono stati aggiunti i server ADFS.  

-   La foresta, di cui l'account del servizio AD FS è membro, deve considerare attendibili tutte le foreste di accesso utente. 
  
-   L'account del servizio ADFS deve disporre delle autorizzazioni per leggere gli attributi utente in ogni dominio che contiene gli utenti l'autenticazione per il servizio ADFS.  
  
## <a name="BKMK_5"></a>Requisiti del database di configurazione  
In questa sezione vengono descritti i requisiti e restrizioni per le farm di ADFS che utilizza rispettivamente la Database interno di Windows (WID) o SQL Server del database:  
  
**WID**  
  
-   Il profilo di risoluzione dell'elemento di SAML 2.0 non è supportato in una farm database interno di Windows.    

-   Rilevamento riproduzione token non è supportata una farm database interno di Windows. (Questa funzionalità viene utilizzata soltanto solo in scenari in cui ADFS è funge da provider di federazione e l'utilizzo di token di sicurezza da provider di attestazioni esterno).  
  
Nella tabella seguente viene fornito un riepilogo di quanti server ADFS sono supportati in Visual Studio un WID una farm SQL Server.    
  
  
|| 1-100 Trust relying party (RP) configurati in AD FS | Più di 100 attendibilità della relying Party configurato  |
| --- |--- | --- |
|server AD FS 1-30|Database interno di Windows supportati|Non supportato con WID-SQL Server obbligatorio |
|Più di 30 server ADFS|Non supportato con WID-SQL Server obbligatorio|Non supportato con WID-SQL Server obbligatorio  
  
**SQL Server**  
  
- Per ADFS in Windows Server 2016, SQL Server 2008 e versioni successive sono supportate.

- Risoluzione artefatto SAML sia il rilevamento riproduzione token sono supportati in una farm di SQL Server.  
  
## <a name="BKMK_6"></a>Requisiti del browser  
Quando viene eseguita l'autenticazione di ADFS tramite un browser o un controllo browser, il browser deve essere conforme ai requisiti seguenti:  
  
- È necessario abilitare JavaScript  
  
- Per single sign-on, il browser client deve essere configurato per consentire i cookie  
  
- Indicazione del nome del server \(SNI\) devono essere supportate  
  
- Per certificato & dispositivo autenticazione dei certificati utente, il browser deve supportare l'autenticazione del certificato client SSL  

- Per l'accesso trasparente usando l'autenticazione integrata di Windows, il nome del servizio federativo (ad\/esempio https:\/FS.contoso.com) deve essere configurato nell'area Intranet locale o nell'area siti attendibili.
  ## <a name="BKMK_7"></a>Requisiti di rete  
 
**Requisiti del firewall**  
  
Entrambi i firewall si trova tra il Proxy dell'applicazione Web e la server farm federativa e il firewall tra i client e il Proxy dell'applicazione Web deve avere la porta TCP 443 abilitato in ingresso.  
  
Inoltre, se l'autenticazione del certificato client utente \(autenticazione clientTLS utilizzando X509 i certificati utente\) è obbligatorio e l'endpoint certauth sulla porta 443 non è abilitato, AD FS 2016 richiede che sia attivato 49443 la porta TCP in ingresso nel firewall tra i client e il Proxy dell'applicazione Web. Questa operazione non è necessaria nel firewall tra il Proxy di applicazione Web e i server federativi\). 

Per altre informazioni sui requisiti delle porte ibride [, vedere Porte e protocolli di identità ibrida](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports). 

Per ulteriori informazioni, vedere la pagina relativa alle procedure consigliate [per la protezione Active Directory Federation Services](../deployment/Best-Practices-Securing-AD-FS.md)
  
**Requisiti DNS**  
  
-   Per l'accesso intranet, tutti i client che accedono AD FS service all'interno della rete azienda interna \(intranet\) deve essere in grado di risolvere il nome del servizio ADFS al bilanciamento del carico per i server ADFS o il server ADFS.  
  
-   Per l'accesso extranet, tutti i client che accedono AD FS service dall'esterno della rete aziendale \(extranet\/internet\) deve essere in grado di risolvere il nome del servizio ADFS al bilanciamento del carico per il server Proxy applicazione Web o il server Proxy applicazione Web.  
  
-   Ogni server di Proxy applicazione Web nella rete Perimetrale deve essere in grado di risolvere il nome di servizio ADFS al bilanciamento del carico per i server ADFS o il server ADFS. Ciò può essere ottenuto tramite un server DNS alternativo nella rete Perimetrale o modificando la risoluzione del server locale utilizzando il file HOSTS.  
  
-   Per l'autenticazione integrata di Windows, è necessario utilizzare un record DNS A \(non CNAME\) per il nome del servizio federativo.  

-   Per l'autenticazione del certificato utente sulla porta 443, "certauth.\<nome del servizio federativo\>"deve essere configurato in DNS per risolvere al proxy di applicazione web o server federativo.

-   Per la registrazione del dispositivo o per l'autenticazione moderna per le risorse locali tramite i client precedenti a Windows 10, "enterpriseregistration.\<suffisso UPN\>", per ogni suffisso UPN in uso nell'organizzazione, deve essere configurato per risolvere il proxy di applicazione web o server federativo.

**Requisiti di Load Balancer**
- Il servizio di bilanciamento del carico non deve terminare il protocollo SSL. AD FS supporta più casi di utilizzo con l'autenticazione del certificato che si interrompe durante la terminazione di SSL. Il termine SSL al servizio di bilanciamento del carico non è supportato per alcun caso d'uso. 
- È consigliabile usare un servizio di bilanciamento del carico che supporti SNI. In caso contrario, l'utilizzo dell'associazione di fallback 0.0.0.0 sul server del proxy dell'applicazione AD FS/Web dovrebbe fornire una soluzione alternativa.
- È consigliabile usare gli endpoint del probe di integrità HTTP (non HTTPS) per eseguire i controlli di integrità del servizio di bilanciamento del carico per il routing del traffico. In questo modo si evitano eventuali problemi relativi a SNI. La risposta a questi endpoint Probe è un HTTP 200 OK e viene servita localmente senza alcuna dipendenza dai servizi back-end. È possibile accedere al Probe HTTP tramite HTTP usando il percorso '/ADFS/probe '
    - &lt;nome&gt;del proxy dell'applicazione Web http:///ADFS/Probe
    - http://&lt;nome&gt;server ADFS/ADFS/Probe
    - &lt;indirizzo&gt;IP del proxy dell'applicazione Web http:///ADFS/Probe
    - http://&lt;indirizzo&gt;IP ADFS/ADFS/Probe
- NON è consigliabile usare DNS round robin come metodo per il bilanciamento del carico. L'uso di questo tipo di bilanciamento del carico non fornisce un modo automatico per rimuovere un nodo dal servizio di bilanciamento del carico usando i probe di integrità. 
- NON è consigliabile usare l'affinità di sessione basata su IP o le sessioni permanenti per il traffico di autenticazione per AD FS all'interno del servizio di bilanciamento del carico. Questo può causare un sovraccarico di determinati nodi quando si usa il protocollo di autenticazione legacy per i client di posta elettronica per la connessione ai servizi di posta elettronica di Office 365 (Exchange Online). 

## <a name="BKMK_13"></a>Requisiti per le autorizzazioni  
L'amministratore che esegue l'installazione e la configurazione iniziale di ADFS deve disporre delle autorizzazioni di amministratore locale sul server ADFS.  Se l'amministratore locale non dispone delle autorizzazioni per creare oggetti in Active Directory, è necessario innanzitutto disporre di un amministratore di dominio creare gli oggetti di Active Directory necessari, quindi configurare la farm di ADFS tramite il parametro AdminConfiguration.  
  
  

