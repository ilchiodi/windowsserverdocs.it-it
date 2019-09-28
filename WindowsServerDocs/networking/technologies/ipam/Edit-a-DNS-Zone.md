---
title: Modificare una zona DNS
description: Questo argomento fa parte della Guida alla gestione di gestione indirizzi IP in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a35164e1-11ad-47c8-9843-580d30c70d07
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2175cf9c740d7b727ba017922a77c94d4379c891
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355282"
---
# <a name="edit-a-dns-zone"></a>Modificare una zona DNS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile usare questo argomento per modificare una zona DNS nella console del client di gestione indirizzi IP.  
  
Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente.  
  
### <a name="to-edit-a-dns-zone"></a>Per modificare una zona DNS  
  
1.  In Server Manager **fare clic su**gestione indirizzi IP. Verrà visualizzata la console di gestione indirizzi IP client.  
  
2.  Nel riquadro di spostamento, in **MONITORAGGIO e GESTIONE**, fare clic su **zone DNS**. Riquadro di spostamento viene suddivisa in un riquadro di spostamento superiore e un riquadro di navigazione inferiore.  
  
3.  Nel riquadro di spostamento inferiore effettuare una delle selezioni seguenti:  
  
    -   Ricerca diretta  
  
    -   Ricerca inversa IPv4  
  
    -   Ricerca inversa IPv6  
  
4.  Selezionare, ad esempio, ricerca inversa IPv4.  
  
    ![Selezionare un tipo di zona](../../media/Edit-a-DNS-Zone/ipam_EditZone_01.jpg)  
  
5.  Nel riquadro informazioni fare clic con il pulsante destro del mouse sulla zona che si desidera modificare, quindi scegliere **modifica zona DNS**.  
  
    ![Modificare la zona DNS](../../media/Edit-a-DNS-Zone/ipam_EditZone_02.jpg)  
  
6.  Verrà visualizzata la finestra di dialogo **modifica zona DNS** con la pagina **generale** selezionata. Se necessario, modificare le proprietà generali della zona: **Server DNS**, **categoria zona**e **tipo di zona**, quindi fare clic su **applica** o, se le modifiche sono state completate, **OK**.  
  
    ![Modifica proprietà zona e Salva](../../media/Edit-a-DNS-Zone/ipam_EditZone_03a.jpg)  
  
7.  Nella finestra di dialogo **modifica zona DNS** fare clic su **Avanzate**. Verrà visualizzata la pagina Proprietà zona **avanzata** . Se necessario, modificare le proprietà che si desidera modificare, quindi fare clic su **applica** o, se le modifiche sono state completate, **OK**.  
  
    ![Modificare le proprietà di zona avanzate](../../media/Edit-a-DNS-Zone/ipam_EditZone_04a.jpg)  
  
8.  Se necessario, selezionare i nomi di pagina delle proprietà della zona aggiuntiva (server dei nomi, SOA, trasferimenti di zona), apportare le modifiche e fare clic su **applica** o **OK**. Per esaminare tutte le modifiche alle zone, fare clic su **Riepilogo**, quindi su **OK**.  
  
## <a name="see-also"></a>Vedere anche  
[Gestione delle zone DNS](DNS-Zone-Management.md)  
[Gestire Gestione indirizzi IP](Manage-IPAM.md)  
  


