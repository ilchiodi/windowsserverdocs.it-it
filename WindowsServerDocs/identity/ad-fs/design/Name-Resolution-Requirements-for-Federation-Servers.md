---
ms.assetid: 74ef34c8-e13f-499b-b2bb-952ad7036622
title: Requisiti di risoluzione dei nomi per i server federativi
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 74701cbaa403611b081942f016b21db1c0b3ff70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845462"
---
# <a name="name-resolution-requirements-for-federation-servers"></a>Requisiti di risoluzione dei nomi per i server federativi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

I computer client nella rete aziendale quando tentano di accedere a un'applicazione o un servizio Web protetto da Active Directory Federation Services \(ADFS\), devono prima autenticarsi a un server federativo. Un modo per eseguire l'autenticazione è affinché i client di rete aziendale di accedere a un server federativo locale tramite l'autenticazione integrata di Windows.  
  
## <a name="configure-corporate-dns"></a>Configurare il DNS aziendale  
In modo che la risoluzione dei nomi corretta tramite l'autenticazione integrata di Windows nel server federativo locale può essere eseguita, Domain Name System \(DNS\) nella rete aziendale dell'account del partner deve essere configurato per un nuovo host \(Un'\) record di risorse che risolverà il nome di dominio completo \(FQDN\) nome host del server federativo per l'indirizzo IP del cluster di server federativo.  
  
La figura seguente illustra come viene eseguita questa attività per un determinato scenario. In questo scenario, Bilanciamento carico di rete Microsoft \(NLB\) fornisce un nome FQDN del cluster singolo e un indirizzo IP singolo cluster per una server farm federativa esistente.  
  
![requisiti dei nomi](media/adfs2_deploy_single_fs.gif)  
  
Per informazioni su come configurare un indirizzo IP del cluster o cluster FQDN utilizzando bilanciamento carico di rete, vedere [specificando i parametri del Cluster](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
Per informazioni su come configurare DNS aziendale per un server federativo, vedere [aggiunge un Host &#40;A&#41; Record di risorse al DNS aziendale per un Server federativo](../../ad-fs/deployment/Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
Per informazioni su come configurare i server federativi nella rete perimetrale, vedere [requisiti di risoluzione dei nomi per i Server federativi](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  

## <a name="see-also"></a>Vedere anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
