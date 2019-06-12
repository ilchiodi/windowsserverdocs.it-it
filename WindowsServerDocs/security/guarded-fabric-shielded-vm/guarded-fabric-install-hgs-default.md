---
title: Installare servizio HGS in una nuova foresta
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 84f88d96f1e16767dec3b21b34aa226e544afaac
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447400"
---
# <a name="install-hgs-in-a-new-forest"></a>Installare servizio HGS in una nuova foresta 

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

## <a name="add-the-hgs-server-role"></a>Aggiungere il ruolo del server HGS

Eseguire i comandi seguenti in una sessione di PowerShell con privilegi elevata per aggiungere il ruolo del server HGS e installare HGS.

[!INCLUDE [Install the HGS server role](../../../includes/guarded-fabric-install-hgs-server-role.md)] 

## <a name="install-hgs"></a>Installare HGS 

[!INCLUDE [Install HGS by default](../../../includes/install-hgs-default.md)] 

## <a name="next-steps"></a>Passaggi successivi

- Per i passaggi successivi configurare l'attestazione basata su TPM, vedere [inizializzare il cluster HGS uso della modalità TPM in una nuova foresta dedicata (impostazione predefinita)](guarded-fabric-initialize-hgs-tpm-mode-default.md).
- Per i passaggi successivi configurare l'attestazione chiave host, vedere [inizializzare il cluster HGS uso della modalità chiave in una nuova foresta dedicata (impostazione predefinita)](guarded-fabric-initialize-hgs-key-mode-default.md).
- Per i prossimi passaggi per configurare l'attestazione basata su Admin (deprecata in Windows Server 2019), vedere [inizializzare il cluster HGS uso della modalità di AD in una nuova foresta dedicata (impostazione predefinita)](guarded-fabric-initialize-hgs-ad-mode-default.md).

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Inizializzare HGS](guarded-fabric-initialize-hgs.md)


