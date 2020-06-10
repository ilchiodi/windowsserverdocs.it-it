---
ms.assetid: 1ea2e1be-874f-4df3-bc9a-eb215002da91
title: Configurare il supporto AD FS per l'autenticazione dei certificati utente
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 4aa2f219852dc97833365645e7455f8141a0988e
ms.sourcegitcommit: d23f880e144acf0912831557c70f777d48e3152b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/09/2020
ms.locfileid: "84632785"
---
# <a name="configuring-ad-fs-for-user-certificate-authentication"></a>Configurazione AD FS per l'autenticazione dei certificati utente

L'autenticazione del certificato utente viene utilizzata principalmente in 2 casi d'uso
* Gli utenti usano smart card per accedere al sistema AD FS
* Gli utenti usano i certificati di cui è stato effettuato il provisioning nei dispositivi mobili


## <a name="prerequisites"></a>Prerequisiti
1) Determinare la modalità di AD FS autenticazione del certificato utente che si vuole abilitare usando una delle modalità descritte in [questo articolo](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)
2) Assicurarsi che la catena di certificati utente sia installata & considerata attendibile da tutti i server AD FS e WAP, incluse eventuali autorità di certificazione intermedie. Questa operazione viene in genere eseguita tramite oggetti Criteri di gruppo nei server AD FS/WAP
3)  Verificare che il certificato radice della catena di certificati per i certificati utente si trovi nell'archivio NTAuth in Active Directory
4) Se si usa AD FS in modalità di autenticazione con certificati alternativi, assicurarsi che i server AD FS e WAP dispongano di certificati SSL che contengono il nome host di AD FS con prefisso "certauth", ad esempio "certauth.fs.contoso.com", e che il traffico verso questo nome host sia consentito tramite il firewall
5) Se si usa l'autenticazione del certificato dalla rete Extranet, assicurarsi che almeno un'AIA e almeno una posizione CDP o OCSP dall'elenco specificato nei certificati siano accessibili da Internet.
6) Per l'autenticazione Azure AD certificato, inoltre, per i client Exchange ActiveSync, il certificato client deve avere l'indirizzo di posta elettronica instradabile degli utenti in Exchange Online nel nome dell'entità o nel valore del nome RFC822 del campo nome alternativo oggetto. Azure Active Directory esegue il mapping del valore RFC822 all'attributo dell'indirizzo proxy nella directory.
7) Quando si usa l'autenticazione basata su smart card o certificato, l'oggetto del certificato potrebbe non corrispondere a quello di UserPricipalName nell'account AD. In questo caso l'accesso ha esito negativo con "utente non trovato".


## <a name="configure-ad-fs-for-user-certificate-authentication"></a>Configurare AD FS per l'autenticazione del certificato utente  

Abilitare l'autenticazione del certificato utente come metodo di autenticazione Intranet o Extranet in AD FS, usando la console di gestione AD FS o il cmdlet di PowerShell `Set-AdfsGlobalAuthenticationPolicy` .

Se si sta configurando AD FS per l'autenticazione Azure AD certificato, verificare di aver configurato le [impostazioni del Azure ad](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-get-started#step-2-configure-the-certificate-authorities) e le [regole attestazione ad FS richieste](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-ios#requirements) per l'autorità emittente del certificato e il numero di serie

Sono inoltre disponibili alcuni aspetti facoltativi.
- Se si desidera utilizzare attestazioni basate su campi ed estensioni del certificato, oltre ad EKU (tipo di attestazione https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku) , configurare le regole di pass-through di attestazione aggiuntive nell'Active Directory attendibilità del provider di attestazioni.  Per un elenco completo delle attestazioni di certificato disponibili, vedere di seguito.  
- Se è necessario limitare l'accesso in base al tipo di certificato, è possibile usare le proprietà aggiuntive del certificato in AD FS le regole di autorizzazione di rilascio per l'applicazione. Gli scenari comuni sono "Consenti solo certificati con provisioning da un provider MDM" o "Consenti solo certificati smart card"
>[!IMPORTANT]
> I clienti che usano il flusso del codice del dispositivo per l'autenticazione e l'esecuzione dell'autenticazione del dispositivo con un IDP diverso da Azure AD (ad esempio AD FS) non saranno in grado di applicare l'accesso in base al dispositivo (ad esempio, consentire solo i dispositivi gestiti che usano un servizio MDM di terze parti) per Azure AD Per proteggere l'accesso alle risorse aziendali in Azure AD e impedire eventuali perdite di dati, i clienti devono configurare Azure AD l'accesso condizionale basato su dispositivo, ad esempio "Richiedi segnalazione del dispositivo da contrassegnare" in Azure AD l'accesso condizionale.
- Configurare le autorità di certificazione di emissione consentite per i certificati client seguendo le istruzioni riportate in "gestione di autorità emittenti attendibili per l'autenticazione client" in [questo articolo](https://technet.microsoft.com/library/dn786429(v=ws.11).aspx).
- È consigliabile modificare le pagine di accesso in base alle esigenze degli utenti finali quando si esegue l'autenticazione del certificato. Casi comuni: (a) modificare "accesso con il certificato X509" a un altro utente finale descrittivo

## <a name="configure-seamless-certificate-authentication-for-chrome-browser-on-windows-desktops"></a>Configurare l'autenticazione del certificato trasparente per il browser Chrome nei desktop Windows
Quando nel computer sono presenti più certificati utente (ad esempio certificati Wi-Fi) che soddisfano le finalità dell'autenticazione client, il browser Chrome in Windows Desktop richiederà all'utente di selezionare il certificato corretto. Questa operazione può generare confusione per l'utente finale. Per ottimizzare questa esperienza, è possibile impostare un criterio per Chrome per selezionare automaticamente il certificato appropriato per migliorare l'esperienza utente. Questo criterio può essere impostato manualmente rendendo una modifica del registro di sistema o configurata automaticamente tramite GPO (per impostare le chiavi del registro di sistema). A questo scopo, è necessario che i certificati client dell'utente per l'autenticazione su AD FS dispongano di emittenti distinte da altri casi d'uso. 

Per ulteriori informazioni sulla configurazione di questo per Chrome, consultare questo [collegamento](http://www.chromium.org/administrators/policy-list-3#AutoSelectCertificateForUrls).  


## <a name="troubleshoot-certificate-authentication"></a>Risolvere i problemi di autenticazione del certificato
Questo documento è incentrato sulla risoluzione dei problemi comuni quando AD FS viene configurato per l'autenticazione dei certificati per gli utenti. 

### <a name="check-if-certificate-trusted-issuers-is-configured-properly-in-all-the-ad-fswap-servers"></a>Verificare che le autorità emittenti attendibili del certificato siano configurate correttamente in tutti i server AD FS/WAP
*Sintomo comune: HTTP 204 "nessun contenuto da https \: //certauth.ADFS.contoso.com"*

AD FS usa il sistema operativo Windows sottostante per dimostrare il possesso del certificato utente e verificare che corrisponda a un'autorità emittente attendibile eseguendo la convalida della catena di certificati attendibili. Per trovare la corrispondenza con l'emittente attendibile, è necessario assicurarsi che tutte le autorità radice e intermedie siano configurate come autorità emittenti attendibili nell'archivio Autorità di certificazione del computer locale. Per convalidare questa operazione automaticamente, usare lo [strumento Analizzatore diagnostica ad FS](https://adfshelp.microsoft.com/DiagnosticsAnalyzer/Analyze). Lo strumento esegue una query su tutti i server e garantisce che venga eseguito correttamente il provisioning dei certificati corretti. 
1)  Scaricare ed eseguire lo strumento in base alle istruzioni fornite nel collegamento precedente
2)  Caricare i risultati ed esaminare eventuali errori

### <a name="check-if-certificate-authentication-is-enabled-in-the-ad-fs-authentication-policy"></a>Controllare se l'autenticazione del certificato è abilitata nei criteri di autenticazione AD FS
Per impostazione predefinita, AD FS non Abilita l'autenticazione del certificato. Fare riferimento all'inizio di questo documento su come abilitare l'autenticazione del certificato. 

### <a name="check-if-certificate-authentication-is-enabled-in-the-ad-fs-authentication-policy"></a>Controllare se l'autenticazione del certificato è abilitata nei criteri di autenticazione AD FS
Per impostazione predefinita, AD FS l'autenticazione del certificato utente sulla porta 49443 con lo stesso nome host AD FS (ad esempio `adfs.contoso.com` ). È inoltre possibile configurare AD FS per l'utilizzo della porta 443 (porta HTTPS predefinita) utilizzando l'associazione SSL alternativa. Tuttavia, l'URL usato in questa configurazione è `certauth.<adfs-farm-name>` (ad esempio `certauth.contoso.com` ). Per ulteriori informazioni, vedere [questo collegamento](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md) . Il caso più comune di connettività di rete è che un firewall è stato configurato in modo non corretto e blocca o interferisce il traffico di autenticazione dei certificati utente. In genere, quando si verifica questo problema, viene visualizzata una schermata vuota o un errore 500 del server. 
1)  Prendere nota del nome host e della porta configurati in AD FS
2)  Assicurarsi che qualsiasi firewall davanti a AD FS o proxy applicazione Web (WAP) sia configurato per consentire la `hostname:port` combinazione della ad FS farm. Per eseguire questo passaggio, è necessario fare riferimento al progettista di rete. 

### <a name="check-certificate-revocation-list-connectivity"></a>Controlla connettività elenco di revoche di certificati
Gli elenchi di revoche di certificati (CRL) sono endpoint codificati nel certificato utente per eseguire i controlli di revoca del runtime. Ad esempio, se viene rubato un dispositivo che contiene un certificato, un amministratore può aggiungere il certificato all'elenco dei certificati revocati. Tutti gli endpoint che hanno accettato questo certificato in precedenza avranno ora esito negativo per l'autenticazione.

Ogni AD FS e il server WAP dovranno raggiungere l'endpoint CRL per convalidare se il certificato presentato è ancora valido e non è stato revocato. La convalida CRL può verificarsi tramite HTTPS, HTTP, LDAP o tramite OCSP (protocollo di stato del certificato online). Se i server AD FS/WAP non riescono a raggiungere l'endpoint, l'autenticazione avrà esito negativo. Per risolvere il problemi, attenersi alla procedura riportata di seguito. 
1) Consultare il tecnico PKI per determinare gli endpoint CRL usati per revocare i certificati utente dal sistema PKI. 
2)  In ogni server AD FS/WAP verificare che gli endpoint CRL siano raggiungibili tramite il protocollo usato (in genere HTTPS o HTTP)
3)  Per la convalida avanzata, [abilitare la registrazione degli eventi di CAPI2](https://blogs.msdn.microsoft.com/benjaminperkins/2013/09/30/enable-capi2-event-logging-to-troubleshoot-pki-and-ssl-certificate-issues/) in ogni server ad FS/WAP
4) Verificare l'ID evento 41 (verificare la revoca) nei log operativi di CAPI2
5) Verifica`‘\<Result value="80092013"\>The revocation function was unable to check revocation because the revocation server was offline.\</Result\>'`

***Suggerimento***: è possibile fare riferimento a un singolo server ad FS o WAP per semplificare la risoluzione dei problemi configurando la risoluzione DNS (file hosts in Windows) in modo che punti a un server specifico. In questo modo è possibile abilitare la traccia per un server. 

### <a name="check-if-this-is-a-server-name-indication-sni-issue"></a>Controllare se si tratta di un problema Indicazione nome server (SNI)
AD FS richiede che il dispositivo client (o i browser) e i bilanciamenti del carico supportino SNI. Alcuni dispositivi client (in genere versioni precedenti di Android) potrebbero non supportare SNI. Inoltre, i bilanciamenti del carico potrebbero non supportare SNI o non sono stati configurati per SNI. In questi casi è probabile che vengano visualizzati errori di certificazione utente. 
1)  Collaborare con il tecnico di rete per assicurarsi che la Load Balancer per AD FS/WAP supporti SNI
2)  Nel caso in cui non sia possibile supportare SNI AD FS è possibile risolvere i problemi attenendosi alla procedura seguente
    *   Aprire una finestra del prompt dei comandi con privilegi elevati nel server di AD FS primario
    *   Tipo in ```Netsh http show sslcert```
    *   Copiare "GUID dell'applicazione" e "hash del certificato" del servizio federativo
    *   Tipo in `netsh http add sslcert ipport=0.0.0.0:{your_certauth_port} certhash={your_certhash} appid={your_applicaitonGUID}`

### <a name="check-if-the-client-device-has-been-provisioned-with-the-certificate-correctly"></a>Controllare se il provisioning del dispositivo client è stato eseguito correttamente con il certificato
È possibile notare che alcuni dispositivi funzionano correttamente, ma non altri dispositivi. In questo caso, si tratta in genere di un risultato del mancato corretto provisioning del certificato utente nel dispositivo client. Attenersi alla procedura descritta di seguito. 
1)  Se il problema è specifico di un dispositivo Android, il problema più comune è che la catena di certificati non è completamente attendibile nel dispositivo Android.  Fare riferimento al fornitore MDM per verificare che il provisioning del certificato sia stato eseguito correttamente e che l'intera catena sia completamente attendibile nel dispositivo Android. 
2)  Se il problema è specifico di un dispositivo Windows, controllare se il provisioning del certificato è stato eseguito correttamente controllando l'archivio certificati di Windows per l'utente connesso (non sistema/computer).
3)  Esportare il certificato utente client in un file con estensione CER ed eseguire il comando ' certutil-f-UrlFetch-Verify certificatefilename. cer '


### <a name="check-if-the-tls-version-is-compatible-between-ad-fswap-servers-and-the-client-device"></a>Controllare se la versione di TLS è compatibile tra i server AD FS/WAP e il dispositivo client
In rari casi, un dispositivo client (in genere dispositivi mobili) viene aggiornato in modo da supportare solo una versione più elevata di TLS (ad 1,3) oppure è possibile che si verifichi il problema inverso in cui i server AD FS/WAP sono stati aggiornati per usare solo una versione di TLS più elevata e il dispositivo client non lo supporta. È possibile usare gli strumenti SSL online per controllare i server AD FS/WAP e verificare se è compatibile con il dispositivo. Per ulteriori informazioni su come controllare le versioni di TLS, vedere [questo collegamento](manage-ssl-protocols-in-ad-fs.md).

### <a name="check-if-azure-ad-promptloginbehavior-is-configured-correctly-on-your-federated-domain-settings"></a>Verificare che Azure AD PromptLoginBehavior sia configurato correttamente nelle impostazioni del dominio federato
Molte applicazioni Office 365 inviano prompt = login per Azure AD. Azure AD, per impostazione predefinita, lo converte in un nuovo account di accesso con password AD FS. Di conseguenza, anche se è stata configurata l'autenticazione del certificato in AD FS, gli utenti finali visualizzeranno solo un account di accesso con password. 
1)  Ottenere le impostazioni del dominio federato usando il comando ' Get-MsolDomainFederationSettings ' Let
2)  Verificare che il parametro PromptLoginBehavior sia impostato su uno dei parametri ' disabled ' o ' NativeSupport '

Per ulteriori informazioni, vedere [questo collegamento](ad-fs-prompt-login.md). 

### <a name="additional-troubleshooting"></a>Risoluzione dei problemi aggiuntiva
Si tratta di occorrenze rare
1)  Se gli elenchi CRL sono molto lunghi, potrebbe verificarsi un timeout durante il tentativo di download. In tal caso, è necessario aggiornare "MaxFieldLength" e "MaxRequestByte" in base ahttps://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows




## <a name="reference-complete-list-of-user-certificate-claim-types-and-example-values"></a>Riferimento: elenco completo dei tipi di attestazione del certificato utente e valori di esempio

|                                         Tipo di attestazione                                         |                              Valore di esempio                               |
|--------------------------------------------------------------------------------------------|--------------------------------------------------------------------------|
|         https://schemas.microsoft.com/2012/12/certificatecontext/field/x509version         |                                    3                                     |
|     https://schemas.microsoft.com/2012/12/certificatecontext/field/signaturealgorithm      |                                sha256RSA                                 |
|           https://schemas.microsoft.com/2012/12/certificatecontext/field/issuer            |                 CN = CAOrg, DC = Domain, DC = contoso, DC = com                  |
|         https://schemas.microsoft.com/2012/12/certificatecontext/field/issuername          |                 CN = CAOrg, DC = Domain, DC = contoso, DC = com                  |
|          https://schemas.microsoft.com/2012/12/certificatecontext/field/notbefore          |                           12/05/2016 20:50:18                            |
|          https://schemas.microsoft.com/2012/12/certificatecontext/field/notafter           |                           12/05/2017 20:50:18                            |
|           https://schemas.microsoft.com/2012/12/certificatecontext/field/subject           |   E = user@contoso.com , CN = User, CN = Users, DC = Domain, DC = contoso, DC = com   |
|         https://schemas.microsoft.com/2012/12/certificatecontext/field/subjectname         |   E = user@contoso.com , CN = User, CN = Users, DC = Domain, DC = contoso, DC = com   |
|           https://schemas.microsoft.com/2012/12/certificatecontext/field/rawdata           |                {Dati del certificato digitale con codifica Base64}                 |
|        https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage         |                             DigitalSignature                             |
|        https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage         |                             KeyEncipherment                              |
|  https://schemas.microsoft.com/2012/12/certificatecontext/extension/subjectkeyidentifier   |                 9D11941EC06FACCCCB1B116B56AA97F3987D620A                 |
| https://schemas.microsoft.com/2012/12/certificatecontext/extension/authoritykeyidentifier  |    KeyID = D6 13 E3 6B BC E5 D8 15 52 0A FD 36 6a D5 0B 51 F3 0B 25 7f     |
| https://schemas.microsoft.com/2012/12/certificatecontext/extension/certificatetemplatename |                                   Utente                                   |
|           https://schemas.microsoft.com/2012/12/certificatecontext/extension/san           | Altro nome: nome entità = user@contoso.com , nome RFC822 =user@contoso.com |
|           https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku           |                          1.3.6.1.4.1.311.10.3.4                          |

## <a name="related-links"></a>Collegamenti correlati
* [Configurare un'associazione del nome host alternativo per l'autenticazione del certificato AD FS](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)
* [Configurare le autorità di certificazione in Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-get-started#step-2-configure-the-certificate-authorities)
