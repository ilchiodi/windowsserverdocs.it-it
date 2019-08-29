---
ms.assetid: eefcc989-8763-45ee-8a64-3a97b4397160
title: Operazioni di AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 594e1605f44dad69ab7eee8b22e6a620ade02ad0
ms.sourcegitcommit: c307886e96622e9595700c94128103b84f5722ce
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2019
ms.locfileid: "70108737"
---
# <a name="ad-fs-operations"></a>Operazioni di AD FS



Questo documento contiene un elenco di tutte le operazioni di documentazione per AD FS. 

## <a name="service-configuration"></a>Configurazione del servizio
- [Aggiornare i certificati SSL in AD FS e WAP 2016](../ad-fs/operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)
- [Strumento di ripristino rapido di AD FS](../ad-fs/operations/AD-FS-Rapid-Restore-Tool.md)
- [Configurare il binding del nome host alternativo per l'autenticazione del certificato in AD FS](../ad-fs/operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)
- [Aggiungere un archivio di attributi](../ad-fs/operations/Add-an-Attribute-Store.md)
- [Personalizzare le intestazioni di risposta di sicurezza HTTP con AD FS 2019](../ad-fs/operations/customize-http-security-headers-ad-fs.md)
- [Delegare l'accesso ad AD FS tramite cmdlet di PowerShell agli utenti senza privilegi di amministratore](../ad-fs/operations/delegate-ad-fs-pshell-access.md)
- [Ottimizzazione della latenza di SQL e degli indirizzi](../ad-fs/operations/adfs-sql-latency.md) 


## <a name="authentication-configuration"></a>Configurazione dell'autenticazione
### <a name="strong-authentication-mfa--password-less"></a>Autenticazione avanzata (multi-factor authentication) & senza password
- [Configurare i provider di autenticazione esterni come primari in AD FS (2019 o versione successiva)](../ad-fs/operations/Additional-Authentication-Methods-AD-FS.md)
- [Configurare AD FS (2016 o versione successiva) e l'autenticazione a più fattori di Azure](../ad-fs/operations/Configure-AD-FS-2016-and-Azure-MFA.md)
- [Configurare metodi di autenticazione aggiuntivi per AD FS](../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)

### <a name="lockout-protection"></a>Protezione da blocco
- [Configurare la protezione Extranet Soft Lockout di AD FS](../ad-fs/operations/Configure-AD-FS-Extranet-Soft-Lockout-Protection.md)
- [Configurare la protezione Extranet Smart Lockout di AD FS](../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md)
- [Configurare indirizzi IP esclusi dalla extranet di AD FS](../ad-fs/operations/Configure-AD-FS-Banned-IP.md)

### <a name="policy-configuration"></a>Configurazione dei criteri
- [Configurare i criteri di autenticazione](../ad-fs/operations/Configure-Authentication-Policies.md)
- [Configurazione di un ID di accesso alternativo](../ad-fs/operations/Configuring-Alternate-Login-ID.md)
- [Configurare Azure AD comportamento di accesso rapido per usare i criteri di AD FS](../ad-fs/operations/AD-FS-Prompt-Login.md)

### <a name="kerberos--certificate-authentication"></a>Autenticazione del certificato & Kerberos
- [Abilitare le attestazioni di servizi di dominio Active Directory & l'autenticazione composta Kerberos in AD FS](../ad-fs/operations/AD-FS-Compound-Authentication-and-AD-DS-claims.md)
- [Configurare AD FS per l'autenticazione dei certificati utente](../ad-fs/operations/Configure-User-Certificate-Authentication.md)
- [Configurare il binding del nome host alternativo per l'autenticazione del certificato in AD FS](../ad-fs/operations/AD-FS-support-for-alternate-hostname-binding-for-certificate-authentication.md)


### <a name="device"></a>Dispositivo
- [Controlli dell'autenticazione del dispositivo in AD FS](../ad-fs/operations/device-authentication-controls-in-AD-FS.md) 


## <a name="authorization-configuration"></a>Configurazione dell'autorizzazione
- [Configurare i criteri di controllo di accesso in AD FS](../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)
- [Configurare l'accesso condizionale locale basato su dispositivo](../ad-fs/operations/Configure-Device-based-Conditional-Access-on-Premises.md)

## <a name="rpt--cpt-configuration"></a>Configurazione di RPT & CPT
- [Configurare AD FS per l'autenticazione di utenti memorizzati nelle directory LDAP](../ad-fs/operations/Configure-AD-FS-to-authenticate-users-stored-in-LDAP-directories.md)
- [Configurare le regole delle attestazioni](../ad-fs/operations/Configure-Claim-Rules.md) 
- [Creare un'istanza di attendibilità del provider di attestazioni](../ad-fs/operations/Create-a-Claims-Provider-Trust.md) 
- [Creare un'istanza di attendibilità del componente non in grado di riconoscere attestazioni](../ad-fs/operations/Create-a-Non-Claims-Aware-Relying-Party-Trust.md)
- [Creare un'istanza di attendibilità del componente](../ad-fs/operations/Create-a-Relying-Party-Trust.md)
- [Configurare AD FS per l'utilizzo con il provider di Federazione aggregato (ad esempio, insolito)](../ad-fs/operations/Improved-interoperability-with-SAML-2.0.md)

## <a name="sign-in-experience-configuration"></a>Configurazione dell'esperienza di accesso
- [Configurare le impostazioni di Single Sign-on di AD FS 2016](../ad-fs/operations/AD-FS-2016-Single-Sign-On-Settings.md)
- [Configurare AD FS l'accesso impaginato](../ad-fs/operations/AD-FS-paginated-sign-in.md)
- [Configurare la personalizzazione dell'accesso utente AD FS](../ad-fs/operations/AD-FS-user-sign-in-customization.md)
- [Configurare AD FS per l'invio di attestazioni di scadenza password](../ad-fs/operations/Configure-AD-FS-to-Send-Password-Expiry-Claims.md)
- [Configurare l'autenticazione intranet basata su moduli per i dispositivi che non supportano WIA](../ad-fs/operations/Configure-intranet-forms-based-authentication-for-devices-that-do-not-support-WIA.md)

## <a name="other"></a>Altro
- [Accedere a una rete aziendale da qualsiasi dispositivo per SSO e l'autenticazione a due fattori trasparente per tutte le applicazioni aziendali](../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
- [Gestire i rischi con l'autenticazione a più fattori aggiuntiva per le applicazioni con esigenze particolari](../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
- [Gestire i rischi con il controllo di accesso condizionale](../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)
- [Configurare un ambiente lab per AD FS](../ad-fs/operations/Set-up-an-AD-FS-lab-environment.md)
- [Procedura dettagliata: Gestisci i rischi con Multi-Factor Authentication aggiuntive per le applicazioni sensibili](../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
- [Procedura dettagliata: Gestire i rischi con il controllo di accesso condizionale](../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)
- [Scenario: Aggiungere alla rete aziendale con un dispositivo Windows](../ad-fs/operations/Walkthrough--Workplace-Join-with-a-Windows-Device.md)
- [Scenario: Aggiungere alla rete aziendale con un dispositivo iOS](../ad-fs/operations/Walkthrough--Workplace-Join-with-an-iOS-Device.md)

  


