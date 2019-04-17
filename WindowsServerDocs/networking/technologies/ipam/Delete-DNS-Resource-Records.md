---
title: Eliminare i record di risorse DNS
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
ms.assetid: 366e6fd5-d563-4de3-9551-5614cbb8f2cb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f9ebfeca1da9e36cd00272113f2e86c33174074b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="delete-dns-resource-records"></a>Eliminare i record di risorse DNS

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per eliminare uno o più record di risorse DNS tramite la console del client gestione indirizzi IP.  
  
Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per eseguire questa procedura.  
  
### <a name="to-delete-dns-resource-records"></a>Per eliminare i record di risorse DNS  
  
1.  In Server Manager, fare clic su **gestione indirizzi IP**. Verrà visualizzata la console del client gestione indirizzi IP.  
  
2.  Nel riquadro di spostamento, in **monitoraggio e gestione**, fare clic su **zone DNS**.  Riquadro di spostamento divide in un riquadro di spostamento superiore e un riquadro di spostamento inferiore.  
  
3.  Fare clic per espandere **ricerca diretta** e il dominio in cui si trovano i record di zona e risorse che si desidera eliminare. Fare clic sulla zona, nel riquadro di visualizzazione, quindi fare clic su **visualizzazione corrente**. Fare clic su **record di risorse**.  
  
4.  Nel riquadro di visualizzazione, individuare e selezionare i record di risorse che si desidera eliminare.  
  
    ![Selezionare i record di risorse da eliminare](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_01.jpg)  
  
5.  Fare doppio clic sui record selezionati, quindi fare clic su **record di risorse DNS eliminare**.  
  
    ![Eliminare i record](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_02.jpg)  
  
6.  Il **Elimina Record di risorse DNS** apre la finestra di dialogo. Verificare che sia selezionato il server DNS corretto. In caso contrario, fare clic su **server DNS** e selezionare il server da cui si desidera eliminare i record di risorse. Fare clic su **OK**. Gestione indirizzi IP elimina i record di risorse dal server DNS.  
  
    ![Verificare che sia selezionato il server DNS corretto ed eliminare i record](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_03.jpg)  
  
## <a name="see-also"></a>Vedere anche  
[Gestione Record di risorse DNS](DNS-Resource-Record-Management.md)  
[Gestire gestione indirizzi IP](Manage-IPAM.md)  
  


