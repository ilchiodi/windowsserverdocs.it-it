---
title: Impostare l'ambito di accesso per i record delle risorse DNS
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
ms.assetid: a96a8752-5678-49c5-b069-d2cce8042a51
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2e63c82ef0c58a9b4392ad8b9b1fc896d075ab71
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882912"
---
# <a name="set-access-scope-for-dns-resource-records"></a>Impostare l'ambito di accesso per i record delle risorse DNS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per impostare l'ambito di accesso per un record di risorse utilizzando la console di gestione indirizzi IP client.  
  
Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente.  
  
### <a name="to-set-access-scope-for-dns-resource-records"></a>Per impostare l'ambito di accesso per i record risorsa DNS  
  
1.  In Server Manager, fare clic su  **Gestione indirizzi IP**. Verrà visualizzata la console di gestione indirizzi IP client.  
  
2.  Nel riquadro di spostamento, fare clic su **zone DNS**.  Nel riquadro di spostamento inferiore, espandere **ricerca diretta** e individuare e selezionare l'area che contiene i record di risorse il cui ambito di accesso che si desidera modificare.  
  
3.  Nel riquadro di visualizzazione, individuare e selezionare i record di risorse il cui ambito di accesso che si desidera modificare.  
  
    ![Selezionare i record di risorse](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_02.jpg)  
  
4.  Fare doppio clic sui record risorsa DNS selezionati e quindi fare clic su **Imposta ambito di accesso**.  
  
    ![Imposta ambito di accesso](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_03.jpg)  
  
5.  Il **Imposta ambito di accesso** verrà visualizzata la finestra di dialogo. Se necessario per la distribuzione, fare clic per deselezionare **eredita ambito di accesso da padre**. Nelle **selezionare l'ambito di accesso**, selezionare un elemento e quindi fare clic su **OK**.  
  
    ![Selezionare l'ambito di accesso](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_04.jpg)  
  
## <a name="see-also"></a>Vedere anche  
[Controllo degli accessi in base al ruolo](Role-based-Access-Control.md)  
[Gestire gestione indirizzi IP](Manage-IPAM.md)  
  


