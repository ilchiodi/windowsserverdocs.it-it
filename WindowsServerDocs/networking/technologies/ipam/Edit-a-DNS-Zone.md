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
ms.openlocfilehash: 3e7cc75017c2b59293a042d4af0a677d3eda46c0
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="edit-a-dns-zone"></a>Modificare una zona DNS

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

You can use this topic to edit a DNS zone in the IPAM client console.  
  
Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per eseguire questa procedura.  
  
### <a name="to-edit-a-dns-zone"></a>To edit a DNS zone  
  
1.  In Server Manager, click **IPAM**. Verrà visualizzata la console del client gestione indirizzi IP.  
  
2.  Nel riquadro di spostamento, in **monitoraggio e gestione**, fare clic su **zone DNS**. Riquadro di spostamento divide in un riquadro di spostamento superiore e un riquadro di spostamento inferiore.  
  
3.  Nel riquadro di spostamento inferiore, effettuare una delle opzioni seguenti:  
  
    -   Ricerca diretta  
  
    -   Ricerca inversa IPv4  
  
    -   Ricerca inversa IPv6  
  
4.  For example, select IPv4 Reverse Lookup.  
  
    ![Select a zone type](../../media/Edit-a-DNS-Zone/ipam_EditZone_01.jpg)  
  
5.  In the display pane, right-click the zone that you want to edit, and then click **Edit DNS Zone**.  
  
    ![Edit DNS Zone](../../media/Edit-a-DNS-Zone/ipam_EditZone_02.jpg)  
  
6.  The **Edit DNS Zone** dialog box opens with the **General** page selected. If needed, edit the General zone properties: **DNS server**, **Zone category**, and **Zone type**, and then click **Apply** or, if your edits are complete, **OK**.  
  
    ![Edit zone properties and save](../../media/Edit-a-DNS-Zone/ipam_EditZone_03a.jpg)  
  
7.  In the **Edit DNS Zone** dialog box, click **Advanced**. The **Advanced** zone properties page opens. If needed, edit the properties that you want to change, and then click **Apply** or, if your edits are complete, **OK**.  
  
    ![Edit advanced zone properties](../../media/Edit-a-DNS-Zone/ipam_EditZone_04a.jpg)  
  
8.  If needed, select the additional zone properties page names (Name Servers, SOA, Zone Transfers), make your edits, and click **Apply** or **OK**. To review all of your zone edits, click **Summary**, and then click **OK**.  
  
## <a name="see-also"></a>Vedere anche  
[Gestire le Zone DNS](DNS-Zone-Management.md)  
[Gestire gestione indirizzi IP](Manage-IPAM.md)  
  


