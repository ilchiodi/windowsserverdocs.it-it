---
title: Proprietà avanzate NIC
description: È possibile gestire interfacce di rete e tutte le funzionalità con Windows PowerShell o il pannello di controllo di rete.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/20/2018
ms.openlocfilehash: d1a5fb57bf71fd981e001cfd9ac595ab5bc3cfc5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819902"
---
# <a name="nic-advanced-properties"></a>Proprietà avanzate NIC

È possibile gestire interfacce di rete e tutte le funzionalità tramite Windows PowerShell usando il [NetAdapter](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps&viewFallbackFrom=winserverr2-ps) cmdlet.  È anche possibile gestire interfacce di rete e tutte le funzionalità usando il pannello di controllo di rete (ncpa. cpl). 

1. Nelle **Windows PowerShell**, eseguire il `Get‑NetAdapterAdvancedProperties` cmdlet su due diversi marca o modello di schede di rete.

   ![Get-NetAdapterAdvancedProperty m1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-m1.png)

   ![Get-NetAdapterAdvancedProperty c1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-c1.png)

   Esistono analogie e differenze in questi due NIC avanzata delle proprietà sono elencate.

2. Nel **Pannello di controllo rete** (ncpa. cpl), eseguire le operazioni seguenti:

   a. e fare doppio clic su interfaccia di rete.

   ![Finestra di dialogo connessioni di rete](../../media/network-offload-and-optimization/network-connections-dialog.png)

   b. Nella finestra di dialogo proprietà, fare clic su **configura**.

    ![Proprietà C1](../../media/network-offload-and-optimization/c1-properties.png)

   c. Scegliere il **avanzate** scheda per visualizzare le proprietà avanzate.<p>Mette in correlazione gli elementi in questo elenco per gli elementi nel `Get-NetAdapterAdvancedProperties` output.

   ![Proprietà scheda di rete Chelsio](../../media/network-offload-and-optimization/chelsio-network-adapter-properties.png)

---