---
ms.assetid: a8558c9d-0606-4881-93b2-f2d2716b18e7
title: Guida alla progettazione di AD FS in Windows Server 2012 R2
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e8675c255032bca4623a9649bfc0bcca478008e3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854894"
---
# <a name="ad-fs-design-guide-in-windows-server"></a>Guida alla progettazione di AD FS in Windows Server 

Active Directory Federation Services \(AD FS\) fornisce la Federazione delle identità semplificata e protetta e il Web Single Sign\-sulle funzionalità \(SSO\) per gli utenti finali che desiderano accedere alle applicazioni all'interno di un AD FS\-azienda protetta, nelle organizzazioni partner della Federazione o nel cloud.  
  
In Windows Server&reg; 2012 R2 AD FS include un servizio ruolo del servizio federativo che funge da provider di identità \(autentica gli utenti per fornire i token di sicurezza alle applicazioni che considerano attendibile AD FS\) o come provider di Federazione \(utilizza i token di altri provider di identità e quindi fornisce i token di sicurezza alle applicazioni che considerano attendibile AD FS\).  
  
La funzione che fornisce l'accesso Extranet alle applicazioni e ai servizi protetti da AD FS in Windows Server 2012 R2 ora è svolta da un nuovo servizio ruolo di Accesso remoto chiamato Proxy applicazione Web. Esiste una forte differenza con le versioni precedenti di Windows Server in cui questa funzione era gestita da un proxy server federativo AD FS. Proxy applicazione Web è un ruolo del server progettato per fornire l'accesso per il AD FS\-scenario Extranet correlato e altri scenari Extranet. Per ulteriori informazioni sul proxy applicazione Web, vedere [Guida alla procedura dettagliata del proxy dell'applicazione Web](https://technet.microsoft.com/library/dn280944.aspx).  
  
## <a name="about-this-guide"></a>Informazioni sulla guida  
Questa guida fornisce indicazioni utili per pianificare una nuova distribuzione di AD FS, in base ai requisiti dell'organizzazione. Questa guida è destinata a uno specialista di infrastrutture o un progettista del sistema. Vengono evidenziati i punti decisionali principali durante la pianificazione della distribuzione di AD FS. Prima di leggere questa guida, è opportuno conoscere il modo in cui AD FS funziona a livello funzionale. Per altre informazioni, vedere [Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md).  
  
## <a name="in-this-guide"></a>In questa guida  
  
-   [Identificare gli obiettivi di distribuzione di AD FS](Identify-Your-AD-FS-Deployment-Goals.md)  
  
-   [Pianificare la topologia di distribuzione di AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
  
-   [Requisiti di AD FS](AD-FS-Requirements.md)  
  
  
## <a name="see-also"></a>Vedi anche  
[Progettazione di AD FS](../../ad-fs/AD-FS-Design.md)  
  

