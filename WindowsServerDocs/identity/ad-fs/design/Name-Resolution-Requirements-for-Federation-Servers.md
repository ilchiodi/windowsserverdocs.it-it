---
ms.assetid: 74ef34c8-e13f-499b-b2bb-952ad7036622
title: Requisiti di risoluzione dei nomi per i server federativi
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b455596910e1b04569e087843da8b88f9f020137
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853034"
---
# <a name="name-resolution-requirements-for-federation-servers"></a>Requisiti di risoluzione dei nomi per i server federativi

Quando i computer client nella rete aziendale tentano di accedere a un'applicazione o a un servizio Web protetto da Active Directory Federation Services \(AD FS\), devono prima eseguire l'autenticazione a un server federativo. Un modo per eseguire l'autenticazione consiste nel fare in modo che i client della rete aziendale accedano a un server federativo locale tramite l'autenticazione integrata di Windows.  
  
## <a name="configure-corporate-dns"></a>Configurare il DNS aziendale  
Per consentire la corretta risoluzione dei nomi tramite l'autenticazione integrata di Windows nei server federativi locali, Domain Name System \(\) DNS nella rete aziendale del partner account deve essere configurata per un nuovo host \(un\) record di risorse che risolverà il nome di dominio completo \(FQDN\) nome host del server federativo all'indirizzo IP del cluster di server federativo.  
  
La figura seguente illustra come viene eseguita questa attività per un determinato scenario. In questo scenario, bilanciamento carico di rete Microsoft \(NLB\) fornisce un solo nome FQDN del cluster e un singolo indirizzo IP del cluster per una server farm federativa esistente.  
  
![requisiti del nome](media/adfs2_deploy_single_fs.gif)  
  
Per informazioni su come configurare un indirizzo IP o un FQDN del cluster usando NLB, vedere [specifica dei parametri del cluster](https://go.microsoft.com/fwlink/?LinkId=75282).  
  
Per informazioni su come configurare il DNS aziendale per un server federativo, vedere [aggiungere un host &#40;a&#41; record di risorse al DNS aziendale per un server federativo](../../ad-fs/deployment/Add-a-Host--A--Resource-Record-to-Corporate-DNS-for-a-Federation-Server.md).  
  
Per informazioni su come configurare i proxy server federativi nella rete perimetrale, vedere [requisiti di risoluzione dei nomi per i proxy server federativi](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  

## <a name="see-also"></a>Vedi anche
[Guida alla progettazione di AD FS in Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
