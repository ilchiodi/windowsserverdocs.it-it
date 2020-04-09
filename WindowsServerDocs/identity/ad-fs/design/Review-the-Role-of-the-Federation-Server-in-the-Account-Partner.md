---
ms.assetid: d0ba3c0d-869f-4e24-89d7-499da7576f22
title: Rivedere il ruolo del server federativo nel partner account
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 2d51f0091746d2d5e816108e3b92d0cb1a8267cd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858534"
---
# <a name="review-the-role-of-the-federation-server-in-the-account-partner"></a>Rivedere il ruolo del server federativo nel partner account

Un server federativo in Active Directory Federation Services \(AD FS\) funge da emittente del token di sicurezza. Un server federativo genera attestazioni basate sui valori dell'account che si trovano in un archivio di attributi locale e li inserisce in token di sicurezza in modo che gli utenti possano accedere facilmente alle applicazioni basate su Web\-browser\-\(utilizzando Single Sign\-su \(SSO\)\) ospitati in un'organizzazione partner risorse.  
  
> [!NOTE]  
> Quando gli utenti accedono alle applicazioni federate utilizzando una Web browser, un server federativo rilascia automaticamente ai cookie gli utenti per mantenere lo stato di accesso per l'applicazione Web\-browser\-. Questi cookie includono le attestazioni per gli utenti. I cookie abilitano le funzionalità SSO in modo che gli utenti non debbano immettere le credenziali ogni volta che visitano diverse applicazioni basate su Web\-browser\-nel partner risorse.  
  
Nel progetto Web SSO le organizzazioni con una rete perimetrale che vuole che gli utenti Internet abbiano accesso alle applicazioni devono installare un proxy server federativo nella rete perimetrale. Nel progetto SSO Web federativo deve essere installato almeno un server federativo nella rete aziendale dell'organizzazione partner account e almeno un server federativo installato nella rete aziendale dell'organizzazione partner risorse.  
  
> [!NOTE]  
> Prima di poter configurare un computer server federativo nell'organizzazione partner account, è innanzitutto necessario aggiungere il computer a un dominio nella foresta Active Directory in cui il server federativo verrà usato per autenticare gli utenti da tale foresta. Per altre informazioni, vedere [Checklist: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Vedi anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
