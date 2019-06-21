---
title: Eliminare i record di risorse DNS
description: Questo argomento fa parte della Guida di gestione di gestione indirizzi IP (IPAM) in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 366e6fd5-d563-4de3-9551-5614cbb8f2cb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ecbe5dcc452aa39a9afca7e1c8d5fe70d8d4528d
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283974"
---
# <a name="delete-dns-resource-records"></a>Eliminare i record di risorse DNS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per eliminare uno o più record di risorse utilizzando la console di gestione indirizzi IP client.  
  
Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente.  
  
### <a name="to-delete-dns-resource-records"></a>Per eliminare i record risorsa DNS  
  
1.  In Server Manager, fare clic su  **Gestione indirizzi IP**. Verrà visualizzata la console di gestione indirizzi IP client.  
  
2.  Nel riquadro di spostamento, in **MONITORAGGIO e GESTIONE**, fare clic su **zone DNS**.  Riquadro di spostamento viene suddivisa in un riquadro di spostamento superiore e un riquadro di navigazione inferiore.  
  
3.  Fare clic per espandere **ricerca diretta** e il dominio in cui si trovano i record di zona e risorse che si desidera eliminare. Fare clic sulla zona e nel riquadro di visualizzazione, fare clic su **vista corrente**. Fare clic su **record di risorse**.  
  
4.  Nel riquadro di visualizzazione, individuare e selezionare i record di risorse che si desidera eliminare.  
  
    ![Selezionare i record di risorse da eliminare](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_01.jpg)  
  
5.  Fare doppio clic sui record selezionati e quindi fare clic su **record di risorse DNS Elimina**.  
  
    ![Eliminare i record](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_02.jpg)  
  
6.  Il **Elimina Record risorsa DNS** verrà visualizzata la finestra di dialogo. Verificare che sia selezionato il server DNS corretto. In caso contrario, fare clic su **server DNS** e selezionare il server da cui si desidera eliminare i record di risorse. Fare clic su **OK**. Gestione indirizzi IP consente di eliminare i record di risorse dal server DNS.  
  
    ![Verificare che sia selezionato il server DNS corretto ed eliminare i record](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_03.jpg)  
  
## <a name="see-also"></a>Vedere anche  
[Gestione dei Record risorse DNS](DNS-Resource-Record-Management.md)  
[Gestire Gestione indirizzi IP](Manage-IPAM.md)  
  


