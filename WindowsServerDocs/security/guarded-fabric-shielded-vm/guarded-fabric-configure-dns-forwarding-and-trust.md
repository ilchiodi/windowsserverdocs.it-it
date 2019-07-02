---
title: Configurare l'inoltro di DNS e trust di dominio
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 3f9083d749ba9251ba47ecb64b7cb3d7c6290f1d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443777"
---
# <a name="configure-dns-forwarding-in-the-hgs-domain-and-a-one-way-trust-with-the-fabric-domain"></a>Configurare l'inoltro DNS nel dominio di HGS e una relazione di trust unidirezionale con il dominio dell'infrastruttura

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

>[!IMPORTANT]
>Modalità di Active Directory è deprecata a partire da Windows Server 2019. Per gli ambienti in cui l'attestazione TPM non è possibile, configurare [ospitare l'attestazione chiave](guarded-fabric-initialize-hgs-key-mode.md). Attestazione chiave host fornisce una garanzia analoga alla modalità di Active Directory ed è più semplice da configurare. 

Usare la procedura seguente per configurare l'inoltro di DNS e stabilire un trust unidirezionale con il dominio dell'infrastruttura. Questi passaggi consentono il servizio HGS per individuare l'infrastruttura di controller di dominio e verificare l'appartenenza al gruppo degli host Hyper-V.

1.  Eseguire il comando seguente in una sessione di PowerShell con privilegi elevata per configurare il DNS di inoltro. Sostituire fabrikam.com con il nome del dominio dell'infrastruttura e digitare gli indirizzi IP dei server DNS nel dominio dell'infrastruttura. Per una maggiore disponibilità, fare riferimento più di un server DNS.

    ```powershell
    Add-DnsServerConditionalForwarderZone -Name "fabrikam.com" -ReplicationScope "Forest" -MasterServers <DNSserverAddress1>, <DNSserverAddress2>
    ```

2.  Per creare un trust tra foreste unidirezionale, eseguire il comando seguente in un prompt dei comandi con privilegi elevati:

    Sostituire `bastion.local` con il nome del dominio HGS e `fabrikam.com` con il nome del dominio dell'infrastruttura. Specificare la password per un amministratore del dominio dell'infrastruttura.

        netdom trust bastion.local /domain:fabrikam.com /userD:fabrikam.com\Administrator /passwordD:<password> /add

## <a name="next-step"></a>Passaggio successivo 

> [!div class="nextstepaction"]
> [Configurare l'HTTPS](guarded-fabric-configure-hgs-https.md)