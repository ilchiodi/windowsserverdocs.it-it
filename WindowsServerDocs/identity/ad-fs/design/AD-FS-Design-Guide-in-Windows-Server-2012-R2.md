---
ms.assetid: a8558c9d-0606-4881-93b2-f2d2716b18e7
title: Guida alla progettazione di AD FS in Windows Server 2012 R2
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2d15f680f28c54da75100a03f7b85e880442d9be
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191745"
---
# <a name="ad-fs-design-guide-in-windows-server"></a>Guida alla progettazione di AD FS in Windows Server 

Active Directory Federation Services \(ADFS\) fornisce federazione delle identità semplificate e protette e Web single sign\-sul \(SSO\) funzionalità agli utenti finali che vogliono accedere alle applicazioni all'interno di un'istanza di ADFS\-protetta dell'organizzazione, in organizzazioni partner della federazione o nel cloud.  
  
In Windows Server® 2012 R2 AD FS include un servizio ruolo servizio federativo che funge da provider di identità \(autentica gli utenti per fornire i token di sicurezza alle applicazioni che considerano attendibile AD FS\) o da provider federativo \( utilizza i token di altri provider di identità e quindi fornisce i token di sicurezza per le applicazioni che considerano attendibile AD FS\).  
  
La funzione che fornisce l'accesso Extranet alle applicazioni e ai servizi protetti da AD FS in Windows Server 2012 R2 ora è svolta da un nuovo servizio ruolo di Accesso remoto chiamato Proxy applicazione Web. Esiste una forte differenza con le versioni precedenti di Windows Server in cui questa funzione era gestita da un proxy server federativo AD FS. Proxy applicazione Web è un ruolo del server progettato per fornire l'accesso per AD FS\-correlati scenario extranet e altri scenari extranet. Per altre informazioni sul Proxy applicazione Web, vedere [Guida allo scenario di Web Application Proxy](https://technet.microsoft.com/library/dn280944.aspx).  
  
## <a name="about-this-guide"></a>Informazioni sulla guida  
Questa guida fornisce indicazioni che consentono di pianificare una nuova distribuzione di AD FS, in base ai requisiti dell'organizzazione. Questa guida è destinata a uno specialista di infrastrutture o un progettista del sistema. Evidenzia i punti decisionali principali si pianifica la distribuzione di AD FS. Prima di leggere questa Guida, è necessario avere una buona conoscenza del funzionamento di ADFS a livello funzionale. Per altre informazioni, vedere [Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md).  
  
## <a name="in-this-guide"></a>Contenuto della guida  
  
-   [Identificare gli obiettivi di distribuzione di AD FS](Identify-Your-AD-FS-Deployment-Goals.md)  
  
-   [Pianificare la topologia di distribuzione di AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
  
-   [Requisiti di AD FS](AD-FS-Requirements.md)  
  
  
## <a name="see-also"></a>Vedere anche  
[Progettazione di AD FS](../../ad-fs/AD-FS-Design.md)  
  

