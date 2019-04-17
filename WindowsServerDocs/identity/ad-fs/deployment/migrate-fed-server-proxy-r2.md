---
title: La migrazione del server proxy federativo 2.0 di ADFS
description: Fornisce informazioni sulla migrazione di un server proxy ADFS a Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 33ab29fd5efdb0bdd1fe25580e3f4434071e1c7d
ms.sourcegitcommit: 03ce78a1624dbd7f4e6abf2ec1ef185b55de29a1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/12/2017
---
# <a name="migrate-the-active-directory-federation-services-proxy-server-to-windows-server-2012-r2"></a>Eseguire la migrazione il Server Proxy di Active Directory Federation Services a Windows Server 2012 R2

In Active Directory Federation Services (ADFS) in Windows Server 2012 R2, il ruolo di un proxy server federativo viene gestito da un nuovo servizio ruolo Accesso remoto denominato Proxy applicazione Web. In Windows Server 2012 R2, per rendere ADFS accessibile dall'esterno della rete aziendale, è possibile distribuire uno o più proxy applicazione Web. Tuttavia, è possibile eseguire la migrazione di un proxy server federativo in esecuzione in Windows Server 2008 R2 o Windows Server 2012 a un Proxy applicazione Web in esecuzione su Windows Server 2012 R2.  
  
> [!IMPORTANT]
>  NON è supportata la migrazione di un proxy server federativo in esecuzione in Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 a un Proxy applicazione Web in esecuzione su Windows Server 2012 R2.  
  
Se si desidera configurare ADFS in una farm migrata di Windows Server 2012 R2 per l'accesso extranet, è necessario eseguire una nuova distribuzione di uno o più computer Proxy applicazione Web nell'ambito dell'infrastruttura di ADFS.  
  
Per pianificare la distribuzione di Proxy applicazione Web, è possibile esaminare le informazioni negli argomenti seguenti:  
  
-   [Pianificare l'infrastruttura di Proxy applicazione Web](https://technet.microsoft.com/en-us/library/dn383648.aspx)  
  
-   [Pianificare il Server di Proxy applicazione Web](https://technet.microsoft.com/en-us/library/dn383647.aspx)  
  
 Per distribuire proxy applicazione Web, è possibile seguire le procedure negli argomenti seguenti:  
  
-   [Configurare l'infrastruttura del Proxy applicazione Web](https://technet.microsoft.com/en-us/library/dn383644.aspx)  
  
-   [Installare e configurare il Server Proxy applicazione Web](https://technet.microsoft.com/en-us/library/dn383662.aspx)  
  
## <a name="next-steps"></a>Passaggi successivi
 [Eseguire la migrazione di ruolo di Active Directory Federation Services a Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Preparazione alla migrazione del Server federativo ADFS](prepare-migrate-ad-fs-server-r2.md)   
 [La migrazione del Server federativo ADFS](migrate-ad-fs-fed-server-r2.md)    
 [Verifica della migrazione di ADFS a Windows Server 2012 R2](verify-ad-fs-migration.md)

