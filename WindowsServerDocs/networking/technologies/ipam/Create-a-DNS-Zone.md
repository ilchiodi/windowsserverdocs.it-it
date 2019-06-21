---
title: Creare una zona DNS
description: Questo argomento fa parte della Guida di gestione di gestione indirizzi IP (IPAM) in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a030ff51-a815-4fc4-b26d-aae41c3e4ce5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 83e3865308fd45e88b800753b20ab298f9a14c96
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282248"
---
# <a name="create-a-dns-zone"></a>Creare una zona DNS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per creare una zona DNS usando la console di gestione indirizzi IP client.  
  
Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente.  
  
### <a name="to-create-a-dns-zone"></a>Per creare una zona DNS  
  
1.  In Server Manager, fare clic su  **Gestione indirizzi IP**. Verrà visualizzata la console di gestione indirizzi IP client.  
  
2.  Nel riquadro di spostamento, in **monitoraggio e gestione**, fare clic su **server DNS e DHCP**. Nel riquadro informazioni fare clic su **tipo di Server**, quindi fare clic su **DNS**. Nei risultati della ricerca sono elencati tutti i server DNS gestiti da Gestione indirizzi IP.  
  
3.  Individuare il server in cui si desidera aggiungere una zona e fare doppio clic su server.  Fare clic su **Crea zona DNS**.  
  
    ![Crea zona DNS](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_01a.jpg)  
  
4.  Il **Crea zona DNS** verrà visualizzata la finestra di dialogo. Nelle **delle proprietà generali**, selezionare una categoria di zona, un tipo di zona e immettere un nome nella **nome zona**. Selezionare anche i valori appropriati per la distribuzione nella **delle proprietà avanzate**, quindi fare clic su **OK**.  
  
    ![Proprietà avanzate](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_02a.jpg)  
  
## <a name="see-also"></a>Vedere anche  
[Gestire le Zone DNS](DNS-Zone-Management.md)  
[Gestire Gestione indirizzi IP](Manage-IPAM.md)  
  


