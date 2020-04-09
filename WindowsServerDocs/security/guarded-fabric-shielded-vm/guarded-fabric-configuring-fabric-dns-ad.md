---
title: Configurare il DNS dell'infrastruttura per gli host sorvegliati (AD)
ms.prod: windows-server
ms.topic: article
ms.assetid: 074b6d09-f16e-49bf-b88a-377139d35067
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 5b7deafb49083ad55d5a49d1ad3c5e063e91483f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856824"
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
