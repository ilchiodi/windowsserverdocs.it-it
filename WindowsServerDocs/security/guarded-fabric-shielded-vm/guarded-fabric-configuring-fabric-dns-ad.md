---
title: Configurare l'infrastruttura DNS per gli host sorvegliati (AD)
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 074b6d09-f16e-49bf-b88a-377139d35067
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 8a2ddc0f2ab495b2500d4bfe48f3ee83c333c769
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842752"
---
# <a name="configure-the-fabric-dns-for-guarded-hosts"></a>Configurare l'infrastruttura DNS per gli host sorvegliati

>Si applica a: Windows Server 2016


>[!IMPORTANT]
>Modalità di Active Directory è deprecata a partire da Windows Server 2019. Per gli ambienti in cui l'attestazione TPM non è possibile, configurare [ospitare l'attestazione chiave](guarded-fabric-initialize-hgs-key-mode.md). Attestazione chiave host fornisce una garanzia analoga alla modalità di Active Directory ed è più semplice da configurare. 

Un amministratore dell'infrastruttura deve configurare l'infrastruttura che DNS accetta per consentire gli host sorvegliati risolvere il cluster del servizio HGS. Deve essere già nel cluster del servizio HGS [impostato dall'amministratore HGS](/WindowsServerDocs/virtualization/guarded-fabric-shielded-vm/guarded-fabric-setting-up-the-host-guardian-service-hgs.md).



[!INCLUDE [Configure fabric DNS](../../../includes/guarded-fabric-configure-fabric-dns.md)] 


## <a name="next-step"></a>Passaggio successivo

>[!div class="nextstepaction"]
[Configurare HGS DNS e una relazione di trust unidirezionale](guarded-fabric-configure-dns-forwarding-and-trust.md)
