---
ms.assetid: ''
title: Criteri di controllo di accesso client in AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cc91cd9a446c8ca30471b65374ca99a7bd49d369
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882372"
---
# <a name="controlling-access-to-organizational-data-with-active-directory-federation-services"></a>Controllo dell'accesso ai dati dell'organizzazione con Active Directory Federation Services

Questo documento viene fornita una panoramica del controllo di accesso con AD FS in locale, gli scenari ibridi e cloud.  

## <a name="ad-fs-and-conditional-access-to-on-premises-resources"></a>AD FS e l'accesso condizionale per le risorse locali 
Dall'introduzione di Active Directory Federation Services, i criteri di autorizzazione sono stati disponibili per limitare o consentire agli utenti di accedere alle risorse in base agli attributi della richiesta e la risorsa.  Come AD FS è stato spostato da una versione a altra, come vengono implementati questi criteri è cambiato.  Per informazioni dettagliate sulle funzionalità di controllo di accesso dalla versione vedere:
- [Criteri di controllo di accesso in ADFS in Windows Server 2016](Access-Control-Policies-in-AD-FS.md)
- [Controllo degli accessi in ADFS in Windows Server 2012 R2](Manage-Risk-with-Conditional-Access-Control.md)


## <a name="ad-fs-and-conditional-access-in-a-hybrid-organization"></a>AD FS e l'accesso condizionale in un'organizzazione ibrida  

ADFS fornisce il componente locale dei criteri di accesso condizionale in uno scenario ibrido. Le regole di autorizzazione di AD FS in base da utilizzare per le risorse non Azure AD, ad esempio in locale applicazioni federate direttamente a AD FS.  Il componente cloud viene fornito da [accesso condizionale di Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access).  Azure AD Connect offre il piano di controllo connetterli fra loro.

Ad esempio, quando si registra i dispositivi con Azure AD per l'accesso condizionale alle risorse cloud, il dispositivo di Azure AD Connect writeback funzionalità rende disponibili le informazioni di registrazione dispositivi in locale per i criteri di ADFS utilizzare e l'applicazione.  In questo modo, è necessario un approccio coerente per i criteri di controllo di accesso per entrambi in locale e risorse del cloud.  

![accesso condizionale](../deployment/media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  


### <a name="the-evolution-of-client-access-policies-for-office-365"></a>L'evoluzione di criteri di accesso Client per Office 365
Molti di voi usano criteri di accesso client con AD FS per limitare l'accesso a Office 365 e altri Microsoft Online services dipende da fattori quali il percorso del client e il tipo di applicazione client in uso.  
- [Criteri di accesso client in AD FS di Windows Server 2012 R2](Access-Control-Policies-W2K12.md)
- [Criteri di accesso client in AD FS 2.0](Access-Control-Policies-in-AD-FS-2.md)

Alcuni esempi di tali criteri includono:
- Bloccare completamente l'accesso a Office 365 client extranet
- Bloccare l'accesso a tutti i client extranet a Office 365, ad eccezione di dispositivi che accedono a Exchange Online per Exchange Active Sync

Spesso l'esigenza sottostante protetti da questi criteri è mitigando i rischi di perdita di dati di ottenere solo i client autorizzati, le applicazioni che non memorizza nella cache i dati, o i dispositivi che possono essere disabilitati in modalità remota è possono ottenere l'accesso alle risorse.

Anche se i criteri descritti in precedenza documentati per AD FS funzionano negli scenari specifici documentati, presentano limitazioni poiché dipendono i dati client che non sono disponibili in modo coerente.  Ad esempio, l'identità dell'applicazione client è stata disponibile per i servizi basati su Exchange Online e non per le risorse, ad esempio SharePoint Online, in cui gli stessi dati potrebbe essere possibile accedere tramite il browser o thick client, ad esempio Word o Excel.  Anche AD FS è a conoscenza della risorsa all'interno di Office 365 a cui si accede, ad esempio SharePoint Online o Exchange Online.

Per risolvere queste limitazioni e fornire un modo più efficiente usare criteri per gestire l'accesso ai dati business in Office 365 o ad altre risorse basata su Azure AD, Microsoft ha introdotto l'accesso condizionale di Azure AD.  Criteri di accesso condizionale AD Azure possono essere configurati per una risorsa specifica o per qualsiasi o tutte le risorse all'interno di Office 365, SaaS o applicazioni personalizzate in Azure AD.  Questi criteri pivot su attendibile di dispositivo, posizione e altri fattori.

Per altre informazioni sull'accesso condizionale di Azure AD, vedere [accesso condizionale in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access)

È una modifica della chiave abilitazione di questi scenari [autenticazione moderna](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/), un nuovo modo di autenticazione di utenti e dispositivi che funziona allo stesso modo tra i client Office, Skype, Outlook e i browser.

## <a name="next-steps"></a>Passaggi successivi
Per altre informazioni su controllo degli accessi nel cloud e in locale, vedere:

- [Accesso condizionale in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access)
- [Criteri di controllo di accesso in AD FS 2016](Access-Control-Policies-in-AD-FS.md)
