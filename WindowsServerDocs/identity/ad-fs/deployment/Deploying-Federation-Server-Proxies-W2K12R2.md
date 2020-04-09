---
ms.assetid: 222e9f93-7c41-4527-8a98-8f7fbc7a58af
title: Distribuzione di proxy server federativi
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 04837c8b38f1f6cdf048f7d8744d9cd0f61ebb0d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855454"
---
# <a name="deploying-federation-server-proxies"></a>Distribuzione di proxy server federativi

In Active Directory Federation Services \(AD FS\) in Windows Server 2012 R2, il ruolo di proxy server federativo viene gestito da un nuovo servizio ruolo di accesso remoto denominato proxy applicazione Web. Per abilitare la AD FS per l'accessibilità dall'esterno della rete aziendale, che era lo scopo della distribuzione di un proxy server federativo nelle versioni legacy di AD FS, ad esempio AD FS 2,0 e AD FS in Windows Server 2012, è possibile distribuire uno o più proxy applicazione Web per AD FS in Windows Server 2012 R2.  
  
Nel contesto di AD FS, proxy applicazione Web funge da AD FS proxy server federativo. Oltre a questo, Proxy applicazione Web rende disponibili funzionalità di proxy inverso per le applicazioni Web all'interno della rete aziendale, in modo da consentire agli utenti con qualsiasi dispositivo di accedere a tali applicazioni dall'esterno della rete aziendale. Per altre informazioni sul servizio ruolo Proxy applicazione Web, vedere la panoramica di Proxy applicazione Web.  
  
Per pianificare la distribuzione di Proxy applicazione Web, è possibile consultare le informazioni negli argomenti seguenti:  
  
-   [Pianificare l'infrastruttura del proxy dell'applicazione Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx)  
  
-   [Pianificare il server proxy applicazione Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
Per distribuire il proxy applicazione Web, è possibile seguire le procedure negli argomenti seguenti:  
  
-   [Configurare l'infrastruttura del proxy dell'applicazione Web](https://technet.microsoft.com/library/dn383644.aspx)  
  
-   [Installare e configurare il server proxy applicazione Web](https://technet.microsoft.com/library/dn383662.aspx)  
  
 
## <a name="see-also"></a>Vedi anche 

[Distribuzione di AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guida alla distribuzione di Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Distribuzione di una server farm federativa](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

