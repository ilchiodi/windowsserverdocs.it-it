---
title: Esaminare i prerequisiti HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: f4b4d1a8-bf6d-4881-9150-ddeca8b48038
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: dddf694aaceab93bd102456dbe86df17a001cb01
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879882"
---
# <a name="review-prerequisites-for-the-host-guardian-service"></a>Esaminare i prerequisiti per il servizio sorveglianza Host

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016


Questo argomento illustra i prerequisiti HGS e passaggi iniziali per preparare per la distribuzione del servizio HGS.

## <a name="prerequisites"></a>Prerequisiti 

-   **Hardware**: HGS può essere eseguito in macchine fisiche o virtuali, ma sono consigliate le macchine fisiche.

    Se si desidera eseguire HGS come un cluster fisico a tre nodi (per la disponibilità), sono necessari tre server fisici. (Come procedura consigliata per il clustering, i tre server deve avere hardware molto simili).
  
-   **Sistema operativo**: Attestazione chiave host richiede Windows Server 2019 Standard o Datacenter edition operare con il [attestazione v2](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md#versioned-attestation-policies). Per l'attestazione basata su TPM, è possibile eseguire HGS 2019 Server Windows o Windows Server 2016, Standard o Datacenter edition.

-   **I ruoli del server**: Servizio sorveglianza host e i ruoli del server di supporto.

-   **Configurazione delle autorizzazioni/privilegi per il dominio dell'infrastruttura (host)**: È necessario configurare l'inoltro di DNS tra il dominio dell'infrastruttura (host) e il servizio HGS. 
    
## <a name="upgrading-hgs"></a>L'aggiornamento di HGS

Se già stato distribuito HGS e si vuole eseguire l'aggiornamento del sistema operativo, seguire le [aggiornamento indicazioni](guarded-fabric-upgrade-to-2019.md) ad aggiornare i server HGS e Hyper-V per il sistema operativo più recente.

## <a name="next-step"></a>Passaggio successivo

>[!div class="nextstepaction"]
[Ottenere i certificati per HGS](guarded-fabric-obtain-certs.md)