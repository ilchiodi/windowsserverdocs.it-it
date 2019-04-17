---
ms.assetid: b7bf7579-ca53-49e3-a26a-6f9f8690762f
title: Procedure ottimali per proteggere ADFS e Proxy applicazione Web
description: Questo documento fornisce le procedure consigliate per la pianificazione della protezione e la distribuzione di Active Directory Federation Services (ADFS) e Proxy applicazione Web.
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6026246228b22fdea6001528ab7621a1704f1983
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
## <a name="best-practices-for-securing-active-directory-federation-services"></a>Procedure consigliate per la protezione di Active Directory Federation Services

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo documento fornisce le procedure consigliate per la pianificazione della protezione e la distribuzione di Active Directory Federation Services (ADFS) e Proxy applicazione Web.  Contiene informazioni sui comportamenti predefiniti di questi componenti e i suggerimenti per le configurazioni di sicurezza aggiuntive per un'organizzazione con casi di utilizzo specifici e requisiti di sicurezza.

Questo documento si applica a ADFS e WAP in Windows Server 2012 R2 e Windows Server 2016 (anteprima).  Se l'infrastruttura viene distribuita in una rete locale in o in un ambiente ospitato nel cloud, come Microsoft Azure, è possono utilizzare questi suggerimenti.

## <a name="standard-deployment-topology"></a>Topologia di distribuzione standard
Per la distribuzione in ambienti locali, è consigliabile una topologia di distribuzione standard costituito da uno o più server ADFS nella rete azienda interna, con uno o più server Proxy applicazione Web (WAP) in una rete Perimetrale o in rete extranet.  A ogni livello, ADFS e WAP, un bilanciamento del carico hardware o software viene posizionato davanti la server farm e gestisce routing del traffico.  I firewall vengono inseriti come necessari in primo piano dell'indirizzo IP esterno del bilanciamento del carico davanti ogni farm (ADFS e proxy).

![Topologia ADFS Standard di Active Directory](media/Best-Practices-Securing-AD-FS/adfssec1.png)

## <a name="ports-required"></a>Porte necessarie
Il diagramma seguente illustra le porte del firewall che devono essere abilitate tra e tra i componenti della distribuzione di ADFS e WAP.  Se la distribuzione non include Azure AD o Office 365, i requisiti di sincronizzazione può essere ignorato.

>Si noti che 49443 la porta è solo necessario se viene utilizzata l'autenticazione del certificato utente, che è facoltativo per Azure AD e Office 365.

![Topologia ADFS Standard di Active Directory](media/Best-Practices-Securing-AD-FS/adfssec2.png)

### <a name="azure-ad-connect-and-federation-serverswap"></a>Azure AD Connect e i server federativi/WAP
Questa tabella descrive le porte e i protocolli necessari per la comunicazione tra il server Azure AD Connect e i server federativi/WAP.  

Protocollo |Porte |Descrizione
--------- | --------- |---------
HTTP|80 (TCP/UDP)|Utilizzato per il download di CRL (degli elenchi di revoche) per verificare i certificati SSL.
HTTPS|443(TCP/UDP)|Consente di sincronizzare con Azure AD.
Gestione remota Windows|5985| Listener WinRM

### <a name="wap-and-federation-servers"></a>WAP e i server federativi
Questa tabella descrive le porte e i protocolli necessari per la comunicazione tra i server federativi e i server WAP.

Protocollo |Porte |Descrizione
--------- | --------- |---------
HTTPS|443(TCP/UDP)|Utilizzato per l'autenticazione.

### <a name="wap-and-users"></a>Utenti e WAP
Questa tabella descrive le porte e i protocolli necessari per la comunicazione tra utenti e i server WAP.

Protocollo |Porte |Descrizione
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)|Usato per l'autenticazione del dispositivo.
TCP|49443 (TCP)|Usato per l'autenticazione del certificato.

Per ulteriori informazioni su porte necessarie e i protocolli necessari per le distribuzioni ibride, vedere il documento [qui](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect-ports/).

Per informazioni dettagliate sulle porte e protocolli richiesti per Azure AD e la distribuzione di Office 365, vedere il documento [qui](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).

### <a name="endpoints-enabled"></a>Endpoint abilitati

Quando vengono installati ADFS e WAP, un set predefinito di endpoint ADFS abilitati nel servizio federativo e il proxy.  Queste impostazioni predefinite sono stati scelti in base alle scenari più comunemente necessari e utilizzati e non è necessario modificarle.  

### <a name="optional-min-set-of-endpoints-proxy-enabled-for-azure-ad--office-365"></a>[Facoltativo] Set minimo di proxy endpoint abilitato per Azure AD o Office 365
Le organizzazioni che distribuiscono ADFS e WAP solo per Azure AD e Office 365 scenari possono limitare ulteriormente il numero degli endpoint ADFS abilitati nel proxy per ottenere una superficie di attacco più minima.
Ecco l'elenco degli endpoint che deve essere abilitata nel proxy in questi scenari:

|Endpoint|Scopo
|-----|-----
|/ adfs/ls|Flussi di autenticazione basata su browser e le versioni correnti di Microsoft Office utilizzano questo endpoint per Azure AD e autenticazione di Office 365
|/ADFS/Services/trust/2005/usernamemixed|Utilizzato per Exchange Online con Office client più vecchi di aggiornamento di maggio 2015 di Office 2013.  I client successivi utilizzano l'endpoint \adfs\ls passivo.
|/ADFS/Services/trust/13/usernamemixed|Utilizzato per Exchange Online con Office client più vecchi di aggiornamento di maggio 2015 di Office 2013.  I client successivi utilizzano l'endpoint \adfs\ls passivo.
|/ adfs/oauth2|Questo viene utilizzato per le app moderne (in locale o nel cloud) è stato configurato per l'autenticazione direttamente in ADFS (ad esempio, non tramite AAD)
|/ADFS/Services/trust/mex|Utilizzato per Exchange Online con Office client più vecchi di aggiornamento di maggio 2015 di Office 2013.  I client successivi utilizzano l'endpoint \adfs\ls passivo.
|/ADFS/ls/FederationMetadata/2007-06/FederationMetadata.Xml |Requisito per tutti i flussi passivi; e utilizzato da Office 365 o Azure AD per controllare i certificati AD FS


Gli endpoint di AD FS possono essere disabilitati nel proxy usando il cmdlet PowerShell seguente:
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath <address path> -Proxy $false

Per esempio:
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/13/certificatemixed -Proxy $false
    

### <a name="extended-protection-for-authentication"></a>Protezione estesa per l'autenticazione
La protezione estesa per l'autenticazione è una funzionalità che consente di prevenire man attacchi (MITM) ed è abilitata per impostazione predefinita con AD FS.

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>Per verificare le impostazioni, è possibile eseguire le operazioni seguenti:
L'impostazione può essere verificata tramite il seguente cmdlet PowerShell.  
    
   `PS:\>Get-ADFSProperties`

La proprietà è `ExtendedProtectionTokenCheck`.  L'impostazione predefinita è Consenti, in modo che i vantaggi di sicurezza possono essere ottenuti senza i problemi di compatibilità con i browser che non supportano la funzionalità.  

### <a name="congestion-control-to-protect-the-federation-service"></a>Controllo della congestione per proteggere il servizio federativo
Proxy servizio federativo (incluso il WAP) fornisce un controllo della congestione per proteggere il servizio ADFS da un flusso eccessivo di richieste.  Se il server federativo è in overload rilevate tramite la latenza tra il Proxy dell'applicazione Web e il server federativo, il Proxy dell'applicazione Web rifiuterà le richieste di autenticazione client esterni.  Questa funzionalità viene configurata per impostazione predefinita con un livello di soglia di latenza consigliata.

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>Per verificare le impostazioni, è possibile eseguire le operazioni seguenti:
1.  Sul computer Proxy applicazione Web, avviare una finestra del prompt dei comandi.
2.  Passare alla directory di ADFS, % WINDIR%\adfs\config.
3.  Modificare le impostazioni di controllo della congestione da valori predefiniti per '<congestionControl latencyThresholdInMSec="8000" minCongestionWindowSize="64" enabled="true" />'.
4.  Salvare e chiudere il file.
5.  Riavviare il servizio ADFS eseguendo 'adfssrv net stop' e 'adfssrv net start'.
Per riferimento, è possono trovare indicazioni su questa funzionalità [qui](https://msdn.microsoft.com/en-us/library/azure/dn528859.aspx ).

### <a name="standard-http-request-checks-at-the-proxy"></a>Controlla il proxy di richiesta HTTP standard
Il proxy esegue inoltre i seguenti controlli standard da tutto il traffico:

- ADFS-P stesso esegue l'autenticazione ad ADFS tramite un certificato di breve durato.  In uno scenario di sospetta dei server di rete perimetrale, ADFS può "revocare il trust proxy" in modo che non è più trusted tutte le richieste in ingresso dai potenzialmente compromessi proxy. Revocare il trust proxy revoca ogni certificato del proxy in modo che è possibile effettuare l'autenticazione per qualsiasi scopo per il server ADFS
- Il FS-P termina tutte le connessioni e crea una nuova connessione HTTP per il servizio ADFS nella rete interna. Ciò fornisce un buffer di livello di sessione tra dispositivi esterni e il servizio ADFS. Il dispositivo esterno mai si connette direttamente al servizio ADFS.
- Il FS-P esegue la convalida delle richieste HTTP in modo specifico per escludere le intestazioni HTTP che non sono richiesti dal servizio ADFS.

## <a name="recommended-security-configurations"></a>Configurazioni di protezione consigliate
Verificare tutti ADFS e WAP server ricevono gli aggiornamenti più recenti che è l'indicazione di sicurezza più importante per l'infrastruttura di ADFS per assicurarsi che disporre di un mezzo per mantenere i server ADFS e WAP con tutti gli aggiornamenti della sicurezza, nonché gli aggiornamenti facoltativi specificati come importanti per AD FS in questa pagina.

Il metodo consigliato per i clienti di Azure AD monitorare e mantenere aggiornati i loro infrastruttura è tramite Azure Active Directory connettersi integrità per AD FS, una funzionalità di Azure AD Premium.  Integrità connettersi AD Azure include monitoraggi e avvisi che generano se un computer ADFS o WAP manca uno degli aggiornamenti importanti in modo specifico per ADFS e WAP.

Informazioni sull'installazione di integrità di connettersi AD Azure per ADFS sono reperibili [qui](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect-health-agent-install/).

## <a name="additional-security-configurations"></a>Configurazioni di sicurezza aggiuntive
Le seguenti funzionalità aggiuntive può essere configurata facoltativamente per fornire protezioni aggiuntive a quelle fornite nella distribuzione predefinito.

### <a name="extranet-soft-lockout-protection-for-accounts"></a>Protezione il blocco della Extranet "soft" per gli account
Con la funzionalità di blocco della extranet in Windows Server 2012 R2, un amministratore di AD FS può impostare un numero massimo consentito di richieste di autenticazione non riuscito (ExtranetLockoutThreshold) e una "finestra di osservazione periodo di tempo (ExtranetObservationWindow). Quando viene raggiunto il numero massimo (ExtranetLockoutThreshold) delle richieste di autenticazione, AD FS interrompere i tentativi di autenticare le credenziali dell'account fornito da ADFS per il periodo di tempo impostato (ExtranetObservationWindow). Questa azione consente di proteggere l'account da un blocco degli account di Active Directory, in altre parole, questo account consente di proteggere da perdere l'accesso alle risorse aziendali che si basano su ADFS per l'autenticazione dell'utente. Queste impostazioni si applicano a tutti i domini in grado di autenticare il servizio ADFS.

È possibile utilizzare il comando di Windows PowerShell seguente per impostare il blocco della extranet di AD FS (esempio): 

    PS:\>Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow ( new-timespan -Minutes 30 )

Per informazioni di riferimento, la documentazione pubblica di questa funzionalità è [qui](https://technet.microsoft.com/en-us/library/dn486806.aspx ). 

### <a name="differentiate-access-policies-for-intranet-and-extranet-access"></a>Distinguere i criteri di accesso per l'accesso intranet ed extranet
ADFS è in grado di distinguere i criteri di accesso per le richieste provenienti dalle richieste di rete aziendale locale vs fornite in da internet tramite il proxy.  Questa operazione può essere eseguita per ogni applicazione o a livello globale.  Per le applicazioni di valore elevato per l'azienda o le applicazioni con dati sensibili o personale, è consigliabile richiedere autenticazione a più fattori.  Questa operazione può essere eseguita tramite lo snap-in Gestione ADFS.  

### <a name="require-multi-factor-authentication-mfa"></a>Richiedono l'autenticazione a più fattori (MFA)
ADFS può essere configurato per richiedere l'autenticazione avanzata (ad esempio, l'autenticazione a più fattori), in particolare per le richieste in entrata tramite il proxy per le singole applicazioni e per l'accesso condizionale ad entrambi Azure AD o Office 365 e risorse locali.  Supportati i metodi di autenticazione a più fattori includono provider di autenticazione a più fattori di Microsoft Azure e di terze parti.  L'utente viene richiesto di fornire le informazioni aggiuntive (ad esempio un testo SMS contenente un codice di tempo), e AD FS funziona con la specifica del provider del plug-in per consentire l'accesso.  

Provider di autenticazione a più fattori esterni supportati comprendono quelli elencati [questo](https://technet.microsoft.com/en-us/library/dn758113.aspx) pagina, nonché HDI globale.

### <a name="hardware-security-module-hsm"></a>Modulo di protezione hardware (HSM)
Nella configurazione predefinita, le chiavi ADFS viene utilizzato per firmare i token mai lascia i server federativi nella rete intranet.  Non sono mai presenti nella rete Perimetrale o sui computer proxy.  Facoltativamente per fornire protezione aggiuntiva, queste chiavi possono essere protetti in un modulo di protezione hardware collegato ad ADFS.  Microsoft non produce un prodotto HSM, tuttavia, sono presenti diversi sul mercato che supportano ADFS.  Per implementare questa indicazione, segui le indicazioni del fornitore per creare il X509 i certificati per la firma e crittografia, quindi utilizzare i cmdlet powershell di installazione di AD FS, specificando i certificati personalizzati, come indicato di seguito:

    PS:\>Install-AdfsFarm -CertificateThumbprint <String> -DecryptionCertificateThumbprint <String> -FederationServiceName <String> -ServiceAccountCredential <PSCredential> -SigningCertificateThumbprint <String>

dove:


- `CertificateThumbprint` è il certificato SSL
- `SigningCertificateThumbprint` il certificato di firma (con chiavi protette del modulo)
- `DecryptionCertificateThumbprint` il certificato di crittografia (con chiavi protette del modulo)



