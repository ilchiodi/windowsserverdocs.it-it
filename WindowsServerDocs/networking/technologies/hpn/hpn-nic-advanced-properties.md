---
title: Proprietà avanzate NIC
description: È possibile gestire le schede di rete e tutte le funzionalità tramite Windows PowerShell o il pannello di controllo di rete.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/20/2018
ms.openlocfilehash: 1395cefca5d9ef696eed3f2735334954b9ee02a5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405715"
---
# <a name="nic-advanced-properties"></a>Proprietà avanzate NIC

È possibile gestire le schede di rete e tutte le funzionalità tramite Windows PowerShell usando il cmdlet [NetAdapter](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps&viewFallbackFrom=winserverr2-ps) .  È anche possibile gestire le schede di rete e tutte le funzionalità usando il pannello di controllo della rete (ncpa. cpl). 

1. In **Windows PowerShell**eseguire il cmdlet `Get‑NetAdapterAdvancedProperties` su due diverse marca/modello di schede di rete.

   ![Get-NetAdapterAdvancedProperty M1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-m1.png)

   ![Get-NetAdapterAdvancedProperty C1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-c1.png)

   Sono presenti analogie e differenze in questi due elenchi di proprietà avanzate NIC.

2. Nel **Pannello di controllo della rete** (ncpa. cpl) eseguire le operazioni seguenti:

   a. Fare clic con il pulsante destro del mouse sulla scheda NIC.

   ![Finestra di dialogo connessioni di rete](../../media/network-offload-and-optimization/network-connections-dialog.png)

   b. Nella finestra di dialogo Proprietà fare clic su **Configura**.

    ![Proprietà C1](../../media/network-offload-and-optimization/c1-properties.png)

   c. Fare clic sulla scheda **Avanzate** per visualizzare le proprietà avanzate.<p>Gli elementi in questo elenco sono correlati agli elementi nell'output `Get-NetAdapterAdvancedProperties`.

   ![Proprietà della scheda di rete Chelsio](../../media/network-offload-and-optimization/chelsio-network-adapter-properties.png)

---
