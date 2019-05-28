---
ms.assetid: 14aa112d-ae31-4181-97e4-92623b5c9270
title: Rivedere il ruolo del proxy server federativo nel partner risorse
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 377baa8f282f3886284a53b686944fe145b1b15e
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190895"
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-resource-partner"></a>Rivedere il ruolo del proxy server federativo nel partner risorse

Un proxy server federativo in Active Directory Federation Services \(ADFS\) può avere uno o più dei ruoli seguenti, a seconda di come si configura il server per soddisfare le esigenze dell'organizzazione partner risorse:  
  
-   **Individuazione del partner account**: un computer client Internet deve identificare il partner account con cui eseguirà l'autenticazione. Il client individua il partner account con un partner account Web form di individuazione \(Discoverclientrealm. aspx\), che viene archiviato nel proxy server federativo nel partner risorse. Se più di un partner account è configurato nello snap di gestione di AD FS\-in un calo\-menu a discesa viene visualizzato al client con tutti i partner account disponibili visibili ai computer client Internet che accedono a partner account modulo per l'individuazione Web. È possibile modificare il modo in cui il modulo Web per l'individuazione del partner account è presentato ai computer client personalizzando il file discoverclientrealm.aspx.  
  
-   **Reindirizzamento del token di sicurezza**: Il proxy server federativo nel partner account invia i token di sicurezza al partner risorse. Il proxy server federativo di risorsa accetta i token e li passa al server federativo nel partner risorse. Il server federativo di risorsa rilascia quindi un token di sicurezza associata per un server Web di risorse specifico. Il proxy server federativo di risorsa Reindirizza quindi il token al client.  
  
Per riepilogare, un proxy server federativo di risorsa semplifica il processo di accesso federato reindirizzando i computer client a un server federativo che può autenticare i client. Un proxy server federativo di risorsa funge anche da proxy per i token di sicurezza client a server federativi di risorsa.  
  
> [!NOTE]  
> Quando è necessario ridurre la quantità di hardware e il numero di certificati richiesti, il proxy server federativo può trovarsi nello stesso computer come server Web.  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

