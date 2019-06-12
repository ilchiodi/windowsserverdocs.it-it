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
ms.openlocfilehash: 954393126333bf04d2aa46a01089d88bc91151cb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447318"
---
# <a name="verify-the-hgs-configuration"></a>Verificare la configurazione HGS

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016


Successivamente, è necessario verificare che tutto funzioni come previsto. A tale scopo, eseguire il comando seguente in una console di Windows PowerShell con privilegi elevata:

```powershell
Get-HgsTrace -RunDiagnostics
```

Poiché la configurazione del servizio HGS non contiene ancora le informazioni sugli host che si trova l'infrastruttura sorvegliata, i dati di diagnostica indica che nessun host saranno in grado di attestare correttamente ancora. Ignorare questo risultato ed esaminare le altre informazioni fornite dalla diagnostica.

[!INCLUDE [Guarded fabric diagnostics tool](../../../includes/guarded-fabric-diagnostics-tool.md)] 

<!-- When a link is available for an updated troubleshooting guide, add a sentence like the following and create a link to the troubleshooting guide:
If failures did occur, please review the remediation steps provided or see the Troubleshooting Guide.
-->

Eseguire la diagnostica in ogni nodo del cluster del servizio HGS.

## <a name="next-step"></a>Passaggio successivo

> [!div class="nextstepaction"]
> [Distribuire host protetti](guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)

