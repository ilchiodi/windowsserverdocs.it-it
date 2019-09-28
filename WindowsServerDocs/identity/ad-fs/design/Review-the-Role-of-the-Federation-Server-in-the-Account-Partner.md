---
ms.assetid: d0ba3c0d-869f-4e24-89d7-499da7576f22
title: Rivedere il ruolo del server federativo nel partner account
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ead8868f38faa570a0e524630e23d99e276a7c79
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407924"
---
# <a name="review-the-role-of-the-federation-server-in-the-account-partner"></a>Rivedere il ruolo del server federativo nel partner account

Un server federativo in Active Directory Federation Services \(AD FS @ no__t-1 funge da emittente del token di sicurezza. Un server federativo genera attestazioni in base ai valori dell'account che si trovano in un archivio di attributi locale e li inserisce in token di sicurezza in modo che gli utenti possano accedere facilmente alle applicazioni Web @ no__t-0Browser @ no__t-1Basato \(using Single Sign @ no__t-3op de afstandsbediening \(SSO @ no__t-5 @ no__t-6 ospitati in un'organizzazione partner risorse.  
  
> [!NOTE]  
> Quando gli utenti accedono alle applicazioni federate utilizzando un Web browser, un server federativo rilascia automaticamente ai cookie gli utenti per mantenere lo stato di accesso per l'applicazione Web @ no__t-0Browser @ no__t-1Basato. Questi cookie includono le attestazioni per gli utenti. I cookie abilitano le funzionalità SSO in modo che gli utenti non debbano immettere le credenziali ogni volta che visitano diverse applicazioni Web @ no__t-0Browser @ no__t-1Basato nel partner risorse.  
  
Nel progetto Web SSO le organizzazioni con una rete perimetrale che vuole che gli utenti Internet abbiano accesso alle applicazioni devono installare un proxy server federativo nella rete perimetrale. Nel progetto SSO Web federativo deve essere installato almeno un server federativo nella rete aziendale dell'organizzazione partner account e almeno un server federativo installato nella rete aziendale dell'organizzazione partner risorse.  
  
> [!NOTE]  
> Prima di poter configurare un computer server federativo nell'organizzazione partner account, è innanzitutto necessario aggiungere il computer a un dominio nella foresta Active Directory in cui il server federativo verrà usato per autenticare gli utenti da tale foresta. Per ulteriori informazioni, vedere [Elenco di controllo: Configurazione di un server federativo @ no__t-0.  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
