---
ms.assetid: a8558c9d-0606-4881-93b2-f2d2716b18e7
title: Guida alla progettazione di ADFS in Windows Server 2012 R2
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 498b399818fb8c9e463f9990fa13c87648c0a33d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="ad-fs-design-guide-in-windows-server-2012-r2"></a>Guida alla progettazione di ADFS in Windows Server 2012 R2

>Si applica a: Windows Server 2016, Windows Server 2012 R2

Active Directory Federation Services \(AD FS\) offre federazione delle identità semplificate e protette e Web singolo accesso-on \(SSO\) funzionalità agli utenti finali che vogliono accedere alle applicazioni di un'organizzazione protette FS\ Active Directory, in organizzazioni partner della federazione o nel cloud.  
  
In Windows Server® 2012 R2 AD FS include un servizio ruolo servizio federativo che funge da provider di identità \ (autentica gli utenti per fornire i token di sicurezza alle applicazioni che considerano attendibile AD FS\) o da provider federativo \ (utilizza i token di altri provider di identità e quindi fornisce i token di sicurezza alle applicazioni che considerano attendibile AD FS\).  
  
La funzione che fornisce l'accesso extranet alle applicazioni e servizi protetti da ADFS in Windows Server 2012 R2 ora è svolta da un nuovo servizio ruolo Accesso remoto denominato Proxy applicazione Web. Si tratta di una forte differenza con le versioni precedenti di Windows Server in cui questa funzione era gestita da un proxy server federativo di ADFS. Proxy applicazione Web è un ruolo del server progettato per fornire l'accesso per lo scenario extranet correlato AD FS\ e altri scenari extranet. Per ulteriori informazioni su Proxy applicazione Web, vedere [Guida allo scenario di Proxy applicazione Web](https://technet.microsoft.com/library/dn280944.aspx).  
  
## <a name="about-this-guide"></a>Informazioni sulla Guida  
Questa guida fornisce indicazioni per la pianificazione di una nuova distribuzione di ADFS, in base ai requisiti dell'organizzazione. Questa guida è destinata per l'uso da un architetto di sistema o uno specialista dell'infrastruttura. Evidenzia i punti principali decisioni da prendere quando si pianifica la distribuzione di ADFS. Prima di leggere questa Guida, è una buona comprensione del funzionamento di AD FS in un livello di funzionalità. Per ulteriori informazioni, vedere [Understanding Key AD FS concetti](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md).  
  
## <a name="in-this-guide"></a>In questa Guida  
  
-   [Identificare gli obiettivi di distribuzione AD FS](Identify-Your-AD-FS-Deployment-Goals.md)  
  
-   [Pianificare la topologia di distribuzione AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
  
-   [Requisiti per ADFS](AD-FS-Requirements.md)  
  
  
## <a name="see-also"></a>Vedere anche  
[Progettazione di ADFS](../../ad-fs/AD-FS-Design.md)  
  

