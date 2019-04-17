---
ms.assetid: 
title: Criteri di controllo di accesso client in ADFS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5e1e1b907e6fccbf2b9906106d3360bd9a6fd69d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="controlling-access-to-organizational-data-with-active-directory-federation-services"></a>Controllo dell'accesso ai dati dell'organizzazione con Active Directory Federation Services

Questo documento viene fornita una panoramica del controllo di accesso con AD FS in locale, ibrido e cloud scenari.  

## <a name="ad-fs-and-conditional-access-to-on-premises-resources"></a>AD FS e l'accesso condizionale alle risorse locali 
Fin dall'introduzione di Active Directory Federation Services, i criteri di autorizzazione sono stati disponibili per limitare o consentire agli utenti di accedere alle risorse basate su attributi della richiesta e la risorsa.  Come ADFS è spostato tra le varie versioni, la modalità di implementazione di questi criteri è cambiato.  Per informazioni dettagliate sulle funzionalità di controllo di accesso dalla versione, vedere:
- [Criteri di controllo di accesso in ADFS in Windows Server 2016](Access-Control-Policies-in-AD-FS.md)
- [Controllo di accesso in ADFS in Windows Server 2012 R2](Manage-Risk-with-Conditional-Access-Control.md)


## <a name="ad-fs-and-conditional-access-in-a-hybrid-organization"></a>AD FS e l'accesso condizionale in un'organizzazione ibrida  

ADFS fornisce il componente locale dei criteri di accesso condizionale in uno scenario ibrido. Le regole di autorizzazione AD FS in base devono essere utilizzate per le risorse non Azure AD, ad esempio in locale applicazioni federati direttamente ad ADFS.  Il componente cloud viene fornito da [accesso condizionale di Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access).  Azure AD Connect fornisce il piano di controllo che collega le due.

Ad esempio, quando si registrano i dispositivi con Azure AD per l'accesso condizionale alle risorse del cloud, il dispositivo di Azure AD Connect riscritto funzionalità rende disponibili le informazioni di registrazione di dispositivi in locale per i criteri di ADFS utilizzare e l'applicazione.  In questo modo, hai un approccio coerente per criteri di controllo di accesso per entrambi in locale e risorse cloud.  

![Accesso condizionale](../deployment/media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  


### <a name="the-evolution-of-client-access-policies-for-office-365"></a>L'evoluzione dei criteri di accesso Client per Office 365
Molti di voi utilizza criteri di accesso client con AD FS per limitare l'accesso a Office 365 e altri servizi Microsoft Online basati su fattori quali la posizione del client e il tipo di applicazione client in uso.  
- [Criteri di accesso client in Windows Server 2012 R2 AD ADFS](Access-Control-Policies-W2K12.md)
- [Criteri di accesso client in AD FS 2.0](Access-Control-Policies-in-AD-FS-2.md)

Alcuni esempi di questi criteri:
- Bloccare completamente l'accesso extranet client a Office 365
- Bloccare completamente l'accesso extranet client a Office 365, fatta eccezione per i dispositivi di accedere a Exchange Online per Exchange Active Sync

Spesso la necessità di sottostante dietro questi criteri è per ridurre i rischi di perdita di dati assicurando solo i client autorizzati, le applicazioni che non viene memorizzata, dati o i dispositivi che possono essere disabilitati in modalità remota possono ottenere l'accesso alle risorse.

Anche se i criteri documentati sopra per AD FS funzionano negli scenari specifici documentati, presentano limitazioni perché dipendono dati client che non sono disponibili in modo coerente.  Ad esempio, l'identità dell'applicazione client è stata disponibile per i servizi basati su Exchange Online e non per le risorse, ad esempio SharePoint Online, in cui gli stessi dati potrebbero essere accessibile tramite il browser o thick client, ad esempio Word o Excel.  Anche AD FS è a conoscenza di risorsa all'interno di Office 365 accedere, ad esempio SharePoint Online o Exchange Online.

Per risolvere queste limitazioni e fornire un modo più efficiente per utilizzare criteri per gestire l'accesso ai dati aziendali in Office 365 o altre risorse di Azure AD basato su, Microsoft ha introdotto l'accesso condizionale di Azure AD.  Criteri di accesso condizionale AD Azure possono essere configurati per una risorsa specifica o per qualsiasi o tutte le risorse all'interno di Office 365, SaaS o applicazioni personalizzate in Azure AD.  Questi criteri pivot trust dispositivo, posizione e altri fattori.

Per trovare ulteriori informazioni su accesso condizionale di Azure Active Directory, vedere [accesso condizionale in Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access)

Abilitazione di questi scenari è [l'autenticazione moderna](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/), una nuova modalità di autenticazione di utenti e dispositivi che funziona allo stesso modo tra i client di Office, Skype, Outlook e browser.

## <a name="next-steps"></a>Passaggi successivi
Per ulteriori informazioni sul controllo dell'accesso tra cloud e in locale, vedere:

- [Accesso condizionale in Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access)
- [Criteri di controllo di accesso in AD FS 2016](Access-Control-Policies-in-AD-FS.md)
