---
title: Modificare una zona DNS
description: Questo argomento fa parte della Guida alla gestione di gestione indirizzi IP in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ipam
ms.topic: article
ms.assetid: a35164e1-11ad-47c8-9843-580d30c70d07
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 916e745082db64cf548e4b9650ee1f0ec5ba3c24
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860694"
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
  
6.  Verrà visualizzata la finestra di dialogo **modifica zona DNS** con la pagina **generale** selezionata. Se necessario, modificare le proprietà generali della zona: **server DNS**, **categoria zona**e **tipo di zona**, quindi fare clic su **applica** o, se le modifiche sono state completate, **OK**.  
  
    ![Modifica proprietà zona e Salva](../../media/Edit-a-DNS-Zone/ipam_EditZone_03a.jpg)  
  
7.  Nella finestra di dialogo **modifica zona DNS** fare clic su **Avanzate**. Verrà visualizzata la pagina Proprietà zona **avanzata** . Se necessario, modificare le proprietà che si desidera modificare, quindi fare clic su **applica** o, se le modifiche sono state completate, **OK**.  
  
    ![Modificare le proprietà di zona avanzate](../../media/Edit-a-DNS-Zone/ipam_EditZone_04a.jpg)  
  
8.  Se necessario, selezionare i nomi di pagina delle proprietà della zona aggiuntiva (server dei nomi, SOA, trasferimenti di zona), apportare le modifiche e fare clic su **applica** o **OK**. Per esaminare tutte le modifiche alle zone, fare clic su **Riepilogo**, quindi su **OK**.  
  
## <a name="see-also"></a>Vedi anche  
[Gestione delle zone DNS](DNS-Zone-Management.md)  
[Gestire Gestione indirizzi IP](Manage-IPAM.md)  
  


