---
ms.assetid: 68979914-8a1c-465a-bd37-08df30722d69
title: Mapping degli obiettivi di distribuzione a una progettazione ADFS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ca10f8e784fea3b99a60b2117f65ba1ccaf6501e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359095"
---
# <a name="mapping-your-deployment-goals-to-an-ad-fs-design"></a>Mapping degli obiettivi di distribuzione a una progettazione ADFS


Dopo aver esaminato gli obiettivi di distribuzione esistenti di Active Directory Federation Services \(AD FS @ no__t-1 e aver determinato gli obiettivi correlati alla distribuzione, è possibile eseguire il mapping di tali obiettivi a una specifica progettazione di AD FS. Per altre informazioni su AD FS obiettivi di distribuzione predefiniti, vedere [identificazione degli obiettivi di distribuzione di ad FS](Identifying-Your-AD-FS-Deployment-Goals.md).  
  
Usare la tabella seguente per determinare quale progettazione di AD FS viene mappata alla combinazione appropriata di obiettivi di distribuzione AD FS per l'organizzazione. Questa tabella si riferisce solo alle due progettazioni di AD FS principali, come descritto in questa guida. Tuttavia, è possibile creare una progettazione di AD FS ibrida o personalizzata usando qualsiasi combinazione degli obiettivi di distribuzione AD FS per soddisfare le esigenze dell'organizzazione.  
  
|Obiettivo della distribuzione AD FS|[Progetto di Web SSO](Web-SSO-Design.md)|[Progetto di Web SSO federativo](Federated-Web-SSO-Design.md)|  
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|  
|[Fornire agli utenti di Active Directory l'accesso ai servizi e alle applicazioni in grado di riconoscere attestazioni](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)|No|Sì, nel partner account|  
|[Fornire agli utenti di Active Directory l'accesso ai servizi e alle applicazioni di altre organizzazioni](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)|No|Sì, facoltativo nel partner account|  
|[Fornire agli utenti di un'altra organizzazione l'accesso ai propri servizi e alle applicazioni in grado di riconoscere attestazioni](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)|Yes|Yes|  

## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
  

