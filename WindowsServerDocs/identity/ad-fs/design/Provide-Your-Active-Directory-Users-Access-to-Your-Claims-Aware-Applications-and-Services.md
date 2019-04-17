---
ms.assetid: d254fca3-85a1-424d-ac22-d6687ec3798e
title: Fornire agli utenti di Active Directory accesso ai propri servizi e applicazioni in grado di riconoscere attestazioni
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f6fb37c16c20915c0051e3a24cdb0c147ae92d9c
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="provide-your-active-directory-users-access-to-your-claims-aware-applications-and-services"></a>Fornire agli utenti di Active Directory accesso ai propri servizi e applicazioni in grado di riconoscere attestazioni

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quando un amministratore nell'organizzazione partner account in una distribuzione di Active Directory Federation Services \(AD FS\) e si dispone di un obiettivo di distribuzione per fornire l'accesso in apartment \(SSO\) accesso per i dipendenti nella rete aziendale alle risorse ospitate:  
  
-   I dipendenti che sono connessi a una foresta di Active Directory nella rete aziendale possono usare SSO per accedere a più applicazioni o servizi nella rete perimetrale dell'organizzazione. Queste applicazioni e servizi protetti da ADFS.  
  
    Ad esempio, è possibile Fabrikam voglia dipendenti della rete aziendale per l'accesso federativo ai applicazioni basata sul Web ospitate nella rete perimetrale per Fabrikam.  
  
-   I dipendenti remoti connessi a un dominio Active Directory possono ottenere i token AD FS dal server federativo nell'organizzazione per ottenere l'accesso federativo alle applicazioni basate sul Web protette FS\ Active Directory o servizi che risiedono anche nell'organizzazione.  
  
-   Informazioni nell'archivio di attributi di Active Directory possono essere popolate nei token AD FS dei dipendenti.  
  
I componenti seguenti sono necessari per questo obiettivo di distribuzione:  
  
-   **Active Directory Domain Services \(AD DS\):** Active Directory contiene gli account utente dei dipendenti che vengono utilizzati per generare i token ADFS. Informazioni, ad esempio l'appartenenza al gruppo e gli attributi, vengono popolate nei token AD FS come attestazioni di gruppo e attestazioni personalizzate.  
  
    > [!NOTE]  
    > È inoltre possibile utilizzare \(LDAP\) Lightweight Directory Access Protocol o Structured Query Language \(SQL\) per includere le identità per la generazione dei token AD FS.  
  
-   **DNS aziendale:** questa implementazione di Domain Name System \(DNS\) contiene un record di risorse host semplice \(A\) in modo che i client intranet possano individuare il server federativo di account. Inoltre, l'implementazione di DNS può ospitare altri record DNS necessari nella rete aziendale. Per ulteriori informazioni, vedere [requisiti di risoluzione dei nomi per i server federativi](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   **Server federativo del partner account:** il server federativo viene aggiunto a un dominio nella foresta del partner account. Autentica gli account utente dei dipendenti e genera i token AD FS. Il computer client per il dipendente esegue l'autenticazione integrata di Windows sul server federativo per generare un token di ADFS. Per ulteriori informazioni, vedere [rivedere il ruolo del Server federativo nel Partner Account](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md).  
  
    Il server federativo del partner account può autenticare gli utenti seguenti:  
  
    -   Dipendenti con account utente nel dominio  
  
    -   Dipendenti con account utente in qualsiasi punto della foresta  
  
    -   Dipendenti con account utente remoto via Internet di foreste trusted dalla foresta corrente \ (tramite un trust\ Windows vie forma)  
  
-   **Dipendente:** un dipendente accede a un servizio basato sul Web \(through an application\) o un'applicazione basata sul Web \ (tramite un browser\ Web supportato) mentre egli è connesso alla rete aziendale. Computer client del dipendente nella rete aziendale comunica direttamente con il server federativo per l'autenticazione.  
  
Dopo aver esaminato le informazioni negli argomenti collegati, è possibile iniziare a distribuire questo obiettivo seguendo i passaggi descritti in [Checklist: Implementing a Federated Web SSO Design](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
La figura seguente mostra tutti i componenti necessari per questo obiettivo di distribuzione di ADFS.  
  
![Accedere al tuo attestazioni](media/31394ea8-fecb-4372-ac3f-cc3cf566ffc9.gif)  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
