---
title: Ripulire i dati di utilizzo
description: Questo argomento fa parte della Guida alla gestione di gestione indirizzi IP in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ipam
ms.topic: article
ms.assetid: 45cada9e-69b9-43df-b6f5-6d3942435809
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a4454131c6a73626b0668b3ca3ab4eefb3e3334f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860624"
---
# <a name="purge-utilization-data"></a>Ripulire i dati di utilizzo

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile usare questo argomento per informazioni su come eliminare i dati di utilizzo dal database di gestione indirizzi IP.  

Per eseguire questa procedura, è necessario essere un membro di **amministratori**di gestione indirizzi IP, del gruppo **Administrators** del computer locale o di un gruppo equivalente.

## <a name="to-purge-the-ipam-database"></a>Per eliminare il database di gestione indirizzi IP  
1. Aprire Server Manager, quindi passare all'interfaccia del client di gestione indirizzi IP.
2. Passare a una delle seguenti posizioni: **blocchi indirizzi IP**, **inventario indirizzi IP**o gruppi di **intervalli di indirizzi IP**.  
3. Fare clic su **attività**e quindi fare clic su **Ripulisci dati di utilizzo**. Verrà visualizzata la finestra di dialogo **Elimina dati di utilizzo** .
4. In **Ripulisci tutti i dati di utilizzo in o prima**fare clic su **Seleziona una data**.
5. Scegliere la data per la quale si desidera eliminare tutti i record del database sia in data che prima di tale data.
6. Fare clic su **OK**. Gestione indirizzi IP elimina tutti i record specificati.
