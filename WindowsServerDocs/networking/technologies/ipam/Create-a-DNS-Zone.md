---
title: Creare una zona DNS
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
ms.assetid: a030ff51-a815-4fc4-b26d-aae41c3e4ce5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e532837e6c98694fa040a6d47a8e536eecb4c3da
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-dns-zone"></a>Creare una zona DNS

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per creare una zona DNS tramite la console client di gestione indirizzi IP.  
  
Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per eseguire questa procedura.  
  
### <a name="to-create-a-dns-zone"></a>Per creare una zona DNS  
  
1.  In Server Manager, fare clic su **gestione indirizzi IP**. Verrà visualizzata la console del client gestione indirizzi IP.  
  
2.  Nel riquadro di spostamento, in **monitoraggio e gestione**, fare clic su **server DNS e DHCP **. Nel riquadro informazioni fare clic su **tipo Server**, quindi fare clic su **DNS **. Nei risultati della ricerca sono elencati tutti i server DNS gestiti da Gestione indirizzi IP.  
  
3.  Individuare il server in cui si desidera aggiungere una zona e fare doppio clic su server.  Fare clic su **zona DNS creare **.  
  
    ![Creare una zona DNS](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_01a.jpg)  
  
4.  Il **zona DNS crea** apre la finestra di dialogo. In **proprietà generali**, selezionare una categoria di zona, un tipo di zona e immettere un nome in **nome zona **. Selezionare anche i valori appropriati per la distribuzione in **proprietà avanzate**, quindi fare clic su **OK **.  
  
    ![Proprietà avanzate](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_02a.jpg)  
  
## <a name="see-also"></a>Vedere anche  
[Gestire le Zone DNS](DNS-Zone-Management.md)  
[Gestire gestione indirizzi IP](Manage-IPAM.md)  
  


