---
ms.assetid: 68979914-8a1c-465a-bd37-08df30722d69
title: Mapping degli obiettivi di distribuzione a una progettazione ADFS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 048bce75c52895b2d9e215bdccef9cb13dc23533
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866822"
---
# <a name="mapping-your-deployment-goals-to-an-ad-fs-design"></a>Mapping degli obiettivi di distribuzione a una progettazione ADFS

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dopo aver esaminato i servizi esistenti di Active Directory Federation \(ADFS\) degli obiettivi di distribuzione e avere determinato gli obiettivi correlati alla distribuzione, è possibile eseguire il mapping di tali obiettivi a una specifica progettazione di AD FS. Per altre informazioni su AD FS predefinite degli obiettivi di distribuzione, vedere [identificazione degli obiettivi di distribuzione di AD FS](Identifying-Your-AD-FS-Deployment-Goals.md).  
  
Usare la tabella seguente per determinare quale progettazione ADFS viene eseguito il mapping alla combinazione appropriata di AD FS degli obiettivi di distribuzione per l'organizzazione. Questa tabella fa riferimento solo alle due progettazioni di AD FS primarie, come descritto in questa Guida. Tuttavia, è possibile creare ibrida o personalizzata progettazione di AD FS usando qualsiasi combinazione di obiettivi di distribuzione di AD FS per soddisfare le esigenze della propria organizzazione.  
  
|Obiettivo di distribuzione di AD FS|[Progettazione di Web SSO](Web-SSO-Design.md)|[Progetto SSO Web federativo](Federated-Web-SSO-Design.md)|  
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|  
|[Fornire agli utenti di Active Directory accesso ai servizi e alle applicazioni in grado di riconoscere attestazioni](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)|No|Sì, nel partner account|  
|[Fornire l'accesso agli utenti di Active Directory per le applicazioni e servizi di altre organizzazioni](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)|No|Sì, facoltativo nel partner account|  
|[Fornire agli utenti in un'altra organizzazione l'accesso ai servizi e alle applicazioni in grado di riconoscere attestazioni](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)|Yes|Yes|  

## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
  

