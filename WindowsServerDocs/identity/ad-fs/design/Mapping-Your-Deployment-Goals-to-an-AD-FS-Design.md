---
ms.assetid: 68979914-8a1c-465a-bd37-08df30722d69
title: Mapping degli obiettivi di distribuzione per un progetto di ADFS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 048bce75c52895b2d9e215bdccef9cb13dc23533
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="mapping-your-deployment-goals-to-an-ad-fs-design"></a>Mapping degli obiettivi di distribuzione per un progetto di ADFS

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dopo aver esaminato gli obiettivi di distribuzione \(AD FS\) Active Directory Federation Services esistenti e determinare gli obiettivi correlati alla distribuzione, è possibile mappare tali obiettivi a una specifica progettazione di ADFS. Per ulteriori informazioni su AD FS predefiniti obiettivi di distribuzione, vedere [identificazione degli obiettivi di distribuzione di AD FS](Identifying-Your-AD-FS-Deployment-Goals.md).  
  
Utilizzare la tabella seguente per determinare quale progettazione ADFS corrisponde alla combinazione appropriata di AD FS obiettivi di distribuzione per l'organizzazione. Questa tabella fa riferimento solo alle due progettazioni di ADFS primarie, come descritto in questa Guida. Tuttavia, è possibile creare un ibrido o progettazione di ADFS usando qualsiasi combinazione di obiettivi di distribuzione di ADFS per soddisfare le esigenze dell'organizzazione.  
  
|Obiettivo di distribuzione di AD FS|[Progettazione di Web SSO](Web-SSO-Design.md)|[Progetto Web SSO federativo](Federated-Web-SSO-Design.md)|  
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|  
|[Fornire agli utenti di Active Directory accesso ai propri servizi e applicazioni in grado di riconoscere attestazioni](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)|No|Sì, nel partner account|  
|[Fornire agli utenti di Active Directory accesso alle applicazioni e servizi di altre organizzazioni](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)|No|Sì, facoltativo nel partner account|  
|[Fornire agli utenti in un'altra organizzazione l'accesso ai propri servizi e applicazioni in grado di riconoscere attestazioni](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)|Sì|Sì|  

## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
  

