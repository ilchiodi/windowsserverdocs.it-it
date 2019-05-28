---
ms.assetid: 39acccd9-0402-49ca-8ce1-b239e1e7e455
title: Distribuzione di AD FS nell'organizzazione partner risorse
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 87849e1d7a5eb8fef24a551dfc681c65a202f027
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191544"
---
# <a name="deploying-ad-fs-in-the-resource-partner-organization"></a>Distribuzione di AD FS nell'organizzazione partner risorse

L'organizzazione partner risorse in Active Directory Federation Services \(ADFS\) rappresenta l'organizzazione cui server Web possono essere protetti da una risorsa\-server federativo lato. Il server federativo del partner risorse Usa i token di sicurezza generati dal partner account per fornire attestazioni ai server Web che si trovano nel partner risorse.  
  
Negli scenari in cui è necessario fornire l'accesso ai servizi federati o applicazioni a molti utenti diversi, ovvero quando alcuni utenti si trovano in organizzazioni diverse, è possibile configurare il server federativo di risorsa in modo da poter distribuire più partner account.  
  
Per altre informazioni su come installare e configurare un'organizzazione partner risorse, vedere [elenco di controllo: Configuring the Resource Partner Organization](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md).  
  
## <a name="in-this-section"></a>Contenuto della sezione  
  
-   [Rivedere il ruolo del server federativo nel partner risorse](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md)  
  
-   [Rivedere il ruolo del proxy server federativo nel partner risorse](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)  
  
-   [Determinare la strategia per le applicazioni federate nel partner risorse](Determine-Your-Federated-Application-Strategy-in-the-Resource-Partner.md)  
  

## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
