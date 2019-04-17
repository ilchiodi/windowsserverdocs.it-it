---
ms.assetid: d0ba3c0d-869f-4e24-89d7-499da7576f22
title: Rivedere il ruolo del Server federativo nel Partner Account
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0914d32e8f24d5e7db0a25c733342c1bde3e0329
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="review-the-role-of-the-federation-server-in-the-account-partner"></a>Rivedere il ruolo del Server federativo nel Partner Account

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un server federativo in Active Directory Federation Services \(AD FS\) funge da un'autorità di certificazione token di sicurezza. Un server federativo genera attestazioni basate su account in cui archiviare i valori che si trovano in un attributo locale e pacchetti nei token di sicurezza in modo che gli utenti possono accedere con facilità basata sul Web browser\ applicazioni \ (con singolo accesso-on \(SSO\)\) che sono ospitati in un'organizzazione partner risorse.  
  
> [!NOTE]  
> Quando agli utenti l'accesso federato applicazioni utilizzando un Web browser, un server federativo rilascia automaticamente i cookie per gli utenti di mantenere lo stato di accesso per tale applicazione basata sul Web browser\. Questi cookie includono le attestazioni per gli utenti. I cookie abilitano le funzionalità SSO in modo che gli utenti non è necessario immettere le credenziali ogni volta che visitano diverse applicazioni basata sul Web browser\ nel partner risorse.  
  
Nella progettazione Web SSO le organizzazioni con una rete perimetrale che vogliono agli utenti di Internet di accedere alle applicazioni devono installare un proxy server federativo nella rete perimetrale. Nella progettazione Web SSO federativo deve essere presente almeno un server federativo installato nella rete aziendale dell'organizzazione partner account e almeno un server federativo installato nella rete aziendale dell'organizzazione partner risorse.  
  
> [!NOTE]  
> Prima di impostare un server federativo nell'organizzazione partner account, è innanzitutto necessario aggiungere il computer in qualsiasi dominio nella foresta di Active Directory in cui verrà utilizzato il server federativo per autenticare gli utenti da tale foresta. Per ulteriori informazioni, vedere [Checklist: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
