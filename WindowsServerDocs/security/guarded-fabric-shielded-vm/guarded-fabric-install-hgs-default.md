---
title: Installare HGS in una nuova foresta
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 8f896b0cea49f9dd26a828a2580b59a78348763a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856604"
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


