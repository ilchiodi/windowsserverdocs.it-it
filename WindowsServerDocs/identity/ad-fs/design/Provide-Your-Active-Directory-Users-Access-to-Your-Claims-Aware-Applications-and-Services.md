---
ms.assetid: d254fca3-85a1-424d-ac22-d6687ec3798e
title: Fornire agli utenti di Active Directory l'accesso ai servizi e alle applicazioni in grado di riconoscere attestazioni
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 48436f8e98af965f2bc2b38d296c4a15924e4db1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407959"
---
# <a name="provide-your-active-directory-users-access-to-your-claims-aware-applications-and-services"></a>Fornire agli utenti di Active Directory l'accesso ai servizi e alle applicazioni in grado di riconoscere attestazioni

Quando si è un amministratore dell'organizzazione partner account in un Active Directory Federation Services \(AD FS la distribuzione\) e si dispone di un obiettivo di distribuzione per fornire\-Single Sign\-per \(SSO\) accesso per i dipendenti nella rete aziendale alle risorse ospitate:  
  
-   I dipendenti che sono connessi a una foresta di Active Directory nella rete aziendale possono usare SSO per accedere a più applicazioni o servizi nella rete perimetrale dell'organizzazione. Tali applicazioni e servizi sono protetti da AD FS.  
  
    È possibile, ad esempio, che Fabrikam desideri che i dipendenti della rete aziendale dispongano dell'accesso federato alle applicazioni basate su Web\-ospitate nella rete perimetrale per fabrikam.  
  
-   I dipendenti remoti connessi a un dominio di Active Directory possono ottenere AD FS token dal server federativo nell'organizzazione per ottenere l'accesso federato a AD FS applicazioni o servizi basati su Web\-\-protetti che risiedono anche nell'organizzazione.  
  
-   Le informazioni nell'archivio di attributi di Active Directory possono essere popolate nei token AD FS dei dipendenti.  
  
Per questo obiettivo di distribuzione, sono necessari i componenti seguenti:  
  
-   **\)Active Directory Domain Services \(servizi di dominio Active Directory:** Servizi di dominio Active Directory contiene gli account utente dei dipendenti usati per generare token AD FS. Le informazioni come l'appartenenza ai gruppi e gli attributi vengono popolate nei token AD FS come attestazioni basate su gruppo e attestazioni personalizzate.  
  
    > [!NOTE]  
    > È anche possibile usare Lightweight Directory Access Protocol \(\) LDAP o Structured Query Language \(SQL\) per includere le identità per la generazione di token AD FS.  
  
-   **DNS aziendale:** Questa implementazione di Domain Name System \(\) DNS contiene un host semplice \(un\) record di risorse in modo che i client Intranet possano individuare il server federativo di account. Questa implementazione di DNS può ospitare anche altri record DNS necessari nella rete aziendale. Per altre informazioni, vedere [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   **Server federativo partner account:** Questo server federativo è aggiunto a un dominio nella foresta del partner account. Autentica gli account utente dei dipendenti e genera i token AD FS. Il computer client per il dipendente esegue l'autenticazione integrata di Windows su questo server federativo per generare un token di AD FS. Per altre informazioni, vedere [Review the Role of the Federation Server in the Account Partner](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md).  
  
    Il server federativo partner account può autenticare gli utenti seguenti:  
  
    -   Dipendenti con account utente nel dominio  
  
    -   Dipendenti con account utente in qualsiasi punto della foresta  
  
    -   I dipendenti con account utente in qualsiasi punto di foreste considerati attendibili da questa foresta \(tramite un trust di Windows a due\-\)  
  
-   **Dipendente:** Un dipendente accede a un servizio basato su Web\-\(tramite un\) di applicazioni o un'applicazione basata su\-Web \(tramite un Web browser\) supportato mentre è connesso alla rete aziendale. Il computer client del dipendente nella rete aziendale comunica direttamente con il server federativo per l'autenticazione.  
  
Dopo aver esaminato le informazioni negli argomenti collegati, è possibile iniziare a distribuire questo obiettivo eseguendo la procedura descritta in [Checklist: Implementing a Federated Web SSO Design](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
Nella figura seguente vengono illustrati tutti i componenti necessari per questo obiettivo di distribuzione AD FS.  
  
![accesso alle attestazioni](media/31394ea8-fecb-4372-ac3f-cc3cf566ffc9.gif)  
  
## <a name="see-also"></a>Vedi anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
