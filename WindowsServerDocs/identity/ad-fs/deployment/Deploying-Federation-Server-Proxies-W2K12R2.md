---
ms.assetid: 222e9f93-7c41-4527-8a98-8f7fbc7a58af
title: Distribuzione di proxy Server federativi
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: dc49d8f4b656fdbb92083aa3c60bc4ce81091e9b
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="deploying-federation-server-proxies"></a>Distribuzione di proxy Server federativi

>Si applica a: Windows Server 2016, Windows Server 2012 R2

In Active Directory Federation Services \(AD FS\) in Windows Server 2012 R2, il ruolo di un proxy server federativo viene gestito da un nuovo servizio ruolo Accesso remoto denominato Proxy applicazione Web. Per rendere ADFS accessibile dall'esterno della rete aziendale, che lo scopo della distribuzione di un proxy server federativo nelle versioni legacy di ADFS, ad esempio ADFS 2.0 e AD FS in Windows Server 2012, è possibile distribuire uno o più proxy applicazione web per ADFS in Windows Server 2012 R2.  
  
Nel contesto di ADFS, Proxy applicazione Web funziona come un proxy server federativo di ADFS. Oltre a questo, Proxy applicazione Web fornisce funzionalità di proxy inverso per le applicazioni web all'interno della rete aziendale consentire agli utenti in qualsiasi dispositivo per accedervi dall'esterno della rete aziendale. Per ulteriori informazioni sul servizio ruolo Proxy applicazione Web, vedere Panoramica di Proxy applicazione Web.  
  
Per pianificare la distribuzione di proxy applicazione Web, è possibile esaminare le informazioni negli argomenti seguenti:  
  
-   [Pianificare l'infrastruttura di Proxy applicazione Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx)  
  
-   [Pianificare il Server di Proxy applicazione Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
Per distribuire proxy applicazione Web, è possibile seguire le procedure negli argomenti seguenti:  
  
-   [Configurare l'infrastruttura del Proxy applicazione Web](https://technet.microsoft.com/library/dn383644.aspx)  
  
-   [Installare e configurare il Server Proxy applicazione Web](https://technet.microsoft.com/library/dn383662.aspx)  
  
 
## <a name="see-also"></a>Vedere anche 

[Distribuzione di ADFS](../../ad-fs/AD-FS-Deployment.md)  

[Guida alla distribuzione di Windows Server 2012 R2 AD ADFS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Distribuzione di una Server Farm federativa](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

