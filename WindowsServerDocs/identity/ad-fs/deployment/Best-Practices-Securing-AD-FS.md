---
ms.assetid: b7bf7579-ca53-49e3-a26a-6f9f8690762f
title: Procedure consigliate per la protezione di AD FS e proxy applicazione Web
description: In questo documento vengono illustrate le procedure consigliate per la pianificazione e la distribuzione sicure di Active Directory Federation Services (AD FS) e del proxy dell'applicazione Web.
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 717308a157d7f4a5f54e3aef2e829fbed9f12152
ms.sourcegitcommit: 1c75e4b3f5895f9fa33efffd06822dca301d4835
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/20/2020
ms.locfileid: "77517546"
---
# <a name="best-practices-for-securing-active-directory-federation-services"></a>Procedure consigliate per la protezione di Active Directory Federation Services

In questo documento vengono illustrate le procedure consigliate per la pianificazione e la distribuzione sicure di Active Directory Federation Services (AD FS) e del proxy dell'applicazione Web.  Contiene informazioni sui comportamenti predefiniti di questi componenti e indicazioni per configurazioni di sicurezza aggiuntive per un'organizzazione con specifici casi d'uso e requisiti di sicurezza.

Questo documento si applica a AD FS e WAP in Windows Server 2012 R2 e Windows Server 2016 (anteprima).  Questi consigli possono essere usati se l'infrastruttura viene distribuita in una rete locale o in un ambiente cloud ospitato, ad esempio Microsoft Azure.

## <a name="standard-deployment-topology"></a>Topologia di distribuzione standard
Per la distribuzione in ambienti locali, è consigliabile usare una topologia di distribuzione standard costituita da uno o più server AD FS nella rete aziendale interna, con uno o più server proxy applicazione Web (WAP) in una rete perimetrale o Extranet.  A ogni livello, AD FS e WAP, un servizio di bilanciamento del carico hardware o software viene posizionato davanti ai server farm e gestisce il routing del traffico.  I firewall vengono posizionati come richiesto davanti all'indirizzo IP esterno del servizio di bilanciamento del carico davanti a ogni farm (FS e proxy).

![AD FS topologia standard](media/Best-Practices-Securing-AD-FS/adfssec1.png)

>[!NOTE]
> AD FS richiede un controller di dominio scrivibile completo per funzionare anziché un controller di dominio di sola lettura. Se una topologia pianificata include un controller di dominio di sola lettura, il controller di dominio di sola lettura può essere utilizzato per l'autenticazione, ma l'elaborazione delle attestazioni LDAP richiederà una connessione al controller di dominio scrivibile.

## <a name="ports-required"></a>Porte richieste
Il diagramma seguente illustra le porte del firewall che devono essere abilitate tra e tra i componenti della distribuzione di AD FS e WAP.  Se la distribuzione non include Azure AD/Office 365, i requisiti di sincronizzazione possono essere ignorati.

>Si noti che la porta 49443 è obbligatoria solo se si usa l'autenticazione del certificato utente, che è facoltativa per Azure AD e Office 365.

![AD FS topologia standard](media/Best-Practices-Securing-AD-FS/adfssec2.png)

>[!NOTE]
> La porta 808 (Windows Server 2012R2) o la porta 1501 (Windows Server 2016 +) è la porta Net. TCP AD FS USA per l'endpoint WCF locale per trasferire i dati di configurazione al processo del servizio e a PowerShell. Questa porta può essere visualizzata eseguendo Get-AdfsProperties | Selezionare NetTcpPort. Si tratta di una porta locale che non deve essere aperta nel firewall ma che verrà visualizzata in un'analisi delle porte. 

### <a name="azure-ad-connect-and-federation-serverswap"></a>Azure AD Connect e server federativi/WAP
Questa tabella descrive le porte e i protocolli necessari per la comunicazione tra il server Azure AD Connect e i server federativi/WAP.  

Protocollo |Porte |Descrizione
--------- | --------- |---------
HTTP|80 (TCP/UDP)|Utilizzato per scaricare i CRL (elenchi di revoche di certificati) per verificare i certificati SSL.
HTTPS|443 (TCP/UDP)|Utilizzato per sincronizzare con Azure AD.
WinRM|5985| Listener WinRM

### <a name="wap-and-federation-servers"></a>Server WAP e federativi
Questa tabella descrive le porte e i protocolli necessari per la comunicazione tra i server federativi e i server WAP.

Protocollo |Porte |Descrizione
--------- | --------- |---------
HTTPS|443 (TCP/UDP)|Usato per l'autenticazione.

### <a name="wap-and-users"></a>WAP e utenti
Questa tabella descrive le porte e i protocolli necessari per la comunicazione tra gli utenti e i server WAP.

Protocollo |Porte |Descrizione
--------- | --------- |--------- |
HTTPS|443 (TCP/UDP)|Usato per l'autenticazione del dispositivo.
TCP|49443 (TCP)|Usato per l'autenticazione del certificato.

Per ulteriori informazioni sulle porte e sui protocolli richiesti per le distribuzioni ibride, vedere il documento [qui](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-ports).

Per informazioni dettagliate sulle porte e i protocolli necessari per una distribuzione di Azure AD e Office 365, vedere il documento [qui](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).

### <a name="endpoints-enabled"></a>Endpoint abilitati

Quando vengono installati AD FS e WAP, viene abilitato un set predefinito di AD FS endpoint sul servizio federativo e sul proxy.  Queste impostazioni predefinite sono state scelte in base agli scenari più comunemente richiesti e usati e non è necessario modificarle.  

### <a name="optional-min-set-of-endpoints-proxy-enabled-for-azure-ad--office-365"></a>Opzionale Set minimo di endpoint proxy abilitato per Azure AD/Office 365
Le organizzazioni che distribuiscono AD FS e WAP solo per gli scenari Azure AD e Office 365 possono limitare ulteriormente il numero di AD FS endpoint abilitati sul proxy per ottenere una superficie di attacco più minima.
Di seguito è riportato l'elenco degli endpoint che devono essere abilitati nel proxy in questi scenari:

|Endpoint|Scopo
|-----|-----
|/ADFS/ls|I flussi di autenticazione basati su browser e le versioni correnti di Microsoft Office usano questo endpoint per l'autenticazione Azure AD e Office 365
|/adfs/services/trust/2005/usernamemixed|Usato per Exchange Online con client di Office precedenti a Office 2013, maggio 2015.  I client successivi usano l'endpoint \adfs\ls passivo.
|/adfs/services/trust/13/usernamemixed|Usato per Exchange Online con client di Office precedenti a Office 2013, maggio 2015.  I client successivi usano l'endpoint \adfs\ls passivo.
|/adfs/oauth2|Questo viene usato per tutte le app moderne (in locale o nel cloud) che sono state configurate per l'autenticazione diretta a AD FS (ad esempio, non tramite AAD)
|/adfs/services/trust/mex|Usato per Exchange Online con client di Office precedenti a Office 2013, maggio 2015.  I client successivi usano l'endpoint \adfs\ls passivo.
|/adfs/ls/federationmetadata/2007-06/federationmetadata.xml |Requisito per i flussi passivi; e usati da Office 365/Azure AD per controllare AD FS certificati


AD FS gli endpoint possono essere disabilitati sul proxy usando il cmdlet di PowerShell seguente:
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath <address path> -Proxy $false

Ad esempio,
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/13/certificatemixed -Proxy $false
    

### <a name="extended-protection-for-authentication"></a>Protezione estesa per l'autenticazione
La protezione estesa per l'autenticazione è una funzionalità che attenua gli attacchi di tipo man in the Middle (MITM) ed è abilitata per impostazione predefinita con AD FS.

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>Per verificare le impostazioni, è possibile eseguire le operazioni seguenti:
L'impostazione può essere verificata usando il cmdlet di PowerShell seguente.  
    
   `PS:\>Get-ADFSProperties`

La proprietà è `ExtendedProtectionTokenCheck`.  L'impostazione predefinita è Consenti, in modo da ottenere i vantaggi di sicurezza senza i problemi di compatibilità con i browser che non supportano la funzionalità.  

### <a name="congestion-control-to-protect-the-federation-service"></a>Controllo di congestione per la protezione del servizio federativo
Il proxy del servizio federativo (parte del WAP) fornisce il controllo della congestione per proteggere il servizio AD FS da un diluvio di richieste.  Il proxy dell'applicazione Web rifiuterà le richieste di autenticazione client esterne se il server federativo è sovraccarico come rilevato dalla latenza tra il proxy dell'applicazione Web e il server federativo.  Questa funzionalità è configurata per impostazione predefinita con un livello di soglia di latenza consigliato.

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>Per verificare le impostazioni, è possibile eseguire le operazioni seguenti:
1.  Nel computer proxy applicazione Web aprire una finestra di comando con privilegi elevati.
2.  Passare alla directory ADFS, in%WINDIR%\adfs\config.
3.  Modificare le impostazioni di controllo di congestione da valori predefiniti a "<congestionControl latencyThresholdInMSec="8000" minCongestionWindowSize="64" enabled="true" />".
4.  Salvare e chiudere il file.
5.  Riavviare il servizio AD FS eseguendo ' net stop adfssrv ' e quindi ' net start adfssrv '.
Per informazioni di riferimento, vedere la sezione relativa a questa funzionalità disponibile [qui](https://msdn.microsoft.com/library/azure/dn528859.aspx ).

### <a name="standard-http-request-checks-at-the-proxy"></a>Controlli della richiesta HTTP standard nel proxy
Il proxy esegue anche i controlli standard seguenti rispetto a tutto il traffico:

- Il FS-P esegue l'autenticazione per AD FS tramite un certificato di breve durata.  In uno scenario di sospetta compromissione dei server DMZ, AD FS possibile "revocare il trust del proxy" in modo che non richieda più le richieste in ingresso provenienti da proxy potenzialmente compromessi. Revocando il trust del proxy, viene revocato il certificato di ogni proxy in modo che non possa essere autenticato correttamente per qualsiasi scopo nel server AD FS
- FS-P termina tutte le connessioni e crea una nuova connessione HTTP al servizio AD FS sulla rete interna. In questo modo viene fornito un buffer a livello di sessione tra dispositivi esterni e il servizio AD FS. Il dispositivo esterno non si connette mai direttamente al servizio AD FS.
- FS-P esegue la convalida delle richieste HTTP che filtra in modo specifico le intestazioni HTTP non richieste dal servizio AD FS.

## <a name="recommended-security-configurations"></a>Configurazioni di sicurezza consigliate
Assicurarsi che tutti i server AD FS e WAP ricevano gli aggiornamenti più aggiornati. l'indicazione di sicurezza più importante per l'infrastruttura AD FS consiste nel garantire che siano disponibili i mezzi per mantenere aggiornati i server AD FS e WAP con tutti gli aggiornamenti della sicurezza, oltre a quelli facoltativi aggiornamenti specificati come importanti per AD FS in questa pagina.

Il modo consigliato per Azure AD ai clienti di monitorare e tenere aggiornata la loro infrastruttura è tramite Azure AD Connect Health per AD FS, una funzionalità di Azure AD Premium.  Azure AD Connect Health include i monitoraggi e gli avvisi che vengono attivati se nel computer AD FS o WAP manca uno degli aggiornamenti importanti specifici per AD FS e WAP.

Per informazioni sull'installazione di Azure AD Connect Health per AD FS, vedere [qui](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-health-agent-install/).

## <a name="additional-security-configurations"></a>Configurazioni di sicurezza aggiuntive
Le funzionalità aggiuntive seguenti possono essere configurate facoltativamente per fornire protezioni aggiuntive a quelle offerte nella distribuzione predefinita.

### <a name="extranet-soft-lockout-protection-for-accounts"></a>Protezione blocco "soft" Extranet per gli account
Con la funzionalità di blocco Extranet in Windows Server 2012 R2, un amministratore AD FS può impostare un numero massimo consentito di richieste di autenticazione non riuscite (ExtranetLockoutThreshold) e un "periodo di tempo della finestra di osservazione (ExtranetObservationWindow). Quando viene raggiunto questo numero massimo (ExtranetLockoutThreshold) di richieste di autenticazione, AD FS interrompe il tentativo di autenticare le credenziali dell'account fornite rispetto AD FS per il periodo di tempo impostato (ExtranetObservationWindow). Questa azione protegge questo account da un blocco dell'account di Active Directory, in altre parole protegge questo account dalla perdita di accesso alle risorse aziendali basate su AD FS per l'autenticazione dell'utente. Queste impostazioni si applicano a tutti i domini che possono essere autenticati dal servizio AD FS.

È possibile utilizzare il comando di Windows PowerShell seguente per impostare il blocco AD FS Extranet (esempio): 

    PS:\>Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow ( new-timespan -Minutes 30 )

Per riferimento, la documentazione pubblica di questa funzionalità è disponibile [qui](https://technet.microsoft.com/library/dn486806.aspx ). 

### <a name="disable-ws-trust-windows-endpoints-on-the-proxy-ie-from-extranet"></a>Disabilitare gli endpoint di Windows WS-Trust sul proxy, ad esempio dalla Extranet

Gli endpoint di Windows WS-Trust ( */ADFS/Services/Trust/2005/windowstransport* e */ADFS/Services/Trust/13/windowstransport*) sono destinati solo ad endpoint con rete Intranet che usano il binding WIA su HTTPS. L'esposizione a Extranet potrebbe consentire le richieste a questi endpoint di ignorare le protezioni di blocco. Questi endpoint devono essere disabilitati sul proxy (disabilitato dalla Extranet) per proteggere il blocco degli account di Active Directory usando i comandi di PowerShell seguenti. Non vi è alcun effetto noto sull'utente finale disabilitando questi endpoint sul proxy.

    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/2005/windowstransport -Proxy $false
    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/13/windowstransport -Proxy $false
    
### <a name="differentiate-access-policies-for-intranet-and-extranet-access"></a>Differenziazione dei criteri di accesso per l'accesso Intranet ed Extranet
AD FS è in grado di distinguere i criteri di accesso per le richieste provenienti dalla rete aziendale locale rispetto alle richieste provenienti da Internet tramite il proxy.  Questa operazione può essere eseguita per ogni applicazione o a livello globale.  Per applicazioni con valori di business elevati o applicazioni con informazioni sensibili o personali, provare a richiedere l'autenticazione a più fattori.  Questa operazione può essere eseguita tramite lo snap-in gestione AD FS.  

### <a name="require-multi-factor-authentication-mfa"></a>Richiedi multi-factor authentication
AD FS possono essere configurate per richiedere l'autenticazione avanzata (ad esempio, l'autenticazione a più fattori) in modo specifico per le richieste in arrivo tramite il proxy, per le singole applicazioni e per l'accesso condizionale a Azure AD/Office 365 e alle risorse locali.  I metodi di autenticazione a più fattori supportati includono sia Microsoft Azure autenticazione a più fattori che provider di terze parti  All'utente viene richiesto di fornire le informazioni aggiuntive (ad esempio, un testo SMS contenente un codice monouso) e AD FS interagisce con il plug-in specifico del provider per consentire l'accesso.  

I provider di autenticazione a più fattori esterni supportati includono quelli elencati in [questa](https://technet.microsoft.com/library/dn758113.aspx) pagina, oltre a HDI globale.

### <a name="hardware-security-module-hsm"></a>Modulo di protezione hardware (HSM)
Nella configurazione predefinita, le chiavi AD FS utilizza per firmare i token non lasciano mai i server federativi nella Intranet.  Non sono mai presenti nella rete perimetrale o nei computer proxy.  Facoltativamente, per fornire protezione aggiuntiva, queste chiavi possono essere protette in un modulo di protezione hardware collegato a AD FS.  Microsoft non produce un prodotto HSM, tuttavia nel mercato sono disponibili numerosi prodotti che supportano AD FS.  Per implementare questa raccomandazione, seguire le indicazioni del fornitore per creare i certificati X509 per la firma e la crittografia, quindi usare il cmdlet di PowerShell per l'installazione AD FS, specificando i certificati personalizzati nel modo seguente:

    PS:\>Install-AdfsFarm -CertificateThumbprint <String> -DecryptionCertificateThumbprint <String> -FederationServiceName <String> -ServiceAccountCredential <PSCredential> -SigningCertificateThumbprint <String>

dove:


- `CertificateThumbprint` è il certificato SSL
- `SigningCertificateThumbprint` è il certificato di firma (con chiave protetta dal modulo di protezione hardware)
- `DecryptionCertificateThumbprint` è il certificato di crittografia (con chiave protetta dal modulo di protezione hardware)



