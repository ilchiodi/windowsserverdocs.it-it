---
title: Impostare l'ambito di accesso per una zona DNS
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
ms.assetid: 6a211dde-80eb-4888-b5bb-4e28fe8dc7df
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 73d96a9be7c13afbe4c96d46fffefc0096046c8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813982"
---
# <a name="set-access-scope-for-a-dns-zone"></a>Impostare l'ambito di accesso per una zona DNS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per impostare l'ambito di accesso per una zona DNS usando la console di gestione indirizzi IP client.  
  
Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente.  
  
### <a name="to-set-the-access-scope-for-a-dns-zone"></a>Per impostare l'ambito di accesso per una zona DNS  
  
1.  In Server Manager, fare clic su  **Gestione indirizzi IP**. Verrà visualizzata la console di gestione indirizzi IP client.  
  
2.  Nel riquadro di spostamento, fare clic su **zone DNS**. Nel riquadro di visualizzazione, pulsante destro del mouse la zona DNS per il quale si desidera modificare l'ambito di accesso. e quindi fare clic su **Imposta ambito di accesso**.  
  
    ![Imposta ambito di accesso](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_02.jpg)  
  
3.  Il **Imposta ambito di accesso** verrà visualizzata la finestra di dialogo. Se necessario per la distribuzione, fare clic per deselezionare **eredita ambito di accesso da padre**. Nelle **selezionare l'ambito di accesso**, selezionare un elemento e quindi fare clic su **OK**.  
  
    ![Selezionare l'ambito di accesso](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_03.jpg)  
  
4.  Nel riquadro di visualizzazione IPAM client console verificare che l'ambito di accesso per la zona è stato modificato.  
  
    ![Verificare che l'ambito di accesso per la zona è stato modificato](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_04.jpg)  
  
## <a name="see-also"></a>Vedere anche  
[Controllo degli accessi in base al ruolo](Role-based-Access-Control.md)  
[Gestire gestione indirizzi IP](Manage-IPAM.md)  
  


