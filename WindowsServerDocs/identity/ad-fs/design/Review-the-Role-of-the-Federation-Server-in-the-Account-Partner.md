---
ms.assetid: d0ba3c0d-869f-4e24-89d7-499da7576f22
title: Rivedere il ruolo del server federativo nel partner account
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0914d32e8f24d5e7db0a25c733342c1bde3e0329
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835132"
---
# <a name="review-the-role-of-the-federation-server-in-the-account-partner"></a>Rivedere il ruolo del server federativo nel partner account

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un server federativo in Active Directory Federation Services \(ADFS\) funziona come un emittente di token di sicurezza. Un server federativo genera attestazioni basate su account in cui archiviano i valori che si trovano in un attributo locale e le include in token di sicurezza in modo che gli utenti possano accedere facilmente alle Web\-browser\-le applicazioni basate su \(usando l'accesso Single sign\-sul \(SSO\) \) che sono ospitati in un'organizzazione partner risorse.  
  
> [!NOTE]  
> Quando gli utenti accedono alle applicazioni federate usando un Web browser, un server federativo rilascia automaticamente i cookie agli utenti di mantenere lo stato di accesso per il Web\-browser\-applicazione basata su. Questi cookie includono le attestazioni per gli utenti. I cookie abilitano le funzionalità SSO, in modo che gli utenti non debbano immetterle ogni volta che visitano una diversa del Web\-browser\-basato su applicazioni nel partner risorse.  
  
Nella progettazione di Web SSO, le organizzazioni con una rete perimetrale che gli utenti remoti di accedere alle applicazioni è necessario installare un proxy server federativo nella rete perimetrale. Nella progettazione di Web SSO federativo, deve essere presente almeno un server federativo installato nella rete aziendale dell'organizzazione partner account e almeno un server federativo installato nella rete aziendale dell'organizzazione partner risorse.  
  
> [!NOTE]  
> Prima di impostare backup di un computer server federativo nell'organizzazione partner account, è innanzitutto necessario aggiungere il computer in qualsiasi dominio nella foresta di Active Directory in cui il server federativo verrà utilizzato per autenticare gli utenti da tale foresta. Per ulteriori informazioni, vedere [Elenco di controllo: Configurazione di un Server federativo](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
