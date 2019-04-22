---
ms.assetid: 97999892-29c6-4076-be19-5e5259d8ada6
title: Distribuzione di server federativi
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: f80425b6f062040c51357353038fd07ff0a79ae6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812172"
---
# <a name="interoperating-with-ad-fs-1x"></a>Interazione con AD FS 1.x

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Per l'interoperabilità tra Active Directory Federation Services \(ADFS\) in Windows Server® 2012 e AD FS 1. *x*, completare una o più delle attività seguenti, a seconda delle esigenze dell'organizzazione:  
  
-   Piano per l'interoperabilità tra ADFS in Windows Server 2012 e versioni precedenti di ADFS e apprendere che informazioni sull'ID nome del tipo di attestazione. Per altre informazioni, vedere [pianificazione per l'interoperabilità con AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx).  
  
-   Se si inviano le attestazioni da un servizio federativo di AD FS in Windows Server 2012 che possono essere utilizzati da un'istanza di ADFS 1. *x* servizio federativo, vedere [elenco di controllo: Configurazione di AD FS per l'invio di attestazioni a un servizio federativo di AD FS 1.x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md).  
  
-   Se si inviano le attestazioni da un servizio federativo di AD FS in Windows Server 2012 che possono essere utilizzati da un'applicazione ospitata da un server Web che esegue AD FS 1. *x* attestazioni\-agente Web compatibile con, vedere [elenco di controllo: Configurazione di AD FS per l'invio di attestazioni a un agente di AD FS 1.x Claims-Aware Web](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md).  
  
-   Se si inviano le attestazioni da un'istanza di ADFS 1. *x* servizio federativo deve essere utilizzato da un servizio federativo di AD FS in Windows Server 2012, vedere [elenco di controllo: Configurazione di ADFS per utilizzare attestazioni ADFS 1.x](Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md).  
  
## <a name="differences-between-federation-service-settings"></a>Differenze tra le impostazioni del servizio federativo  
Sebbene la maggior parte di AD FS 1. *x* interagiscono le impostazioni del servizio federativo in modo simile come il servizio federativo ADFS nelle impostazioni di Windows Server 2012, sono stati modificati alcuni nomi di impostazione. Nella tabella seguente elenca i nomi delle impostazioni per un'istanza di ADFS 1. *x* servizio federativo e i relativi nomi equivalenti per un servizio federativo di AD FS in Windows Server 2012.  
  
|Impostazione di servizio federativo AD FS 1.x|Servizio federativo AD FS equivalente nelle impostazioni di Windows Server 2012  
|----------------------------------------|---------------------------------------------------------------------------------------------------------- 
|Partner account|Trust del provider di attestazioni  
|Partner risorse|Trust della relying party 
|Applicazione|Trust della relying party  
|Proprietà dell'applicazione|Proprietà di attendibilità di terze parti componente  
|URL dell'applicazione|Identificatore di terze parti e WS relying\-Federation Passive URL dell'Endpoint  
|URI del servizio federativo|Identificatore del servizio federativo  
|URL dell'endpoint del servizio federativo|WS\-federazione passiva Endpoint URL  
  
## <a name="see-also"></a>Vedere anche  
[AD FS e l'interoperabilità di AD FS 1.x](https://go.microsoft.com/fwlink/?LinkId=200776)  
  

