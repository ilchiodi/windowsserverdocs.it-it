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
ms.openlocfilehash: e2776cc29b8c9ede884a6b304cd541f700f516ca
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191264"
---
# <a name="name-resolution-requirements-for-federation-servers"></a>Requisiti di risoluzione dei nomi per i server federativi

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
