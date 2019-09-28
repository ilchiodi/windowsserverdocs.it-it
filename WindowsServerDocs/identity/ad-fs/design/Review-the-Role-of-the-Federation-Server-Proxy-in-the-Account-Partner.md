---
ms.assetid: 1b3a03c0-5558-4177-9b2f-e9d6ce3271cd
title: Rivedere il ruolo del proxy server federativo nel partner account
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 876dee31a38506f7d7934bd8adfb51766bb60ce5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407935"
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-account-partner"></a>Rivedere il ruolo del proxy server federativo nel partner account

Il ruolo principale del proxy server federativo nella rete perimetrale dell'organizzazione partner account in Active Directory Federation Services \(ad FS\) consiste nel raccogliere le credenziali di autenticazione da un computer client che esegue l'accesso tramite Internet e per passare le credenziali al server federativo, che si trova nella rete aziendale dell'organizzazione partner account. L'account del computer client viene archiviato nell'archivio di attributi del partner account.  
  
Un proxy server federativo può inoltre funzionare in uno o più dei ruoli seguenti, a seconda di come viene configurato per soddisfare le esigenze dell'organizzazione partner account:  
  
-   Token di sicurezza di inoltro: il server federativo emette un token di sicurezza per il proxy server federativo, che quindi inoltra il token al computer client. Il token di sicurezza viene usato per fornire l'accesso per tale computer a una relying party specifica.  
  
-   Raccogli credenziali: il proxy server federativo utilizza un Web \(form di accesso client predefinito clientlogon. aspx\) per raccogliere le credenziali basate su\-password\-tramite l'autenticazione basata su form. È tuttavia possibile personalizzare questo modulo per accettare altri tipi di autenticazione supportati, ad esempio Secure Sockets Layer \(autenticazione client SSL.\) Per altre informazioni su come personalizzare questa pagina, vedere \(personalizzazione delle pagine di accesso client e di individuazione dell'area di autenticazione principale [http:\/\/go.Microsoft.com\/\/fwlink? LinkId\=104275](https://go.microsoft.com/fwlink/?LinkId=104275).\) Un proxy server federativo non accetta le credenziali tramite l'autenticazione integrata di Windows.  
  
Per riepilogare, un proxy server federativo nel partner account funge da proxy per gli accessi client a un server federativo situato nella rete aziendale. Il proxy server federativo facilita anche la distribuzione dei token di sicurezza ai client Internet destinati alle relying party.  
  
> [!CAUTION]  
> Esponendo un proxy server federativo nell'Extranet del partner account, il Web Form di accesso client potrà essere accessibile da chiunque disponga di accesso a Internet. Questo può potenzialmente lasciare l'organizzazione vulnerabile ad alcuni attacchi\-basati su password, ad esempio attacchi con dizionario o attacchi di forza bruta che possono attivare blocchi per gli account utente archiviati nel dominio di Active Directory aziendale Servizi \(di dominio\)Active Directory.  
  

## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
