---
ms.assetid: de7e1e4a-f96d-4b59-ac9b-f65f5d37a96f
title: Fornire agli utenti in un'altra organizzazione l'accesso ai propri servizi e applicazioni in grado di riconoscere attestazioni
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b4ec08182e2523b0fcb16088ec9c1d094a5923fe
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="provide-users-in-another-organization-access-to-your-claims-aware-applications-and-services"></a>Fornire agli utenti in un'altra organizzazione l'accesso ai propri servizi e applicazioni in grado di riconoscere attestazioni

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quando un amministratore nell'organizzazione partner risorse in Active Directory Federation Services \(AD FS\) e si dispone di un obiettivo di distribuzione per fornire l'accesso federato per gli utenti in un'altra organizzazione \ (organization\ il partner account) a un'applicazione in grado di riconoscere claims\ o un servizio basata sul Web che si trova all'interno dell'organizzazione \ (organization\ il partner risorse):  
  
-   Gli utenti sia all'interno dell'organizzazione che di altre organizzazioni che hanno configurato una federazione trust per l'organizzazione federati \ (organizations\ partner account) possono accedere di AD FS applicazione o servizio che è ospitato dalla propria organizzazione protetto. Per ulteriori informazioni, vedere [Federated Web SSO Design](Federated-Web-SSO-Design.md).  
  
    Ad esempio, è possibile Fabrikam voglia i dipendenti della rete aziendale per l'accesso federativo ai servizi Web ospitati in Contoso.  
  
-   Gli utenti che non dispongono di alcuna associazione diretta a un'organizzazione considerata attendibile federati \ (ad esempio singoli customers\), che sono connessi a un archivio di attributi che è ospitato nella rete perimetrale, possono accedere a più applicazioni protette FS\ Active Directory, anch'esse ospitate nella rete perimetrale, effettuando l'accesso una sola volta da computer client che si trovano su Internet. In altre parole, quando si ospitano account cliente per abilitare l'accesso alle applicazioni o servizi nella rete perimetrale, i clienti ospitati in un archivio di attributi possono accedere uno o più applicazioni o servizi nella rete perimetrale semplicemente effettuando l'accesso una sola volta. Per ulteriori informazioni, vedere [progettazione di Web SSO](Web-SSO-Design.md).  
  
    Ad esempio, è possibile Fabrikam voglia ai clienti di avere accesso in apartment \(SSO\) accesso a più applicazioni o servizi ospitati nella rete perimetrale.  
  
I componenti seguenti sono necessari per questo obiettivo di distribuzione:  
  
-   **Active Directory Domain Services \(AD DS\):** il server federativo del partner risorse deve appartenere a un dominio Active Directory.  
  
-   **DNS perimetrale:** Domain Name System \(DNS\) deve contenere un record di risorse host semplice \(A\) in modo che i computer client possano trovare il server federativo del partner risorse e il server Web. Il server DNS può ospitare altri record DNS anch'essi necessari nella rete perimetrale. Per ulteriori informazioni, vedere [requisiti di risoluzione dei nomi per i server federativi](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   **Server federativo del partner risorse:** il server federativo del partner risorse convalida i token AD FS inviati dai partner account. Individuazione del partner account viene eseguita tramite il server federativo. Per ulteriori informazioni, vedere [rivedere il ruolo del Server federativo nel Partner risorse](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md).  
  
-   **Server Web:** il server Web può ospitare un'applicazione Web o un servizio Web. Il server Web di conferma ricevuto token AD FS validi dagli utenti federati prima di consentire l'accesso al servizio Web o applicazione Web protetto.  
  
    Utilizzando Windows Identity Foundation \(WIF\), è possibile sviluppare l'applicazione Web o il servizio in modo che accetti le richieste di accesso utente federati effettuate con qualsiasi metodo di accesso standard, ad esempio nome utente e password.  
  
Dopo aver esaminato le informazioni negli argomenti collegati, è possibile iniziare a distribuire questo obiettivo seguendo i passaggi descritti in [Checklist: Implementing a Federated Web SSO Design](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md) e [elenco di controllo: implementazione di una progettazione di Web SSO](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md).  
  
La figura seguente mostra tutti i componenti necessari per questo obiettivo di distribuzione di ADFS.  
  
![Accedere al tuo attestazioni](media/75358b16-2a6f-4e16-9cc4-b0e614480305.gif)  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
