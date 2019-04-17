---
title: Imposta ambito di accesso per una zona DNS
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
ms.openlocfilehash: a4e84f249e57df6bfd04203c8b825ff5d4d643b2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="set-access-scope-for-a-dns-zone"></a>Imposta ambito di accesso per una zona DNS

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Per impostare l'ambito di accesso per una zona DNS tramite la console di gestione indirizzi IP client, è possibile utilizzare questo argomento.  
  
Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per eseguire questa procedura.  
  
### <a name="to-set-the-access-scope-for-a-dns-zone"></a>Per impostare l'ambito di accesso per una zona DNS  
  
1.  In Server Manager, fare clic su **gestione indirizzi IP**. Verrà visualizzata la console del client gestione indirizzi IP.  
  
2.  Nel riquadro di spostamento, fare clic su **zone DNS**. Nel riquadro informazioni, fare clic sulla zona DNS per il quale si desidera modificare l'ambito di accesso. e quindi fare clic su **Imposta ambito di accesso**.  
  
    ![Imposta ambito di accesso](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_02.jpg)  
  
3.  Il **Imposta ambito di accesso** apre la finestra di dialogo. Se richiesto per la distribuzione, fare clic per deselezionare **eredita ambito di accesso da padre**. In **selezionare l'ambito di accesso**, selezionare un elemento e quindi fare clic su **OK**.  
  
    ![Selezionare l'ambito di accesso](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_03.jpg)  
  
4.  Nel riquadro visualizzazione delle console client di gestione indirizzi IP, verificare che l'ambito di accesso per la zona viene modificato.  
  
    ![Verificare che l'ambito di accesso per la zona è stato modificato](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_04.jpg)  
  
## <a name="see-also"></a>Vedere anche  
[Controllo degli accessi in base al ruolo](Role-based-Access-Control.md)  
[Gestire gestione indirizzi IP](Manage-IPAM.md)  
  


