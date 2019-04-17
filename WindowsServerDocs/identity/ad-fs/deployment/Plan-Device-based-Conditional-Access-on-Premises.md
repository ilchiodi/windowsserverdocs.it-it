---
ms.assetid: c5eb3fa0-550c-4a2f-a0bc-698b690c4199
title: Pianificare l'accesso condizionale basato su dispositivo locale
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6303bba92ce4f4f99ae8e5095060b06885e427d6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="plan-device-based-conditional-access-on-premises"></a>Pianificare l'accesso condizionale basato su dispositivo locale

>Si applica a: Windows Server 2016

Questo documento descrive i criteri di accesso condizionale in base ai dispositivi in uno scenario ibrido, in cui le directory locale sono connessi ad Azure AD con Azure AD Connect.     

## <a name="ad-fs-and-hybrid-conditional-access"></a>Accesso condizionale AD FS e ibrida  

ADFS fornisce il componente locale dei criteri di accesso condizionale in uno scenario ibrido.  Quando si registrano i dispositivi con Azure AD per l'accesso condizionale alle risorse del cloud, il dispositivo di Azure AD Connect riscritto funzionalità rende disponibili le informazioni di registrazione di dispositivi in locale per i criteri di ADFS utilizzare e l'applicazione.  In questo modo, hai un approccio coerente per criteri di controllo di accesso per entrambi in locale e risorse cloud.  

![Accesso condizionale](media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

### <a name="types-of-registered-devices"></a>Tipi di dispositivi registrati  
Esistono tre tipi di dispositivi registrati, ognuno dei quali sono rappresentati come oggetti dispositivo in Azure AD e può essere usato per l'accesso condizionale con AD FS in locale anche.  

| |Aggiunta di lavoro o dell'istituto di istruzione Account  |Appartenenza ad Azure AD  |Windows 10 Domian Join    
| --- | --- |--- | --- |
|Descrizione    |  Gli utenti aggiungere il proprio lavoro o dell'istituto di istruzione in modo interattivo account per i dispositivi BYOD.  **Nota:** Aggiungi Account aziendale o dell'istituto di istruzione è la sostituzione di aggiunta alla rete aziendale in Windows 8/8.1       | Gli utenti di aggiungere il dispositivo di lavoro di Windows 10 ad Azure AD.|Registrare automaticamente i dispositivi appartenenti a un dominio di Windows 10 con Azure AD.|           
|Come gli utenti accedono al dispositivo     |  Nessun accesso a Windows con l'account aziendale o dell'istituto di istruzione.  Esegui l'accesso con un account Microsoft.       |   Esegui l'accesso a Windows come l'account (aziendale o dell'istituto di istruzione) che ha registrato il dispositivo.      |     Esegui l'accesso utilizzando l'account di Active Directory.|      
|Modalità di gestione dispositivi    |      Criteri MDM (con registrazione aggiuntivi Intune)   | Criteri MDM (con registrazione aggiuntivi Intune)        |   Criteri di gruppo, System Center Configuration Manager (SCCM) |
|Tipo di AD Trust Azure|All'area di lavoro|Aggiunto ad Azure AD|Dominio aggiunto  |     
|Percorso delle impostazioni W10    | Impostazioni > account > account > Aggiungi un account aziendale o dell'istituto di istruzione        | Impostazioni > sistema > su > aggiunta ad Azure AD       |   Impostazioni > sistema > su > aggiunta a un dominio |       
|È disponibile anche per dispositivi iOS e Android?   |    Sì     |       No  |   No   |   

  

Per ulteriori informazioni sui vari modi per registrare i dispositivi, vedere anche:  
* [Con i dispositivi Windows 10 nella rete aziendale](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-windows10-devices/)  
* [Configurare i dispositivi Windows 10 per lavoro](https://jairocadena.com/2016/01/18/setting-up-windows-10-devices-for-work-domain-join-azure-ad-join-and-add-work-or-school-account/)  
[Aggiungere Windows 10 Mobile ad Azure Active Directory](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory)  

### <a name="how-windows-10-user-and-device-sign-on-is-different-from-previous-versions"></a>Come utente di Windows 10 e dispositivo l'accesso è diverso rispetto alle versioni precedenti  
Per Windows 10 e AD FS 2016 sono disponibili alcuni nuovi aspetti della registrazione del dispositivo e l'autenticazione è necessario conoscere (specialmente se si ha dimestichezza con registrazione del dispositivo e il "area di lavoro" nelle versioni precedenti).  

Prima di tutto, in Windows 10 e AD FS in Windows Server 2016, autenticazione e registrazione del dispositivo non sono più basati esclusivamente su un X509 certificato utente.  Esiste un protocollo di nuovo e più affidabile che offre una migliore protezione e un'esperienza utente più fluida.  Le differenze principali sono che, per Windows 10 aggiunta a un dominio e l'aggiunta ad Azure AD, esiste un X509 certificato del computer e una nuova credenziale chiamato un PRT.  È possibile leggere la [qui](https://jairocadena.com/2016/01/18/how-domain-join-is-different-in-windows-10-with-azure-ad/) e [qui](https://jairocadena.com/2016/02/01/azure-ad-join-what-happens-behind-the-scenes/).  

In secondo luogo, Windows 10 e AD FS 2016 supporta l'autenticazione utente con Microsoft Passport for Work, che è possibile leggere la documentazione relativa [qui](https://jairocadena.com/2016/03/09/azure-ad-and-microsoft-passport-for-work-in-windows-10/) e [qui](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport-deployment/).  

AD FS 2016 fornisce dispositivo trasparente e SSO in base alle credenziali PRT sia Passport utente.  Utilizzare i passaggi in questo documento, è possibile abilitare queste funzionalità e vederli di funzionare.  

### <a name="device-access-control-policies"></a>Criteri di controllo di accesso dispositivo  
Dispositivi possono essere utilizzati nelle regole di controllo di accesso AD FS semplice, ad esempio:  

- consentire l'accesso solo da un dispositivo registrato   
- richiedere autenticazione a più fattori quando un dispositivo non è registrato.  

Queste regole possono quindi essere combinate con altri fattori come percorso di accesso di rete e l'autenticazione a più fattori, la creazione di criteri di accesso condizionale RTF, ad esempio:  


- necessaria autenticazione a più fattori per i dispositivi non registrati l'accesso dall'esterno della rete aziendale, fatta eccezione per i membri di un particolare gruppo o gruppi  

Con AD FS 2016, questi criteri possono essere configurati in modo specifico per richiedere anche un livello di attendibilità particolare dispositivo: entrambi **autenticato**, **gestiti**, o **conforme**.  

Per ulteriori informazioni sulla configurazione di ADFS di criteri di controllo di accesso, vedere [criteri di controllo di accesso in ADFS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md).  

#### <a name="authenticated-devices"></a>Dispositivi autenticati  
Dispositivi autenticati sono registrati i dispositivi che non sono registrati in MDM (Intune e 3rd party sistemi MDM per Windows 10, Intune solo per iOS e Android).   

Dispositivi autenticati avrà il **isManaged** ADFS un'attestazione con valore **FALSE**. (Mentre i dispositivi che non sono registrati affatto verranno non dispongono di questa attestazione.) Dispositivi autenticati (e i dispositivi registrati tutti) avrà il isKnown AD ADFS un'attestazione con valore **TRUE**.  

#### <a name="managed-devices"></a>Dispositivi gestiti:   

Dispositivi gestiti sono registrati i dispositivi registrati con Mdm.  

Dispositivi gestiti avranno il isManaged AD ADFS un'attestazione con valore **TRUE**.  

#### <a name="devices-compliant-with-mdm-or-group-policies"></a>Dispositivi conformi (con MDM o criteri di gruppo)  
Dispositivi conformi sono registrati i dispositivi che non solo registrati con MDM ma conformi ai criteri MDM. (Le informazioni sulla conformità ha origine con il servizio MDM e viene scritto in Azure AD).  

Dispositivi conformi avranno il **isCompliant** ADFS un'attestazione con valore **TRUE**.    

Per un elenco completo di dispositivi AD FS 2016 e attestazioni di accesso condizionale, vedere [riferimento](#reference).  


## <a name="reference"></a>Riferimento  
#### <a name="complete-list-of-new-ad-fs-2016-and-device-claims"></a>Elenco completo delle nuove attestazioni AD FS 2016 e dispositivo  

* https://schemas.microsoft.com/ws/2014/01/identity/claims/anchorclaimtype  
* http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/implicitupn  
* https://schemas.microsoft.com/2014/03/psso  
* https://schemas.microsoft.com/2015/09/prt  
* http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/UPN  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/primarygroupsid  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid  
* http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/Name  
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
