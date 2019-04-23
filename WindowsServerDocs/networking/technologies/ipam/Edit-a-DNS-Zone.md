---
title: Modificare una zona DNS
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
ms.assetid: a35164e1-11ad-47c8-9843-580d30c70d07
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b632203289c3affd16735026e0c553be09c5e9e6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847442"
---
# <a name="edit-a-dns-zone"></a>Modificare una zona DNS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per modificare una zona DNS nella console di gestione indirizzi IP client.  
  
Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente.  
  
### <a name="to-edit-a-dns-zone"></a>Per modificare una zona DNS  
  
1.  In Server Manager fare clic su **IPAM**. Verrà visualizzata la console di gestione indirizzi IP client.  
  
2.  Nel riquadro di spostamento, in **MONITORAGGIO e GESTIONE**, fare clic su **zone DNS**. Riquadro di spostamento viene suddivisa in un riquadro di spostamento superiore e un riquadro di navigazione inferiore.  
  
3.  Nel riquadro di spostamento inferiore, apportare una delle seguenti selezioni:  
  
    -   Ricerca diretta  
  
    -   Ricerca inversa IPv4  
  
    -   Ricerca inversa IPv6  
  
4.  Ad esempio, selezionare una ricerca inversa IPv4.  
  
    ![Selezionare un tipo di zona](../../media/Edit-a-DNS-Zone/ipam_EditZone_01.jpg)  
  
5.  Nel riquadro di visualizzazione, pulsante destro del mouse la zona che si desidera modificare e quindi fare clic su **zona di DNS modifica**.  
  
    ![Zona di DNS di modifica](../../media/Edit-a-DNS-Zone/ipam_EditZone_02.jpg)  
  
6.  Il **zona di DNS di modifica** verrà visualizzata la finestra di dialogo con la **generali** pagina selezionata. Se necessario, modificare le proprietà generali della zona: **Server DNS**, **categoria zona**, e **tipo di zona**, quindi fare clic su **applica** oppure, se sono state completate, tutte le modifiche **OK**.  
  
    ![Modificare le proprietà di zona e salvare](../../media/Edit-a-DNS-Zone/ipam_EditZone_03a.jpg)  
  
7.  Nel **zona di DNS di modifica** finestra di dialogo, fare clic su **avanzate**. Il **avanzate** verrà visualizzata la pagina delle proprietà delle zone. Se necessario, modificare le proprietà che si desidera modificare e quindi fare clic su **Apply** oppure, se sono state completate, tutte le modifiche **OK**.  
  
    ![Modificare le proprietà avanzate di zona](../../media/Edit-a-DNS-Zone/ipam_EditZone_04a.jpg)  
  
8.  Se necessario, selezionare i nomi di pagina delle proprietà di zona aggiuntive (server dei nomi, SOA, i trasferimenti di zona), apportare le modifiche desiderate e fare clic su **Apply** oppure **OK**. Per esaminare tutte le modifiche desiderate fuso, fare clic su **Summary**, quindi fare clic su **OK**.  
  
## <a name="see-also"></a>Vedere anche  
[Gestire le Zone DNS](DNS-Zone-Management.md)  
[Gestire gestione indirizzi IP](Manage-IPAM.md)  
  


