---
title: Eliminare i record di risorse DNS
description: Questo argomento fa parte della Guida alla gestione di gestione indirizzi IP in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ipam
ms.topic: article
ms.assetid: 366e6fd5-d563-4de3-9551-5614cbb8f2cb
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a5658e53aebf9f6fa20a5de9f3f7cb5a0287b408
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860674"
---
# <a name="delete-dns-resource-records"></a>Eliminare i record di risorse DNS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile usare questo argomento per eliminare uno o più record di risorse DNS usando la console client di gestione indirizzi IP.  
  
Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente.  
  
### <a name="to-delete-dns-resource-records"></a>Per eliminare i record di risorse DNS  
  
1.  In Server Manager, fare clic su  **Gestione indirizzi IP**. Verrà visualizzata la console di gestione indirizzi IP client.  
  
2.  Nel riquadro di spostamento, in **MONITORAGGIO e GESTIONE**, fare clic su **zone DNS**.  Riquadro di spostamento viene suddivisa in un riquadro di spostamento superiore e un riquadro di navigazione inferiore.  
  
3.  Fare clic per espandere **ricerca diretta** e il dominio in cui si trovano i record di zona e di risorsa che si desidera eliminare. Fare clic sulla zona e, nel riquadro informazioni, fare clic su **visualizzazione corrente**. Fare clic su **record di risorse**.  
  
4.  Nel riquadro informazioni individuare e selezionare i record di risorse che si desidera eliminare.  
  
    ![Selezionare i record di risorse da eliminare](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_01.jpg)  
  
5.  Fare clic con il pulsante destro del mouse sui record selezionati, quindi scegliere **Elimina record di risorse DNS**.  
  
    ![Eliminare i record](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_02.jpg)  
  
6.  Verrà visualizzata la finestra di dialogo **Elimina record di risorse DNS** . Verificare che sia selezionato il server DNS corretto. In caso contrario, fare clic su **server DNS** e selezionare il server da cui si desidera eliminare i record di risorse. Fare clic su **OK**. Gestione indirizzi IP elimina i record di risorse dal server DNS.  
  
    ![Verificare che sia selezionato il server DNS corretto ed eliminare i record](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_03.jpg)  
  
## <a name="see-also"></a>Vedi anche  
[Gestione dei record di risorse DNS](DNS-Resource-Record-Management.md)  
[Gestire Gestione indirizzi IP](Manage-IPAM.md)  
  


