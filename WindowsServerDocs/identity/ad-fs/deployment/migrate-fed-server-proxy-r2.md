---
title: Eseguire la migrazione del server proxy federativo AD FS 2,0
description: Vengono fornite informazioni sulla migrazione di un server proxy AD FS a Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 57367cadd3c7ce3d031c6eb3a53c333422543dae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359366"
---
# <a name="migrate-the-active-directory-federation-services-proxy-server-to-windows-server-2012-r2"></a>Eseguire la migrazione del server proxy Active Directory Federation Services a Windows Server 2012 R2

In Active Directory Federation Services (AD FS) in Windows Server 2012 R2, il ruolo di proxy server federativo viene gestito da un nuovo servizio ruolo di accesso remoto denominato proxy applicazione Web. In Windows Server 2012 R2, per abilitare la AD FS per l'accessibilità dall'esterno della rete aziendale, è possibile distribuire uno o più proxy applicazione Web. Tuttavia, non è possibile eseguire la migrazione di un proxy server federativo in esecuzione in Windows Server 2008 R2 o Windows Server 2012 a un proxy applicazione Web in esecuzione in Windows Server 2012 R2.  
  
> [!IMPORTANT]
>  La migrazione di un proxy server federativo in esecuzione in Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 a un proxy applicazione Web in esecuzione in Windows Server 2012 R2 non è supportata.  
  
Se si desidera configurare AD FS in una farm di Windows Server 2012 R2 migrata per l'accesso Extranet, è necessario eseguire una nuova distribuzione di uno o più computer proxy applicazione Web come parte dell'infrastruttura AD FS.  
  
Per pianificare la distribuzione di Proxy applicazione Web, è possibile consultare le informazioni negli argomenti seguenti:  
  
- [Pianificare l'infrastruttura del proxy dell'applicazione Web](https://technet.microsoft.com/library/dn383648.aspx)  
  
- [Pianificare il server proxy applicazione Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
  Per distribuire il proxy applicazione Web, è possibile seguire le procedure negli argomenti seguenti:  
  
- [Configurare l'infrastruttura del proxy dell'applicazione Web](https://technet.microsoft.com/library/dn383644.aspx)  
  
- [Installare e configurare il server proxy applicazione Web](https://technet.microsoft.com/library/dn383662.aspx)  
  
## <a name="next-steps"></a>Passaggi successivi
 [Eseguire la migrazione dei servizi ruolo Active Directory Federation Services a Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Preparazione alla migrazione del server federativo di AD FS](prepare-migrate-ad-fs-server-r2.md)   
 [Migrazione del server federativo AD FS](migrate-ad-fs-fed-server-r2.md)    
 [Verifica della migrazione del AD FS a Windows Server 2012 R2](verify-ad-fs-migration.md)

