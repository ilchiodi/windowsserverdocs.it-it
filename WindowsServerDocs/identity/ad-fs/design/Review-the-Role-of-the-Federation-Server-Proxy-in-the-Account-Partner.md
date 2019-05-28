---
ms.assetid: 1b3a03c0-5558-4177-9b2f-e9d6ce3271cd
title: Rivedere il ruolo del proxy server federativo nel partner account
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: dbceb19d31738bdc5b628a9a2b069e5d3022d145
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190948"
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-account-partner"></a>Rivedere il ruolo del proxy server federativo nel partner account

Il ruolo primario del proxy server federativo nella rete perimetrale dell'organizzazione partner account in Active Directory Federation Services \(ADFS\) di raccogliere le credenziali di autenticazione da un computer client che esegue l'accesso la rete Internet e passarle al server federativo, che si trova all'interno della rete azienda dell'organizzazione partner account. L'account del computer client viene archiviato nell'archivio di attributi del partner account.  
  
Un proxy server federativo può essere utilizzato anche in uno o più dei ruoli seguenti, a seconda di come è configurata in modo da soddisfare le esigenze dell'organizzazione partner account:  
  
-   Inoltrare i token di sicurezza, ovvero il server federativo rilascia un token di sicurezza per il proxy server federativo, che quindi inoltra il token al computer client. Il token di sicurezza viene usato per fornire l'accesso per tale computer a una relying party specifica.  
  
-   Raccogliere le credenziali, ovvero il proxy server federativo Usa un modulo Web di accesso client predefinito \(Clientlogon. aspx\) raccogliere password\-basati su credenziali tramite moduli\-l'autenticazione basata su. Tuttavia, è possibile personalizzare questo modulo per accettare altri tipi supportati di autenticazione, ad esempio Secure Sockets Layer \(SSL\) l'autenticazione del client. Per altre informazioni su come personalizzare questa pagina, vedere la personalizzazione di accesso Client e Home Realm Discovery pagine \( [http:\/\/go.microsoft.com\/fwlink\/? LinkId\=104275](https://go.microsoft.com/fwlink/?LinkId=104275)\). Un proxy server federativo non accetta le credenziali tramite l'autenticazione integrata di Windows.  
  
Per riepilogare, un proxy server federativo nel partner account funge da proxy per gli accessi client a un server federativo che si trova nella rete aziendale. Il proxy server federativo facilita inoltre la distribuzione dei token di sicurezza per i client Internet destinati a relying party.  
  
> [!CAUTION]  
> Esposizione di un proxy server federativo nel partner account extranet avranno accesso l'account di accesso client modulo Web accessibile da qualsiasi utente con Internet. Ciò può rendere l'organizzazione potenzialmente vulnerabile ad alcuni password\-basato attacchi, ad esempio attacchi con dizionario o attacchi di forza bruta che possono attivare blocchi per gli account utente archiviati nel dominio di Active Directory aziendale I servizi \(Active Directory Domain Services\).  
  

## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
