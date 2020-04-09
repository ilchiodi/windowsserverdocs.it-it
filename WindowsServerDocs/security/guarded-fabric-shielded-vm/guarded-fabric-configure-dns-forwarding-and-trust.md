---
title: Configurare l'invio DNS e l'attendibilità del dominio
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 6d6ad10dacf9c667069ecd43f38473a3f20bc781
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856854"
---
# <a name="configure-dns-forwarding-in-the-hgs-domain-and-a-one-way-trust-with-the-fabric-domain"></a>Configurare l'invio DNS nel dominio HGS e un trust unidirezionale con il dominio dell'infrastruttura

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

>[!IMPORTANT]
>La modalità Active Directory è deprecata a partire da Windows Server 2019. Per gli ambienti in cui non è possibile attestazione TPM, configurare l' [attestazione chiave host](guarded-fabric-initialize-hgs-key-mode.md). L'attestazione chiave host offre una garanzia simile alla modalità AD ed è più semplice da configurare. 

Usare la procedura seguente per configurare l'invio DNS e stabilire una relazione di trust unidirezionale con il dominio infrastruttura. Questi passaggi consentono al HGS di individuare i controller di dominio dell'infrastruttura e di convalidare l'appartenenza al gruppo degli host Hyper-V.

1.  Eseguire il comando seguente in una sessione di PowerShell con privilegi elevati per configurare l'invio DNS. Sostituire fabrikam.com con il nome del dominio dell'infrastruttura e digitare gli indirizzi IP dei server DNS nel dominio dell'infrastruttura. Per una disponibilità più elevata, puntare a più di un server DNS.

    ```powershell
    Add-DnsServerConditionalForwarderZone -Name "fabrikam.com" -ReplicationScope "Forest" -MasterServers <DNSserverAddress1>, <DNSserverAddress2>
    ```

2.  Per creare un trust tra foreste unidirezionale, eseguire il comando seguente in un prompt dei comandi con privilegi elevati:

    Sostituire `bastion.local` con il nome del dominio HGS e `fabrikam.com` con il nome del dominio dell'infrastruttura. Fornire la password per un amministratore del dominio dell'infrastruttura.

        netdom trust bastion.local /domain:fabrikam.com /userD:fabrikam.com\Administrator /passwordD:<password> /add

## <a name="next-step"></a>Passaggio successivo 

> [!div class="nextstepaction"]
> [Configurare l'HTTPS](guarded-fabric-configure-hgs-https.md)
