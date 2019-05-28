---
ms.assetid: aa892a85-f95a-4bf1-acbb-e3c36ef02b0d
title: Novità di Active Directory Federation Services per Windows Server 2016
description: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 04/23/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fbb289c16d82da79aded49e3af4134ac7f6df325
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188700"
---
# <a name="whats-new-in-active-directory-federation-services"></a>Novità di Active Directory Federation Services


## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2019"></a>Che cosa sono le novità di Active Directory Federation Services per Windows Server 2019

### <a name="protected-logins"></a>Account di accesso protetto
Di seguito è riportato un breve riepilogo degli aggiornamenti agli account di accesso protetta disponibili in AD FS 2019:
- **Provider di autenticazione esterni come primario** -i clienti possono ora usare prodotti di autenticazione di terze parti 3rd come il primo fattore e non esporre le password come il primo fattore. Nei casi in cui un provider di autenticazione esterni può dimostrare 2 fattori possibile attestazione MFA. 
- **L'autenticazione della password come metodo di autenticazione aggiuntiva** -i clienti hanno un'opzione completamente supportata nella posta in arrivo di usare password solo per fattore aggiuntivo dopo una password meno opzione viene utilizzato come il primo fattore. Ciò migliora l'esperienza dei clienti da ad FS 2016 in cui i clienti dovevano scaricare un adattatore di github che è supportato come è. 
- **Modulo plug-in valutazione dei rischi** -i clienti possono ora creare i propri plug-in moduli per bloccare determinati tipi di richieste durante la fase di preautenticazione. Questo rende più semplice per i clienti a usare l'intelligence per il cloud, ad esempio Identity protection per bloccare l'account di accesso per gli utenti a rischio o transazioni a rischio.  Per altre informazioni vedere [ creare Plug-in con modello di valutazione del rischio 2019 AD FS](../../ad-fs/development/ad-fs-risk-assessment-model.md) 
- **Miglioramenti di ESL** -migliora la correzione QFE ESL nel 2016 aggiungendo le funzionalità seguenti
    - Consente ai clienti di essere in modalità di controllo mentre viene protetto dalla funzionalità di blocco extranet "classici" disponibile a partire da ad FS 2012 R2. Attualmente i clienti 2016 non avrebbe alcuna protezione in modalità di controllo. 
    - Abilita la soglia di blocco indipendenti per posizioni note. Questo rende possibile per più istanze delle App in esecuzione con un account del servizio comune per il rollover delle password con il minimo impatto. 

### <a name="additional-security-improvements"></a>Miglioramenti della sicurezza aggiuntive
I seguenti miglioramenti di sicurezza aggiuntive sono disponibili in AD FS 2019:
- **PSH remota usando account di accesso smart card** : i clienti possono ora utilizzare smart card in remoto la connessione ad ad FS tramite PSH e Usa che per gestire PSH tutte le funzioni includono cmdlet PSH a nodi multipli.
- **Personalizzazione di intestazione HTTP** -i clienti possono ora personalizzare le intestazioni HTTP durante le risposte di ad FS. Ciò include le intestazioni seguenti
     - HSTS: Ciò indica che gli endpoint ad FS sono utilizzabili solo gli endpoint HTTPS per un browser conforme per imporre
     - x-frame-options: Consente agli amministratori di ad FS consentire alle relying party specifiche di incorporamento IFrame per le pagine di accesso interattivo di ad FS. Deve essere utilizzata con cautela e solo negli host HTTPS. 
     - Intestazione futuri: È possibile configurare anche altre intestazioni future. 

Per altre informazioni vedere [intestazioni della risposta HTTP Personalizza sicurezza con AD FS 2019](../../ad-fs/operations/customize-http-security-headers-ad-fs.md) 

### <a name="authenticationpolicy-capabilities"></a>Funzionalità autenticazione/criteri
Le seguenti funzionalità di autenticazione o criterio sono in AD FS 2019:
- **Specificare il metodo di autenticazione per l'autenticazione aggiuntiva per ogni applicazione relying Party** -i clienti possono ora usare le attestazioni le regole per decidere quale provider di autenticazione aggiuntivo da richiamare per provider di autenticazione aggiuntivo. Ciò è utile per casi d'uso 2
    - I clienti stanno passando dal provider di autenticazione aggiuntivo uno a altro. In questo modo, man mano che gli utenti di eseguire l'onboarding a un nuovo provider di autenticazione possono usare i gruppi per controllare quali l'autenticazione aggiuntiva del provider viene chiamato.
    - I clienti hanno esigenze per un provider di autenticazione aggiuntivo specifico (ad esempio, il certificato) per determinate applicazioni. 
- **Limitare l'autenticazione TLS basato su dispositivo solo alle applicazioni che lo richiedono** -i clienti ora possono limitare le autenticazioni di dispositivo per basati su TLS, solo le applicazioni che eseguono dispositivi l'accesso condizionale basato su client. Ciò impedisce che vengano visualizzati messaggi indesiderati per l'autenticazione dispositivo (o errori se l'applicazione client non è possibile gestire) per le applicazioni che non richiedono l'autenticazione TLS basato su dispositivo.
- **Supporto MFA freshness** -AD FS supporta ora la funzionalità per eseguire nuovamente la validità della credenziale 2nd factor in base 2 ° credenziale di fattore. In questo modo agli utenti di eseguire una transazione iniziale con 2 fattori e Richiedi solo 2nd factor su base periodica. Questo è disponibile solo per le applicazioni che possono fornire un parametro aggiuntivo nella richiesta e non sono un'impostazione configurabile in ad FS. Questo parametro è supportato da Azure AD quando sono configurati "Ricordarsi di my MFA per X giorni" e il flag 'supportsMFA' è impostato su true per le impostazioni del trust di dominio federato in Azure AD. 

### <a name="sign-in-sso-improvements"></a>Miglioramenti di accesso SSO
In AD FS 2019 sono stati apportati i miglioramenti di SSO sign-in seguenti:

- [Impaginato dell'esperienza utente con il tema centrato](../operations/AD-FS-paginated-sign-in.md) -ADFS sono ora spostate in un flusso dell'esperienza utente impaginato che consente ad FS convalidare e fornire un'esperienza di accesso più più uniforme. Ad FS Usa ora un'interfaccia utente centrata (anziché il lato destro della schermata). Le immagini più recenti di logo e di sfondo per la compatibilità con l'esperienza potrebbe essere necessaria. Ciò riflette anche funzionalità disponibile in Azure AD.
- **Correzione di bug: Stato di accesso SSO persistente per i dispositivi Windows 10 quando si esegue l'autenticazione PRT** Ciò contribuisce a risolvere un problema in cui lo stato di autenticazione a più fattori non è stato mantenuto quando si usa l'autenticazione PRT per i dispositivi Windows 10. Il risultato del problema è che gli utenti finali sarebbe venga chiesta 2nd credenziale a fattori (MFA) di frequente. La correzione inoltre rende l'esperienza coerente quando dispositivo autenticazione viene eseguita correttamente tramite il client TLS e tramite il meccanismo di PRT. 


### <a name="suppport-for-building-modern-line-of-business-apps"></a>Supporto per la creazione di App line-of-business moderne
Il supporto per la creazione di App LOB moderne seguente è stato aggiunto ad AD FS 2019:

 - **Flusso OAuth dispositivo/profilo** -AD FS supporta ora il profilo di flusso OAuth dispositivo per eseguire gli account di accesso nei dispositivi che non è una superficie di attacco per il supporto delle esperienze di accesso avanzate dell'interfaccia utente. Ciò consente all'utente completare l'esperienza di accesso su un altro dispositivo. Questa funzionalità è necessaria per un'esperienza della riga di comando di Azure in Azure Stack e può essere usata in altri casi. 
 - **Rimozione del parametro 'Resource'** -ADFS ora ha rimosso la necessità di specificare un parametro di risorsa che è in linea con le specifiche di Oauth corrente. I client possono ora specificare l'identificatore dell'attendibilità della Relying Party come parametro di ambito anche per le autorizzazioni richieste. 
 - **Le intestazioni CORS nelle risposte ADFS** -i clienti possono ora creare applicazioni a pagina singola che consentono a client librerie JS lato convalidare la firma dell'id_token eseguendo una query per le chiavi di firma del documento di individuazione OIDC in AD FS. 
 - **Supporto per PKCE** -ADFS aggiunge supporto per PKCE per fornire un flusso del codice di autenticazione sicura all'interno di OAuth. Si aggiunge un ulteriore livello di sicurezza per questo flusso per impedire Hijack il codice e riproduzione, da un altro client. 
 - **Correzione di bug: Attestazione di trasmissione x5t e kid** -si tratta di una correzione di bug minore. A questo punto ADFS invia inoltre l'attestazione "kid" per indicare l'hint di id chiave per la verifica della firma. Inviati in precedenza ADFS solo questo come attestazione "x5t".

### <a name="supportability-improvements"></a>Miglioramenti al supporto
I seguenti miglioramenti al supporto non fanno parte di AD FS 2019:
- **Inviare i dettagli dell'errore per gli amministratori di AD FS** -consente agli amministratori di configurare gli utenti finali per inviare i log di debug relative a un errore nell'autenticazione degli utenti finali da archiviare come presentata una compresso per la facilità di utilizzo. Gli amministratori possono anche configurare una connessione SMTP a automail il file compresso in un account di posta elettronica di valutazione o a auto creare un ticket basato sul messaggio di posta elettronica. 

### <a name="deployment-updates"></a>Aggiornamenti della distribuzione
Gli aggiornamenti di distribuzione seguenti sono ora inclusi in AD FS 2019:
- **2019 a livello di comportamento farm** : come con AD FS 2016, è una nuova versione di livello di comportamento Farm che è necessaria per abilitare nuove funzionalità in precedenza. In questo modo, passando da:
    - 2012 R2-> 2019
    - 2016 -> 2019   

### <a name="saml-updates"></a>Aggiornamenti SAML
È il seguente aggiornamento SAML in AD FS 2019:
- **Correzione di bug: Correggere i bug di federazione aggregata** -sono state apportate numerose correzioni di bug relativi al supporto federazione aggregato (ad esempio InCommon). Le correzioni sono stati tutto quanto segue: 
  - Migliorata la scalabilità di grandi dimensioni n. di entità nel documento metadati federazione aggregati. In precedenza, questo avrà esito negativo con errore "ADMIN0017". 
  - Eseguire query con parametri 'ScopeGroupID' tramite il cmdlet Get-AdfsRelyingPartyTrustsGroup PSH. 
  - La gestione delle condizioni di errore intorno entityID duplicati


### <a name="azure-ad-style-resource-specification-in-scope-parameter"></a>Definizione di risorsa dello stile AD Azure nel parametro di ambito 
AD FS richiedevano in precedenza, la risorsa desiderata e l'ambito in un parametro separato eventuali richieste di autenticazione. Ad esempio, una richiesta oauth tipica sarà simile di sotto: 7 **https:&#47;&#47;fs.contoso.com/adfs/oauth2/authorize?</br> valore response_type = codice & client_id = claimsxrayclient & resource = urn: microsoft:</br>adfs:claimsxray a & mbito = oauth e URI di reindirizzamento = https:&#47;&#47;adfshelp.microsoft.com/</br> ClaimsXray / TokenResponse & prompt = login**
 
Con AD FS 2019 Server, è ora possibile passare il valore della risorsa incorporato nel parametro di ambito. Ciò è coerenza con la modalità è possibile eseguire l'autenticazione con Azure AD anche. 

Il parametro di ambito ora può essere organizzato in un elenco separato da spazi in cui ogni voce è struttura come risorsa/ambito. Per esempio  

**< crea una richiesta di esempio valida >**
> [!NOTE]
> Solo una risorsa può essere specificata nella richiesta di autenticazione. Se più di una risorsa è incluso nella richiesta, AD FS restituisce che un errore e l'autenticazione non riuscirà. 

### <a name="proof-key-for-code-exchange-pkce-support-for-oauth"></a>Chiave di prova per il supporto di Code Exchange (PKCE) per oAuth 
I client pubblici OAuth tramite la concessione del codice di autorizzazione sono vulnerabili agli attacchi di intercettazione codice di autorizzazione.  L'attacco viene anche descritto in RFC 7636. Per prevenire questo attacco, AD FS nel Server 2019 supporta chiave di prova per Code Exchange (PKCE) per il flusso di concessione del codice di autorizzazione OAuth. 
 
Per sfruttare il supporto per PKCE, questa specifica aggiunge parametri aggiuntivi per l'autorizzazione di OAuth 2.0 e le richieste di Token di accesso.

![Proofkey](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/adfs2019.png)

A. Il client crea e registra un segreto denominato "code_verifier", deriva una versione trasformata "t(code_verifier)" (detto "code_challenge"), che viene inviata in OAuth 2.0 Authorization Request insieme al metodo di trasformazione "t_m". 

B. L'Endpoint di autorizzazione risponde normalmente ma t(code_verifier)"record" e il metodo di trasformazione. 

C. Il client quindi invia il codice di autorizzazione in Access Token Request come di consueto, ma include la chiave privata "code_verifier" generata a (A). 

D. ADFS trasforma "code_verifier" e lo confronta con "t(code_verifier)" di (B).  Accesso negato se non sono uguali. 

#### <a name="faq"></a>Domande frequenti 
**Q.** È possibile passare il valore di risorsa come parte del valore di ambito, ad esempio come le richieste vengono eseguite da Azure AD? 
</br>**A.** Con AD FS 2019 Server, è ora possibile passare il valore della risorsa incorporato nel parametro di ambito. Il parametro di ambito ora può essere organizzato in un elenco separato da spazi in cui ogni voce è struttura come risorsa/ambito. Per esempio  
**< crea una richiesta di esempio valida >**

**Q.** ADFS supporta estensione PKCE?
</br>**A.** AD FS nel Server 2019 supporta chiave di prova per Code Exchange (PKCE) per il flusso di concessione del codice di autorizzazione OAuth 

## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2016"></a>Novità di Active Directory Federation Services per Windows Server 2016   
Se si sta cercando informazioni sulle versioni precedenti di ADFS, vedere gli articoli seguenti:  
 [ADFS in Windows Server 2012 o 2012 R2](https://technet.microsoft.com/library/hh831502.aspx) e [AD FS 2.0](https://technet.microsoft.com/library/adfs2.aspx)  

 Active Directory Federation Services fornisce controllo di accesso e single sign-on in un'ampia gamma di applicazioni, tra cui Office 365, cloud basato su applicazioni SaaS e le applicazioni nella rete aziendale.  
* Per l'organizzazione IT, consente di fornire l'accesso e controllo di accesso alle applicazioni moderne sia legacy, in locale e nel cloud, in base allo stesso set di credenziali e i criteri.    
* Per l'utente fornisce un accesso trasparente utilizzando le credenziali di account familiare.  
* Per lo sviluppatore fornisce un modo semplice per autenticare gli utenti la cui identità si trovano nelle directory dell'organizzazione in modo che è possibile concentrare l'attenzione su applicazione, non l'autenticazione o identità.  

In questo articolo vengono descritte le nuove in ADFS in Windows Server 2016 (AD FS 2016).  

## <a name="eliminate-passwords-from-the-extranet"></a>Eliminare le password dalla rete Extranet  
AD FS 2016 consente tre nuove opzioni per l'accesso senza password, consentendo alle organizzazioni di evitare rischi di rete compromettono da phished, password persa o rubata. 

### <a name="sign-in-with-azure-multi-factor-authentication"></a>Accedere con Azure multi-factor Authentication
AD FS 2016 si basa l'autenticazione a più fattori funzionalità (MFA) di ADFS in Windows Server 2012 R2, consentendo l'accesso utilizzando solo un codice di autenticazione a più Fattori di Azure, senza prima inserire un nome utente e password.

* Con Azure multi-factor Authentication come metodo di autenticazione principale, l'utente è richiesto per il proprio nome utente e il codice OTP dall'app Azure Authenticator.  
* Con Azure multi-factor Authentication come metodo di autenticazione aggiuntivi o secondari, l'utente fornisce l'autenticazione principale credenziali (tramite l'autenticazione integrata di Windows, username e password, smart card o certificato utente o dispositivo), quindi viene visualizzato un prompt dei comandi per il testo, audio o OTP basato su account di accesso di autenticazione a più Fattori di Azure.  
* Con la nuova scheda Azure MFA incorporata, il programma di installazione e configurazione per Azure MFA con ADFS non è mai stata più semplice.
* Le organizzazioni possono sfruttare Azure MFA senza la necessità di un server Azure multi-factor Authentication locale.
* Autenticazione a più Fattori Azure configurabili per la rete intranet o extranet o come parte di alcun criterio di controllo di accesso.

Per ulteriori informazioni su Azure multi-factor Authentication con ADFS
*  [Configurare AD FS 2016 e Azure MFA](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-and-azure-mfa)  

### <a name="password-less-access-from-compliant-devices"></a>Password senza accesso da dispositivi compatibili
AD FS 2016 si basa sulle funzionalità di registrazione dei dispositivi precedenti abilitazione dell'accesso e controllo di accesso in base allo stato di conformità del dispositivo. Gli utenti possono accedere utilizzando le credenziali di dispositivo e valutazione della conformità è nuovamente quando modificare gli attributi dei dispositivi, in modo che è possibile garantire sempre i criteri vengono applicati.  In questo modo, ad esempio i criteri

* Abilitare l'accesso solo da dispositivi che sono gestiti o conformi  
* Abilitare l'accesso Extranet solo dai dispositivi che sono gestiti o conformi  
* Richiedere l'autenticazione a più fattori per i computer che non sono gestiti o non conforme  

ADFS fornisce il componente locale dei criteri di accesso condizionale in uno scenario ibrido. Quando si registrano i dispositivi con Azure AD per l'accesso condizionale alle risorse cloud, l'identità del dispositivo può essere utilizzata per nonché i criteri di ADFS.

![nuove funzionalità](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/ADFS_ITPRO4.png)  

 Per ulteriori informazioni sull'utilizzo di dispositivi basati su accesso condizionale nel cloud   
 *  [Accesso condizionale di Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access/)

Per ulteriori informazioni sull'utilizzo di dispositivi basati su accesso condizionale con AD FS
*  [Pianificazione per dispositivo basato su accesso condizionale con AD FS](../../ad-fs/deployment/Plan-Device-based-Conditional-Access-on-Premises.md)  
* [Criteri di controllo di accesso in AD FS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="sign-in-with-windows-hello-for-business"></a>Accedere con Windows Hello for Business   
I dispositivi Windows 10 introducono Windows Hello e Windows Hello for Business, sostituendo le password degli utenti con le credenziali utente associate ai dispositivi sicuro protette da movimenti dell'utente (un PIN, un movimento biometrico impronta digitale o il riconoscimento facciale). AD FS 2016 supporta queste nuove funzionalità di Windows 10 in modo che gli utenti possono accedere alle applicazioni di ADFS dalla intranet o extranet senza la necessità di fornire una password.

Per ulteriori informazioni sull'utilizzo di Microsoft Windows Hello for Business dell'organizzazione
*  [Abilita Windows Hello for Business nell'organizzazione](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)

## <a name="secure-access-to-applications"></a>Accesso sicuro alle applicazioni

### <a name="modern-authentication"></a>Autenticazione moderna
AD FS 2016 supporta i protocolli moderni più recenti che forniscono una migliore esperienza utente per Windows 10, nonché i più recenti e i dispositivi Android App iOS e.  

Per ulteriori informazioni vedere [AD FS scenari per gli sviluppatori](../../ad-fs/overview/AD-FS-Scenarios-for-Developers.md)  


### <a name="configure-access-control-policies-without-having-to-know-claim-rules-language"></a>Configurare criteri di controllo di accesso senza conoscere language regole attestazione  
Gli amministratori di ADFS era necessario configurare i criteri mediante il linguaggio di regola attestazione AD FS, rendendo difficile configurare e gestire i criteri. Con i criteri di controllo di accesso, gli amministratori possono utilizzare i modelli incorporati per applicare i criteri comuni, ad esempio
* Consentire solo l'accesso intranet
* Consenti tutti gli utenti e richiedere l'autenticazione a più Fattori dalla rete Extranet
* Consenti tutti gli utenti e richiedere l'autenticazione a più Fattori da un gruppo specifico

I modelli sono facili da personalizzare mediante una procedura guidata basato su processo per aggiungere le eccezioni o le regole di criteri aggiuntive e possono essere applicati a una o più applicazioni per l'applicazione coerente dei criteri.

Per ulteriori informazioni vedere [criteri di controllo di accesso in ADFS.](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="enable-sign-on-with-non-ad-ldap-directories"></a>Abilitazione dell'accesso con le directory LDAP non Active Directory  
Molte organizzazioni utilizzano una combinazione di Active Directory e le directory di terze parti. Con l'aggiunta del supporto di ADFS per l'autenticazione degli utenti memorizzati nelle directory v3 conforme a LDAP, è ora possibile utilizzare ADFS per:
* Utenti di terze parti, directory compatibili LDAP v3
* Utenti di foreste di Active Directory a cui non è configurato un trust bidirezionale con Active Directory
* Utenti in Active Directory Lightweight Directory Services (AD LDS)

Per ulteriori informazioni vedere [Configura ADFS per l'autenticazione degli utenti memorizzati nelle directory LDAP.](../../ad-fs/operations/Configure-AD-FS-to-authenticate-users-stored-in-LDAP-directories.md)  

## <a name="better-sign-in-experience"></a>Migliore esperienza di accesso
### <a name="customize-sign-in-experience-for-ad-fs-applications"></a>Personalizzare l'esperienza per le applicazioni di ADFS di accesso  
Sono state formulate da parte dell'utente che la possibilità di personalizzare l'esperienza di accesso per ogni applicazione sarebbe un miglioramento di uso, soprattutto per le organizzazioni che fornire l'accesso per le applicazioni che rappresentano più diverse aziende o i marchi.  

In precedenza, ADFS in Windows Server 2012 R2 fornito un'accesso più comune sull'esperienza per tutte le applicazioni relying party, con la possibilità di personalizzare un sottoinsieme del testo basato su contenuto per ogni applicazione. Con Windows Server 2016, è possibile personalizzare non solo messaggi, ma le immagini, web e logo tema per ogni applicazione. Inoltre, è possibile creare nuovi temi web personalizzati e applicarli al relying party.  

Per ulteriori informazioni vedere [personalizzazione di accesso utente AD FS.](../../ad-fs/operations/AD-FS-user-sign-in-customization.md)  



## <a name="manageability-and-operational-enhancements"></a>Facilità di gestione e operativi miglioramenti  
Nella sezione seguente descrive gli scenari operativi migliorati introdotte con Active Directory Federation Services in Windows Server 2016.  

### <a name="streamlined-auditing-for-easier-administrative-management"></a>Il controllo di semplice gestione amministrativa semplificata  
In AD FS per Windows Server 2012 R2 sono stati generati per una singola richiesta e le informazioni relative a un registro in numerosi eventi di controllo o attività di rilascio dei token è assente (in alcune versioni di AD FS) o suddiviso in più eventi di controllo. Per impostazione predefinita, ADFS gli eventi di controllo sono disattivati per loro natura dettagliato.  
Con il rilascio di AD FS 2016, il controllo è diventato più semplice e meno dettagliato.  

Per ulteriori informazioni vedere [miglioramenti relativi ad ADFS in Windows Server 2016 al controllo.](../../ad-fs/technical-reference/auditing-enhancements-to-ad-fs-in-windows-server.md)  

### <a name="improved-interoperability-with-saml-20-for-participation-in-confederations"></a>Migliorare l'interoperabilità con SAML 2.0 per la partecipazione a confederations  
AD FS 2016 contiene supporto del protocollo SAML aggiuntivo, incluso il supporto per l'importazione di relazioni di trust in base ai metadati che contiene più entità. Ciò consente di configurare ADFS per partecipare confederations come Federation InCommon e altre implementazioni conformi al eGov 2.0 standard.  

Per ulteriori informazioni vedere [migliorato l'interoperabilità con SAML 2.0.](../../ad-fs/operations/Improved-interoperability-with-SAML-2.0.md)  

### <a name="simplified-password-management-for-federated-o365-users"></a>Gestione delle password semplificata per utenti di Office 365 federati  
È possibile configurare Active Directory Federation Services (ADFS) per inviare le attestazioni di scadenza password per l'attendibilità della relying party (applicazioni) che sono protetti da ADFS. Utilizzo di tali attestazioni dipende dall'applicazione. Ad esempio, con Office 365 come la relying party, gli aggiornamenti sono stati implementati a Exchange e Outlook per notificare agli utenti federati delle password appena-a--scaduto.  

Per ulteriori informazioni vedere [configurare ADFS per l'invio attestazioni scadenza password.](../../ad-fs/operations/Configure-AD-FS-to-Send-Password-Expiry-Claims.md)  

### <a name="moving-from-ad-fs-in-windows-server-2012-r2-to-ad-fs-in-windows-server-2016-is-easier"></a>Lo spostamento da ADFS in Windows Server 2012 R2 ad ADFS in Windows Server 2016 è più semplice  
In precedenza, la migrazione a una nuova versione di ADFS, è necessario esportazione configurazione dalla farm precedente e l'importazione in una farm completamente nuova e parallelo.  

A questo punto, lo spostamento da ADFS in Windows Server 2012 R2 ad ADFS in Windows Server 2016 è diventato molto più semplice. È sufficiente aggiungere un nuovo server di Windows Server 2016 a una farm di Windows Server 2012 R2 e che la farm fungono a livello di comportamento di farm di Windows Server 2012 R2, quindi esegue la ricerca e si comporta come una farm di Windows Server 2012 R2.  

Quindi, aggiungere nuovi server di Windows Server 2016 alla farm, verificare la funzionalità e rimuovere i server meno recenti dal bilanciamento del carico. Quando tutti i nodi della farm sono in esecuzione Windows Server 2016, si è pronti per l'aggiornamento al livello di comportamento farm 2016 e iniziare a utilizzare le nuove funzionalità.  

Per ulteriori informazioni vedere [l'aggiornamento a AD FS in Windows Server 2016.](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)  
