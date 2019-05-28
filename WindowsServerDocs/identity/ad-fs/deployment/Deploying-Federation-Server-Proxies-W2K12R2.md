---
ms.assetid: 222e9f93-7c41-4527-8a98-8f7fbc7a58af
title: Distribuzione di proxy server federativi
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 330214e83b6da5bf711c36995306f8f1a098fa24
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192209"
---
# <a name="deploying-federation-server-proxies"></a>Distribuzione di proxy server federativi

In Active Directory Federation Services \(ADFS\) in Windows Server 2012 R2, il ruolo di un proxy server federativo viene gestito da un nuovo servizio ruolo Accesso remoto denominato Proxy applicazione Web. Per rendere ADFS accessibile dall'esterno della rete aziendale, che lo scopo della distribuzione di un proxy server federativo nelle versioni legacy di ADFS, ad esempio AD FS 2.0 e ADFS in Windows Server 2012, è possibile distribuire uno o più proxy applicazione web per oggetto D ADFS in Windows Server 2012 R2.  
  
Nel contesto di AD FS, Proxy applicazione Web funziona come un proxy server federativo AD FS. Oltre a questo, Proxy applicazione Web rende disponibili funzionalità di proxy inverso per le applicazioni Web all'interno della rete aziendale, in modo da consentire agli utenti con qualsiasi dispositivo di accedere a tali applicazioni dall'esterno della rete aziendale. Per altre informazioni sul servizio ruolo Proxy applicazione Web, vedere la panoramica di Proxy applicazione Web.  
  
Per pianificare la distribuzione di Proxy applicazione Web, è possibile consultare le informazioni negli argomenti seguenti:  
  
-   [Pianificare l'infrastruttura del Proxy applicazione Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx)  
  
-   [Pianificare il Server Proxy applicazione Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
Per distribuire il proxy applicazione Web, è possibile seguire le procedure negli argomenti seguenti:  
  
-   [Configurare l'infrastruttura del Proxy applicazione Web](https://technet.microsoft.com/library/dn383644.aspx)  
  
-   [Installare e configurare il Server Proxy applicazione Web](https://technet.microsoft.com/library/dn383662.aspx)  
  
 
## <a name="see-also"></a>Vedere anche 

[Distribuzione di AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guida alla distribuzione di Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Distribuzione di una server farm federativa](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

