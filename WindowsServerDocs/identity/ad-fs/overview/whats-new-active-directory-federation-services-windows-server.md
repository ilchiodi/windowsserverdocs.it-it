---
ms.assetid: aa892a85-f95a-4bf1-acbb-e3c36ef02b0d
title: What's new in Active Directory Federation Services per Windows Server 2016
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/08/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b8dd9174a869110d98ba1e65cbf2c2b6ec000b71
ms.sourcegitcommit: f26d2668f57624a3865ca4ffd36a698eea7b503e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/12/2018
---
# <a name="whats-new-in-active-directory-federation-services-for-windows-server-2016"></a>What's new in Active Directory Federation Services per Windows Server 2016

>Si applica a: Windows Server 2016

## <a name="whats-new-in-active-directory-federation-services-for-windows-server-2016"></a>What's new in Active Directory Federation Services per Windows Server 2016   
Se si sta cercando informazioni sulle versioni precedenti di ADFS, vedere gli articoli seguenti:  
 [ADFS in Windows Server 2012 o 2012 R2](https://technet.microsoft.com/library/hh831502.aspx) e [AD FS 2.0](https://technet.microsoft.com/library/adfs2.aspx)  

 Active Directory Federation Services fornisce controllo di accesso e single sign-on in un'ampia gamma di applicazioni, tra cui Office 365, cloud basato su applicazioni SaaS e le applicazioni nella rete aziendale.  
* Per l'organizzazione IT, consente di fornire l'accesso e controllo di accesso alle applicazioni moderne sia legacy, in locale e nel cloud, in base allo stesso set di credenziali e i criteri.    
* Per l'utente, fornisce un accesso trasparente utilizzando le credenziali di account familiare.  
* Per lo sviluppatore fornisce un modo semplice per autenticare gli utenti cui identità si trovano nella directory dell'organizzazione in modo che è possibile concentrare l'attenzione su applicazione, non l'autenticazione o identità.  

Questo articolo descrive what ' s new in AD FS in Windows Server 2016 (AD FS 2016).  

## <a name="eliminate-passwords-from-the-extranet"></a>Eliminare le password dalla rete Extranet  
AD FS 2016 consente tre nuove opzioni per l'accesso senza password, consentendo alle organizzazioni di evitare rischi di rete compromettono da phished, password persa o rubata. 

### <a name="sign-in-with-azure-multi-factor-authentication"></a>Accedere con Azure multi-factor Authentication
AD FS 2016 si basa sull'autenticazione a più fattori funzionalità (MFA) di ADFS in Windows Server 2012 R2, consentendo l'accesso utilizzando solo un codice di autenticazione a più fattori di Azure, senza prima inserire un nome utente e password.

* Con autenticazione a più fattori Azure come metodo di autenticazione principale, l'utente viene richiesto per il proprio nome utente e il codice OTP dall'app Azure Authenticator.  
* Con autenticazione a più fattori Azure come metodo di autenticazione aggiuntivi o secondari, l'utente fornisce l'autenticazione principale credenziali (tramite autenticazione integrata di Windows, nome utente e password, smart card o certificato utente o dispositivo) e quindi visualizza un prompt dei comandi per testo, audio o OTP basato su account di accesso di Azure MFA.  
* Con la nuova scheda Azure MFA incorporata, il programma di installazione e configurazione per Azure MFA con ADFS non è mai stata più semplice.
* Le organizzazioni possono sfruttare Azure MFA senza la necessità di un server di autenticazione a più fattori di Azure in locale.
* Autenticazione a più fattori Azure può essere configurato per la rete intranet o extranet o come parte di qualsiasi criterio di controllo di accesso.

Per ulteriori informazioni su Azure MFA con ADFS
*  [Configurare AD FS 2016 e Azure MFA](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ad-fs-and-azure-mfa)  

### <a name="password-less-access-from-compliant-devices"></a>Password senza accesso da dispositivi compatibili
AD FS 2016 si basa sulle funzionalità di registrazione dei dispositivi precedenti abilitazione dell'accesso e controllo degli accessi basato lo stato di conformità del dispositivo. Gli utenti possono accedere utilizzando le credenziali di dispositivo e conformità viene valutata nuovamente quando modificare gli attributi dei dispositivi, in modo che è possibile garantire sempre i criteri vengono applicati.  In questo modo, ad esempio criteri

* Abilitare l'accesso solo dai dispositivi che sono gestiti o conformi  
* Abilitare l'accesso Extranet solo dai dispositivi che sono gestiti o conformi  
* Richiedere l'autenticazione a più fattori per i computer che non sono gestiti o non conforme  

ADFS fornisce il componente locale dei criteri di accesso condizionale in uno scenario ibrido. Quando si registrano i dispositivi con Azure AD per l'accesso condizionale alle risorse del cloud, l'identità del dispositivo può essere utilizzata per nonché i criteri di ADFS.

![nuove funzionalità](media/whats-new-in-active-directory-federation-services-for-windows-server-2016/ADFS_ITPRO4.png)  

 Per ulteriori informazioni sull'utilizzo di dispositivi basati su accesso condizionale nel cloud   
 *  [Accesso condizionale di Azure Active Directory](https://azure.microsoft.com/en-us/documentation/articles/active-directory-conditional-access/)

Per ulteriori informazioni sull'utilizzo di dispositivi basati su accesso condizionale con AD FS
*  [Pianificazione per dispositivo basato su accesso condizionale con AD FS](../../ad-fs/deployment/Plan-Device-based-Conditional-Access-on-Premises.md)  
* [Criteri di controllo di accesso in ADFS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="sign-in-with-windows-hello-for-business"></a>Accedere con Windows Hello for Business   
I dispositivi Windows 10 introducono Windows Hello e Windows Hello for Business, sostituendo le password utente con credenziali utente associate ai dispositivi sicuro protette da movimenti dell'utente (un PIN, un movimento biometrico impronta digitale o il riconoscimento facciale). AD FS 2016 supporta questi nuovi queste nuove funzionalità di Windows 10 in modo che gli utenti possono accedere alle applicazioni di ADFS dalla intranet o extranet senza la necessità di fornire una password.

Per ulteriori informazioni sull'uso di Microsoft Windows Hello for Business nell'organizzazione
*  [Abilitare Windows Hello for Business dell'organizzazione](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport-deployment/)

## <a name="secure-access-to-applications"></a>Accesso sicuro alle applicazioni

### <a name="modern-authentication"></a>Autenticazione moderna
AD FS 2016 supporta i protocolli moderni più recenti che forniscono una migliore esperienza utente per Windows 10, nonché iOS più recenti e i dispositivi Android e App.  

Per ulteriori informazioni vedere [AD FS scenari per gli sviluppatori](../../ad-fs/overview/AD-FS-Scenarios-for-Developers.md)  


### <a name="configure-access-control-policies-without-having-to-know-claim-rules-language"></a>Configurare i criteri di controllo di accesso senza conoscere language regole attestazione  
Gli amministratori di ADFS erano necessario configurare i criteri mediante il linguaggio di regola attestazione AD FS, rendendo difficile configurare e gestire i criteri. Con i criteri di controllo di accesso, gli amministratori possono utilizzare i modelli incorporati per applicare i criteri comuni, ad esempio
* Consentire solo l'accesso intranet
* Consenti tutti gli utenti e richiedere l'autenticazione a più fattori dalla rete Extranet
* Consenti tutti gli utenti e richiedere l'autenticazione a più fattori da un gruppo specifico

I modelli sono facili da personalizzare mediante una procedura guidata basato su processo per aggiungere le eccezioni o le regole di criteri aggiuntive e possono essere applicati a uno o più applicazioni per l'applicazione coerente dei criteri.

Per ulteriori informazioni vedere [criteri di controllo di accesso in ADFS.](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)  

### <a name="enable-sign-on-with-non-ad-ldap-directories"></a>Abilitazione dell'accesso con le directory LDAP non Active Directory  
Molte organizzazioni utilizzano una combinazione di Active Directory e le directory di terze parti. Con l'aggiunta del supporto di ADFS per l'autenticazione degli utenti memorizzati nelle directory v3 conforme a LDAP, ADFS può ora essere usato per:
* Utenti di terze parti, directory compatibili LDAP v3
* Utenti di foreste di Active Directory a cui non è configurato un trust bidirezionale con Active Directory
* Utenti in Active Directory Lightweight Directory Services (AD LDS)

Per ulteriori informazioni vedere [configurare ADFS per l'autenticazione degli utenti memorizzati nelle directory LDAP.](../../ad-fs/operations/Configure-AD-FS-to-authenticate-users-stored-in-LDAP-directories.md)  

## <a name="better-sign-in-experience"></a>Migliore esperienza di accesso
### <a name="customize-sign-in-experience-for-ad-fs-applications"></a>Personalizzare l'esperienza per le applicazioni di ADFS di accesso  
Abbiamo ricevuto da parte dell'utente che consente di personalizzare l'esperienza di accesso per ogni applicazione sarebbe un miglioramento di uso, soprattutto per le organizzazioni che fornire l'accesso per le applicazioni che rappresentano più diverse aziende o i marchi.  

In precedenza, ADFS in Windows Server 2012 R2 fornito un'accesso più comune sull'esperienza per tutte le applicazioni relying party, con la possibilità di personalizzare un sottoinsieme del testo basato su contenuto per ogni applicazione. Con Windows Server 2016, è possibile personalizzare non solo messaggi, ma le immagini, web e logo tema per ogni applicazione. È inoltre possibile creare nuovi temi web personalizzati e applicarli al relying party.  

Per ulteriori informazioni vedere [personalizzazione dell'accesso utente AD FS.](../../ad-fs/operations/AD-FS-user-sign-in-customization.md)  



## <a name="manageability-and-operational-enhancements"></a>Facilità di gestione e operativi miglioramenti  
Nella sezione seguente vengono descritti gli scenari operativi migliorati introdotte con Active Directory Federation Services in Windows Server 2016.  

### <a name="streamlined-auditing-for-easier-administrative-management"></a>Il controllo di semplice gestione amministrativa semplificata  
In AD FS per Windows Server 2012 R2 sono stati generati per una singola richiesta e le informazioni relative a un registro in numerosi eventi di controllo o attività di rilascio dei token è assente (in alcune versioni di AD FS) o suddiviso in più eventi di controllo. Per impostazione predefinita, ADFS gli eventi di controllo sono disattivati per loro natura dettagliato.  
Con il rilascio di AD FS 2016, il controllo è diventato più semplice e meno dettagliato.  

Per ulteriori informazioni vedere [miglioramenti relativi ad ADFS in Windows Server 2016 al controllo.](../../ad-fs/technical-reference/auditing-enhancements-to-ad-fs-in-windows-server.md)  

### <a name="improved-interoperability-with-saml-20-for-participation-in-confederations"></a>Migliorare l'interoperabilità con SAML 2.0 per la partecipazione a confederations  
AD FS 2016 contiene supporto del protocollo SAML aggiuntivo, incluso il supporto per l'importazione di relazioni di trust in base ai metadati che contiene più entità. Ciò consente di configurare ADFS per partecipare confederations come Federation InCommon e altre implementazioni conformi al eGov 2.0 standard.  

Per ulteriori informazioni vedere [migliorato l'interoperabilità con SAML 2.0.](../../ad-fs/operations/Improved-interoperability-with-SAML-2.0.md)  

### <a name="simplified-password-management-for-federated-o365-users"></a>Gestione delle password semplificata per federato utenti di Office 365  
È possibile configurare Active Directory Federation Services (ADFS) per inviare attestazioni scadenza password per il trust della relying party (applicazioni) che sono protetti da ADFS. Utilizzo di tali attestazioni dipende dall'applicazione. Ad esempio, con Office 365 come la relying party, gli aggiornamenti sono stati implementati a Exchange e Outlook per notificare agli utenti federati delle password appena-a--scaduto.  

Per ulteriori informazioni vedere [configurare ADFS per l'invio attestazioni scadenza password.](../../ad-fs/operations/Configure-AD-FS-to-Send-Password-Expiry-Claims.md)  

### <a name="moving-from-ad-fs-in-windows-server-2012-r2-to-ad-fs-in-windows-server-2016-is-easier"></a>Lo spostamento da ADFS in Windows Server 2012 R2 ad ADFS in Windows Server 2016 è più facile  
In precedenza, la migrazione a una nuova versione di ADFS, è necessario esportazione configurazione dalla farm precedente e l'importazione in una farm completamente nuova e parallela.  

A questo punto, lo spostamento da ADFS in Windows Server 2012 R2 ad ADFS in Windows Server 2016 è diventato molto più semplice. È sufficiente aggiungere un nuovo server Windows Server 2016 a una farm di Windows Server 2012 R2, e la farm fungono a livello di comportamento farm di Windows Server 2012 R2, in modo che abbia un aspetto e si comporta come una farm di Windows Server 2012 R2.  

Quindi, aggiungere nuovi server di Windows Server 2016 alla farm, verificare le funzionalità e rimuovere i server meno recenti dal bilanciamento del carico. Quando tutti i nodi di farm sono in esecuzione Windows Server 2016, sei pronto per aggiornare il livello di comportamento farm a 2016 e iniziare a usare le nuove funzionalità.  

Per ulteriori informazioni vedere [l'aggiornamento a AD FS in Windows Server 2016.](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)  
