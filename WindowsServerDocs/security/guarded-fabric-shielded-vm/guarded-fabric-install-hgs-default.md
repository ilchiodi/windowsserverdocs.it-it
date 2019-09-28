---
title: Installare HGS in una nuova foresta
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 6dfbe24fb4d9011b48f366d7e5df92fdb80685d5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386587"
---
# <a name="install-hgs-in-a-new-forest"></a>Installare HGS in una nuova foresta 

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016

## <a name="add-the-hgs-server-role"></a>Aggiungere il ruolo del server HGS

Eseguire i comandi seguenti in una sessione di PowerShell con privilegi elevati per aggiungere il ruolo del server HGS e installare HGS.

[!INCLUDE [Install the HGS server role](../../../includes/guarded-fabric-install-hgs-server-role.md)] 

## <a name="install-hgs"></a>Installare HGS 

[!INCLUDE [Install HGS by default](../../../includes/install-hgs-default.md)] 

## <a name="next-steps"></a>Passaggi successivi

- Per i passaggi successivi per la configurazione dell'attestazione basata su TPM, vedere [inizializzare il cluster HGS con la modalità TPM in una nuova foresta dedicata (impostazione predefinita)](guarded-fabric-initialize-hgs-tpm-mode-default.md).
- Per i passaggi successivi per la configurazione dell'attestazione della chiave host, vedere [inizializzare il cluster HGS usando la modalità chiave in una nuova foresta dedicata (impostazione predefinita)](guarded-fabric-initialize-hgs-key-mode-default.md).
- Per i passaggi successivi per la configurazione dell'attestazione basata sull'amministratore (deprecata in Windows Server 2019), vedere [inizializzare il cluster HGS usando la modalità ad in una nuova foresta dedicata (impostazione predefinita)](guarded-fabric-initialize-hgs-ad-mode-default.md).

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Inizializzare HGS](guarded-fabric-initialize-hgs.md)


