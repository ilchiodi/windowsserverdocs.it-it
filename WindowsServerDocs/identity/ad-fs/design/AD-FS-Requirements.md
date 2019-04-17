---
ms.assetid: 8ce6e7c4-cf8e-4b55-980c-048fea28d50f
title: Server Farm federativa con SQL Server
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e23154b20dfcd642178a5ac0de17f1b6a490f91f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="ad-fs-requirements"></a>Requisiti per ADFS

>Si applica a: Windows Server 2012 R2

Di seguito sono illustrati i requisiti che è necessario rispettare i durante la distribuzione di ADFS:  
  
-   [Requisiti del certificato](AD-FS-Requirements.md#BKMK_1)  
  
-   [Requisiti hardware](AD-FS-Requirements.md#BKMK_2)  
  
-   [Requisiti software](AD-FS-Requirements.md#BKMK_3)  
  
-   [Requisiti di Active Directory](AD-FS-Requirements.md#BKMK_4)  
  
-   [Requisiti del database di configurazione](AD-FS-Requirements.md#BKMK_5)  
  
-   [Requisiti del browser](AD-FS-Requirements.md#BKMK_6)  
  
-   [Requisiti di rete Extranet](AD-FS-Requirements.md#BKMK_extranet)  
  
-   [Requisiti di rete](AD-FS-Requirements.md#BKMK_7)  
  
-   [Requisiti dell'archivio attributi](AD-FS-Requirements.md#BKMK_8)  
  
-   [Requisiti dell'applicazione](AD-FS-Requirements.md#BKMK_9)  
  
-   [Requisiti di autenticazione](AD-FS-Requirements.md#BKMK_10)  
  
-   [Requisiti di join all'area di lavoro](AD-FS-Requirements.md#BKMK_11)  
  
-   [Requisiti di crittografia](AD-FS-Requirements.md#BKMK_12)  
  
-   [Requisiti di autorizzazione](AD-FS-Requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>Requisiti del certificato  
I certificati rivestono il ruolo più importante nel rendere sicure le comunicazioni tra server federativi, proxy applicazione Web, applicazioni compatibili con claims\ e i client del Web. I requisiti per i certificati variano, a seconda se si configura un server federativo o un computer proxy, come descritto in questa sezione.  
  
**Certificati dei server federativi**  
  
|||  
|-|-|  
|**Tipo di certificato**|**Requisiti, supporto e informazioni importanti**|  
|**Sicuro dei certificati SSL \(SSL\):** si tratta di un certificato SSL standard che viene utilizzato per proteggere le comunicazioni tra client e server federativi.|-Questo certificato deve essere un pubblicamente trusted\ * X509 certificato v3.<br />-Tutti i client che accedono a qualsiasi endpoint ADFS devono considerare attendibile questo certificato. Si consiglia di utilizzare i certificati vengono rilasciati da un'autorità di certificazione pubblica \(third\-party\) \(CA\). È possibile utilizzare un certificato SSL firmato e correttamente nel server federativo in un ambiente di testing. Tuttavia, per un ambiente di produzione, si consiglia di ottenere il certificato da una CA pubblica.<br />-Supporta qualsiasi dimensione di chiave supportati da Windows Server 2012 R2 per i certificati SSL.<br />-Non supportati i certificati che utilizzano chiavi CNG.<br />-Quando utilizzato insieme all'area di lavoro Join\/Device Registration Service, il nome alternativo del soggetto del certificato SSL per il servizio ADFS deve contenere il valore enterpriseregistration seguito dal nome dell'entità utente \(UPN\) suffisso dell'organizzazione, ad esempio, enterpriseregistration.contoso.com.<br />-Certificati con caratteri jolly sono supportati. Quando si crea la farm ADFS, verrà richiesto di fornire il nome del servizio per il servizio ADFS \ (ad esempio, **adfs.contoso.com**.<br />-Si consiglia di utilizzare lo stesso certificato SSL per il Proxy dell'applicazione Web. Si tratta tuttavia **necessari** essere lo stesso per supportare gli endpoint di autenticazione integrata di Windows tramite il Proxy dell'applicazione Web e quando l'autenticazione di protezione estesa è attivato \(default setting\).<br />-Il nome del soggetto del certificato viene utilizzato per rappresentare il nome servizio federativo per ogni istanza di ADFS da distribuire. Per questo motivo, potresti voler consigliabile scegliere un nome soggetto qualsiasi nuova CA\ emissione di certificati che meglio rappresenti il nome della società o organizzazione partner.<br />    L'identità del certificato deve corrispondere al nome di servizio federativo \ (ad esempio, fs.contoso.com\). L'identità è un'estensione di nome alternativo oggetto di tipo dNSName o, se non sono presenti voci di nome alternativo del soggetto, il nome del soggetto specificato come un nome comune. Più voci di nome alternativo soggetto possono essere presenti nel certificato, purché una di esse corrisponda il nome del servizio federativo.<br />-   **Importante:** si consiglia vivamente di usare lo stesso certificato SSL in tutti i nodi della farm ADFS, nonché tutti i proxy applicazione Web della farm di ADFS.|  
|**Certificato di comunicazione del servizio:** questo certificato consente di sicurezza dei messaggi WCF per proteggere le comunicazioni tra server federativi.|-Per impostazione predefinita, il certificato SSL viene utilizzato come certificato per le comunicazioni del servizio.  Ma è anche possibile configurare un altro certificato come certificato di comunicazione del servizio.<br />-   **Importante:** se si utilizza il certificato SSL come certificato di comunicazione del servizio, alla scadenza del certificato SSL, assicurarsi di configurare il certificato SSL rinnovato come certificato di comunicazione del servizio. Ciò non avviene automaticamente.<br />-Questo certificato deve essere considerato attendibile dai client di ADFS che utilizzano la protezione dei messaggi WCF.<br />-Si consiglia di utilizzare un certificato di autenticazione server rilasciato da un'autorità di certificazione pubblica \(third\-party\) \(CA\).<br />-Il certificato di comunicazione del servizio non può essere un certificato che utilizza chiavi CNG.<br />-Questo certificato può essere gestito utilizzando la console di gestione di ADFS.|  
|**Certificato di firma Token\:** questo è un standard X509 certificato utilizzato per firmare in modo sicuro tutti i token che il server federativo rilascia.|-Per impostazione predefinita, ADFS crea un certificato firmato e con chiavi a 2048 bit.<br />-Certificati CA rilasciato sono inoltre supportati e possono essere modificati utilizzando la gestione di ADFS snap-in<br />-Ha rilasciata i certificati CA necessario archiviata e accessibili tramite un Provider di crittografia CSP.<br />-Il certificato di firma di token non può essere un certificato che utilizza chiavi CNG.<br />-ADFS non richiede certificati registrati esternamente per la firma di token.<br />    ADFS rinnova automaticamente, questi certificati con firma e prima della scadenza, prima configurazione nuovi certificati come certificati secondari per consentire ai partner di utilizzarli, quindi capovolgimento a primario in un processo di attivazione automatica del certificato. Si consiglia di utilizzare l'impostazione predefinita, i certificati generati automaticamente per la firma di token.<br />    Se l'organizzazione dispone di criteri che richiedono certificati diversi da configurare per la firma di token, è possibile specificare i certificati al momento dell'installazione tramite Powershell \ (usare il parametro – SigningCertificateThumbprint il cmdlet\ Install\-AdfsFarm).  Dopo l'installazione, è possibile visualizzare e gestire certificati di firma del token utilizzando la console di gestione di ADFS o i cmdlet di Powershell set-AdfsCertificate e get-AdfsCertificate.<br />    Quando vengono utilizzati i certificati registrati esternamente per la firma di token, ADFS non esegue rinnovo automatico del certificato o rollover.  Questo processo deve essere eseguito da un amministratore.<br />    Per consentire il rollover dei certificati prossimi alla scadenza, è possibile configurare un certificato di firma di token secondario in ADFS. Per impostazione predefinita, tutti i certificati di firma del token vengono pubblicati nei metadati federativi, ma solo il primario token\ certificato di firma viene utilizzato da ADFS per firmare effettivamente i token.|  
|**Certificato Token\ decryption\ la crittografia:** è un standard X509 certificato che utilizzata per crittografare/decrypt\ i token in ingresso. Viene pubblicato anche nei metadati federativi.|-Per impostazione predefinita, ADFS crea un certificato firmato e con chiavi a 2048 bit.<br />-Certificati CA rilasciato sono inoltre supportati e possono essere modificati utilizzando la gestione di ADFS snap-in<br />-Ha rilasciata i certificati CA necessario archiviata e accessibili tramite un Provider di crittografia CSP.<br />-Il certificato token\ decryption\ la crittografia non può essere un certificato che utilizza chiavi CNG.<br />-Per impostazione predefinita, ADFS genera e utilizza i proprio, certificati generati e internamente firmato e per la decrittografia di token.  ADFS non richiede certificati registrati esternamente per questo scopo.<br />    Inoltre, ADFS rinnova automaticamente, questi certificati con firma e prima della scadenza.<br />    **Si consiglia di utilizzare l'impostazione predefinita, i certificati generati automaticamente per la decrittografia di token.**<br />    Se l'organizzazione dispone di criteri che richiedono certificati diversi da configurare per la decrittografia di token, è possibile specificare i certificati al momento dell'installazione tramite Powershell \ (usare il parametro – DecryptionCertificateThumbprint il cmdlet\ Install\-AdfsFarm).  Dopo l'installazione, è possibile visualizzare e gestire i certificati di decrittografia di token utilizzando la console di gestione di ADFS o i cmdlet di Powershell set-AdfsCertificate e get-AdfsCertificate.<br />    **Quando vengono utilizzati i certificati registrati esternamente per la decrittografia di token, ADFS non esegue rinnovo automatico del certificato. Questo processo deve essere eseguito da un amministratore**.<br />-L'account del servizio ADFS deve avere accesso alla chiave privata del certificato di firma token\ nell'archivio personale del computer locale. Questo viene preso in considerazione dal programma di installazione. È anche possibile utilizzare la gestione di ADFS snap-in per verificare l'accesso se successivamente si modifica il certificato di firma token\.|  
  
> [!CAUTION]  
> I certificati utilizzati per la firma token\ e token\ decrypting\/crittografia sono fondamentali per la stabilità del servizio federativo. I clienti di gestire i propri certificati di firma token\ & token\ decrypting\/crittografia devono assicurarsi che questi certificati vengono sottoposti a backup e sono disponibili in modo indipendente durante un evento di ripristino.  
  
> [!NOTE]  
> In ADFS è possibile modificare il livello di Secure Hash Algorithm \(SHA\) che viene utilizzato per le firme digitali \(more secure\) SHA\-1 o SHA\-256. ADFS non supporta l'uso di certificati con altri metodi hash, ad esempio MD5 \ (l'algoritmo hash predefinito usato con il tool\ della riga di comando Makecert.exe). Per una protezione ottimale, ti consigliamo di usare SHA\-256 \ (che per impostazione default\) per tutte le firme. Per usare solo in scenari in cui è necessario interoperare con un prodotto che non supporta le comunicazioni tramite SHA\-256, ad esempio un versioni fornitori non Microsoft di prodotto o legacy di ADFS, è consigliabile SHA\-1.  
  
> [!NOTE]  
> Dopo aver ricevuto un certificato da una CA, assicurarsi che tutti i certificati vengono importati nell'archivio certificati personali del computer locale. È possibile importare i certificati nell'archivio personale con snap-in MMC certificati.  
  
## <a name="BKMK_2"></a>Requisiti hardware  
Si applicano i seguenti requisiti hardware minimi e consigliati per i server federativi AD FS in Windows Server 2012 R2:  
  
||||  
|-|-|-|  
|**Requisito hardware**|**Requisito minimo**|**Requisito consigliato**|  
|Velocità della CPU|Processore a 64 bit da 1,4 GHz|Quad\-core, 2 GHz|  
|RAM|512 MB|4 GB|  
|Spazio su disco|32 GB|100 GB|  
  
## <a name="BKMK_3"></a>Requisiti software  
I seguenti requisiti di ADFS sono per le funzionalità di server sono integrata nel sistema operativo Windows Server® 2012 R2:  
  
-   Per l'accesso extranet, è necessario distribuire il servizio ruolo Proxy applicazione Web \ - parte del ruolo del server di accesso remoto di Windows Server® 2012 R2. Le versioni precedenti di un proxy server federativo non sono supportate con AD FS in Windows Server® 2012 R2.  
  
-   Un server federativo e il servizio ruolo Proxy applicazione Web non può essere installati nello stesso computer.  
  
## <a name="BKMK_4"></a>Requisiti di Active Directory  
**Requisiti del controller di dominio**  
  
I controller di dominio in tutti i domini utente e il dominio a cui sono stati aggiunti i server ADFS devono essere in esecuzione Windows Server 2008 o versione successiva.  
  
> [!NOTE]  
> Tutto il supporto per gli ambienti con controller di dominio di Windows Server 2003 terminerà dopo l'estesi supportano End data per Windows Server 2003. I clienti consiglia di aggiornare i controller di dominio appena possibile. Visitare [questa pagina](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO) per ulteriori informazioni sul ciclo di vita del supporto Microsoft. Per problemi rilevati che sono specifiche per ambienti di controller di dominio di Windows Server 2003, correzioni verranno generate solo per problemi di sicurezza e se è possibile emettere una correzione prima della scadenza di estendere il supporto per Windows Server 2003.  
  
**Requisiti a livello functional\ dominio**  
  
Tutti i domini di account utente e il dominio a cui sono stati aggiunti i server ADFS deve funzionare a livello funzionale di dominio di Windows Server 2003 o versione successiva.  
  
La maggior parte delle funzionalità di ADFS non richiedono modifiche functional\ a livello di dominio Active Directory per il corretto funzionamento. Tuttavia, funzionalità del dominio Windows Server 2008 livello o versione successiva è richiesto per l'autenticazione del certificato client per il corretto funzionamento se il certificato è mappato in modo esplicito a un account utente di dominio Active Directory.  
  
**Requisiti dello schema**  
  
-   ADFS non richiede modifiche dello schema o modifiche functional\ livello di dominio Active Directory.  
  
-   Per utilizzare la funzionalità aggiunta alla rete aziendale, è necessario impostare lo schema dell'insieme di strutture collegati al server AD FS per Windows Server 2012 R2.  
  
**Requisiti dell'account del servizio**  
  
-   Qualsiasi account di servizio standard può essere utilizzato come account di servizio per AD FS. Sono supportati anche gli account del servizio gestito del gruppo. Questa operazione richiede almeno un controller di dominio \ (è consigliabile distribuire due o more\) che esegue Windows Server 2012 o versione successiva.  
  
-   Per l'autenticazione Kerberos tra client aggiunti al domain \ e AD FS, il ' HOST\ / < adfs\_service\_name >' deve essere registrato come un nome SPN per l'account del servizio. Per impostazione predefinita, ADFS configurerà questo quando si crea una nuova farm ADFS se dispone di autorizzazioni sufficienti per eseguire questa operazione.  
  
-   L'account del servizio ADFS deve essere considerato attendibile in ogni dominio utente che contiene gli utenti l'autenticazione per il servizio ADFS.  
  
**Requisiti di dominio**  
  
-   Tutti i server ADFS devono essere di un dominio di Active Directory.  
  
-   Tutti i server ADFS all'interno di una farm devono essere distribuiti in un singolo dominio.  
  
-   Il dominio che fanno parte di server ADFS deve considerare attendibile ogni dominio dell'account utente che contenga gli utenti l'autenticazione per il servizio ADFS.  
  
**Requisiti di più foreste**  
  
-   Deve considerare attendibile il dominio che fanno parte dei server ADFS per ogni dominio dell'account utente o un insieme di strutture che contiene gli utenti l'autenticazione per il servizio ADFS.  
  
-   L'account del servizio ADFS deve essere considerato attendibile in ogni dominio utente che contiene gli utenti l'autenticazione per il servizio ADFS.  
  
## <a name="BKMK_5"></a>Requisiti del database di configurazione  
Di seguito sono i requisiti e limitazioni applicabili in base al tipo di archivio di configurazione:  
  
**DATABASE INTERNO DI WINDOWS**  
  
-   Se si dispone di un massimo di 100 trust della relying party, una farm WID ha un limite di 30 server federativi.  
  
-   Profilo di risoluzione artefatto SAML 2.0 non è supportato nel database di configurazione database interno di Windows.  Il rilevamento riproduzione token non è supportato nel database di configurazione database interno di Windows. Questa funzionalità viene solo utilizzata solo in scenari in cui ADFS è funge da provider di federazione e utilizzo di token di sicurezza da provider di attestazioni esterno.  
  
-   Centri di distribuzione di server ADFS in dati distinto per il failover o bilanciamento del carico geografico è supportato, purché il numero di server senza superare i 30.  
  
Nella tabella seguente fornisce un riepilogo di utilizzo di una farm database interno di Windows.  Usarlo per pianificare l'implementazione.  
  
||||  
|-|-|-|  
||1 \-100 attendibilità della Relying party|Più di 100 attendibilità della Relying party|  
|1 \-30 AD FS nodi|Database interno di Windows supportati|Non supportato utilizzando WID \-SQL necessarie|  
|Più di 30 AD FS nodi|Non supportato utilizzando WID \-SQL necessarie|Non supportato utilizzando WID \-SQL necessarie|  
  
**SQL Server**  
  
Per ADFS in Windows Server 2012 R2, è possibile utilizzare SQL Server 2008 e versioni successive  
  
## <a name="BKMK_6"></a>Requisiti del browser  
Quando viene eseguita l'autenticazione di ADFS tramite un browser o un controllo browser, il browser deve essere conforme ai requisiti seguenti:  
  
-   È necessario abilitare JavaScript  
  
-   È necessario attivare i cookie  
  
-   \(SNI\) indicazione nome server deve essere supportata.  
  
-   Per l'autenticazione del certificato dispositivo & certificato utente \ (tra di join all'area di lavoro), il browser deve supportare l'autenticazione del certificato client SSL  
  
Diverse piattaforme e browser chiave sottoposte a convalida per il rendering e funzionalità, i cui dettagli sono elencati di seguito. Browser e dispositivi che non trattate in questa tabella sono ancora supportati se soddisfano i requisiti sopra indicati:  
  
|||  
|-|-|  
|**Browser**|**Piattaforme**|  
|INTERNET EXPLORER 10.0|Windows 7, Windows 8.1, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2|  
|INTERNET EXPLORER 11.0|Windows7, Windows 8.1, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2|  
|Gestore di autenticazioni Web di Windows|Windows 8.1|  
|Firefox \[v21\]|Windows 7, Windows 8.1|  
|Safari \[v7\]|iOS 6, Mac OS\ X 10.7|  
|Chrome \[v27\]|Windows 7, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Mac OS\-X 10.7|  
  
> [!IMPORTANT]  
> Problema noto \-Firefox: funzionalità area di lavoro che identifica il dispositivo utilizzando il certificato di dispositivo non è funzionale su piattaforme Windows. Firefox non supporta attualmente esecuzione autenticazione del certificato client SSL utilizzando i certificati di provisioning nell'archivio certificati utente sui client Windows.  
  
**Cookie**  
  
ADFS crea cookie permanenti e basati su session\ che devono essere archiviati nel computer client per fornire accesso singolo, accesso in uscita \(SSO\) accesso-on e altre funzionalità. Di conseguenza, il browser client deve essere configurato per accettare i cookie. I cookie usati per l'autenticazione sono sempre cookie di sessione \(HTTPS\) Secure Hypertext Transfer Protocol scritti per il server di origine. Se il browser client non è configurato per abilitare questi cookie, ADFS non può funzionare correttamente. I cookie permanenti vengono usati per mantenere la selezione dell'utente del provider di attestazioni. È possibile disabilitarli usando un'impostazione di configurazione nel file di configurazione per le pagine di accesso AD FS. Supporto per TLS\/SSL è necessario per motivi di sicurezza.  
  
## <a name="BKMK_extranet"></a>Requisiti di rete Extranet  
Per fornire l'accesso extranet al servizio ADFS, è necessario distribuire il servizio ruolo Proxy applicazione Web come ruolo pubblico extranet che le richieste di autenticazione proxy in modo sicuro per il servizio ADFS. In questo modo l'isolamento degli endpoint del servizio ADFS nonché isolamento di tutte le chiavi di sicurezza \ (ad esempio token certificates\ firma) dalle richieste provenienti da internet. Inoltre, funzionalità, ad esempio il blocco degli Account Extranet Soft richiedono l'utilizzo di Proxy applicazione Web. Per ulteriori informazioni sul Proxy dell'applicazione Web, vedere [Proxy applicazione Web](https://technet.microsoft.com/library/dn584107.aspx).  
  
Se si desidera utilizzare un proxy di terze parti third\ per l'accesso extranet, questo proxy third\ parti deve supportare il protocollo definito nella [http:///\/download.microsoft.com\/download\/9\/5\/E\/95EF66AF\-9026\-4BB0\-A41D\-A4F81802D92C\/%5bMS\-ADFSPIP%5d.pdf](https://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-ADFSPIP%5d.pdf).  
  
## <a name="BKMK_7"></a>Requisiti di rete  
La configurazione dei seguenti servizi di rete in modo appropriato è fondamentale per la corretta distribuzione di ADFS nell'organizzazione:  
  
**Configurazione di Firewall aziendale**  
  
Il firewall si trova tra il Proxy dell'applicazione Web e la server farm federativa e il firewall tra i client e il Proxy dell'applicazione Web devono avere la porta TCP 443 abilitato in ingresso.  
  
Inoltre, se l'autenticazione del certificato client utente \ (autenticazione clientTLS utilizzando X509 certificates\ utente) è obbligatorio, ADFS in Windows Server 2012 R2 richiede che sia attivato 49443 la porta TCP in ingresso nel firewall tra i client e il Proxy dell'applicazione Web. Questo non è richiesto nel firewall tra il Proxy dell'applicazione Web e il servers\ federativo).  
  
**Configurazione del DNS**  
  
-   Per l'accesso intranet, tutti i client che accedono servizio ADFS all'interno di \(intranet\) la rete aziendale interna devono essere in grado di risolvere il nome del servizio ADFS \ (nome fornito per il certificate\ SSL) per il bilanciamento del carico per i server ADFS o il server AD FS.  
  
-   Per l'accesso extranet, tutti i client che accedono servizio ADFS dall'esterno \(extranet\/internet\) la rete aziendale devono essere in grado di risolvere il nome del servizio ADFS \ (nome fornito per il certificate\ SSL) per il bilanciamento del carico per il server Proxy applicazione Web o il server Proxy applicazione Web.  
  
-   Per l'accesso extranet funzionare correttamente, ogni server di Proxy applicazione Web nella rete Perimetrale deve essere in grado di risolvere il nome del servizio ADFS \ (nome fornito per il certificate\ SSL) per il bilanciamento del carico per i server ADFS o il server AD FS. Ciò può essere ottenuto tramite un server DNS alternativo nella rete Perimetrale o modificando la risoluzione di un server locale utilizzando file host.  
  
-   Per l'autenticazione integrata di Windows lavorare all'interno della rete e all'esterno della rete per un subset degli endpoint esposti tramite il Proxy dell'applicazione Web, è necessario utilizzare un \(not CNAME\) record A per puntare al bilanciamento del carico.  
  
Per informazioni sulla configurazione DNS aziendale per il servizio federativo e il servizio DRS, vedere [configurare DNS aziendale per il servizio federativo e DRS](https://technet.microsoft.com/library/dn486786.aspx).  
  
Per informazioni sulla configurazione DNS aziendale per i proxy applicazione Web, vedere la sezione "Configurare DNS" in [passaggio 1: configurare l'infrastruttura del Proxy applicazione Web](https://technet.microsoft.com/library/dn383644.aspx).  
  
Per informazioni su come configurare un indirizzo IP del cluster o cluster FQDN utilizzando bilanciamento carico di rete, vedere Specifica dei parametri del Cluster in [http:///\/go.microsoft.com\/fwlink\/? LinkId\ = 75282](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
## <a name="BKMK_8"></a>Requisiti dell'archivio attributi  
ADFS richiede almeno un archivio di attributi da utilizzare per l'autenticazione degli utenti e l'estrazione delle attestazioni di sicurezza per gli utenti. Per un elenco di attributi di archivi che supportano ADFS, vedere [il ruolo degli archivi attributi](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md).  
  
> [!NOTE]  
> ADFS crea automaticamente un archivio di attributi "Active Directory", per impostazione predefinita. Requisiti dell'archivio attributi dipendono se l'organizzazione funge da partner account \ (che ospita la federazione MSOCache\All Users \) o il partner risorse \ (che ospita il application\ federati).  
  
**Archivi di attributi LDAP**  
  
Quando si utilizzano altri Lightweight Directory Access Protocol \ archivi di attributi \-based (LDAP\), è necessario connettersi a un server LDAP che supporta l'autenticazione integrata di Windows. La stringa di connessione LDAP deve inoltre essere scritto nel formato di URL LDAP, come descritto nella RFC 2255.  
  
È inoltre necessario che l'account del servizio per il servizio ADFS ha il diritto di recuperare le informazioni nell'archivio di attributi LDAP.  
  
**Archivi di attributi di SQL Server**  
  
Per ADFS in Windows Server 2012 R2 per il corretto funzionamento, i computer che ospitano l'archivio di attributi di SQL Server devono essere in esecuzione Microsoft SQL Server 2008 o versione successiva. Quando si usano archivi attributi basati su SQL\, è necessario configurare una stringa di connessione.  
  
**Archivi di attributi personalizzati**  
  
È possibile sviluppare archivi di attributi personalizzati per abilitare scenari avanzati.  
  
-   Il linguaggio dei criteri integrato in ADFS può fare riferimento archivi di attributi personalizzati, in modo che può essere migliorato uno qualsiasi dei seguenti scenari:  
  
    -   Creazione di attestazioni per un utente autenticato localmente  
  
    -   Integrazione di attestazioni per un utente autenticato esternamente  
  
    -   Autorizzazione di un utente di ottenere un token  
  
    -   Autorizzazione di un servizio per ottenere un token sul comportamento di un utente  
  
    -   Emittente dati aggiuntivi nei token di sicurezza emesso da ADFS per le relying party.  
  
-   Tutti gli archivi di attributi personalizzati devono essere compilati su .NET 4.0 o versione successiva.  
  
Quando si lavora con un archivio attributi personalizzato, si potrebbe anche essere necessario configurare una stringa di connessione. In tal caso, è possibile immettere un codice personalizzato di propria scelta che consente la connessione all'archivio di attributi personalizzati. La stringa di connessione in questa situazione è un set delle / coppie nome valore che vengono interpretate come implementato dallo sviluppatore dell'archivio di attributi personalizzati. Per ulteriori informazioni sullo sviluppo e l'uso di archivi di attributi personalizzati, vedere [Cenni preliminari sull'archivio di attributi](https://go.microsoft.com/fwlink/?LinkId=190782).  
  
## <a name="BKMK_9"></a>Requisiti dell'applicazione  
ADFS supporta claims\ compatibile con applicazioni che utilizzano i protocolli seguenti:  
  
-   WS-Federation  
  
-   WS-Trust  
  
-   Protocollo SAML 2.0 utilizzo dei profili IDPLite, SPLite & eGov1.5.  
  
-   Profilo di concessione di autorizzazione OAuth 2.0  
  
ADFS supporta inoltre l'autenticazione e autorizzazione per le applicazioni compatibili claims\ che sono supportate da Proxy applicazione Web.  
  
## <a name="BKMK_10"></a>Requisiti di autenticazione  
**Servizi di dominio Active Directory \(Primary Authentication\) autenticazione**  
  
Per l'accesso intranet, sono supportati i seguenti meccanismi di autenticazione standard per Active Directory:  
  
-   Autenticazione integrata di Windows utilizza Negotiate per Kerberos e NTLM  
  
-   Autenticazione basata su form tramite username\/password  
  
-   Autenticazione dei certificati utilizzando certificati mappati ad account utente di dominio Active Directory  
  
Per l'accesso extranet, sono supportati i meccanismi di autenticazione seguenti:  
  
-   Autenticazione basata su form tramite username\/password  
  
-   Autenticazione dei certificati utilizzando certificati mappati ad account utente di dominio Active Directory  
  
-   Autenticazione integrata di Windows utilizzando Negotiate \(NTLM only\) per gli endpoint WS-Trust che accettano l'autenticazione integrata di Windows.  
  
Per l'autenticazione del certificato:  
  
-   Estende alla smart card che può essere un pin protetto.  
  
-   L'interfaccia utente grafica per l'utente di immettere il pin non viene fornito da ADFS ed è necessario far parte del sistema operativo client che viene visualizzato quando si utilizza client TLS.  
  
-   Il lettore e il provider del servizio di crittografia \(CSP\) per la smart card devono funzionare nel computer in cui si trova il browser.  
  
-   Il certificato smart card deve conduce a una radice attendibile in tutti i server ADFS e server Proxy applicazione Web.  
  
-   Il certificato deve essere mappato all'account utente di dominio Active Directory utilizzando uno dei metodi seguenti:  
  
    -   Il nome soggetto del certificato corrisponde al nome distinto LDAP di un account utente di dominio Active Directory.  
  
    -   L'estensione relativa al nome alternativo soggetto del certificato ha dell'entità utente nome \(UPN\) di un account utente di dominio Active Directory.  
  
Per l'autenticazione integrata di Windows senza problemi utilizzando Kerberos nella rete intranet,  
  
-   È necessario per il nome del servizio da parte di siti attendibili o ai siti Intranet locali.  
  
-   Inoltre, il HOST\ / < adfs\_service\_name > nome SPN deve essere impostato sull'account di servizio che viene eseguita la farm ADFS in.  
  
**Autenticazione a più fattori**  
  
ADFS supporta l'autenticazione aggiuntiva \ (oltre l'autenticazione principale supportata da AD DS\) con un provider di modello quale vendors\/utenti possono creare i propri scheda autenticazione più fattori che un amministratore può registrare e utilizzare durante l'accesso.  
  
Ogni scheda di autenticazione a più fattori deve essere compilato in .NET 4.5.  
  
Per ulteriori informazioni sull'autenticazione a più fattori, vedere [gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni sensibili](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).  
  
**Autenticazione del dispositivo**  
  
ADFS supporta l'autenticazione del dispositivo utilizzando i certificati di provisioning dal servizio Registrazione dispositivi durante l'atto di un'unione di dispositivo all'area di lavoro degli utenti finali.  
  
## <a name="BKMK_11"></a>Requisiti di join all'area di lavoro  
Gli utenti finali possono all'area di lavoro i propri dispositivi a un'organizzazione tramite ADFS. Questa funzionalità è supportata dal servizio Registrazione dispositivi di ADFS. Di conseguenza, gli utenti finali ottengono l'ulteriore vantaggio di SSO tra le applicazioni supportate da ADFS. Inoltre, gli amministratori possono gestire i rischi limitando l'accesso alle applicazioni solo ai dispositivi che sono stati aggiunto all'organizzazione all'area di lavoro. Di seguito sono i requisiti seguenti per abilitare questo scenario.  
  
-   ADFS supporta all'area di lavoro per Windows 8.1 e IOS 5 \ +  
  
-   Per utilizzare la funzionalità aggiunta alla rete aziendale, lo schema dell'insieme di strutture collegati al server ADFS deve essere Windows Server 2012 R2.  
  
-   Il nome alternativo del soggetto del certificato SSL per il servizio ADFS deve contenere il valore enterpriseregistration seguito dal nome dell'entità utente \(UPN\) suffisso dell'organizzazione, ad esempio, enterpriseregistration.  
  
## <a name="BKMK_12"></a>Requisiti di crittografia  
La tabella seguente fornisce informazioni sul supporto di crittografia aggiuntivi nel token di ADFS firma, funzionalità encryption\/decrittografia di token:  
  
||||  
|-|-|-|  
|**Algoritmo**|**Lunghezze di chiave**|**Applications\/Protocols\/commenti**|  
|TripleDES – Default 192 \ (supportati 192 – 256\) \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#tripledes\-cbc](http://www.w3.org/2001/04/xmlenc)|>\= 192|Algoritmo supportato per crittografare il token di sicurezza.|  
|Aes128 \-http:///\/ www.w3.org \/2001\/04\/xmlenc\#aes128\-cbc|128|Algoritmo supportato per crittografare il token di sicurezza.|  
|AES192 \-http:///\/ www.w3.org \/2001\/04\/xmlenc\#aes192\-cbc|192|Algoritmo supportato per crittografare il token di sicurezza.|  
|AES256 \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#aes256\-cbc](http://www.w3.org/2001/04/xmlenc)|256|**Predefinito**. Algoritmo supportato per crittografare il token di sicurezza.|  
|Aes128keywrap \-http:///\/ www.w3.org \/2001\/04\/xmlenc\#kw\-tripledes|Tutte le dimensioni di chiave supportate da .NET 4.0\+|Algoritmo supportato per crittografare la chiave simmetrica che crittografa un token di sicurezza.|  
|AES128KeyWrap \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#kw\-aes128](http://www.w3.org/2001/04/xmlenc)|128|Algoritmo supportato per crittografare la chiave simmetrica che crittografa il token di sicurezza.|  
|AES192KeyWrap \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#kw\-aes192](http://www.w3.org/2001/04/xmlenc)|192|Algoritmo supportato per crittografare la chiave simmetrica che crittografa il token di sicurezza.|  
|AES256KeyWrap \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#kw\-aes256](http://www.w3.org/2001/04/xmlenc)|256|Algoritmo supportato per crittografare la chiave simmetrica che crittografa il token di sicurezza.|  
|RsaV15KeyWrap \-http:///\/ www.w3.org \/2001\/04\/xmlenc\#rsa\-1\_5|1024|Algoritmo supportato per crittografare la chiave simmetrica che crittografa il token di sicurezza.|  
|RsaOaepKeyWrap \- [http:///\/ www.w3.org \/2001\/04\/xmlenc\#rsa\-oaep\-mgf1p](http://www.w3.org/2001/04/xmlenc)|1024|Per impostazione predefinita. Algoritmo supportato per crittografare la chiave simmetrica che crittografa il token di sicurezza.|  
|SHA1\ http: / / / \ / www.w3.org \/PICS\/DSig\/SHA1\_1\_0.html|N \ / A|Utilizzato dal Server ADFS nella generazione di elementi SourceId: In questo scenario, il servizio token di sicurezza utilizza SHA1 \ (per la raccomandazione nello standard\ di SAML 2.0) per creare un valore breve a 160 bit per l'ID di origine dell'elemento.<br /><br />Anche utilizzato dall'agente web ADFS \ (componente legacy dal WS2003 timeframe\) per identificare le modifiche in fase di "ultimo aggiornamento" valore in modo da determinare quando aggiornare le informazioni dal servizio token di sicurezza.|  
|SHA1withRSA\-<br /><br />http:///\/ www.w3.org \/PICS\/DSig\/RSA\-SHA1\_1\_0.html|N \ / A|Utilizzata quando il Server ADFS convalida la firma di SAML AuthenticationRequest, firmare la richiesta di risoluzione artefatto o la risposta, creare il certificato di firma token\.<br /><br />In questi casi, il valore predefinito è SHA256 e SHA1 viene utilizzato solo se il partner \(relying party\) non è in grado di supportare SHA256 e devono utilizzare SHA1.|  
  
## <a name="BKMK_13"></a>Requisiti di autorizzazione  
L'amministratore che esegue l'installazione e la configurazione iniziale di ADFS deve disporre delle autorizzazioni di amministratore di dominio nel dominio locale \ (in altre parole, il dominio a cui il server federativo viene aggiunto a. \)  
  
## <a name="see-also"></a>Vedere anche  
[Guida alla progettazione di ADFS in Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

