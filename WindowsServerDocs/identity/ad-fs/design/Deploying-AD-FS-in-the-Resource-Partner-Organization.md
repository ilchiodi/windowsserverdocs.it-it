---
ms.assetid: 41d6b897-1e72-4522-aad6-eece1154a154
title: Distribuzione di AD FS nell'organizzazione partner risorse
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d9afc19be9ee92c23198b7fd8a7716379eb0821d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853164"
---
# <a name="deploying-ad-fs-in-the-resource-partner-organization"></a>Distribuzione di AD FS nell'organizzazione partner risorse

L'organizzazione partner risorse in Active Directory Federation Services \(AD FS\) rappresenta l'organizzazione i cui server Web possono essere protetti da una risorsa\-server federativo lato. Il server federativo nel partner risorse usa i token di sicurezza generati dal partner account per fornire attestazioni ai server Web che si trovano nel partner risorse.  
  
Negli scenari in cui è necessario fornire l'accesso alle applicazioni o ai servizi federati a molti utenti diversi, quando alcuni utenti risiedono in organizzazioni diverse, è possibile configurare il server federativo di risorsa in modo da poter distribuire più partner account.  
  
Per altre informazioni su come installare e configurare un'organizzazione partner risorse, vedere [Checklist: Configuring the Resource Partner Organization](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md).  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Rivedere il ruolo del server federativo nel partner risorse](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md)  
  
-   [Rivedere il ruolo del proxy server federativo nel partner risorse](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)  
  
-   [Determinare la strategia per le applicazioni federate nel partner risorse](Determine-Your-Federated-Application-Strategy-in-the-Resource-Partner.md)  
  

## <a name="see-also"></a>Vedi anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
