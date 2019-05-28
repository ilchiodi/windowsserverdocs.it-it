---
ms.assetid: d254fca3-85a1-424d-ac22-d6687ec3798e
title: Fornire agli utenti di Active Directory l'accesso ai servizi e alle applicazioni in grado di riconoscere attestazioni
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8c3db2873e1c7a0fa217ba37b9439cc38dfafc36
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190997"
---
# <a name="provide-your-active-directory-users-access-to-your-claims-aware-applications-and-services"></a>Fornire agli utenti di Active Directory l'accesso ai servizi e alle applicazioni in grado di riconoscere attestazioni

Quando si è amministratore dell'organizzazione partner account in una Active Directory Federation Services \(ADFS\) distribuzione e si dispone di un obiettivo di distribuzione per fornire single\-sign\-su \( SSO\) accesso per i dipendenti nella rete aziendale alle risorse ospitate:  
  
-   I dipendenti che sono connessi a una foresta di Active Directory nella rete aziendale possono usare SSO per accedere a più applicazioni o servizi nella rete perimetrale dell'organizzazione. Tali applicazioni e servizi protetti da AD FS.  
  
    Ad esempio, è possibile che Fabrikam voglia dipendenti della rete aziendale per l'accesso federativo ai Web\-basato su applicazioni ospitate nella rete perimetrale per Fabrikam.  
  
-   I dipendenti remoti connessi a un dominio Active Directory possono ottenere i token di AD FS dal server federativo nell'organizzazione per ottenere l'accesso federato ad AD FS\-protetti Web\-basato su applicazioni o servizi che risiedono anche nel l'organizzazione.  
  
-   Le informazioni nell'archivio di attributi di Active Directory possono essere popolate nei token AD FS dei dipendenti.  
  
Per questo obiettivo di distribuzione, sono necessari i componenti seguenti:  
  
-   **Active Directory Domain Services \(AD DS\):** AD DS contiene gli account utente dei dipendenti usati per generare i token AD FS. Le informazioni come l'appartenenza ai gruppi e gli attributi vengono popolate nei token AD FS come attestazioni basate su gruppo e attestazioni personalizzate.  
  
    > [!NOTE]  
    > È anche possibile usare Lightweight Directory Access Protocol \(LDAP\) Structured Query Language \(SQL\) per contenere le identità per AD FS il token di generazione.  
  
-   **DNS aziendale:** Questa implementazione di Domain Name System \(DNS\) contiene un host semplice \(oggetto\) di record di risorsa in modo che i client intranet possano individuare il server federativo di account. Questa implementazione di DNS può ospitare anche altri record DNS necessari nella rete aziendale. Per altre informazioni, vedere [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   **Server federativo del partner account:** Il server federativo viene aggiunto a un dominio nella foresta del partner account. Autentica gli account utente dei dipendenti e genera i token AD FS. Il computer client per il dipendente esegue l'autenticazione integrata di Windows sul server federativo per generare un token di AD FS. Per altre informazioni, vedere [Review the Role of the Federation Server in the Account Partner](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md).  
  
    Il server federativo del partner account può autenticare gli utenti seguenti:  
  
    -   Dipendenti con account utente nel dominio  
  
    -   Dipendenti con account utente in qualsiasi punto della foresta  
  
    -   Dipendenti con account utente remoto via Internet di foreste trusted dalla foresta \(tramite due\-modalità di attendibilità di Windows\)  
  
-   **Dipendente:** Un dipendente accede a un sito Web\-servizio basato su \(tramite un'applicazione\) o un sito Web\-applicazione basata su \(tramite un Web browser supportato\) mentre è connesso direste al rete aziendale. Computer client del dipendente nella rete aziendale comunica direttamente con il server federativo per l'autenticazione.  
  
Dopo aver esaminato le informazioni negli argomenti collegati, è possibile iniziare a distribuire questo obiettivo seguendo i passaggi descritti in [elenco di controllo: Implementazione di un progetto SSO Web federativo](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
Nella figura seguente viene illustrato ognuno dei componenti necessari per questo obiettivo di distribuzione di AD FS.  
  
![per le attestazioni di accesso](media/31394ea8-fecb-4372-ac3f-cc3cf566ffc9.gif)  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
