---
ms.assetid: 2d62386c-b466-4a54-b6fa-5d16cda120d8
title: Fornire agli utenti di Active Directory l'accesso ai servizi e alle applicazioni di altre organizzazioni
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fc0cbb461a48d04ddaa677d4de2369ef58fd5390
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190964"
---
# <a name="provide-your-active-directory-users-access-to-the-applications-and-services-of-other-organizations"></a>Fornire agli utenti di Active Directory l'accesso ai servizi e alle applicazioni di altre organizzazioni

Questo Active Directory Federation Services \(ADFS\) obiettivo di distribuzione si basa sull'obiettivo contenuto in [Your Claims-Aware Applications e servizi di fornire l'accesso di Active Directory gli utenti](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md).  
  
Quando si è amministratori dell'organizzazione partner account e un obiettivo della distribuzione prevede l'accesso federato per i dipendenti alle risorse ospitate in un'altra organizzazione:  
  
-   I dipendenti che sono connessi a un dominio di Active Directory nella rete aziendale possono usare single\-sign\-sul \(SSO\) funzionalità per accedere alle Web più\-basato su applicazioni o servizi, che sono protette da AD FS, quando le applicazioni o i servizi sono in un'altra organizzazione. Per altre informazioni, vedere [Federated Web SSO Design](Federated-Web-SSO-Design.md).  
  
    È possibile, ad esempio, che Fabrikam voglia concedere ai dipendenti della rete aziendale l'accesso federativo ai servizi Web ospitati in Contoso.  
  
-   I dipendenti remoti connessi a un dominio Active Directory possono ottenere i token di AD FS dal server federativo nell'organizzazione per ottenere l'accesso federato ad AD Web protetta con ADFS\-basato su applicazioni o servizi ospitati in un altro organizzazione.  
  
    Possibile, ad esempio, che Fabrikam voglia dipendenti remoti per l'accesso federativo ai servizi protetta con ADFS di Active Directory ospitati in Contoso, senza la necessità di Fabrikam ai dipendenti di essere nella rete aziendale Fabrikam.  
  
Oltre ai componenti fondamentali descritti in [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md) e che sono ombreggiati nella figura seguente, per questo obiettivo di distribuzione sono necessari i componenti seguenti:  
  
-   **Proxy server federativo di account partner:** I dipendenti che accedono al servizio federato o dell'applicazione da Internet possono usare questo componente di ADFS per eseguire l'autenticazione. Per impostazione predefinita, questo componente esegue l'autenticazione basata su form, ma può anche eseguire l'autenticazione di base. È anche possibile configurare questo componente per l'esecuzione di Secure Sockets Layer \(SSL\) l'autenticazione del client se i dipendenti dell'organizzazione hanno certificati da presentare. Per altre informazioni, vedere [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md).  
  
-   **DNS perimetrale:** Questa implementazione di Domain Name System \(DNS\) fornisce i nomi host per la rete perimetrale. Per altre informazioni su come configurare il DNS perimetrale per un proxy server federativo, vedere [requisiti di risoluzione dei nomi per i Server federativi](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
-   **Dipendente remoto:** Il dipendente remoto accede a un sito Web\-applicazione basata su \(tramite un Web browser supportato\) o un sito Web\-servizio basato sul \(tramite un'applicazione\), usando credenziali valide la rete aziendale, quando si trova fuori sede e usa Internet. Computer client del dipendente in posizione remota comunica direttamente con il proxy server federativo per generare un token e l'autenticazione all'applicazione o al servizio.  
  
Dopo aver esaminato le informazioni negli argomenti collegati, è possibile iniziare a distribuire questo obiettivo seguendo i passaggi descritti in [elenco di controllo: Implementazione di un progetto SSO Web federativo](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
Nella figura seguente viene illustrato ognuno dei componenti necessari per questo obiettivo di distribuzione di AD FS.  
  
![accedere alle App](media/50af4837-31e0-451f-a942-e705c2300065.gif)  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
