---
ms.assetid: c5eb3fa0-550c-4a2f-a0bc-698b690c4199
title: Pianificare l'accesso condizionale locale basato su dispositivo
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 00a7edf9529e1f116d951fd69d3bfa381d6d413a
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822754"
---
# <a name="plan-device-based-conditional-access-on-premises"></a>Pianificare l'accesso condizionale locale basato su dispositivo


Questo documento descrive i criteri di accesso condizionale basati sui dispositivi in uno scenario ibrido in cui le directory locali sono connesse a Azure AD usando Azure AD Connect.     

## <a name="ad-fs-and-hybrid-conditional-access"></a>AD FS e accesso condizionale ibrido  

ADFS fornisce il componente locale dei criteri di accesso condizionale in uno scenario ibrido.  Quando si registrano i dispositivi con Azure AD per l'accesso condizionale alle risorse cloud, la funzionalità di writeback del dispositivo Azure AD Connect rende disponibili le informazioni di registrazione del dispositivo in locale per l'utilizzo e l'applicazione dei criteri di AD FS.  In questo modo, è possibile ottenere un approccio coerente ai criteri di controllo dell'accesso sia per le risorse locali che per quelle cloud.  

![accesso condizionale](media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

### <a name="types-of-registered-devices"></a>Tipi di dispositivi registrati  
Sono disponibili tre tipi di dispositivi registrati, tutti rappresentati come oggetti dispositivo in Azure AD e possono essere usati anche per l'accesso condizionale con AD FS locale.  

| |Aggiungere un account aziendale o dell'Istituto di istruzione  |Aggiunta ad Azure AD  |Aggiunta a un dominio di Windows 10    
| --- | --- |--- | --- |
|Descrizione    |  Gli utenti aggiungono il proprio account aziendale o dell'Istituto di istruzione al dispositivo BYOD in modo interattivo.  **Nota:** Aggiungere un account aziendale o dell'Istituto di istruzione è la sostituzione per Workplace Join in Windows 8/8.1       | Gli utenti aggiungono il dispositivo Windows 10 work a Azure AD.|I dispositivi aggiunti a un dominio di Windows 10 vengono registrati automaticamente con Azure AD.|           
|Modalità di accesso degli utenti al dispositivo     |  Nessun accesso a Windows come account aziendale o dell'Istituto di istruzione.  Accedere utilizzando un account Microsoft.       |   Accedere a Windows come account aziendale o dell'Istituto di istruzione che ha registrato il dispositivo.      |     Accedere con l'account AD.|      
|Modalità di gestione dei dispositivi    |      Criteri MDM (con registrazione aggiuntiva di Intune)   | Criteri MDM (con registrazione aggiuntiva di Intune)        |   Criteri di gruppo, Configuration Manager |
|Tipo di trust Azure AD|Aggiunta all'area di lavoro|Azure AD aggiunto|Dominio associato  |     
|Percorso delle impostazioni W10    | Impostazioni > account > account > aggiungere un account aziendale o dell'Istituto di istruzione        | Impostazioni > > di sistema per > join Azure AD       |   Impostazioni > > di sistema per > aggiunta a un dominio |       
|Disponibile anche per dispositivi iOS e Android?   |    Sì     |       No  |   No   |   

  

Per ulteriori informazioni sui diversi modi per registrare i dispositivi, vedere anche:  
* [Uso di dispositivi Windows 10 nell'area di lavoro](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-windows10-devices/)  
* [Configurazione dei dispositivi Windows 10 per il lavoro](https://jairocadena.com/2016/01/18/setting-up-windows-10-devices-for-work-domain-join-azure-ad-join-and-add-work-or-school-account/)  
[Aggiungi Windows 10 mobile a Azure Active Directory](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory)  

### <a name="how-windows-10-user-and-device-sign-on-is-different-from-previous-versions"></a>Come utente di Windows 10 e dispositivo l'accesso è diverso rispetto alle versioni precedenti  
Per Windows 10 e AD FS 2016 sono disponibili alcuni nuovi aspetti relativi alla registrazione e all'autenticazione del dispositivo che è necessario conoscere, specialmente se si ha familiarità con la registrazione dei dispositivi e con l'aggiunta all'area di lavoro nelle versioni precedenti.  

In primo luogo, in Windows 10 e ADFS in Windows Server 2016, autenticazione e registrazione del dispositivo non sono più basati esclusivamente su un X509 certificato utente.  È disponibile un protocollo nuovo e più affidabile che offre una migliore protezione e un'esperienza utente più trasparente.  Le differenze principali sono che, per l'aggiunta a un dominio di Windows 10 e Azure AD join, è disponibile un certificato del computer X509 e una nuova credenziale denominata PRT.  [Qui è](https://jairocadena.com/2016/01/18/how-domain-join-is-different-in-windows-10-with-azure-ad/) possibile leggere tutte le [informazioni.](https://jairocadena.com/2016/02/01/azure-ad-join-what-happens-behind-the-scenes/)  

In secondo luogo, Windows 10 e AD FS 2016 supportano l'autenticazione utente tramite Microsoft Passport for Work, che è possibile leggere [qui](https://jairocadena.com/2016/03/09/azure-ad-and-microsoft-passport-for-work-in-windows-10/) e [qui](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/).  

AD FS 2016 fornisce l'accesso Single Sign-on a dispositivi e utenti senza problemi in base alle credenziali PRT e Passport.  Utilizzando la procedura descritta in questo documento, è possibile abilitare queste funzionalità e visualizzarne il funzionamento.  

### <a name="device-access-control-policies"></a>Criteri di controllo di accesso del dispositivo  
I dispositivi possono essere usati in semplici regole di controllo di accesso di AD FS, ad esempio:  

- Consenti accesso solo da un dispositivo registrato   
- Richiedi autenticazione a più fattori quando un dispositivo non è registrato  

Queste regole possono quindi essere combinate con altri fattori, ad esempio il percorso di accesso alla rete e l'autenticazione a più fattori, creando criteri di accesso condizionale avanzati, ad esempio:  


- richiedere l'autenticazione a più fattori per i dispositivi non registrati che accedono dall'esterno della rete aziendale, tranne che per i membri di un gruppo o gruppi specifici  

Con AD FS 2016, questi criteri possono essere configurati in modo specifico per richiedere anche un livello di attendibilità del dispositivo specifico, ovvero **autenticato**, **gestito**o **conforme**.  

Per ulteriori informazioni sulla configurazione dei criteri di controllo degli accessi AD FS, vedere [criteri di controllo di accesso in ad FS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md).  

#### <a name="authenticated-devices"></a>Dispositivi autenticati  
I dispositivi autenticati sono dispositivi registrati che non sono registrati in MDM (Intune e MDMs di terze parti per Windows 10, Intune solo per iOS e Android).   

I dispositivi autenticati avranno l'attestazione di AD FS **gestita** con valore **false**. (Mentre i dispositivi che non sono registrati a tutti non hanno questa attestazione).  Per i dispositivi autenticati (e per tutti i dispositivi registrati) sarà presente l'attestazione AD FS noknown con valore **true**.  

#### <a name="managed-devices"></a>Dispositivi gestiti:   

I dispositivi gestiti sono dispositivi registrati registrati con MDM.  

I dispositivi gestiti avranno l'attestazione AD FS gestita con valore **true**.  

#### <a name="devices-compliant-with-mdm-or-group-policies"></a>Dispositivi conformi (con MDM o criteri di gruppo)  
I dispositivi conformi sono dispositivi registrati che non sono iscritti solo a MDM ma conformi ai criteri MDM. (Le informazioni di conformità hanno origine con MDM e vengono scritte in Azure AD).  

Dispositivi conformi avranno il **isCompliant** ADFS un'attestazione con valore **TRUE**.    

Per un elenco completo delle attestazioni di accesso condizionale e del dispositivo AD FS 2016, vedere informazioni di [riferimento](#reference).  


## <a name="reference"></a>Informazioni di riferimento  
#### <a name="complete-list-of-new-ad-fs-2016-and-device-claims"></a>Elenco completo delle nuove AD FS 2016 e delle attestazioni del dispositivo  

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
