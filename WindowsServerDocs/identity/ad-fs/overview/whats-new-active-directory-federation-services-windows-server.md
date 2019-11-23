---
ms.assetid: aa892a85-f95a-4bf1-acbb-e3c36ef02b0d
title: Novità di Active Directory Federation Services per Windows Server 2016
description: ''
author: billmath
ms.author: billmath
manager: daveba
ms.date: 04/23/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6294c7b6ead0a9fa338f8b2cc8134b750f7e3e8f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385554"
---
# <a name="whats-new-in-active-directory-federation-services"></a>Novità di Active Directory Federation Services


## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2019"></a>Novità di Active Directory Federation Services per Windows Server 2019

### <a name="protected-logins"></a>Account di accesso protetti
Di seguito è riportato un breve riepilogo degli aggiornamenti per gli account di accesso protetti disponibili in AD FS 2019:
- **Provider di autenticazione esterni come primari** : i clienti possono ora usare i prodotti di autenticazione di terze parti come primo fattore e non esporre le password come primo fattore. Nei casi in cui un provider di autenticazione esterno può dimostrare due fattori, può richiedere l'autenticazione a più fattori. 
- **Autenticazione della password come autenticazione aggiuntiva** : i clienti hanno un'opzione di posta in arrivo completamente supportata per usare la password solo per il fattore aggiuntivo dopo che è stata usata un'opzione di minore password come primo fattore. Ciò consente di migliorare l'esperienza del cliente da ADFS 2016, in cui i clienti dovevano scaricare una scheda GitHub supportata così com'è. 
- **Modulo di valutazione dei rischi di collegamento** : i clienti possono ora compilare moduli plug-in per bloccare determinati tipi di richieste durante la fase di pre-autenticazione. Questo rende più semplice per i clienti usare l'Intelligence per il cloud, ad esempio Identity Protection, per bloccare gli accessi per utenti a rischio o transazioni rischiose.  Per altre informazioni [, vedere compilazione di plug-in con AD FS 2019 modello di valutazione dei rischi](../../ad-fs/development/ad-fs-risk-assessment-model.md) 
- **Miglioramenti per ESL** : migliora il QFE esl in 2016 aggiungendo le funzionalità seguenti
    - Consente ai clienti di essere in modalità di controllo protetto dalla funzionalità di blocco Extranet "classica" disponibile a partire da ADFS 2012R2. Attualmente 2016 i clienti non avrebbero alcuna protezione in modalità di controllo. 
    - Abilita la soglia di blocco indipendente per le posizioni note. Ciò consente a più istanze di app in esecuzione con un account del servizio comune di eseguire il rollover delle password con il minor numero di conseguenze. 

### <a name="additional-security-improvements"></a>Ulteriori miglioramenti alla sicurezza
I seguenti miglioramenti di sicurezza aggiuntivi sono disponibili nella AD FS 2019:
- **Remote PSH using smartcard login** : i clienti possono ora usare le smart card per connettersi in remoto ad ADFS tramite PSH e usarle per gestire tutte le funzioni PSH, inclusi i cmdlet PSH a più nodi.
- **Personalizzazione dell'intestazione HTTP** : i clienti possono ora personalizzare le intestazioni HTTP emesse durante le risposte ad ADFS. Sono incluse le intestazioni seguenti
     - HSTS: in questo modo si comunica che gli endpoint ADFS possono essere utilizzati solo su endpoint HTTPS per applicare un browser conforme
     - x-frame-options: consente agli amministratori di ADFS di consentire a relying party specifici di incorporare gli iFrame per le pagine di accesso interattivo ad ADFS. Questa operazione deve essere utilizzata con cautela e solo per gli host HTTPS. 
     - Intestazione futura: è possibile configurare anche intestazioni future aggiuntive. 

Per altre informazioni, vedere [personalizzare le intestazioni di risposta di sicurezza http con AD FS 2019](../../ad-fs/operations/customize-http-security-headers-ad-fs.md) 

### <a name="authenticationpolicy-capabilities"></a>Funzionalità di autenticazione/criteri
Le funzionalità di autenticazione e criteri seguenti sono disponibili AD FS 2019:
- **Specificare il metodo di autenticazione per l'autenticazione aggiuntiva per RP** : i clienti possono ora usare le regole attestazioni per decidere quale provider di autenticazione aggiuntivo richiamare per il provider di autenticazione aggiuntivo. Questa operazione è utile per 2 casi d'uso
    - I clienti stanno passando da un provider di autenticazione aggiuntivo a un altro. In questo modo, durante l'onboarding degli utenti a un provider di autenticazione più recente, è possibile usare i gruppi per controllare quale provider di autenticazione aggiuntivo viene chiamato.
    - I clienti hanno bisogno di un provider di autenticazione aggiuntivo specifico (ad esempio, un certificato) per alcune applicazioni. 
- Limitare l'autenticazione del **dispositivo basata su TLS solo alle applicazioni che lo richiedono** . i clienti possono ora limitare le autenticazioni dei dispositivi basate su TLS client solo alle applicazioni che eseguono l'accesso condizionale basato su dispositivo. In questo modo si evitano richieste indesiderate per l'autenticazione del dispositivo (o errori se l'applicazione client non è in grado di gestire) per le applicazioni che non richiedono l'autenticazione del dispositivo basata su TLS.
- **Supporto** per l'aggiornamento dell'autenticazione a più fattori: ad FS supporta ora la possibilità di eseguire nuovamente le credenziali di secondo fattore in base all'aggiornamento della credenziale del secondo fattore. Ciò consente ai clienti di eseguire una transazione iniziale con 2 fattori e di richiedere il secondo fattore su base periodica. Questa opzione è disponibile solo per le applicazioni che possono fornire un parametro aggiuntivo nella richiesta e non è un'impostazione configurabile in ADFS. Questo parametro è supportato da Azure AD quando è configurato "memorizza l'autenticazione a più fattori per X giorni" e il flag ' supportsMFA ' è impostato su true nelle impostazioni di attendibilità del dominio federato nel Azure AD. 

### <a name="sign-in-sso-improvements"></a>Miglioramenti dell'accesso SSO
In AD FS 2019 sono stati apportati i miglioramenti seguenti per l'accesso SSO:

- [Impaginato UX con tema centrato](../operations/AD-FS-paginated-sign-in.md) -ADFS ora è stato spostato in un flusso di UX impaginato che consente ad ADFS di convalidare e fornire un'esperienza di accesso più uniforme. ADFS ora usa un'interfaccia utente centrata, anziché il lato destro dello schermo. Per allinearsi a questa esperienza, potrebbe essere necessario un logo e immagini di sfondo più recenti. Questo rispecchia anche le funzionalità offerte in Azure AD.
- **Correzione di bug: stato SSO persistente per i dispositivi WIN10 quando si esegue l'autenticazione PRT**   Questo risolve un problema per cui lo stato dell'autenticazione a più fattori non è stato persistente quando si usa l'autenticazione PRT per i dispositivi Windows 10. Il risultato del problema era che gli utenti finali ricevevano spesso la richiesta di credenziali di secondo fattore (multi-factor Credential). La correzione rende anche l'esperienza coerente quando l'autenticazione del dispositivo viene eseguita correttamente tramite il meccanismo TLS del client e via PRT. 


### <a name="suppport-for-building-modern-line-of-business-apps"></a>Supporto per la creazione di applicazioni line-of-business moderne
Il seguente supporto per la compilazione di app LOB moderne è stato aggiunto a AD FS 2019:

 - **Flusso/profilo del dispositivo OAuth** : ad FS supporta ora il profilo di flusso del dispositivo OAuth per eseguire gli accessi nei dispositivi che non dispongono di una superficie di attacco dell'interfaccia utente per supportare esperienze di accesso avanzate. Ciò consente all'utente di completare l'esperienza di accesso in un dispositivo diverso. Questa funzionalità è necessaria per l'esperienza dell'interfaccia della riga di comando di Azure in Azure Stack e può essere usata in altri casi. 
 - La **rimozione del parametro ' Resource '** -ad FS ha rimosso la necessità di specificare un parametro di risorsa che sia in linea con le specifiche OAuth correnti. I client possono ora specificare l'identificatore del trust della relying party come parametro di ambito, oltre alle autorizzazioni richieste. 
 - **Intestazioni CORS nelle risposte ad FS** : i clienti possono ora compilare applicazioni a pagina singola che consentono alle librerie js lato client di convalidare la firma del id_token eseguendo una query per le chiavi di firma dal documento di individuazione OIDC in ad FS. 
 - **Supporto di PKCE** : ad FS aggiunge il supporto di PKCE per fornire un flusso del codice di autenticazione protetto in OAuth. In questo modo viene aggiunto un ulteriore livello di sicurezza a questo flusso per impedire il hijack del codice e la riproduzione da un client diverso. 
 - **Correzione di bug: inviare l'attestazione X5T e Kid** . si tratta di una correzione di bug secondaria. AD FS ora invia anche l'attestazione "Kid" per indicare l'hint ID chiave per la verifica della firma. In precedenza AD FS stata inviata solo come attestazione "X5T".

### <a name="supportability-improvements"></a>Miglioramenti al supporto tecnico
I seguenti miglioramenti al supporto tecnico non fanno parte di AD FS 2019:
- **Inviare i dettagli dell'errore a ad FS Admins** : consente agli amministratori di configurare gli utenti finali per l'invio di log di debug relativi a un errore nell'autenticazione dell'utente finale da archiviare come file compresso per facilitarne l'uso. Gli amministratori possono anche configurare una connessione SMTP per inviare automaticamente il file compresso a un account di posta elettronica di valutazione o per creare automaticamente un ticket in base al messaggio di posta elettronica. 

### <a name="deployment-updates"></a>Aggiornamenti della distribuzione
Gli aggiornamenti della distribuzione seguenti sono ora inclusi nel AD FS 2019:
- **Livello di comportamento della farm 2019** -come con ad FS 2016, è disponibile una nuova versione del livello di comportamento della farm necessaria per abilitare le nuove funzionalità descritte in precedenza. Questo consente di passare da:
    - 2012 R2-> 2019
    - 2016-> 2019   

### <a name="saml-updates"></a>Aggiornamenti SAML
L'aggiornamento SAML seguente è AD FS 2019:
- **Correzione di bug: correzione di bug nella Federazione aggregata** . sono state apportate numerose correzioni di bug sul supporto di Federazione aggregato (ad esempio, insolito). Sono state apportate le correzioni seguenti: 
  - Miglioramento della scalabilità per entità di grandi dimensioni nel documento di metadati di Federazione aggregato. in precedenza, l'errore avrebbe avuto esito negativo con l'errore "ADMIN0017". 
  - Eseguire una query usando il parametro ' ScopeGroupID ' tramite il cmdlet Get-AdfsRelyingPartyTrustsGroup PSH. 
  - Gestione delle condizioni di errore circa entityID duplicati


### <a name="azure-ad-style-resource-specification-in-scope-parameter"></a>Specifica della risorsa di tipo Azure AD nel parametro di ambito 
In precedenza, AD FS necessario che la risorsa e l'ambito desiderati siano in un parametro separato in qualsiasi richiesta di autenticazione. Ad esempio, una tipica richiesta OAuth avrà un aspetto simile al seguente: 7 **https:&#47;&#47;FS.contoso.com/ADFS/OAuth2/Authorize?</br>response_type = code & client_id = claimsxrayclient & Resource = urn: Microsoft:</br>ADFS: claimsxray & scope = OAuth & redirect_uri =&#47;&#47;https: adfshelp.Microsoft.com/</br> claimsxray/TokenResponse & prompt = login**
 
Con AD FS sul server 2019, è ora possibile passare il valore della risorsa incorporato nel parametro scope. Questo è coerente con il modo in cui è possibile eseguire l'autenticazione anche per Azure AD. 

Il parametro scope può ora essere organizzato come un elenco separato da spazi in cui ogni voce è una struttura come risorsa/ambito. Per esempio  

**< creare una richiesta di esempio valida >**
> [!NOTE]
> Nella richiesta di autenticazione è possibile specificare una sola risorsa. Se nella richiesta sono incluse più risorse, AD FS restituirà un errore e l'autenticazione avrà esito negativo. 

### <a name="proof-key-for-code-exchange-pkce-support-for-oauth"></a>Supporto della chiave di prova per lo scambio di codice (PKCE) per oAuth 
I client OAuth pubblici che usano la concessione del codice di autorizzazione sono suscettibili all'attacco di intercettazione del codice di autorizzazione.  L'attacco è descritto correttamente nella RFC 7636. Per attenuare questo attacco, AD FS nel server 2019 supporta la chiave di prova per lo scambio di codice (PKCE) per il flusso di concessione del codice di autorizzazione OAuth. 
 
Per sfruttare il supporto PKCE, questa specifica aggiunge parametri aggiuntivi alle richieste di autorizzazione e token di accesso OAuth 2,0.

![Proofkey](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/adfs2019.png)

A. Il client crea e registra un segreto denominato "code_verifier" e deriva una versione trasformata "t (code_verifier)" (definita "code_challenge"), che viene inviata nella richiesta di autorizzazione OAuth 2,0 insieme al metodo di trasformazione "t_m". 

B. L'endpoint di autorizzazione risponde come di consueto, ma registra "t (code_verifier)" e il metodo di trasformazione. 

C. Il client invia quindi il codice di autorizzazione nella richiesta del token di accesso come di consueto, ma include il segreto "code_verifier" generato in (A). 

D. Il AD FS trasforma "code_verifier" e lo confronta con "t (code_verifier)" da (B).  L'accesso viene negato se non sono uguali. 

#### <a name="faq"></a>Domande frequenti 
**D.** È possibile passare il valore della risorsa come parte del valore dell'ambito, ad esempio il modo in cui le richieste vengono eseguite su Azure AD? 
</br>**Un.** Con AD FS sul server 2019, è ora possibile passare il valore della risorsa incorporato nel parametro scope. Il parametro scope può ora essere organizzato come un elenco separato da spazi in cui ogni voce è una struttura come risorsa/ambito. Per esempio  
**< creare una richiesta di esempio valida >**

**D.** AD FS supporta l'estensione PKCE?
</br>**Un.** AD FS nel server 2019 supporta la chiave di prova per lo scambio di codice (PKCE) per il flusso di concessione del codice di autorizzazione OAuth 

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
 *  [Azure Active Directory l'accesso condizionale](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access/)

Per ulteriori informazioni sull'utilizzo di dispositivi basati su accesso condizionale con AD FS
*  [Pianificazione dell'accesso condizionale basato su dispositivo con AD FS](../../ad-fs/deployment/Plan-Device-based-Conditional-Access-on-Premises.md)  
* [Criteri di controllo degli accessi in AD FS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="sign-in-with-windows-hello-for-business"></a>Accedi con Windows Hello for business   
I dispositivi Windows 10 introducono Windows Hello e Windows Hello for business, sostituendo le password utente con credenziali utente associate a dispositivi sicure protette dal gesto di un utente (un PIN, un movimento biometrico come l'impronta digitale o il riconoscimento facciale). AD FS 2016 supporta queste nuove funzionalità di Windows 10 in modo che gli utenti possano accedere alle applicazioni AD FS dalla Intranet o dalla Extranet senza dover specificare una password.

Per ulteriori informazioni sull'utilizzo di Microsoft Windows Hello for Business dell'organizzazione
*  [Abilitare Windows Hello for business nell'organizzazione](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)

## <a name="secure-access-to-applications"></a>Accesso sicuro alle applicazioni

### <a name="modern-authentication"></a>Autenticazione moderna
AD FS 2016 supporta i protocolli moderni più recenti che forniscono una migliore esperienza utente per Windows 10, nonché i più recenti e i dispositivi Android App iOS e.  

Per ulteriori informazioni vedere [AD FS scenari per gli sviluppatori](../../ad-fs/overview/ad-fs-openid-connect-oauth-flows-scenarios.md)  


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
