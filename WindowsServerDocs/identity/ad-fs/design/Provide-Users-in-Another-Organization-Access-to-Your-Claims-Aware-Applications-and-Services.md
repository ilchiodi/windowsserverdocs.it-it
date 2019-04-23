---
ms.assetid: de7e1e4a-f96d-4b59-ac9b-f65f5d37a96f
title: Fornire agli utenti di un'altra organizzazione l'accesso ai propri servizi e alle applicazioni in grado di riconoscere attestazioni
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b4ec08182e2523b0fcb16088ec9c1d094a5923fe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836532"
---
# <a name="provide-users-in-another-organization-access-to-your-claims-aware-applications-and-services"></a>Fornire agli utenti di un'altra organizzazione l'accesso ai propri servizi e alle applicazioni in grado di riconoscere attestazioni

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quando si è un amministratore nell'organizzazione partner risorse in Active Directory Federation Services \(ADFS\) e si dispone di un obiettivo di distribuzione per fornire l'accesso federato per gli utenti in un'altra organizzazione \(il organizzazione partner account\) a delle attestazioni\-consapevole o un'applicazione Web\-basato su servizio che si trova all'interno dell'organizzazione \(organizzazione partner risorse\):  
  
-   Sia all'interno dell'organizzazione che di altre organizzazioni che hanno configurato una federazione trust all'organizzazione di utenti federati \(le organizzazioni partner account\) possono accedere AD FS dell'applicazione o al servizio protetto ospitato dal organizzazione. Per altre informazioni, vedere [Federated Web SSO Design](Federated-Web-SSO-Design.md).  
  
    È possibile, ad esempio, che Fabrikam voglia concedere ai dipendenti della rete aziendale l'accesso federativo ai servizi Web ospitati in Contoso.  
  
-   Gli utenti non hanno alcuna associazione diretta a un'organizzazione considerata attendibile federati \(ad esempio, clienti singoli\), che sono connessi a un archivio di attributi ospitato nella rete perimetrale, possono accedere più AD FS\- applicazioni protette, anch sono esse ospitate nella rete perimetrale, effettuando l'accesso una sola volta da computer client che si trovano su Internet. In altre parole, quando si ospitano account cliente per abilitare l'accesso ad applicazioni o servizi nella rete perimetrale, i clienti ospitati in un archivio di attributi possono accedere a una o più applicazioni o servizi nella rete perimetrale semplicemente effettuando l'accesso una sola volta. Per altre informazioni, vedere [Web SSO Design](Web-SSO-Design.md).  
  
    Ad esempio, possibile che Fabrikam voglia ai clienti single\-sign\-sul \(SSO\) accesso a più applicazioni o servizi ospitati nella rete perimetrale.  
  
Per questo obiettivo di distribuzione, sono necessari i componenti seguenti:  
  
-   **Active Directory Domain Services \(AD DS\):** Il server federativo del partner risorse deve appartenere a un dominio Active Directory.  
  
-   **DNS perimetrale:** Domain Name System \(DNS\) deve contenere un host semplice \(oggetto\) di record di risorsa in modo che i computer client possano trovare il server federativo del partner risorse e il server Web. Il server DNS può ospitare altri record DNS anch'essi necessari nella rete perimetrale. Per altre informazioni, vedere [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   **Server federativo del partner risorse:** Il server federativo del partner risorse convalida i token di AD FS inviati dai partner account. Individuazione del partner account viene eseguita tramite il server federativo. Per altre informazioni, vedere [Review the Role of the Federation Server in the Resource Partner](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md).  
  
-   **Server Web:** il server Web può ospitare un'applicazione Web o un servizio Web. Il server Web verifica di avere ricevuto token AD FS validi dagli utenti federati prima di consentire l'accesso all'applicazione Web o al servizio Web protetto.  
  
    Con Windows Identity Foundation \(WIF\), è possibile sviluppare l'applicazione Web o servizio in modo che accetti federativa le richieste di accesso utente che vengono eseguite con qualsiasi metodo di accesso standard, ad esempio nome utente e password.  
  
Dopo aver esaminato le informazioni negli argomenti collegati, è possibile iniziare a distribuire questo obiettivo seguendo i passaggi descritti in [elenco di controllo: Implementazione di un progetto SSO Web federata](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md) e [elenco di controllo: Implementazione di un progetto Web SSO](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md).  
  
Nella figura seguente viene illustrato ognuno dei componenti necessari per questo obiettivo di distribuzione di AD FS.  
  
![per le attestazioni di accesso](media/75358b16-2a6f-4e16-9cc4-b0e614480305.gif)  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
