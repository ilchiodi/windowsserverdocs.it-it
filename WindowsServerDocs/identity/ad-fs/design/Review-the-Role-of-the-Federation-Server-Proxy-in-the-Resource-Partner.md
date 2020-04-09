---
ms.assetid: 14aa112d-ae31-4181-97e4-92623b5c9270
title: Rivedere il ruolo del proxy server federativo nel partner risorse
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6f49677df16cb1b44d08449a3983ae29fed3cb27
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858544"
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-resource-partner"></a>Rivedere il ruolo del proxy server federativo nel partner risorse

Un proxy server federativo in Active Directory Federation Services \(AD FS\) può funzionare in uno o più dei ruoli seguenti, a seconda di come si configura il server per soddisfare le esigenze dell'organizzazione partner risorse:  
  
-   **Individuazione del partner account**: un computer client Internet deve identificare il partner account con cui eseguirà l'autenticazione. Il client trova il partner account utilizzando un Web Form di individuazione partner account \(discoverclientrealm. aspx\), che è archiviato nel proxy server federativo nel partner risorse. Se è stato configurato più di un partner account nello snap-in gestione AD FS\-in, al client viene visualizzato un menu a\-discesa a discesa con tutti i partner account disponibili visibili ai computer client Internet che accedono al modulo Web per l'individuazione del partner account. È possibile modificare il modo in cui il modulo Web per l'individuazione del partner account è presentato ai computer client personalizzando il file discoverclientrealm.aspx.  
  
-   **Reindirizzamento del token di sicurezza**: il proxy server federativo nel partner account invia i token di sicurezza al partner risorse. Il proxy server federativo di risorsa accetta questi token e li passa al server federativo nel partner risorse. Il server federativo di risorsa rilascia quindi un token di sicurezza associato a un server Web di risorse specifico. Il proxy server federativo di risorsa reindirizza quindi il token al client.  
  
Per riepilogare, un proxy server federativo di risorsa facilita il processo di accesso federato reindirizzando i computer client a un server federativo in grado di autenticare i client. Un proxy server federativo di risorsa funge anche da proxy per i token di sicurezza client nei server federativi di risorse.  
  
> [!NOTE]  
> Quando è necessario ridurre la quantità di hardware e il numero di certificati richiesti, il proxy server federativo può trovarsi nello stesso computer del server Web.  
  
## <a name="see-also"></a>Vedi anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

