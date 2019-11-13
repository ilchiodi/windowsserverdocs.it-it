---
ms.assetid: 97999892-29c6-4076-be19-5e5259d8ada6
title: Distribuzione di server federativi
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: f2aaca5ffc846c41af82c276750c564db38b5020
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359507"
---
# <a name="interoperating-with-ad-fs-1x"></a>Interazione con AD FS 1.x

Per l'interoperabilità tra Active Directory Federation Services \(AD FS\) in Windows Server® 2012 e AD FS 1. *x*, completare una o più delle attività seguenti, a seconda delle esigenze dell'organizzazione:  
  
-   Pianificare l'interoperabilità tra AD FS in Windows Server 2012 e versioni precedenti di AD FS e altre informazioni sul tipo di attestazione ID nome. Per ulteriori informazioni, vedere [pianificazione dell'interoperabilità con ad FS 1. x](https://technet.microsoft.com/library/ff678040.aspx).  
  
-   Se si invieranno attestazioni da un Servizio federativo di AD FS in Windows Server 2012 che può essere utilizzato da un AD FS 1. *x* servizio federativo, vedere [Checklist: configuring ad FS to Send Claims to an ad FS 1. x servizio federativo](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md).  
  
-   Se si invieranno attestazioni da un Servizio federativo di AD FS in Windows Server 2012 che può essere utilizzato da un'applicazione ospitata da un server Web che esegue il AD FS 1. *x* claims\-agente Web in grado di riconoscere, vedere [elenco di controllo: configurazione ad FS per inviare attestazioni a un agente Web in grado di riconoscere attestazioni ad FS 1. x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md).  
  
-   Se si invieranno attestazioni da un AD FS 1. *x* servizio federativo essere utilizzato da un Servizio federativo di ad FS in Windows Server 2012, vedere [elenco di controllo: configurazione ad FS per l'utilizzo di attestazioni da ad FS 1. x](Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md).  
  
## <a name="differences-between-federation-service-settings"></a>Differenze tra le impostazioni di Servizio federativo  
Sebbene la maggior parte dei AD FS 1. *x* servizio federativo le impostazioni funzionano in modo analogo al servizio federativo ad FS nelle impostazioni di Windows Server 2012, alcuni nomi di impostazioni sono stati modificati. Nella tabella seguente sono elencati i nomi delle impostazioni per un AD FS 1. *x* servizio federativo e i relativi nomi equivalenti per un Servizio federativo di ad FS in Windows Server 2012.  
  
|Impostazione Servizio federativo AD FS 1. x|Servizio federativo di AD FS equivalenti nell'impostazione di Windows Server 2012  
|----------------------------------------|---------------------------------------------------------------------------------------------------------- 
|Partner account|Trust del provider di attestazioni  
|Partner risorse|Trust della relying party 
|Applicazione|Trust della relying party  
|Proprietà dell'applicazione|Proprietà attendibilità componente  
|URL applicazione|Identificatore della relying party e URL dell'endpoint passivo di WS\-Federation  
|URI Servizio federativo|Identificatore del servizio federativo  
|URL dell'endpoint Servizio federativo|URL dell'endpoint passivo di WS\-Federation  
  
## <a name="see-also"></a>Vedi anche  
[Interoperabilità AD FS e AD FS 1. x](https://go.microsoft.com/fwlink/?LinkId=200776)  
  

