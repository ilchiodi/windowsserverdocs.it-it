---
ms.assetid: 74ef34c8-e13f-499b-b2bb-952ad7036622
title: Requisiti di risoluzione dei nomi per i server federativi
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 74701cbaa403611b081942f016b21db1c0b3ff70
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="name-resolution-requirements-for-federation-servers"></a>Requisiti di risoluzione dei nomi per i server federativi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quando i computer client nella rete aziendale tentano di accedere a un'applicazione o servizio Web protetto da Active Directory Federation Services \(AD FS\), devono prima autenticarsi a un server federativo. Un modo per eseguire l'autenticazione è affinché i client della rete aziendale di accedere a un server federativo locale tramite autenticazione integrata di Windows.  
  
## <a name="configure-corporate-dns"></a>Configurare il DNS aziendale  
In modo che può verificarsi risoluzione dei nomi corretta tramite autenticazione integrata di Windows nel server federativo locale, Domain Name System \(DNS\) nella rete aziendale del partner account deve essere configurato per un nuovo record di risorse \(A\) host che risolverà il nome di host \(FQDN\) nome dominio completo del server federativo per l'indirizzo IP del cluster di server federativo.  
  
Nella figura seguente, è possibile visualizzare come viene eseguita questa attività per un determinato scenario. In questo scenario, Bilanciamento carico di rete Microsoft \(NLB\) fornisce un nome di dominio completo del cluster singolo e un indirizzo IP del cluster singolo per una server farm federativa esistente.  
  
![Requisiti dei nomi](media/adfs2_deploy_single_fs.gif)  
  
Per informazioni su come configurare un indirizzo IP del cluster o cluster FQDN utilizzando bilanciamento carico di rete, vedere [specifica dei parametri del Cluster](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
Per informazioni su come configurare DNS aziendale per un server federativo, vedere [aggiungere un Host & #40; A & #41; Record di risorse al DNS aziendale per un Server federativo](../../ad-fs/deployment/Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
Per informazioni su come configurare i server federativi nella rete perimetrale, vedere [requisiti di risoluzione dei nomi per i proxy Server federativi](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  

## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di ADFS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
