---
ms.assetid: c5eb3fa0-550c-4a2f-a0bc-698b690c4199
title: Pianificare l'accesso condizionale locale basato su dispositivo
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3a78334f64d9e51515757b01f2d788bf87f67a35
ms.sourcegitcommit: cd12ace92e7251daaa4e9fabf1d8418632879d38
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2019
ms.locfileid: "66501611"
---
# <a name="plan-device-based-conditional-access-on-premises"></a>Pianificare l'accesso condizionale locale basato su dispositivo


Questo documento descrive i criteri di accesso condizionale basati sui dispositivi in uno scenario ibrido in cui le directory locali connessi ad Azure AD con Azure AD Connect.     

## <a name="ad-fs-and-hybrid-conditional-access"></a>Accesso condizionale AD FS e ibrida  

ADFS fornisce il componente locale dei criteri di accesso condizionale in uno scenario ibrido.  Quando si registra i dispositivi con Azure AD per l'accesso condizionale alle risorse cloud, il dispositivo di Azure AD Connect writeback funzionalità rende disponibili le informazioni di registrazione dispositivi in locale per i criteri di ADFS utilizzare e l'applicazione.  In questo modo, è necessario un approccio coerente per i criteri di controllo di accesso per entrambi in locale e risorse del cloud.  

![accesso condizionale](media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

### <a name="types-of-registered-devices"></a>Tipi di dispositivi registrati  
Esistono tre tipi di dispositivi registrati, ognuno dei quali sono rappresentati come oggetti dispositivo in Azure AD e può essere usato per l'accesso condizionale con AD FS in locale anche.  

| |Aggiunta di lavoro o dell'istituto di istruzione  |Aggiunta ad Azure AD  |Windows 10 Domain Join    
| --- | --- |--- | --- |
|Descrizione    |  Gli utenti aggiungere il proprio lavoro o dell'istituto di istruzione al dispositivo BYOD in modo interattivo.  **Nota:** Aggiungere l'Account aziendale o dell'istituto di istruzione è la sostituzione di Workplace Join in Windows 8/8.1       | Gli utenti di aggiungere il dispositivo di lavoro di Windows 10 ad Azure AD.|Registrare automaticamente i dispositivi aggiunti a un dominio di Windows 10 con Azure AD.|           
|Modo in cui gli utenti accedono al dispositivo     |  Nessun account di accesso a Windows come account aziendale o dell'istituto di istruzione.  Account di accesso usando un account Microsoft.       |   Account di accesso di Windows dell'account (aziendale o dell'istituto di istruzione) che la registrazione del dispositivo.      |     Account di accesso tramite account di Active Directory.|      
|Come vengono gestiti i dispositivi    |      Criteri MDM (con la registrazione di Intune aggiuntiva)   | Criteri MDM (con la registrazione di Intune aggiuntiva)        |   Group Policy, System Center Configuration Manager (SCCM) |
|Tipo di AD Trust Azure|All'area di lavoro|Aggiunto ad Azure AD|Dominio associato  |     
|Percorso delle impostazioni W10    | Impostazioni > account > account > aggiungere un account aziendale o dell'istituto di istruzione        | Impostazioni > sistema > su > aggiunti ad Azure AD       |   Impostazioni > sistema > su > aggiunta a un dominio |       
|Disponibile anche per dispositivi iOS e Android?   |    Yes     |       No  |   No   |   

  

Per altre informazioni sui vari modi per registrare i dispositivi, vedere anche:  
* [Uso di dispositivi Windows 10 in azienda](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-windows10-devices/)  
* [Configurazione per i dispositivi Windows 10 per il lavoro](https://jairocadena.com/2016/01/18/setting-up-windows-10-devices-for-work-domain-join-azure-ad-join-and-add-work-or-school-account/)  
[Aggiungere Windows 10 Mobile ad Azure Active Directory](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory)  

### <a name="how-windows-10-user-and-device-sign-on-is-different-from-previous-versions"></a>Come utente di Windows 10 e dispositivo l'accesso è diverso rispetto alle versioni precedenti  
Per Windows 10 e AD FS 2016 sono disponibili alcuni nuovi aspetti della registrazione del dispositivo e l'autenticazione è opportuno conoscere (specialmente se si ha dimestichezza con registrazione dispositivo e il "area di lavoro" nelle versioni precedenti).  

In primo luogo, in Windows 10 e ADFS in Windows Server 2016, autenticazione e registrazione del dispositivo non sono più basati esclusivamente su un X509 certificato utente.  È un protocollo di nuovo e più affidabile che offre una migliore protezione e un'esperienza utente più semplice.  Le principali differenze sono che, per Windows 10 aggiunta a dominio e aggiunta ad Azure AD, è un X509 certificato computer e una nuova credenziale denominata un PRT.  È possibile leggere gli approfondimenti [Ecco](https://jairocadena.com/2016/01/18/how-domain-join-is-different-in-windows-10-with-azure-ad/) e [qui](https://jairocadena.com/2016/02/01/azure-ad-join-what-happens-behind-the-scenes/).  

In secondo luogo, Windows 10 e AD FS 2016 supporta l'autenticazione utente tramite Microsoft Passport for Work, è possibile conoscenza [Ecco](https://jairocadena.com/2016/03/09/azure-ad-and-microsoft-passport-for-work-in-windows-10/) e [qui](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/).  

AD FS 2016 offre dispositivo trasparente e SSO basata sulle credenziali PRT sia Passport dell'utente.  Usando i passaggi descritti in questo documento, è possibile abilitare queste funzionalità e visualizzarli a lavorare.  

### <a name="device-access-control-policies"></a>Criteri di controllo di accesso dispositivo  
È possibile usare dispositivi in semplici regole di controllo di accesso AD FS, ad esempio:  

- consentire l'accesso solo da un dispositivo registrato   
- Richiedi multi-factor authentication quando non è registrato un dispositivo  

Queste regole possono quindi essere combinate con altri fattori, ad esempio percorso di accesso di rete e multi-factor authentication, la creazione di criteri di accesso condizionale avanzato, ad esempio:  


- Richiedi multi-factor authentication per l'accesso dall'esterno della rete aziendale, tranne quelli dei membri di un determinato gruppo o i gruppi di dispositivi non registrati  

Con AD FS 2016, questi criteri possono essere configurati in modo specifico per richiedere un livello di attendibilità specifico dispositivo nonché: entrambi **autenticato**, **gestito**, o **conformi**.  

Per altre informazioni sulla configurazione di AD FS i criteri di controllo di accesso, vedere [criteri di controllo di accesso in AD FS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md).  

#### <a name="authenticated-devices"></a>Dispositivi autenticati  
Dispositivi autenticati vengono registrati i dispositivi non registrati in MDM (Intune e 3rd party MDMs per Windows 10, Intune solo per iOS e Android).   

Dispositivi autenticati avranno il **isManaged** attestazioni per AD FS con il valore **FALSE**. (Mentre i dispositivi non registrati affatto non disporrà di questa attestazione.)  Dispositivi autenticati (e tutti i dispositivi registrati) avranno la isKnown AD ADFS un'attestazione con valore **TRUE**.  

#### <a name="managed-devices"></a>Dispositivi gestiti:   

Dispositivi gestiti si trovano i dispositivi registrati che sono registrati con Mdm.  

I dispositivi gestiti avranno isManaged AD ADFS un'attestazione con valore **TRUE**.  

#### <a name="devices-compliant-with-mdm-or-group-policies"></a>Dispositivi conformi (con MDM o criteri di gruppo)  
Dispositivi conformi vengono registrati i dispositivi che non solo registrati con MDM ma conformi ai criteri MDM. (Le informazioni sulla conformità originata da MDM e vengono scritti in Azure AD).  

Dispositivi conformi avranno il **isCompliant** ADFS un'attestazione con valore **TRUE**.    

Per un elenco completo di dispositivi AD FS 2016 e attestazioni di accesso condizionale, vedere [riferimento](#reference).  


## <a name="reference"></a>Riferimenti  
#### <a name="complete-list-of-new-ad-fs-2016-and-device-claims"></a>Elenco completo delle nuove attestazioni AD FS 2016 e dispositivo  

* https://schemas.microsoft.com/ws/2014/01/identity/claims/anchorclaimtype  
* http://schemas.xmlsoap.org/ws/2005/05/identity/claims/implicitupn  
* https://schemas.microsoft.com/2014/03/psso  
* https://schemas.microsoft.com/2015/09/prt  
* http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/primarygroupsid  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid  
* http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/registrationid  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/displayname  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/identifier  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/ostype  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/osversion  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/ismanaged  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser  
* https://schemas.microsoft.com/2014/02/devicecontext/claims/isknown  
* https://schemas.microsoft.com/2014/02/deviceusagetime  
* https://schemas.microsoft.com/2014/09/devicecontext/claims/iscompliant  
* https://schemas.microsoft.com/2014/09/devicecontext/claims/trusttype  
* https://schemas.microsoft.com/claims/authnmethodsreferences  
* https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent  
* https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path  
* https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork  
* https://schemas.microsoft.com/2012/01/requestcontext/claims/client-request-id  
* https://schemas.microsoft.com/2012/01/requestcontext/claims/relyingpartytrustid  
* https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-ip  
* https://schemas.microsoft.com/2014/09/requestcontext/claims/userip  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod  
