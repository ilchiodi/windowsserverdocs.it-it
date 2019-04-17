---
ms.assetid: 2d62386c-b466-4a54-b6fa-5d16cda120d8
title: Fornire agli utenti di Active Directory accesso alle applicazioni e servizi di altre organizzazioni
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d50d26c5c654e5c91b82f6f209e21f257221c12d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="provide-your-active-directory-users-access-to-the-applications-and-services-of-other-organizations"></a>Fornire agli utenti di Active Directory accesso alle applicazioni e servizi di altre organizzazioni

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo obiettivo di distribuzione di Active Directory Federation Services \(AD FS\) si basa sull'obiettivo contenuto in [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md).  
  
Quando un amministratore nell'organizzazione partner account e si dispone di un obiettivo di distribuzione per fornire l'accesso federato per i dipendenti a risorse ospitate in un'altra organizzazione:  
  
-   I dipendenti che sono connessi a un dominio di Active Directory nella rete aziendale possono usare accesso in apartment \(SSO\) funzionalità per accedere a più applicazioni basate sul Web o servizi, che sono protetti da ADFS, quando le applicazioni o servizi sono in un'altra organizzazione. Per ulteriori informazioni, vedere [Federated Web SSO Design](Federated-Web-SSO-Design.md).  
  
    Ad esempio, è possibile Fabrikam voglia dipendenti della rete aziendale per l'accesso federativo ai servizi Web ospitati in Contoso.  
  
-   I dipendenti remoti connessi a un dominio Active Directory possono ottenere i token AD FS dal server federativo nell'organizzazione per ottenere l'accesso federativo in Active Directory protetta con ADFS basata sul Web applicazioni o servizi ospitati in un'altra organizzazione.  
  
    Ad esempio, è possibile Fabrikam voglia i dipendenti remoti l'accesso federativo ai servizi protetta con ADFS di Active Directory sono ospitati in Contoso senza richiedere l'uso di essere della rete aziendale di Fabrikam.  
  
Oltre ai componenti fondamentali descritti in [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md) e che sono ombreggiati nella figura seguente, per questo obiettivo di distribuzione sono necessari i componenti seguenti:  
  
-   **Proxy server federativo partner account:** i dipendenti che accedono al servizio federato o l'applicazione da Internet possono usare questo componente di ADFS per eseguire l'autenticazione. Per impostazione predefinita, questo componente esegue l'autenticazione basata su form, ma può anche eseguire l'autenticazione di base. È inoltre possibile configurare questo componente per eseguire l'autenticazione client SSL (Secure Sockets Layer) \(SSL\) se i dipendenti dell'organizzazione hanno certificati da presentare. Per ulteriori informazioni, vedere [dove posizionare un Proxy Server federativo](Where-to-Place-a-Federation-Server-Proxy.md).  
  
-   **DNS perimetrale:** questa implementazione di Domain Name System \(DNS\) fornisce i nomi host per la rete perimetrale. Per ulteriori informazioni su come configurare il DNS perimetrale per un proxy server federativo, vedere [requisiti di risoluzione dei nomi per i proxy Server federativi](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
-   **Dipendente remoto:** il dipendente remoto accede a un'applicazione basata sul Web \ (tramite un browser\ Web supportato) o un servizio basato sul Web \ (tramite un application\) usando credenziali valide della rete aziendale, quando si trova fuori sede e usa Internet. Computer client del dipendente in posizione remota comunica direttamente con il proxy server federativo per generare un token e l'autenticazione per l'applicazione o il servizio.  
  
Dopo aver esaminato le informazioni negli argomenti collegati, è possibile iniziare a distribuire questo obiettivo seguendo i passaggi descritti in [Checklist: Implementing a Federated Web SSO Design](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
La figura seguente mostra tutti i componenti necessari per questo obiettivo di distribuzione di ADFS.  
  
![Accedere alle tue App](media/50af4837-31e0-451f-a942-e705c2300065.gif)  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
