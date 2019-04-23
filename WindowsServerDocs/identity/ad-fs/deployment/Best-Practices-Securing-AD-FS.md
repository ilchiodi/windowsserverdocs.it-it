---
ms.assetid: b7bf7579-ca53-49e3-a26a-6f9f8690762f
title: Le procedure consigliate per la protezione di AD FS e Proxy applicazione Web
description: Questo documento fornisce le procedure consigliate per la pianificazione sicura e distribuzione di Active Directory Federation Services (ADFS) e Proxy applicazione Web.
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 38de2bca413ce7f8aeda2af4392f9a616641b189
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873072"
---
## <a name="best-practices-for-securing-active-directory-federation-services"></a>Le procedure consigliate per la protezione di Active Directory Federation Services

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo documento fornisce le procedure consigliate per la pianificazione sicura e distribuzione di Active Directory Federation Services (ADFS) e Proxy applicazione Web.  Contiene informazioni sui comportamenti predefiniti di questi componenti e le raccomandazioni per le configurazioni di sicurezza aggiuntive per un'organizzazione con requisiti di sicurezza e casi d'uso specifici.

Questo documento si applica ad AD FS e WAP in Windows Server 2012 R2 e Windows Server 2016 (anteprima).  Questi consigli possono essere usati se l'infrastruttura viene distribuito in una rete locale o in un ambiente ospitato nel cloud come Microsoft Azure.

## <a name="standard-deployment-topology"></a>Topologia di distribuzione standard
Per la distribuzione in ambienti locali, è consigliabile una topologia di distribuzione standard costituito da uno o più server AD FS nella rete azienda interna, con uno o più server Proxy applicazione Web (WAP) in una rete Perimetrale o in rete extranet.  A ogni livello, AD FS e WAP, bilanciamento del carico hardware o software è posizionato davanti alla server farm e gestisce il routing del traffico.  I firewall vengono inseriti come richiesto in primo piano dell'indirizzo IP esterno del bilanciamento del carico davanti a ogni farm (FS e proxy).

![Topologia di AD FS Standard](media/Best-Practices-Securing-AD-FS/adfssec1.png)

## <a name="ports-required"></a>Porte richieste
Il diagramma seguente mostra le porte del firewall che devono essere abilitate tra e tra i componenti della distribuzione di AD FS e WAP.  Se la distribuzione non include Azure AD / Office 365, i requisiti di sincronizzazione può essere ignorato.

>Si noti che la porta 49443 è solo necessario se viene utilizzata l'autenticazione del certificato utente, che è facoltativo per Azure AD e Office 365.

![Topologia di AD FS Standard](media/Best-Practices-Securing-AD-FS/adfssec2.png)

### <a name="azure-ad-connect-and-federation-serverswap"></a>Azure AD Connect e server federativi/WAP
La tabella seguente descrive le porte e protocolli necessari per la comunicazione tra il server Azure AD Connect e server federativi/WAP.  

Protocollo |Porte |Descrizione
--------- | --------- |---------
HTTP|80 (TCP/UDP)|Utilizzato per il download di CRL (Certificate Revocation List) per verificare i certificati SSL.
HTTPS|443(TCP/UDP)|Utilizzato per sincronizzare con Azure AD.
WinRM|5985| Listener di WinRM

### <a name="wap-and-federation-servers"></a>Server federativi e WAP
La tabella seguente descrive le porte e protocolli necessari per la comunicazione tra i server federativi e i server WAP.

Protocollo |Porte |Descrizione
--------- | --------- |---------
HTTPS|443(TCP/UDP)|Utilizzato per l'autenticazione.

### <a name="wap-and-users"></a>WAP e utenti
La tabella seguente descrive le porte e protocolli necessari per la comunicazione tra utenti e i server WAP.

Protocollo |Porte |Descrizione
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)|Utilizzato per l'autenticazione del dispositivo.
TCP|49443 (TCP)|Utilizzato per l'autenticazione del certificato.

Per altre informazioni sulle porte necessarie e i protocolli necessari per le distribuzioni ibride, vedere il documento [qui](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-ports/).

Per informazioni dettagliate sulle porte e protocolli necessari per un'istanza di AD Azure e la distribuzione di Office 365, vedere il documento [qui](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).

### <a name="endpoints-enabled"></a>Endpoint abilitati

Quando vengono installati AD FS e WAP, un set predefinito degli endpoint ADFS abilitati nel servizio federativo e del proxy.  Queste impostazioni predefinite sono state scelto in base a scenari più comunemente necessari e usati e non è necessario modificarle.  

### <a name="optional-min-set-of-endpoints-proxy-enabled-for-azure-ad--office-365"></a>[Facoltativo] Set minimo di proxy endpoint abilitato per Azure AD / Office 365
Le organizzazioni che distribuiscono solo per Azure AD AD FS e WAP e scenari di Office 365 possono ulteriormente limitare il numero degli endpoint ADFS abilitati sul proxy per ottenere una superficie di attacco più minima.
Seguito è riportato l'elenco di endpoint ai quali è necessario attivare il proxy in questi scenari:

|Endpoint|Scopo
|-----|-----
|/adfs/ls|I flussi di autenticazione basata su browser e le versioni correnti di Microsoft Office usano questo endpoint per Azure AD e l'autenticazione di Office 365
|/adfs/services/trust/2005/usernamemixed|Usato per Exchange Online con i client di Office meno recente aggiornamento di Office 2013 maggio 2015.  I client di versioni successive utilizzano l'endpoint \adfs\ls passivo.
|/adfs/services/trust/13/usernamemixed|Usato per Exchange Online con i client di Office meno recente aggiornamento di Office 2013 maggio 2015.  I client di versioni successive utilizzano l'endpoint \adfs\ls passivo.
|/adfs/oauth2|Questa viene usata per qualsiasi App moderne (in locale o nel cloud) è stato configurato per eseguire l'autenticazione diretta a AD FS (ossia, non tramite AAD)
|/adfs/services/trust/mex|Usato per Exchange Online con i client di Office meno recente aggiornamento di Office 2013 maggio 2015.  I client di versioni successive utilizzano l'endpoint \adfs\ls passivo.
|/adfs/ls/federationmetadata/2007-06/federationmetadata.xml |Requisito per tutti i flussi passivi; e usati da Office 365 / Azure AD per verificare che i certificati di AD FS


Gli endpoint di AD FS possono essere disabilitati nel proxy usando il cmdlet di PowerShell seguente:
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath <address path> -Proxy $false

Ad esempio: 
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/13/certificatemixed -Proxy $false
    

### <a name="extended-protection-for-authentication"></a>Protezione estesa per l'autenticazione
Protezione estesa per l'autenticazione è una funzionalità che consente di ridurre man attacchi (MITM) e viene abilitata per impostazione predefinita con AD FS.

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>Per verificare le impostazioni, è possibile eseguire le operazioni seguenti:
L'impostazione può essere verificata tramite il commandlet di PowerShell di seguito.  
    
   `PS:\>Get-ADFSProperties`

La proprietà è `ExtendedProtectionTokenCheck`.  L'impostazione predefinita è Consenti, in modo che i vantaggi di sicurezza possono essere ottenuti senza le problematiche di compatibilità con i browser che non supportano la funzionalità.  

### <a name="congestion-control-to-protect-the-federation-service"></a>Controllo della congestione per proteggere il servizio federativo
Il proxy servizio federativo (parte di WAP) fornisce il controllo di congestione per proteggere il servizio AD FS da un flusso eccessivo di richieste.  Il Proxy applicazione Web rifiuterà le richieste di autenticazione client esterne se sovraccarico del server federativo rilevato dalla latenza tra il Proxy applicazione Web e il server federativo.  Questa funzionalità viene configurata per impostazione predefinita con un livello di soglia di latenza consigliato.

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>Per verificare le impostazioni, è possibile eseguire le operazioni seguenti:
1.  Nel computer Proxy applicazione Web, avviare un prompt dei comandi con privilegi elevati.
2.  Passare alla directory di ad FS, nel percorso % WINDIR%\adfs\config.
3.  Modificare le impostazioni di controllo di congestione i valori predefiniti per '<congestionControl latencyThresholdInMSec="8000" minCongestionWindowSize="64" enabled="true" />'.
4.  Salvare e chiudere il file.
5.  Riavviare il servizio AD FS eseguendo 'net stop adfssrv' quindi 'adfssrv net start'.
Per riferimento, sono disponibili informazioni aggiuntive su questa funzionalità [qui](https://msdn.microsoft.com/en-us/library/azure/dn528859.aspx ).

### <a name="standard-http-request-checks-at-the-proxy"></a>Richiesta HTTP standard consente di controllare in corrispondenza del proxy
Il proxy effettua anche i controlli standard seguenti per tutto il traffico:

- Esegue l'autenticazione di FS-P stesso ad AD FS tramite un certificato di breve durato.  In uno scenario di sospetta compromissione dei server di rete perimetrale, ADFS può "revoke trust proxy" in modo che lo non riterrà più attendibili le richieste in arrivo da potenzialmente compromessa proxy. Revoca la relazione di trust proxy impedisce ogni certificato del proxy in modo che non è possibile eseguire l'autenticazione per qualsiasi scopo per il server AD FS
- FS-P termina tutte le connessioni e crea una nuova connessione HTTP al servizio AD FS nella rete interna. Ciò fornisce un buffer a livello di sessione tra il servizio AD FS e i dispositivi esterni. I dispositivi esterni si connette mai direttamente al servizio AD FS.
- FS-P esegue la convalida delle richieste HTTP che esclude specificamente le intestazioni HTTP non richiesti dal servizio AD FS.

## <a name="recommended-security-configurations"></a>Configurazioni di sicurezza consigliati
Verificare che tutti i server AD FS e WAP ricevano gli aggiornamenti più recenti che di raccomandazione di sicurezza più importante per l'infrastruttura AD FS è avere a disposizione uno strumento in grado di mantenere i server AD FS e WAP con tutti gli aggiornamenti della sicurezza, come tali facoltativo aggiornamenti specificati come importanti per AD FS in questa pagina.

Il metodo consigliato per i clienti di Azure AD monitorare e mantenere aggiornato l'infrastruttura è tramite Azure AD Connect Health per AD FS, una funzionalità di Azure AD Premium.  Azure AD Connect Health include monitoraggi e avvisi che attivano se un computer WAP o AD FS non è presente uno degli aggiornamenti importanti in modo specifico per AD FS e WAP.

Informazioni sull'installazione di Azure AD Connect Health per AD FS reperibili [qui](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-health-agent-install/).

## <a name="additional-security-configurations"></a>Configurazioni di sicurezza aggiuntive
Le seguenti funzionalità aggiuntive può essere configurata facoltativamente per fornire protezioni aggiuntive a quelle offerte nella distribuzione predefinita.

### <a name="extranet-soft-lockout-protection-for-accounts"></a>Protezione di blocco Extranet "soft" per gli account
La funzionalità di blocco extranet in Windows Server 2012 R2, un amministratore di AD FS può impostare un numero massimo consentito di richieste di autenticazione non riuscita (ExtranetLockoutThreshold) e un "della finestra di osservazione periodo di tempo (ExtranetObservationWindow). Quando viene raggiunto il numero massimo (ExtranetLockoutThreshold) delle richieste di autenticazione, AD FS arresti il tentativo di autenticare le credenziali dell'account fornita in ADFS per il periodo di tempo specifico (ExtranetObservationWindow). Questa azione consente di proteggere l'account da un blocco degli account di Active Directory, in altre parole, questo account consente di proteggere da perdere l'accesso alle risorse aziendali che si basano su AD FS per l'autenticazione dell'utente. Queste impostazioni si applicano a tutti i domini in grado di autenticare il servizio AD FS.

È possibile usare il comando Windows PowerShell seguente per impostare il blocco extranet di AD FS (ad esempio): 

    PS:\>Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow ( new-timespan -Minutes 30 )

Per riferimento, la documentazione pubblica di questa funzionalità viene [qui](https://technet.microsoft.com/library/dn486806.aspx ). 

### <a name="differentiate-access-policies-for-intranet-and-extranet-access"></a>Distinguere i criteri di accesso per l'accesso extranet e intranet
AD FS offre la possibilità di differenziare i criteri di accesso per le richieste originate rete aziendale locale rispetto alle richieste provenienti da internet tramite il proxy.  Questa operazione può essere eseguita per ogni applicazione o a livello globale.  Per le applicazioni di valore elevato per l'azienda o con le informazioni riservate o personali, è consigliabile richiedere multi-factor authentication.  Questa operazione può essere eseguita tramite lo snap-in Gestione ADFS.  

### <a name="require-multi-factor-authentication-mfa"></a>Richiedi Multi-factor authentication (MFA)
ADFS può essere configurato per richiedere l'autenticazione avanzata (ad esempio multi-factor authentication) in modo specifico per le richieste in arrivo tramite il proxy per le singole applicazioni e per l'accesso condizionale per sia per Azure AD / Office 365, nonché le risorse locali.  Metodi supportati di MFA includono i provider di Microsoft Azure MFA e di terze parti.  L'utente viene richiesto di fornire le informazioni aggiuntive (ad esempio un messaggio SMS contenente un codice di tempo), e AD FS funziona con specifici del provider del plug-in per consentire l'accesso.  

Provider di autenticazione a più fattori esterni supportati comprendono quelli elencati nella [ciò](https://technet.microsoft.com/library/dn758113.aspx) pagina, nonché HDI globale.

### <a name="hardware-security-module-hsm"></a>Modulo di protezione hardware (HSM)
La configurazione predefinita, gli utilizzi chiavi ADFS per firmare i token mai lasciare i server federativi nella rete intranet.  Non sono presenti nella rete Perimetrale o sui computer proxy.  Facoltativamente, per fornire protezione aggiuntiva, queste chiavi possono essere protette in un modulo di protezione hardware collegato ad AD FS.  Microsoft non produce un prodotto di modulo di protezione hardware, tuttavia, esistono diversi sul mercato che supportano ADFS.  Per implementare questa raccomandazione, seguire le indicazioni del fornitore per creare X509 i certificati per la firma e crittografia, quindi utilizzare i commandlet di powershell installazione di AD FS, specificando i certificati personalizzati, come indicato di seguito:

    PS:\>Install-AdfsFarm -CertificateThumbprint <String> -DecryptionCertificateThumbprint <String> -FederationServiceName <String> -ServiceAccountCredential <PSCredential> -SigningCertificateThumbprint <String>

dove:


- `CertificateThumbprint` è il certificato SSL
- `SigningCertificateThumbprint` è il certificato di firma (con la chiave HSM protetta)
- `DecryptionCertificateThumbprint` è il certificato di crittografia (con la chiave HSM protetta)



