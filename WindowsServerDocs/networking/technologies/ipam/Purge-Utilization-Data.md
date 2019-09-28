---
title: Ripulire i dati di utilizzo
description: Questo argomento fa parte della Guida alla gestione di gestione indirizzi IP in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 45cada9e-69b9-43df-b6f5-6d3942435809
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9e1db31e4d2d714c358f2a67c2165aef91b314ba
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405608"
---
# <a name="purge-utilization-data"></a>Ripulire i dati di utilizzo

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile usare questo argomento per informazioni su come eliminare i dati di utilizzo dal database di gestione indirizzi IP.  

Per eseguire questa procedura, è necessario essere un membro di **amministratori**di gestione indirizzi IP, del gruppo **Administrators** del computer locale o di un gruppo equivalente.

## <a name="to-purge-the-ipam-database"></a>Per eliminare il database di gestione indirizzi IP  
1. Aprire Server Manager, quindi passare all'interfaccia del client di gestione indirizzi IP.
2. Passare a una delle seguenti posizioni: **Blocchi di indirizzi IP**, **inventario degli indirizzi IP**o **gruppi di intervalli di indirizzi IP**.  
3. Fare clic su **attività**e quindi fare clic su **Ripulisci dati di utilizzo**. Verrà visualizzata la finestra di dialogo **Elimina dati di utilizzo** .
4. In **Ripulisci tutti i dati di utilizzo in o prima**fare clic su **Seleziona una data**.
5. Scegliere la data per la quale si desidera eliminare tutti i record del database sia in data che prima di tale data.
6. Fare clic su **OK**. Gestione indirizzi IP elimina tutti i record specificati.
