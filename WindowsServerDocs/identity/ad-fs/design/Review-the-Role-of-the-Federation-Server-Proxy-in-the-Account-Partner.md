---
ms.assetid: 1b3a03c0-5558-4177-9b2f-e9d6ce3271cd
title: Rivedere il ruolo del Proxy Server federativo nel Partner Account
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d2b60ce593c2ca7eb902595ee6a42850cb7605d9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-account-partner"></a>Rivedere il ruolo del Proxy Server federativo nel Partner Account

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il ruolo primario del proxy server federativo nella rete perimetrale dell'organizzazione partner account in Active Directory Federation Services \(AD FS\) è per raccogliere le credenziali di autenticazione da un computer client che accede a Internet e le credenziali per il server federativo, che si trova all'interno della rete azienda dell'organizzazione partner account. L'account per il computer client viene archiviato nell'archivio di attributi del partner account.  
  
Un proxy server federativo può funzionare anche in uno o più dei ruoli seguenti, a seconda di come si configura per soddisfare le esigenze dell'organizzazione partner account:  
  
-   Inoltrare i token di sicurezza, il server federativo rilascia un token di sicurezza per il proxy server federativo, che quindi inoltra il token al computer client. Il token di sicurezza viene utilizzato per fornire l'accesso per il computer client a una relying party specifica.  
  
-   Raccogliere le credenziali, il proxy server federativo utilizza un predefinito client accesso Web form \(clientlogon.aspx\) per raccogliere le credenziali basate su password\ tramite l'autenticazione basata su forms\. Tuttavia, è possibile personalizzare questo modulo per accettare altri tipi supportati di autenticazione, ad esempio l'autenticazione client SSL (Secure Sockets Layer) \(SSL\). Per ulteriori informazioni su come personalizzare questa pagina, vedere Personalizzazione di accesso Client e Home pagine di individuazione dell'area di autenticazione \ ([http:///\/go.microsoft.com\/fwlink\/? LinkId\ = 104275](https://go.microsoft.com/fwlink/?LinkId=104275)\). Un proxy server federativo non accetta le credenziali tramite autenticazione integrata di Windows.  
  
Per riassumere, un proxy server federativo nel partner account funge da proxy per gli accessi client a un server federativo che si trova nella rete aziendale. Il proxy server federativo facilita inoltre la distribuzione dei token di sicurezza ai client Internet destinati alle relying party.  
  
> [!CAUTION]  
> Esposizione di un proxy server federativo nel partner account extranet potranno accedere l'accesso del client modulo Web accessibile da tutti gli utenti con Internet. Questo può rendere l'organizzazione potenzialmente vulnerabile ad alcuni attacchi basati su password\, ad esempio attacchi con dizionario o attacchi di forza bruta che possono attivare blocchi per gli account utente archiviati in \(AD DS\) i servizi di dominio Active Directory aziendale.  
  

## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
