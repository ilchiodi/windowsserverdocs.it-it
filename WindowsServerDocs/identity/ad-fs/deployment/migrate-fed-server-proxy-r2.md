---
title: Eseguire la migrazione di server proxy AD FS 2.0 federation
description: Vengono fornite informazioni sulla migrazione di un server proxy AD FS per Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 18ce084ec7d1b602dfca913372d6a0e279671a6e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867412"
---
# <a name="migrate-the-active-directory-federation-services-proxy-server-to-windows-server-2012-r2"></a>Eseguire la migrazione del Server di Proxy Active Directory Federation Services a Windows Server 2012 R2

In Active Directory Federation Services (ADFS) in Windows Server 2012 R2, il ruolo di un proxy server federativo viene gestito da un nuovo servizio ruolo Accesso remoto denominato Proxy applicazione Web. In Windows Server 2012 R2, per rendere ADFS accessibile dall'esterno della rete aziendale, è possibile distribuire uno o più proxy applicazione Web. Tuttavia, è possibile eseguire la migrazione di un proxy server federativo in esecuzione in Windows Server 2008 R2 o Windows Server 2012 a un Proxy di applicazione Web in esecuzione in Windows Server 2012 R2.  
  
> [!IMPORTANT]
>  NON è supportata la migrazione di un proxy server federativo in esecuzione in Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 a un Proxy di applicazione Web in esecuzione in Windows Server 2012 R2.  
  
Se si desidera configurare AD FS in una farm migrata di Windows Server 2012 R2 per l'accesso extranet, è necessario eseguire una nuova distribuzione di uno o più computer Proxy applicazione Web nell'ambito dell'infrastruttura di AD FS.  
  
Per pianificare la distribuzione di Proxy applicazione Web, è possibile consultare le informazioni negli argomenti seguenti:  
  
-   [Pianificare l'infrastruttura del Proxy applicazione Web](https://technet.microsoft.com/library/dn383648.aspx)  
  
-   [Pianificare il Server Proxy applicazione Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
 Per distribuire il proxy applicazione Web, è possibile seguire le procedure negli argomenti seguenti:  
  
-   [Configurare l'infrastruttura del Proxy applicazione Web](https://technet.microsoft.com/library/dn383644.aspx)  
  
-   [Installare e configurare il Server Proxy applicazione Web](https://technet.microsoft.com/library/dn383662.aspx)  
  
## <a name="next-steps"></a>Passaggi successivi
 [Eseguire la migrazione di servizi ruolo di Active Directory Federation Services a Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Preparazione alla migrazione del Server federativo ADFS](prepare-migrate-ad-fs-server-r2.md)   
 [Migrazione del Server federativo di AD FS](migrate-ad-fs-fed-server-r2.md)    
 [Verifica della migrazione di ADFS a Windows Server 2012 R2](verify-ad-fs-migration.md)

