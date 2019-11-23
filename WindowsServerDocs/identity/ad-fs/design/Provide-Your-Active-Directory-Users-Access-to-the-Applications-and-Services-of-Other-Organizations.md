---
ms.assetid: 2d62386c-b466-4a54-b6fa-5d16cda120d8
title: Fornire agli utenti di Active Directory l'accesso ai servizi e alle applicazioni di altre organizzazioni
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: bcf9b9ec91c1757ad060747a6aa1589012c1ec14
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359027"
---
# <a name="provide-your-active-directory-users-access-to-the-applications-and-services-of-other-organizations"></a>Fornire agli utenti di Active Directory l'accesso ai servizi e alle applicazioni di altre organizzazioni

Questo Active Directory Federation Services \(AD FS obiettivo della distribuzione\) si basa sull'obiettivo di [fornire agli utenti Active Directory l'accesso alle applicazioni e ai servizi in grado di riconoscere attestazioni](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md).  
  
Quando si è amministratori dell'organizzazione partner account e un obiettivo della distribuzione prevede l'accesso federato per i dipendenti alle risorse ospitate in un'altra organizzazione:  
  
-   I dipendenti che hanno effettuato l'accesso a un dominio Active Directory nella rete aziendale possono utilizzare Single\-Sign\-sulla funzionalità \(SSO\) per accedere a più applicazioni o servizi basati su Web\-, protetti da AD FS, quando le applicazioni o i servizi si trovano in un'altra organizzazione. Per altre informazioni, vedere [Federated Web SSO Design](Federated-Web-SSO-Design.md).  
  
    È possibile, ad esempio, che Fabrikam voglia concedere ai dipendenti della rete aziendale l'accesso federativo ai servizi Web ospitati in Contoso.  
  
-   I dipendenti remoti connessi a un dominio di Active Directory possono ottenere AD FS token dal server federativo nell'organizzazione per ottenere l'accesso federato alle applicazioni o ai servizi basati su\-Web protetti da AD FS ospitati in un'altra organizzazione.  
  
    È possibile, ad esempio, che Fabrikam desideri che i dipendenti remoti dispongano dell'accesso federato ai servizi protetti da AD FS, ospitati in contoso, senza richiedere che i dipendenti Fabrikam siano nella rete aziendale di Fabrikam.  
  
Oltre ai componenti fondamentali descritti in [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md) e che sono ombreggiati nella figura seguente, per questo obiettivo di distribuzione sono necessari i componenti seguenti:  
  
-   **Proxy server federativo del partner account:** I dipendenti che accedono al servizio federato o all'applicazione da Internet possono usare questo componente AD FS per eseguire l'autenticazione. Per impostazione predefinita, questo componente esegue l'autenticazione basata su form, ma può anche eseguire l'autenticazione di base. È anche possibile configurare questo componente per eseguire Secure Sockets Layer \(SSL\) autenticazione client se i dipendenti dell'organizzazione hanno certificati da presentare. Per altre informazioni, vedere [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md).  
  
-   **DNS perimetrale:** Questa implementazione di Domain Name System \(\) DNS fornisce i nomi host per la rete perimetrale. Per ulteriori informazioni su come configurare il DNS perimetrale per un proxy server federativo, vedere [requisiti di risoluzione dei nomi per i proxy server federativi](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
-   **Dipendente remoto:** Il dipendente remoto accede a un'applicazione basata su Web\-\(tramite un Web browser supportato\) o un servizio basato su\-Web \(tramite un'applicazione\), usando credenziali valide della rete aziendale, mentre il dipendente è fuori sede tramite Internet. Il computer client del dipendente nella posizione remota comunica direttamente con il proxy server federativo per generare un token e autenticarsi nell'applicazione o nel servizio.  
  
Dopo aver esaminato le informazioni negli argomenti collegati, è possibile iniziare a distribuire questo obiettivo eseguendo la procedura descritta in [Checklist: Implementing a Federated Web SSO Design](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
Nella figura seguente vengono illustrati tutti i componenti necessari per questo obiettivo di distribuzione AD FS.  
  
![accesso alle app](media/50af4837-31e0-451f-a942-e705c2300065.gif)  
  
## <a name="see-also"></a>Vedi anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
