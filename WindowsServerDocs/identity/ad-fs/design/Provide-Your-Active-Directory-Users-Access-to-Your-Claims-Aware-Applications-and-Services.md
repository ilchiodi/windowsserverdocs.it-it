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

Quando si è un amministratore dell'organizzazione partner account in una distribuzione Active Directory Federation Services \(AD FS @ no__t-1 e si dispone di un obiettivo di distribuzione per fornire l'accesso Single @ no__t-2sign @ no__t-3op de afstandsbediening \(SSO @ no__t-5 per i dipendenti in rete aziendale per le risorse ospitate:  
  
-   I dipendenti che sono connessi a una foresta di Active Directory nella rete aziendale possono usare SSO per accedere a più applicazioni o servizi nella rete perimetrale dell'organizzazione. Tali applicazioni e servizi sono protetti da AD FS.  
  
    È ad esempio possibile che Fabrikam desideri che i dipendenti della rete aziendale dispongano dell'accesso federato alle applicazioni Web @ no__t-0based ospitate nella rete perimetrale per fabrikam.  
  
-   I dipendenti remoti connessi a un dominio Active Directory possono ottenere AD FS token dal server federativo nell'organizzazione per ottenere l'accesso federato alle applicazioni o ai servizi AD FS @ no__t-0secured Web @ no__t-1Basato che risiedono nel organizzazione.  
  
-   Le informazioni nell'archivio di attributi di Active Directory possono essere popolate nei token AD FS dei dipendenti.  
  
Per questo obiettivo di distribuzione, sono necessari i componenti seguenti:  
  
-   **Active Directory Domain Services \(AD DS @ no__t-2:** AD DS contiene gli account utente dei dipendenti usati per generare i token AD FS. Le informazioni come l'appartenenza ai gruppi e gli attributi vengono popolate nei token AD FS come attestazioni basate su gruppo e attestazioni personalizzate.  
  
    > [!NOTE]  
    > È anche possibile usare Lightweight Directory Access Protocol \(LDAP @ no__t-1 o Structured Query Language \(Il programma @ no__t-3 per includere le identità per la generazione di token AD FS.  
  
-   **DNS aziendale:** Questa implementazione di Domain Name System \(DNS @ no__t-1 contiene un semplice host \(A @ no__t-3 record di risorse, in modo che i client Intranet possano individuare il server federativo di account. Questa implementazione di DNS può ospitare anche altri record DNS necessari nella rete aziendale. Per altre informazioni, vedere [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   **Server federativo partner account:** Questo server federativo è aggiunto a un dominio nella foresta del partner account. Autentica gli account utente dei dipendenti e genera i token AD FS. Il computer client per il dipendente esegue l'autenticazione integrata di Windows su questo server federativo per generare un token di AD FS. Per altre informazioni, vedere [rivedere il ruolo del Server federativo nel Partner Account](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md).  
  
    Il server federativo partner account può autenticare gli utenti seguenti:  
  
    -   Dipendenti con account utente nel dominio  
  
    -   Dipendenti con account utente in qualsiasi punto della foresta  
  
    -   Dipendenti con account utente in qualsiasi punto delle foreste considerate attendibili da questa foresta \(through a due @ no__t-1Way Windows Trust @ no__t-2  
  
-   **Dipendente:** Un dipendente accede a un servizio Web @ no__t-0based \(through un'applicazione @ no__t-2 o un'applicazione Web @ no__t-3based \(through un Web browser supportato @ no__t-5 mentre è connesso alla rete aziendale. Il computer client del dipendente nella rete aziendale comunica direttamente con il server federativo per l'autenticazione.  
  
Dopo aver esaminato le informazioni negli argomenti collegati, è possibile iniziare a distribuire questo obiettivo seguendo i passaggi descritti in [Checklist: Implementazione di un progetto Web SSO federativo @ no__t-0.  
  
Nella figura seguente vengono illustrati tutti i componenti necessari per questo obiettivo di distribuzione AD FS.  
  
![accesso alle attestazioni](media/31394ea8-fecb-4372-ac3f-cc3cf566ffc9.gif)  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
