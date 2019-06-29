---
title: Verificare la configurazione HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 8f01df37-f18e-4386-ae73-ecf84feaa9df
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 8098edd1eea475cea1face5541459b262364a07b
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469548"
---
# <a name="verify-the-hgs-configuration"></a>Verificare la configurazione HGS

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016


Successivamente, è necessario verificare che tutto funzioni come previsto. A tale scopo, eseguire il comando seguente in una console di Windows PowerShell con privilegi elevata:

```powershell
Get-HgsTrace -RunDiagnostics
```

Poiché la configurazione del servizio HGS non contiene ancora le informazioni sugli host che si trova l'infrastruttura sorvegliata, i dati di diagnostica indica che nessun host saranno in grado di attestare correttamente ancora. Ignorare questo risultato ed esaminare le altre informazioni fornite dalla diagnostica.

[!INCLUDE [Guarded fabric diagnostics tool](../../../includes/guarded-fabric-diagnostics-tool.md)] 

Eseguire la diagnostica in ogni nodo del cluster del servizio HGS.

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Distribuire host protetti](guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)

