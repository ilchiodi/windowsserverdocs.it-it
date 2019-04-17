---
ms.assetid: 97999892-29c6-4076-be19-5e5259d8ada6
title: Distribuzione di server federativi
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: f80425b6f062040c51357353038fd07ff0a79ae6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="interoperating-with-ad-fs-1x"></a>Interoperabilità con AD FS 1. x

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Per l'interoperabilità tra \(AD FS\) Active Directory Federation Services in Windows Server® 2012 e AD FS 1. *x*, completare una o più delle seguenti operazioni, a seconda delle esigenze dell'organizzazione:  
  
-   Pianificare l'interoperabilità tra ADFS in Windows Server 2012 e versioni precedenti di ADFS e apprendere che informazioni sull'ID nome del tipo di attestazione. Per ulteriori informazioni, vedere [pianificazione per l'interoperabilità con AD FS 1. x](https://technet.microsoft.com/library/ff678040.aspx).  
  
-   Se si inviano le attestazioni da un servizio federativo ADFS in Windows Server 2012 che può essere utilizzata da un'istanza di ADFS 1. *x* servizio federativo, vedere [Checklist: Configuring AD FS to Send Claims to an AD FS 1. x servizio federativo](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md).  
  
-   Se si inviano le attestazioni da un servizio federativo ADFS in Windows Server 2012 che può essere utilizzata da un'applicazione ospitata da un server Web con AD FS 1. *x* agente Web compatibile con claims\, vedere [Checklist: Configuring AD FS to Send Claims to an AD FS 1. x Claims-Aware Web agente](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md).  
  
-   Se si inviano le attestazioni da un'istanza di ADFS 1. *x* servizio federativo per essere utilizzata da un servizio federativo ADFS in Windows Server 2012, vedere [elenco di controllo: configurazione di ADFS per utilizzare attestazioni ADFS 1. x](Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md).  
  
## <a name="differences-between-federation-service-settings"></a>Differenze tra le impostazioni del servizio federativo  
Sebbene la maggior parte dei AD FS 1. *x* lavoro impostazioni servizio federativo in modo analogo a come il servizio federativo ADFS nelle impostazioni di Windows Server 2012, sono stati modificati alcuni nomi di impostazione. La tabella seguente elenca i nomi delle impostazioni per un'istanza di ADFS 1. *x* servizio federativo e i relativi nomi equivalenti per un servizio federativo ADFS in Windows Server 2012.  
  
|Impostazione di servizio federativo AD FS 1. x|Servizio federativo AD FS equivalente in Windows Server 2012 impostazione  
|----------------------------------------|---------------------------------------------------------------------------------------------------------- 
|Partner account|Attendibilità provider di attestazioni  
|Partner risorse|Trust della relying party 
|Applicazione|Trust della relying party  
|Proprietà dell'applicazione|Proprietà di terze parti attendibilità componente  
|URL dell'applicazione|Relying party identificatore e WS-Federation Passive URL dell'Endpoint  
|URI del servizio federativo|Identificatore del servizio federativo  
|URL dell'endpoint del servizio federativo|URL dell'Endpoint passivo WS-Federation  
  
## <a name="see-also"></a>Vedere anche  
[AD FS e interoperabilità di AD FS 1. x](https://go.microsoft.com/fwlink/?LinkId=200776)  
  

