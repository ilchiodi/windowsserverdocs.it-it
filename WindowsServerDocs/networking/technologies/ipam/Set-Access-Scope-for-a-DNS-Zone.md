---
title: Impostare l'ambito di accesso per una zona DNS
description: Questo argomento fa parte della Guida alla gestione di gestione indirizzi IP in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a211dde-80eb-4888-b5bb-4e28fe8dc7df
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 870acde822fb5c8c038139710facb71208df3387
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309560"
---
# <a name="set-access-scope-for-a-dns-zone"></a>Impostare l'ambito di accesso per una zona DNS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile usare questo argomento per impostare l'ambito di accesso per una zona DNS usando la console client di gestione indirizzi IP.  
  
Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente.  
  
### <a name="to-set-the-access-scope-for-a-dns-zone"></a>Per impostare l'ambito di accesso per una zona DNS  
  
1.  In Server Manager, fare clic su  **Gestione indirizzi IP**. Verrà visualizzata la console di gestione indirizzi IP client.  
  
2.  Nel riquadro di spostamento fare clic su **zone DNS**. Nel riquadro informazioni fare clic con il pulsante destro del mouse sulla zona DNS per la quale si desidera modificare l'ambito di accesso, quindi scegliere **Imposta ambito di accesso**.  
  
    ![Imposta ambito di accesso](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_02.jpg)  
  
3.  Verrà visualizzata la finestra di dialogo **Imposta ambito di accesso** . Se necessario per la distribuzione, fare clic per deselezionare **eredita ambito di accesso da padre**. In **selezionare l'ambito di accesso**selezionare un elemento e quindi fare clic su **OK**.  
  
    ![Selezionare l'ambito di accesso](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_03.jpg)  
  
4.  Nel riquadro di visualizzazione della console client di gestione indirizzi IP, verificare che l'ambito di accesso per la zona sia stato modificato.  
  
    ![Verificare che l'ambito di accesso per la zona sia stato modificato](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_04.jpg)  
  
## <a name="see-also"></a>Vedi anche  
[Controllo degli accessi in base al ruolo](Role-based-Access-Control.md)  
[Gestire Gestione indirizzi IP](Manage-IPAM.md)  
  


