---
ms.assetid: de7e1e4a-f96d-4b59-ac9b-f65f5d37a96f
title: Fornire agli utenti di un'altra organizzazione l'accesso ai propri servizi e alle applicazioni in grado di riconoscere attestazioni
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a0b2429599036f2893f23df7921a11c8232d9f67
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359072"
---
# <a name="provide-users-in-another-organization-access-to-your-claims-aware-applications-and-services"></a>Fornire agli utenti di un'altra organizzazione l'accesso ai propri servizi e alle applicazioni in grado di riconoscere attestazioni


Quando si è un amministratore dell'organizzazione partner risorse in Active Directory Federation Services \(AD FS\) e si dispone di un obiettivo di distribuzione per fornire l'accesso federato per gli utenti di un'altra organizzazione \(organizzazione partner account\) a un'applicazione in grado di riconoscere attestazioni\-o a un servizio basato su\-Web che si trova all'interno dell'organizzazione \(organizzazione partner risorse\):  
  
-   Gli utenti federati nell'organizzazione e nelle organizzazioni che hanno configurato un trust federativo per l'organizzazione \(organizzazioni partner account\) possono accedere all'applicazione o al servizio protetto AD FS ospitato dall'organizzazione. Per altre informazioni, vedere [Federated Web SSO Design](Federated-Web-SSO-Design.md).  
  
    È possibile, ad esempio, che Fabrikam voglia concedere ai dipendenti della rete aziendale l'accesso federativo ai servizi Web ospitati in Contoso.  
  
-   Gli utenti federati che non hanno un'associazione diretta a un'organizzazione attendibile \(come singoli clienti\), che sono connessi a un archivio di attributi ospitato nella rete perimetrale, possono accedere a più AD FS\-applicazioni protette, anch ' esse ospitate nella rete perimetrale, effettuando l'accesso una sola volta da computer client che si trovano su Internet. In altre parole, quando si ospitano account cliente per abilitare l'accesso ad applicazioni o servizi nella rete perimetrale, i clienti ospitati in un archivio di attributi possono accedere a una o più applicazioni o servizi nella rete perimetrale semplicemente effettuando l'accesso una sola volta. Per altre informazioni, vedere [Web SSO Design](Web-SSO-Design.md).  
  
    È possibile, ad esempio, che Fabrikam desideri che i clienti dispongano di un singolo\-di accesso\-su \(SSO\) l'accesso a più applicazioni o servizi ospitati nella rete perimetrale.  
  
Per questo obiettivo di distribuzione, sono necessari i componenti seguenti:  
  
-   **\)Active Directory Domain Services \(servizi di dominio Active Directory:** Il server federativo del partner risorse deve appartenere a un dominio Active Directory.  
  
-   **DNS perimetrale:** Domain Name System \(\) DNS deve contenere un host semplice \(un\) record di risorse in modo che i computer client possano individuare il server federativo del partner risorse e il server Web. Il server DNS può ospitare altri record DNS anch'essi necessari nella rete perimetrale. Per altre informazioni, vedere [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   **Server federativo partner risorse:** Il server federativo del partner risorse convalida AD FS token inviati dai partner account. L'individuazione del partner account viene eseguita tramite questo server federativo. Per altre informazioni, vedere [Review the Role of the Federation Server in the Resource Partner](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md).  
  
-   **Server Web:** il server Web può ospitare un'applicazione Web o un servizio Web. Il server Web verifica di avere ricevuto token AD FS validi dagli utenti federati prima di consentire l'accesso all'applicazione Web o al servizio Web protetto.  
  
    Utilizzando Windows Identity Foundation \(WIF\), è possibile sviluppare l'applicazione o il servizio Web in modo che accetti le richieste di accesso utente federato effettuate con qualsiasi metodo di accesso standard, ad esempio nome utente e password.  
  
Dopo aver esaminato le informazioni negli argomenti collegati, è possibile iniziare a distribuire questo obiettivo seguendo i passaggi in elenco di [controllo: implementazione di un progetto Web SSO federativo](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md) ed [elenco di controllo: implementazione di un progetto Web SSO](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md).  
  
Nella figura seguente vengono illustrati tutti i componenti necessari per questo obiettivo di distribuzione AD FS.  
  
![accesso alle attestazioni](media/75358b16-2a6f-4e16-9cc4-b0e614480305.gif)  
  
## <a name="see-also"></a>Vedi anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
