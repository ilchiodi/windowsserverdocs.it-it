---
ms.assetid: 8ce6e7c4-cf8e-4b55-980c-048fea28d50f
title: Server farm federativa che usa SQL Server
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b299ddc823b3fbbd5818f96202e3c01faf0762d7
ms.sourcegitcommit: 9687d3eb221b89061a48bf1e73fb3b25bee69f9a
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/28/2020
ms.locfileid: "78169601"
---
# <a name="ad-fs-requirements"></a>Requisiti per ADFS

Di seguito sono illustrati i requisiti che è necessario rispettare i durante la distribuzione di ADFS:  
  
-   [Requisiti del certificato](AD-FS-Requirements.md#BKMK_1)  
  
-   [Requisiti hardware](AD-FS-Requirements.md#BKMK_2)  
  
-   [Requisiti software](AD-FS-Requirements.md#BKMK_3)  
  
-   [Requisiti di Active Directory Domain Services](AD-FS-Requirements.md#BKMK_4)  
  
-   [Requisiti del database di configurazione](AD-FS-Requirements.md#BKMK_5)  
  
-   [Requisiti del browser](AD-FS-Requirements.md#BKMK_6)  
  
-   [Requisiti Extranet](AD-FS-Requirements.md#BKMK_extranet)  
  
-   [Requisiti di rete](AD-FS-Requirements.md#BKMK_7)  
  
-   [Requisiti dell'archivio attributi](AD-FS-Requirements.md#BKMK_8)  
  
-   [Requisiti delle applicazioni](AD-FS-Requirements.md#BKMK_9)  
  
-   [Requisiti di autenticazione](AD-FS-Requirements.md#BKMK_10)  
  
-   [Requisiti per l'aggiunta all'area di lavoro](AD-FS-Requirements.md#BKMK_11)  
  
-   [Requisiti di crittografia](AD-FS-Requirements.md#BKMK_12)  
  
-   [Requisiti di autorizzazione](AD-FS-Requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>Requisiti del certificato  
I certificati rivestono il ruolo più importante nel rendere sicure le comunicazioni tra la federazione attestazioni, i server proxy applicazione Web,\-applicazioni e client Web. I requisiti per i certificati variano, a seconda se Configura un server federativo o un computer proxy, come descritto in questa sezione.  
  
**Certificati server federativi**  
  
|||  
|-|-|  
|**Tipo di certificato**|**Requisiti, supporto & informazioni**|  
|**Secure Sockets Layer \(SSL\) certificato:** si tratta di un certificato SSL standard che viene utilizzato per proteggere le comunicazioni tra client e server federativi.|-Questo certificato deve essere pubblicamente attendibile\* X509 certificato v3.<br />-Tutti i client che accedono a qualsiasi endpoint ADFS devono considerare attendibile il certificato. Si consiglia di utilizzare i certificati emessi da un pubblico \(terzo\-entità\) autorità di certificazione \(CA\). È possibile utilizzare un self\-firmato SSL certificato correttamente nei federation server in un ambiente di testing. Tuttavia, per un ambiente di produzione, si consiglia di ottenere il certificato da una CA pubblica.<br />-Supporta qualsiasi dimensione di chiave supportati da Windows Server 2012 R2 per i certificati SSL.<br />-Non supportati i certificati che utilizzano chiavi CNG.<br />-Quando utilizzato insieme all'area di lavoro\/Device Registration Service, il nome alternativo soggetto del certificato SSL per il servizio ADFS deve contenere il valore enterpriseregistration seguito dal nome dell'entità utente \(UPN\) suffisso dell'organizzazione, ad esempio, enterpriseregistration.contoso.com.<br />-I certificati con caratteri jolly sono supportati. Quando si crea la farm di ADFS, verrà richiesto di fornire il nome del servizio per il servizio ADFS \(ad esempio, **adfs.contoso.com**.<br />-Si consiglia di utilizzare lo stesso certificato SSL per il Proxy dell'applicazione Web. Si tratta tuttavia **obbligatorio** siano gli stessi per supportare gli endpoint di autenticazione integrata di Windows tramite il Proxy dell'applicazione Web e quando è attivata l'autenticazione di protezione estesa \(impostazione\).<br />-Il nome del soggetto del certificato viene utilizzato per rappresentare il nome del servizio federativo per ogni istanza di ADFS da distribuire. Per questo motivo, è consigliabile scegliere un nome soggetto qualsiasi nuova autorità di certificazione\-ha rilasciato i certificati che meglio rappresenta il nome della società o organizzazione partner.<br />    L'identità del certificato deve corrispondere il nome del servizio federativo \(ad esempio, fs.contoso.com\). L'identità è un'estensione di nome alternativo oggetto di tipo dNSName o, se non sono presenti voci di nome alternativo del soggetto, il nome del soggetto specificato come nome comune. Più voci di nome alternativo soggetto possono essere presenti nel certificato, purché una di esse corrisponda al nome di servizio federativo.<br />-   **importante:** è consigliabile utilizzare lo stesso certificato SSL in tutti i nodi della farm di ad FS, nonché tutti i proxy applicazione Web nella farm di ad FS.|  
|**Certificato di comunicazione del servizio:** questo certificato consente la protezione dei messaggi WCF per proteggere le comunicazioni tra server federativi.|: Per impostazione predefinita, il certificato SSL viene utilizzato come certificato per le comunicazioni del servizio.  Ma è anche possibile configurare un altro certificato come certificato di comunicazione del servizio.<br />-   **importante:** se si usa il certificato SSL come certificato di comunicazione del servizio, quando il certificato SSL scade, assicurarsi di configurare il certificato SSL rinnovato come certificato di comunicazione del servizio. Questo non avviene automaticamente.<br />-Questo certificato deve essere considerato attendibile dai client di ADFS che utilizzano la sicurezza dei messaggi WCF.<br />-È consigliabile utilizzare un certificato di autenticazione server rilasciato da un pubblico \(terzo\-entità\) autorità di certificazione \(CA\).<br />-Il certificato di comunicazione del servizio non può essere un certificato che utilizza chiavi CNG.<br />-Questo certificato può essere gestito utilizzando la console di gestione di ADFS.|  
|**Token\-certificato di firma:** questo è un standard X509 certificato utilizzato per firmare in modo sicuro tutti i token che il server federativo rilascia.|: Per impostazione predefinita, ADFS crea un self\-firmati certificati con chiavi a 2048 bit.<br />-Certificati CA rilasciato sono inoltre supportati e possono essere modificati utilizzando lo snap di gestione di ADFS\-in<br />-Ha rilasciata i certificati CA necessario archiviata e accessibili tramite un Provider di crittografia CSP.<br />-Il certificato di firma di token non può essere un certificato che utilizza chiavi CNG.<br />-ADFS non richiede certificati registrati esternamente per la firma di token.<br />    ADFS rinnova automaticamente, questi self\-firmato certificati prima della scadenza, la prima configurazione nuovi certificati come certificati secondari per consentire ai partner di utilizzarli, capovolgimento a primario in un processo chiamato automatico del rollover dei certificati. È consigliabile utilizzare l'impostazione predefinita, i certificati generati automaticamente per la firma di token.<br />    Se l'organizzazione dispone di criteri che richiedono certificati diversi da configurare per la firma di token, è possibile specificare i certificati al momento dell'installazione tramite Powershell \(utilizzare il parametro – SigningCertificateThumbprint dell'installazione\-cmdlet AdfsFarm\).  Dopo l'installazione, è possibile visualizzare e gestire certificati di firma del token utilizzando la console di gestione di ADFS o i cmdlet di Powershell Set\-AdfsCertificate e Get\-AdfsCertificate.<br />    Quando vengono utilizzati i certificati registrati esternamente per la firma di token, ADFS non esegue il rinnovo automatico del certificato o rollover.  Questo processo deve essere eseguito da un amministratore.<br />    Per consentire rollover del certificato quando si è prossimi alla scadenza di un certificato, è possibile configurare un certificato di firma di token secondario in ADFS. Per impostazione predefinita, tutti i certificati di firma del token vengono pubblicati i metadati della federazione, ma solo il token primario\-certificato di firma viene utilizzato da ADFS per firmare effettivamente i token.|  
|**Token\-decrittografia\/certificato di crittografia:** questo è un standard X509 certificato che viene utilizzata per decrittografare\/crittografare i token in ingresso. Viene pubblicato anche nei metadati federativi.|: Per impostazione predefinita, ADFS crea un self\-firmati certificati con chiavi a 2048 bit.<br />-Certificati CA rilasciato sono inoltre supportati e possono essere modificati utilizzando lo snap di gestione di ADFS\-in<br />-Ha rilasciata i certificati CA necessario archiviata e accessibili tramite un Provider di crittografia CSP.<br />-Il token\-decrittografia\/certificato di crittografia non può essere un certificato che utilizza chiavi CNG.<br />: Per impostazione predefinita, ADFS genera e utilizza il proprio generato e internamente self\-firma i certificati per la decrittografia di token.  ADFS non richiede certificati registrati esternamente a questo scopo.<br />    Inoltre, ADFS rinnova automaticamente, questi self\-firmato certificati prima di scadere.<br />    **Si consiglia di usare i certificati predefiniti generati automaticamente per la decrittografia dei token.**<br />    Se l'organizzazione dispone di criteri che richiedono certificati diversi da configurare per la decrittografia di token, è possibile specificare i certificati al momento dell'installazione tramite Powershell \(utilizzare il parametro – DecryptionCertificateThumbprint dell'installazione\-cmdlet AdfsFarm\).  Dopo l'installazione, è possibile visualizzare e gestire i certificati di decrittografia dei token mediante la console di gestione di ADFS o i cmdlet di Powershell Set\-AdfsCertificate e Get\-AdfsCertificate.<br />    **Quando si usano i certificati registrati esternamente per la decrittografia dei token, ad FS non esegue il rinnovo automatico del certificato.  Questo processo deve essere eseguito da un amministratore**.<br />-L'account del servizio AD FS deve avere accesso al token\-chiave privata del certificato di firma nell'archivio personale del computer locale. Questo viene preso in considerazione dal programma di installazione. È inoltre possibile utilizzare lo snap di gestione di ADFS\-per verificare l'accesso se successivamente si modifica il token\-certificato di firma.|  
  
> [!CAUTION]  
> I certificati utilizzati per token\-firma e il token\-decrittografia\/la crittografia sono fondamentali per la stabilità del servizio federativo. I clienti di gestire i propri token\-firma & token\-decrittografia\/crittografare i certificati devono assicurarsi che questi certificati vengono sottoposti a backup e sono disponibili in modo indipendente durante un evento di ripristino.  
  
> [!NOTE]  
> In ADFS è possibile modificare l'algoritmo Hash sicuro \(SHA\) livello utilizzato per le firme digitali entrambi SHA\-1 o SHA\-256 \(più sicuro\). ADFS non supporta l'utilizzo di certificati con altri metodi hash, ad esempio MD5 \(l'algoritmo hash predefinito utilizzato con il comando Makecert.exe\-strumento da riga di\). Per una protezione ottimale, è consigliabile utilizzare SHA\-256 \(che per impostazione predefinita\) per tutte le firme. SHA\-1 è consigliato per l'uso solo in scenari in cui è necessario interoperare con un prodotto che non supporta le comunicazioni tramite SHA\-256, ad esempio non\-versioni Microsoft di prodotto o legacy di ADFS.  
  
> [!NOTE]  
> Dopo aver ricevuto un certificato da una CA, assicurarsi che tutti i certificati vengano importati nell'archivio certificati personali del computer locale. È possibile importare i certificati nell'archivio personale con lo snap MMC certificati\-in.  
  
## <a name="BKMK_2"></a>Requisiti hardware  
Si applicano i seguenti requisiti hardware minimi e consigliati per i server federativi ADFS in Windows Server 2012 R2:  
  
||||  
|-|-|-|  
|**Requisito hardware**|**Requisito minimo**|**Requisito consigliato**|  
|Velocità CPU|processore\-bit 1,4 GHz 64|Quad\-core, 2 GHz|  
|RAM|512 MB|4 GB|  
|Spazio su disco|32 GB|100 GB|  
  
## <a name="BKMK_3"></a>Requisiti software  
I seguenti requisiti di ADFS sono per la funzionalità server che è incorporata nel sistema operativo Windows Server® 2012 R2:  
  
-   Per l'accesso extranet, è necessario distribuire il servizio ruolo Proxy applicazione Web \- fa parte del ruolo del server di accesso remoto di Windows Server® 2012 R2. Le versioni precedenti di un proxy server federativo non sono supportate con AD FS in Windows Server® 2012 R2.  
  
-   Impossibile installare un server federativo e il servizio ruolo Proxy applicazione Web nello stesso computer.  
  
## <a name="BKMK_4"></a>Requisiti di Active Directory Domain Services  
**Requisiti del controller di dominio**  
  
I controller di dominio in tutti i domini utente e il dominio a cui sono stati aggiunti i server ADFS devono essere in esecuzione Windows Server 2008 o versione successiva.  
  
> [!NOTE]  
> Tutto il supporto per gli ambienti con controller di dominio di Windows Server 2003 terminerà dopo l'estesi supportano End Date per Windows Server 2003. I clienti consiglia di aggiornare i controller di dominio appena possibile. Visitare [questa pagina](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO) Per ulteriori informazioni sul ciclo di vita del supporto tecnico Microsoft. Per problemi rilevati che sono specifiche per ambienti di controller di dominio Windows Server 2003, correzioni verranno generate solo per i problemi di sicurezza e se è possibile emettere una correzione prima della scadenza di estendere il supporto per Windows Server 2003.  



>[!NOTE]
> AD FS richiede un controller di dominio scrivibile completo per funzionare anziché un controller di dominio di sola lettura. Se una topologia pianificata include un controller di dominio di sola lettura, il controller di dominio di sola lettura può essere utilizzato per l'autenticazione, ma l'elaborazione delle attestazioni LDAP richiederà una connessione al controller di dominio scrivibile.
  
**Requisiti a livello di funzionalità del dominio\-  
  
Tutti i domini di account utente e il dominio a cui sono stati aggiunti i server ADFS deve funzionare a livello funzionale di dominio di Windows Server 2003 o versione successiva.  
  
La maggior parte delle funzionalità di ADFS non richiedono funzionale di dominio Active Directory\-modifiche per il corretto funzionamento di livello. Tuttavia, per il corretto funzionamento dell'autenticazione del certificato client, se il certificato è mappato in modo esplicito a un account utente in Servizi di dominio Active Directory, sarà necessario il livello di funzionalità del dominio di Windows Server 2008 o superiore.  
  
**Requisiti dello schema**  
  
-   AD FS non richiede modifiche dello schema o modifiche a livello di\-funzionale per servizi di dominio Active Directory.  
  
-   Per utilizzare la funzionalità aggiunta alla rete aziendale, è necessario impostare lo schema dell'insieme di strutture collegati al server ADFS per Windows Server 2012 R2.  
  
**Requisiti dell'account del servizio**  
  
-   Qualsiasi account di servizio standard può essere utilizzato come account del servizio per ADFS. Sono supportati anche gli account del servizio gestito di gruppo. Questa operazione richiede almeno un controller di dominio \(è consigliabile distribuire due o più\) che esegue Windows Server 2012 o versione successiva.  
  
-   Per il funzionamento dell'autenticazione Kerberos tra il dominio\-client aggiunti e AD FS, è necessario che il nome\_\/HOST < ad FS\_nome > sia registrato come nome SPN nell'account del servizio. Per impostazione predefinita, ADFS configurerà questo quando si crea una nuova farm ADFS se dispone di autorizzazioni sufficienti per eseguire questa operazione.  
  
-   L'account del servizio ADFS deve essere considerato attendibile in ogni dominio utente che contiene gli utenti l'autenticazione per il servizio ADFS.  
  
**Requisiti di dominio**  
  
-   Tutti i server ADFS devono essere di un dominio di Active Directory.  
  
-   Tutti i server ADFS all'interno di una farm devono essere distribuiti in un singolo dominio.  
  
-   Il dominio che fanno parte dei server ADFS deve considerare attendibile ogni dominio dell'account utente che contenga gli utenti l'autenticazione per il servizio ADFS.  
  
**Requisiti per più foreste**  
  
-   Ogni dominio dell'account utente o un insieme di strutture che contiene gli utenti l'autenticazione per il servizio ADFS, deve considerare attendibile il dominio che fanno parte dei server ADFS.  
  
-   L'account del servizio ADFS deve essere considerato attendibile in ogni dominio utente che contiene gli utenti l'autenticazione per il servizio ADFS.  
  
## <a name="BKMK_5"></a>Requisiti del database di configurazione  
Di seguito sono i requisiti e limitazioni applicabili in base al tipo di archivio di configurazione:  
  
**Database interno di Windows**  
  
-   Se si dispone di un massimo di 100 trust della relying party, una farm WID ha un limite di 30 server federativi.  
  
-   Profilo di risoluzione dell'elemento di SAML 2.0 non è supportata nel database di configurazione database interno di Windows.  Il rilevamento riproduzione token non è supportato nel database di configurazione database interno di Windows. Questa funzionalità viene solo utilizzata solo in scenari in cui ADFS è funge da provider di federazione e l'utilizzo di token di sicurezza da provider di attestazioni esterno.  
  
-   Centri di distribuzione dei server ADFS in dati distinto per il failover o il bilanciamento del carico geografico è supportato, purché il numero di server senza superare i 30.  
  
Nella tabella seguente fornisce un riepilogo di utilizzo di una farm database interno di Windows.  Usato per pianificare l'implementazione.  
  
||||  
|-|-|-|  
||1 \- 100 attendibilità della relying Party|Più di 100 attendibilità della relying Party|  
|1 \- 30 AD FS nodi|Database interno di Windows supportati|Non supportato utilizzando WID \- SQL necessarie|  
|Nodi più di 30 AD FS|Non supportato utilizzando WID \- SQL necessarie|Non supportato utilizzando WID \- SQL necessarie|  
  
**SQL Server**  
  
Per AD FS in Windows Server 2012 R2, è possibile usare SQL Server 2008 e versioni successive  
  
## <a name="BKMK_6"></a>Requisiti del browser  
Quando viene eseguita l'autenticazione di ADFS tramite un browser o un controllo browser, il browser deve essere conforme ai requisiti seguenti:  
  
-   È necessario abilitare JavaScript  
  
-   È necessario attivare i cookie  
  
-   Indicazione del nome del server \(SNI\) devono essere supportate  
  
-   Per l'autenticazione del certificato dispositivo & certificato utente \(funzionalità di aggiunta all'area di lavoro\), il browser deve supportare l'autenticazione del certificato client SSL  
  
Diverse piattaforme e browser chiave sottoposte a livello di convalida per il rendering e funzionalità, i cui dettagli sono elencati di seguito. Browser e dispositivi che non trattate in questa tabella sono ancora supportati se soddisfano i requisiti elencati in precedenza:  
  
|||  
|-|-|  
|**Browser**|**Piattaforme**|  
|INTERNET EXPLORER 10.0|Windows 7, Windows 8.1, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2|  
|INTERNET EXPLORER 11.0|Windows7, Windows 8.1, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2|  
|Broker di autenticazione Web di Windows|Windows 8.1|  
|Firefox \[V21\]|Windows 7, Windows 8.1|  
|Safari \[V7\]|iOS 6, Mac OS\-X è 10.7|  
|Chrome \[V27\]|Windows 7, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Mac OS\-X 10.7|  
  
> [!IMPORTANT]  
> Problema noto \- Firefox: funzionalità area di lavoro che identifica il dispositivo utilizzando il certificato di dispositivo non è funzionale su piattaforme Windows. Firefox non supporta attualmente l'autenticazione del certificato client SSL utilizzando i certificati di provisioning per l'archivio certificati utente sui client Windows.  
  
**Cookie**  
  
AD FS crea cookie permanenti e basati su\-di sessione che devono essere archiviati nei computer client per fornire\-di accesso, Sign\-out, Single Sign\-on \(SSO\)e altre funzionalità. Il browser client deve quindi essere configurato in modo da accettare i cookie. I cookie utilizzati per l'autenticazione sono sempre protetti Hypertext Transfer Protocol \(HTTPS\) cookie di sessione scritti per il server di origine. Se il browser client non è configurato per abilitare questi cookie, ADFS non potrà funzionare correttamente. I cookie permanenti vengono usati per mantenere la selezione del provider di attestazioni effettuata dall'utente. È possibile disabilitarli usando un'impostazione di configurazione nel file di configurazione per il\-di AD FS di accesso in pagine. Supporto per TLS\/SSL è obbligatorio per motivi di sicurezza.  
  
## <a name="BKMK_extranet"></a>Requisiti Extranet  
Per fornire l'accesso extranet al servizio ADFS, è necessario distribuire il servizio ruolo Proxy applicazione Web come ruolo pubblico extranet che le richieste di autenticazione proxy in modo sicuro per il servizio ADFS. In questo modo l'isolamento di tutte le chiavi di sicurezza e isolamento degli endpoint del servizio ADFS \(ad esempio i certificati di firma del token\) dalle richieste provenienti da internet. Inoltre, le funzionalità, ad esempio il blocco degli Account Extranet Soft richiedono l'utilizzo del Proxy dell'applicazione Web. Per ulteriori informazioni sul Proxy dell'applicazione Web, vedere [Proxy applicazione Web](https://technet.microsoft.com/library/dn584107.aspx).  
  
Se si desidera utilizzare un terzo proxy\-entità per l'accesso Extranet, questo terzo proxy\-entità deve supportare il protocollo definito in [http:\/\/download.microsoft.com\/download\/9\/5\/E\/95EF66AF\-9026\-4BB0\-A41D\-A4F81802D92C\/% 5bMS\-ADFSPIP %5 d. pdf](https://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-ADFSPIP%5d.pdf).  
  
## <a name="BKMK_7"></a>Requisiti di rete  
Configurazione dei seguenti servizi di rete è essenziale per una corretta distribuzione di ADFS nell'organizzazione:  
  
**Configurazione del firewall aziendale**  
  
Entrambi i firewall si trova tra il Proxy dell'applicazione Web e la server farm federativa e il firewall tra i client e il Proxy dell'applicazione Web deve avere la porta TCP 443 abilitato in ingresso.  
  
Inoltre, se l'autenticazione del certificato client utente \(autenticazione clientTLS utilizzando X509 i certificati utente\) è obbligatorio, ADFS in Windows Server 2012 R2 richiede che sia attivato 49443 la porta TCP in ingresso nel firewall tra i client e il Proxy dell'applicazione Web. Questa operazione non è necessaria nel firewall tra il Proxy di applicazione Web e i server federativi\).  

> [!NOTE]
> inoltre assicurarsi che la porta 49443 non venga utilizzata da altri servizi nella AD FS e nel server proxy applicazione Web.

**Configurazione del DNS**  
  
-   Per l'accesso intranet, tutti i client che accedono AD FS service all'interno della rete azienda interna \(intranet\) deve essere in grado di risolvere il nome del servizio ADFS \(nome fornito per il certificato SSL\) al bilanciamento del carico per i server ADFS o il server ADFS.  
  
-   Per l'accesso extranet, tutti i client che accedono AD FS service dall'esterno della rete aziendale \(extranet\/internet\) deve essere in grado di risolvere il nome del servizio ADFS \(nome fornito per il certificato SSL\) al bilanciamento del carico per il server Proxy applicazione Web o il server Proxy applicazione Web.  
  
-   Per l'accesso extranet funzionare correttamente, ogni server di Proxy applicazione Web nella rete Perimetrale deve essere in grado di risolvere il nome del servizio ADFS \(nome fornito per il certificato SSL\) al bilanciamento del carico per i server ADFS o il server ADFS. Ciò può essere ottenuto tramite un server DNS alternativo nella rete Perimetrale o modificando la risoluzione del server locale utilizzando file HOST.  
  
-   Per l'autenticazione integrata di Windows lavorare all'interno della rete e all'esterno della rete per un subset degli endpoint esposti tramite il Proxy dell'applicazione Web, è necessario utilizzare un record \(non CNAME\) per puntare al bilanciamento del carico.  
  
Per informazioni sulla configurazione DNS aziendale per il servizio federativo e il servizio DRS, vedere [configurare DNS aziendale per il servizio federativo e il servizio DRS](https://technet.microsoft.com/library/dn486786.aspx).  
  
Per informazioni sulla configurazione DNS aziendale per i proxy applicazione Web, vedere la sezione "Configurare DNS" in [passaggio 1: configurare l'infrastruttura del Proxy applicazione Web](https://technet.microsoft.com/library/dn383644.aspx).  
  
Per informazioni su come configurare un indirizzo IP o un FQDN del cluster usando NLB, vedere Specifica dei parametri del cluster in [http:\/\/go.microsoft.com\/fwlink\/? LinkId\=75282](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
## <a name="BKMK_8"></a>Requisiti dell'archivio attributi  
ADFS richiede almeno un archivio di attributi da utilizzare per l'autenticazione degli utenti e l'estrazione delle attestazioni di sicurezza per gli utenti. Per un elenco di attributi di archivi che supportano ADFS, vedere [il ruolo degli archivi attributi](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md).  
  
> [!NOTE]  
> AD FS crea automaticamente un archivio di attributi "Active Directory", per impostazione predefinita. Requisiti dell'archivio attributi dipendono se l'organizzazione funge da partner account \(ospita gli utenti federati\) o il partner risorse \(ospita l'applicazione federata\).  
  
**Archivi di attributi LDAP**  
  
Quando si lavora con altri protocolli di accesso Lightweight Directory \(LDAP\)\-gli archivi di attributi basati su, è necessario connettersi a un server LDAP che supporta l'autenticazione integrata di Windows. Anche la stringa di connessione LDAP deve essere scritta nel formato di URL LDAP, come descritto nella RFC 2255.  
  
È inoltre necessario che l'account del servizio per il servizio ADFS disponga del diritto per recuperare le informazioni utente nell'archivio di attributi LDAP.  
  
**Archivi di attributi SQL Server**  
  
Per il corretto funzionamento di AD FS in Windows Server 2012 R2, i computer che ospitano l'archivio attributi SQL Server devono eseguire Microsoft SQL Server 2008 o versione successiva. Quando si lavora con SQL\-in base agli archivi di attributi, è necessario configurare anche una stringa di connessione.  
  
**Archivi di attributi personalizzati**  
  
Per abilitare scenari avanzati, è possibile sviluppare archivi attributi personalizzati.  
  
-   Il linguaggio dei criteri integrato in ADFS consente di fare riferimento ad archivi attributi personalizzati per il miglioramento di uno qualsiasi degli scenari seguenti:  
  
    -   Creazione di attestazioni per un utente autenticato localmente  
  
    -   Integrazione di attestazioni per un utente autenticato esternamente  
  
    -   Autorizzazione di un utente a ottenere un token  
  
    -   Autorizzazione di un servizio a ottenere un token sul comportamento di un utente  
  
    -   Emittente dati aggiuntivi nei token di sicurezza emesso da ADFS per le relying party.  
  
-   Tutti gli archivi di attributi personalizzati devono essere compilati su .NET 4.0 o versione successiva.  
  
Quando si lavora con un archivio attributi personalizzato, è inoltre necessario configurare una stringa di connessione. In tal caso, è possibile immettere un codice personalizzato di propria scelta che consente la connessione all'archivio di attributi personalizzati. La stringa di connessione in questa situazione è un set di nome\/valore coppie che vengono interpretate come implementato dallo sviluppatore dell'archivio di attributi personalizzati. Per ulteriori informazioni sullo sviluppo e l'uso degli archivi attributi personalizzati, vedere [Panoramica degli archivi attributi](https://go.microsoft.com/fwlink/?LinkId=190782).  
  
## <a name="BKMK_9"></a>Requisiti delle applicazioni  
ADFS supporta attestazioni\-compatibile con applicazioni che utilizzano i protocolli seguenti:  
  
-   WS\-federazione  
  
-   WS\-attendibile  
  
-   Utilizzo dei profili IDPLite, SPLite & eGov1.5 protocollo SAML 2.0.  
  
-   Profilo di concessione di autorizzazione OAuth 2.0  
  
ADFS supporta inoltre l'autenticazione e autorizzazione per qualsiasi non\-attestazioni\-applicazioni che sono supportate dal Proxy di applicazione Web.  
  
## <a name="BKMK_10"></a>Requisiti di autenticazione  
**Autenticazione di servizi di dominio Active Directory \(autenticazione primaria\)**  
  
Per l'accesso intranet, sono supportati i seguenti meccanismi di autenticazione standard per Active Directory:  
  
-   Autenticazione integrata di Windows utilizza Negotiate per Kerberos e NTLM  
  
-   Utilizza il nome utente di autenticazione basata su form\/password  
  
-   Autenticazione dei certificati utilizzando certificati mappati ad account utente di dominio Active Directory  
  
Per l'accesso extranet, sono supportati i meccanismi di autenticazione seguenti:  
  
-   Utilizza il nome utente di autenticazione basata su form\/password  
  
-   Autenticazione dei certificati utilizzando i certificati che sono mappati ad account utente di dominio Active Directory  
  
-   Autenticazione integrata di Windows utilizzando Negotiate \(solo NTLM\) per WS\-Trust endpoint che accetta l'autenticazione integrata di Windows.  
  
Per l'autenticazione del certificato:  
  
-   Si estende alla smart card che può essere un pin protetto.  
  
-   L'interfaccia utente grafica per l'utente di immettere il pin non viene fornito da ADFS ed è necessario far parte del sistema operativo client che viene visualizzato quando si utilizza client TLS.  
  
-   Il lettore e il provider del servizio di crittografia \(CSP\) per la smart card devono funzionare nel computer in cui si trova il browser.  
  
-   Il certificato della smart card deve conduce a una radice attendibile in tutti i server ADFS e server Proxy applicazione Web.  
  
-   Il certificato deve eseguire il mapping all'account utente di dominio Active Directory utilizzando uno dei metodi seguenti:  
  
    -   Il nome del soggetto del certificato corrisponde al nome distinto LDAP di un account utente in servizi di dominio Active Directory.  
  
    -   L'estensione relativa al nome alternativo oggetto del certificato ha il nome dell'entità utente \(UPN\) di un account utente di dominio Active Directory.  
  
Per l'autenticazione integrata di Windows senza problemi utilizzando Kerberos nella rete intranet,  
  
-   È necessario per il nome del servizio da parte di siti attendibili o ai siti Intranet locali.  
  
-   Inoltre, l'HOST\/< adfs\_servizio\_nome > nome SPN deve essere impostato sull'account di servizio che viene eseguita la farm di ADFS in.  
  
**Autenticazione a più\-Factor**  
  
ADFS supporta l'autenticazione aggiuntiva \(oltre l'autenticazione principale supportata da AD DS\) utilizzando un modello di provider in base al quale i fornitori\/gli utenti possono creare i propri più\-adapter per l'autenticazione fattore che un amministratore può registrare e utilizzare durante l'accesso.  
  
Ogni scheda di autenticazione a più Fattori deve essere compilato in .NET 4.5.  
  
Per ulteriori informazioni sull'autenticazione a più Fattori, vedere [gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni con](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md).  
  
**Autenticazione del dispositivo**  
  
ADFS supporta l'autenticazione del dispositivo utilizzando i certificati di provisioning dal servizio Registrazione dispositivi di durante l'atto di un'unione di dispositivo all'area di lavoro degli utenti finali.  
  
## <a name="BKMK_11"></a>Requisiti per l'aggiunta all'area di lavoro  
Gli utenti finali possono all'area di lavoro i propri dispositivi a un'organizzazione che utilizza ADFS. Questa è supportata dal servizio Registrazione dispositivi di ADFS. Di conseguenza, gli utenti finali ottengono l'ulteriore vantaggio di SSO tra le applicazioni supportate da ADFS. Inoltre, gli amministratori possono gestire i rischi limitando l'accesso alle applicazioni solo ai dispositivi che sono stati aggiunto all'organizzazione all'area di lavoro. Di seguito sono i seguenti requisiti per abilitare questo scenario.  
  
-   ADFS supporta all'area di lavoro per Windows 8.1 e iOS 5\+ dispositivi  
  
-   Per utilizzare la funzionalità aggiunta alla rete aziendale, lo schema dell'insieme di strutture collegati al server ADFS deve essere Windows Server 2012 R2.  
  
-   Il nome alternativo soggetto del certificato SSL per il servizio ADFS deve contenere il valore enterpriseregistration seguito dal nome dell'entità utente \(UPN\) suffisso dell'organizzazione, ad esempio, enterpriseregistration.  
  
## <a name="BKMK_12"></a>Requisiti di crittografia  
Nella tabella seguente fornisce informazioni sul supporto di crittografia aggiuntivi nel token di ADFS firma, crittografia dei token\/funzionalità decrittografia:  
  
||||  
|-|-|-|  
|**Algoritmo**|**Lunghezze delle chiavi**|**Protocolli\/applicazioni\/Commenti**|  
|TripleDES-default 192 \(supported 192 – 256\) \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#TripleDES\-CBC](http://www.w3.org/2001/04/xmlenc#tripledes-cbc)|>\= 192|Algoritmo supportato per la decrittografia del token di sicurezza. La crittografia del token di sicurezza con questo algoritmo non è supportata.|  
|AES128 \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#AES128\-CBC](http://www.w3.org/2001/04/xmlenc#aes128-cbc)|128|Algoritmo supportato per la decrittografia del token di sicurezza. La crittografia del token di sicurezza con questo algoritmo non è supportata.|  
|AES192 \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#AES192\-CBC](http://www.w3.org/2001/04/xmlenc#aes192-cbc)|192|Algoritmo supportato per la decrittografia del token di sicurezza. La crittografia del token di sicurezza con questo algoritmo non è supportata.|  
|AES256 \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#AES256\-CBC](http://www.w3.org/2001/04/xmlenc#aes256-cbc)|256|**Predefinito**. Algoritmo supportato per crittografare il token di sicurezza.|  
|TripleDESKeyWrap \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#kw\-TripleDES](http://www.w3.org/2001/04/xmlenc#kw-tripledes)|Tutte le dimensioni delle chiavi supportate da .NET 4,0\+|Algoritmo supportato per crittografare la chiave simmetrica che crittografa un token di sicurezza.|  
|AES128KeyWrap \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#kw\-AES128](http://www.w3.org/2001/04/xmlenc#kw-aes128)|128|Algoritmo supportato per crittografare la chiave simmetrica che consente di crittografare il token di sicurezza.|  
|AES192KeyWrap \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#kw\-Aes192](http://www.w3.org/2001/04/xmlenc#kw-aes192)|192|Algoritmo supportato per crittografare la chiave simmetrica che consente di crittografare il token di sicurezza.|  
|AES256KeyWrap \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#kw\-AES256](http://www.w3.org/2001/04/xmlenc#kw-aes256)|256|Algoritmo supportato per crittografare la chiave simmetrica che consente di crittografare il token di sicurezza.|  
|RsaV15KeyWrap \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#rsa\-1\_5](http://www.w3.org/2001/04/xmlenc#rsa-1_5)|1024|Algoritmo supportato per crittografare la chiave simmetrica che consente di crittografare il token di sicurezza.|  
|RsaOaepKeyWrap \- [http:\/\/www.w3.org\/2001\/04\/xmlenc\#rsa\-oaep\-mgf1p](http://www.w3.org/2001/04/xmlenc#rsa-oaep-mgf1p)|1024|Default. Algoritmo supportato per crittografare la chiave simmetrica che consente di crittografare il token di sicurezza.|  
|SHA1\-[http:\/\/www.w3.org\/PICS\/DSig\/sha1\_1\_0. html](http://www.w3.org/PICS/DSig/SHA1_1_0.html)|N\/A|Utilizzato dal Server ADFS nella generazione di elementi SourceId: In questo scenario, il servizio token di sicurezza utilizza SHA1 \(per la raccomandazione nello standard SAML 2.0\) per creare un valore breve a 160 bit per l'ID di elemento di origine.<br /><br />Anche utilizzato dall'agente web ADFS \(componente legacy dall'intervallo di tempo WS2003\) per identificare le modifiche in un tempo di "ultimo aggiornamento" valore in modo da determinare quando aggiornare le informazioni dal servizio token di sicurezza.|  
|\- SHA1withRSA<br /><br />[http:\/\/www.w3.org\/PICS\/DSig\/RSA\-SHA1\_1\_0. html](http://www.w3.org/PICS/DSig/RSA-SHA1_1_0.html)|N\/A|Utilizzata quando il Server ADFS convalida la firma di SAML AuthenticationRequest, firmare la richiesta di risoluzione dell'elemento o la risposta, creare token\-certificato di firma.<br /><br />In questi casi, il valore predefinito è SHA256 e SHA1 viene utilizzato solo se il partner \(relying party\) non è in grado di supportare SHA256 e devono utilizzare SHA1.|  
  
## <a name="BKMK_13"></a>Requisiti di autorizzazione  
L'amministratore che esegue l'installazione di e la configurazione iniziale di AD FS deve disporre delle autorizzazioni di amministratore di dominio nel dominio locale \(in altre parole, il dominio a cui viene aggiunto il server federativo.\)  
  
## <a name="see-also"></a>Vedere anche  
[Guida alla progettazione di AD FS in Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  
