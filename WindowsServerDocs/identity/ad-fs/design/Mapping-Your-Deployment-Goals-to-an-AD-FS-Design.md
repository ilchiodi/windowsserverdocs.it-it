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
ms.openlocfilehash: 13d8ae8b8f3e4c8160f61284e5fb97e21b6a51b6
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191254"
---
# <a name="mapping-your-deployment-goals-to-an-ad-fs-design"></a>Mapping degli obiettivi di distribuzione a una progettazione ADFS


Dopo aver esaminato i servizi esistenti di Active Directory Federation \(ADFS\) degli obiettivi di distribuzione e avere determinato gli obiettivi correlati alla distribuzione, è possibile eseguire il mapping di tali obiettivi a una specifica progettazione di AD FS. Per altre informazioni su AD FS predefinite degli obiettivi di distribuzione, vedere [identificazione degli obiettivi di distribuzione di AD FS](Identifying-Your-AD-FS-Deployment-Goals.md).  
  
Usare la tabella seguente per determinare quale progettazione ADFS viene eseguito il mapping alla combinazione appropriata di AD FS degli obiettivi di distribuzione per l'organizzazione. Questa tabella fa riferimento solo alle due progettazioni di AD FS primarie, come descritto in questa Guida. Tuttavia, è possibile creare ibrida o personalizzata progettazione di AD FS usando qualsiasi combinazione di obiettivi di distribuzione di AD FS per soddisfare le esigenze della propria organizzazione.  
  
|Obiettivo di distribuzione di AD FS|[Progetto di Web SSO](Web-SSO-Design.md)|[Progetto di Web SSO federativo](Federated-Web-SSO-Design.md)|  
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|  
|[Fornire agli utenti di Active Directory l'accesso ai servizi e alle applicazioni in grado di riconoscere attestazioni](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)|No|Sì, nel partner account|  
|[Fornire agli utenti di Active Directory l'accesso ai servizi e alle applicazioni di altre organizzazioni](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)|No|Sì, facoltativo nel partner account|  
|[Fornire agli utenti di un'altra organizzazione l'accesso ai propri servizi e alle applicazioni in grado di riconoscere attestazioni](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)|Yes|Yes|  

## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
  

