---
ms.assetid: 41d6b897-1e72-4522-aad6-eece1154a154
title: Distribuzione di ADFS nell'organizzazione Partner risorse
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4a556c07e7d6e0bec4c947ea9d1a75eef9964cef
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="deploying-ad-fs-in-the-resource-partner-organization"></a>Distribuzione di ADFS nell'organizzazione Partner risorse

>Si applica a: Windows Server 2016, Windows Server 2012 R2

L'organizzazione partner risorse in Active Directory Federation Services \(AD FS\) rappresenta l'organizzazione cui server Web possono essere protetti da un server federativo resource\-side. Il server federativo del partner risorse utilizza i token di sicurezza generati dal partner account per fornire attestazioni ai server Web che si trovano nel partner risorse.  
  
In scenari in cui è necessario fornire l'accesso ai servizi federati o applicazioni a molti utenti diversi, ovvero quando alcuni utenti che risiedono in organizzazioni diverse, è possibile configurare il server federativo di risorsa in modo che è possibile distribuire più partner account.  
  
Per ulteriori informazioni su come installare e configurare un'organizzazione partner risorse, vedere [elenco di controllo: configurazione dell'organizzazione Partner risorse](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md).  
  
## <a name="in-this-section"></a>In questa sezione  
  
-   [Rivedere il ruolo del Server federativo nel Partner risorse](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md)  
  
-   [Rivedere il ruolo del Proxy Server federativo nel Partner risorse](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)  
  
-   [Determinare la strategia di applicazioni federate nel Partner risorse](Determine-Your-Federated-Application-Strategy-in-the-Resource-Partner.md)  
  

## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
