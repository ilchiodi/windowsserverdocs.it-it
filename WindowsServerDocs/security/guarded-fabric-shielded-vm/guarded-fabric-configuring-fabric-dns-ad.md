---
title: Configurare il DNS dell'infrastruttura per gli host sorvegliati (AD)
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 074b6d09-f16e-49bf-b88a-377139d35067
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 411b845d57c36916dcbc73d51675f5d9f92bfa0e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386761"
---
# <a name="configure-the-fabric-dns-for-guarded-hosts"></a>Configurare il DNS dell'infrastruttura per gli host sorvegliati

>Si applica a: Windows Server 2016


>[!IMPORTANT]
>La modalità Active Directory è deprecata a partire da Windows Server 2019. Per gli ambienti in cui non è possibile attestazione TPM, configurare l' [attestazione chiave host](guarded-fabric-initialize-hgs-key-mode.md). L'attestazione chiave host offre una garanzia simile alla modalità AD ed è più semplice da configurare. 

Un amministratore dell'infrastruttura deve configurare il DNS dell'infrastruttura per consentire agli host sorvegliati di risolvere il cluster HGS. Il cluster HGS deve essere già [configurato dall'amministratore di HGS](/WindowsServerDocs/virtualization/guarded-fabric-shielded-vm/guarded-fabric-setting-up-the-host-guardian-service-hgs.md).



[!INCLUDE [Configure fabric DNS](../../../includes/guarded-fabric-configure-fabric-dns.md)] 


## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Configurare il DNS di HGS e un trust unidirezionale](guarded-fabric-configure-dns-forwarding-and-trust.md)
