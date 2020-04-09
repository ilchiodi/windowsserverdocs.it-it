---
title: Criteri di controllo degli accessi client in AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f9939662c22e9500235bae014b7fb9064afd911b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858114"
---
# <a name="controlling-access-to-organizational-data-with-active-directory-federation-services"></a>Controllo dell'accesso ai dati dell'organizzazione con Active Directory Federation Services

Questo documento offre una panoramica del controllo degli accessi con AD FS in scenari locali, ibridi e cloud.  

## <a name="ad-fs-and-conditional-access-to-on-premises-resources"></a>AD FS e accesso condizionale alle risorse locali 
Dall'introduzione di Active Directory Federation Services, sono disponibili criteri di autorizzazione per limitare o consentire agli utenti l'accesso alle risorse in base agli attributi della richiesta e della risorsa.  Quando AD FS è stato spostato dalla versione alla versione, la modalità di implementazione di questi criteri è cambiata.  Per informazioni dettagliate sulle funzionalità di controllo di accesso in base alla versione, vedere:
- [Criteri di controllo degli accessi in AD FS in Windows Server 2016](Access-Control-Policies-in-AD-FS.md)
- [Controllo di accesso in AD FS in Windows Server 2012 R2](Manage-Risk-with-Conditional-Access-Control.md)


## <a name="ad-fs-and-conditional-access-in-a-hybrid-organization"></a>AD FS e accesso condizionale in un'organizzazione ibrida  

AD FS fornisce il componente locale dei criteri di accesso condizionale in uno scenario ibrido. Le regole di autorizzazione basate su AD FS devono essere usate per le risorse non Azure AD, ad esempio le applicazioni locali federate direttamente al AD FS.  Il componente cloud viene fornito da [Azure ad accesso condizionale](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access).  Azure AD Connect fornisce il piano di controllo che connette i due.

Ad esempio, quando si registrano i dispositivi con Azure AD per l'accesso condizionale alle risorse cloud, la funzionalità di writeback del dispositivo Azure AD Connect rende disponibili le informazioni di registrazione del dispositivo in locale per l'utilizzo e l'applicazione dei criteri di AD FS.  In questo modo, è possibile ottenere un approccio coerente ai criteri di controllo dell'accesso sia per le risorse locali che per quelle cloud.  

![accesso condizionale](../deployment/media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  


### <a name="the-evolution-of-client-access-policies-for-office-365"></a>Evoluzione dei criteri di accesso client per Office 365
Molti di voi usano criteri di accesso client con AD FS per limitare l'accesso a Office 365 e ad altri Microsoft Online Services in base a fattori quali il percorso del client e il tipo di applicazione client usata.  
- [Criteri di accesso client in Windows Server 2012 R2 AD FS](Access-Control-Policies-W2K12.md)
- [Criteri di accesso client in AD FS 2,0](Access-Control-Policies-in-AD-FS-2.md)

Alcuni esempi di questi criteri includono:
- Blocca l'accesso client Extranet a Office 365
- Blocca l'accesso client Extranet a Office 365, ad eccezione dei dispositivi che accedono a Exchange Online per Exchange Active Sync

Spesso l'esigenza sottostante di questi criteri è mitigare il rischio di perdita dei dati garantendo che solo i client autorizzati, le applicazioni che non memorizzano nella cache i dati o i dispositivi che possono essere disabilitati in modalità remota possano ottenere l'accesso alle risorse.

Mentre i criteri documentati in precedenza per AD FS funzionano negli scenari specifici documentati, presentano delle limitazioni perché dipendono dai dati client che non sono costantemente disponibili.  L'identità dell'applicazione client, ad esempio, è disponibile solo per i servizi basati su Exchange Online e non per le risorse, ad esempio SharePoint Online, in cui è possibile accedere agli stessi dati tramite il browser o uno "thick client", ad esempio Word o Excel.  Inoltre AD FS non è a conoscenza della risorsa all'interno di Office 365 a cui si accede, ad esempio SharePoint Online o Exchange Online.

Per risolvere queste limitazioni e fornire un modo più efficace per usare i criteri per gestire l'accesso ai dati aziendali in Office 365 o altre risorse basate su Azure AD, Microsoft ha introdotto Azure AD l'accesso condizionale.  Azure AD i criteri di accesso condizionale possono essere configurati per una risorsa specifica o per tutte le risorse all'interno di Office 365, applicazioni SaaS o personalizzate in Azure AD.  Questi criteri si basano su attendibilità del dispositivo, posizione e altri fattori.

Per altre informazioni sull'accesso condizionale Azure AD, vedere [accesso condizionale in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access)

Una modifica chiave che Abilita questi scenari è l' [autenticazione moderna](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/), un nuovo modo per autenticare gli utenti e i dispositivi che funzionano allo stesso modo tra client Office, Skype, Outlook e browser.

## <a name="next-steps"></a>Passaggi successivi
Per ulteriori informazioni sul controllo dell'accesso nel cloud e in locale, vedere:

- [Accesso condizionale in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access)
- [Criteri di controllo degli accessi in AD FS 2016](Access-Control-Policies-in-AD-FS.md)
