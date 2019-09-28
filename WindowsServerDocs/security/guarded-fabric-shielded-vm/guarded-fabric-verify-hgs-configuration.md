---
title: Verificare la configurazione HGS
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 8f01df37-f18e-4386-ae73-ecf84feaa9df
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: d219b7aa9ca1e17df3281fd756106a6f07864116
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386354"
---
# <a name="verify-the-hgs-configuration"></a>Verificare la configurazione HGS

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016


Successivamente, è necessario verificare che funzioni come previsto. A tale scopo, eseguire il comando seguente in una console di Windows PowerShell con privilegi elevati:

```powershell
Get-HgsTrace -RunDiagnostics
```

Poiché la configurazione di HGS non contiene ancora informazioni sugli host che si troveranno nell'infrastruttura sorvegliata, la diagnostica indicherà che nessun host sarà ancora in grado di attestare correttamente. Ignorare questo risultato ed esaminare le altre informazioni fornite dalla diagnostica.

[!INCLUDE [Guarded fabric diagnostics tool](../../../includes/guarded-fabric-diagnostics-tool.md)] 

Eseguire la diagnostica in ogni nodo del cluster HGS.

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Distribuire host protetti](guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)

