---
title: Elimina dati di utilizzo
description: Questo argomento fa parte della Guida di gestione di gestione indirizzi IP (IPAM) in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 45cada9e-69b9-43df-b6f5-6d3942435809
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c41be119099aed4867df1bae1a55e2fbaa5c9064
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="purge-utilization-data"></a>Elimina dati di utilizzo

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come eliminare i dati di utilizzo dal database di gestione indirizzi IP.  

È necessario essere un membro del **amministratori gestione indirizzi IP**, il computer locale **amministratori** o gruppo equivalente, per eseguire questa procedura.

## <a name="to-purge-the-ipam-database"></a>Per eliminare il database di gestione indirizzi IP  
1. Aprire Server Manager e quindi selezionare l'interfaccia del client gestione indirizzi IP.
2. Passare a una delle seguenti posizioni: **blocchi di indirizzi IP**, **inventario degli indirizzi IP**, o **gruppi di intervalli di indirizzi IP**.  
3. Fare clic su **attività**, quindi fare clic su **Ripulisci dati di utilizzo**. Il **Ripulisci dati di utilizzo** apre la finestra di dialogo.
4. In **ripulire l'utilizzo di tutti i dati oppure prima**, fare clic su **selezionare una data**.
5. Scegliere la data per il quale si desidera eliminare tutti i record di database sia in che prima di tale data.
6. Fare clic su **OK**. Gestione indirizzi IP consente di eliminare tutti i record che è stato specificato.
