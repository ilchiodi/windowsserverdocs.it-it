---
ms.assetid: 14aa112d-ae31-4181-97e4-92623b5c9270
title: Rivedere il ruolo del Proxy Server federativo nel Partner risorse
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 31e285e863e4316a8e0a65f9b68c27442290927d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="review-the-role-of-the-federation-server-proxy-in-the-resource-partner"></a>Rivedere il ruolo del Proxy Server federativo nel Partner risorse

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un proxy server federativo in Active Directory Federation Services \(AD FS\) può funzionare in uno o più dei ruoli seguenti, a seconda di come si configura il server per soddisfare le esigenze dell'organizzazione partner risorse:  
  
-   **Individuazione del partner account**: un computer client Internet deve identificare quali account partner eseguirà l'autenticazione. Il client trova partner account utilizzando un account partner individuazione Web form \(discoverclientrealm.aspx\), che viene archiviato nel proxy server federativo nel partner risorse. Se più di un partner account è configurato in Gestione di AD FS snap-in, che viene visualizzato un menu di elenchi a discesa per il client con tutti i partner account disponibili visibili ai computer client Internet che accedono il modulo Web individuazione del partner account. È possibile modificare la modalità di presentazione di individuazione del partner account modulo Web per i computer client personalizzando il file discoverclientrealm.aspx.  
  
-   **Reindirizzamento del token di sicurezza**: il proxy server federativo nel partner account invia i token di sicurezza al partner risorse. Il proxy server federativo di risorsa accetta i token e li passa al server federativo nel partner risorse. Quindi, il server federativo di risorsa rilascia un token di sicurezza associato a un server Web di risorse. Il proxy server federativo di risorsa Reindirizza quindi il token al client.  
  
Per riassumere, un proxy server federativo di risorsa facilita il processo di accesso federato reindirizzando i computer client a un server federativo che può autenticare i client. Un proxy server federativo di risorsa funge anche da proxy per i token di sicurezza client per i server federativi di risorsa.  
  
> [!NOTE]  
> Quando è necessario ridurre la quantità di hardware e il numero di certificati necessari, il proxy server federativo può trovarsi nello stesso computer del server Web.  
  
## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

