---
title: Proprietà avanzate NIC
description: È possibile gestire le schede di rete e tutte le funzionalità tramite Windows PowerShell o il pannello di controllo di rete.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/20/2018
ms.openlocfilehash: 78d320f4309d60fa0396cbd723feafa07a65aea1
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317006"
---
# <a name="nic-advanced-properties"></a>Proprietà avanzate NIC

È possibile gestire le schede di rete e tutte le funzionalità tramite Windows PowerShell usando il cmdlet [NetAdapter](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps&viewFallbackFrom=winserverr2-ps) .  È anche possibile gestire le schede di rete e tutte le funzionalità usando il pannello di controllo della rete (ncpa. cpl). 

1. In **Windows PowerShell**eseguire il cmdlet `Get‑NetAdapterAdvancedProperties` su due modelli di schede di rete diversi.

   ![Get-NetAdapterAdvancedProperty M1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-m1.png)

   ![Get-NetAdapterAdvancedProperty C1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-c1.png)

   Sono presenti analogie e differenze in questi due elenchi di proprietà avanzate NIC.

2. Nel **Pannello di controllo della rete** (ncpa. cpl) eseguire le operazioni seguenti:

   a. Fare clic con il pulsante destro del mouse sulla scheda NIC.

   ![Finestra di dialogo connessioni di rete](../../media/network-offload-and-optimization/network-connections-dialog.png)

   b. Nella finestra di dialogo Proprietà fare clic su **Configura**.

    ![Proprietà C1](../../media/network-offload-and-optimization/c1-properties.png)

   c. Fare clic sulla scheda **Avanzate** per visualizzare le proprietà avanzate.<p>Gli elementi in questo elenco sono correlati agli elementi nell'output del `Get-NetAdapterAdvancedProperties`.

   ![Proprietà della scheda di rete Chelsio](../../media/network-offload-and-optimization/chelsio-network-adapter-properties.png)

---
