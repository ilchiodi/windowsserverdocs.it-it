---
title: Impostare l'ambito di accesso per i record delle risorse DNS
description: Questo argomento fa parte della Guida alla gestione di gestione indirizzi IP in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ipam
ms.topic: article
ms.assetid: a96a8752-5678-49c5-b069-d2cce8042a51
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 613796c933498d104db4895733c9a9b1957cb952
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860614"
---
# <a name="set-access-scope-for-dns-resource-records"></a>Impostare l'ambito di accesso per i record delle risorse DNS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile usare questo argomento per impostare l'ambito di accesso per i record di risorse DNS usando la console client di gestione indirizzi IP.  
  
Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente.  
  
### <a name="to-set-access-scope-for-dns-resource-records"></a>Per impostare l'ambito di accesso per i record di risorse DNS  
  
1.  In Server Manager, fare clic su  **Gestione indirizzi IP**. Verrà visualizzata la console di gestione indirizzi IP client.  
  
2.  Nel riquadro di spostamento fare clic su **zone DNS**.  Nel riquadro di spostamento inferiore, espandere **ricerca diretta** e individuare e selezionare la zona contenente i record di risorse di cui si desidera modificare l'ambito di accesso.  
  
3.  Nel riquadro informazioni individuare e selezionare i record di risorse di cui si desidera modificare l'ambito di accesso.  
  
    ![Selezionare i record di risorse](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_02.jpg)  
  
4.  Fare clic con il pulsante destro del mouse sui record di risorse DNS selezionati, quindi scegliere **Imposta ambito di accesso**.  
  
    ![Imposta ambito di accesso](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_03.jpg)  
  
5.  Verrà visualizzata la finestra di dialogo **Imposta ambito di accesso** . Se necessario per la distribuzione, fare clic per deselezionare **eredita ambito di accesso da padre**. In **selezionare l'ambito di accesso**selezionare un elemento e quindi fare clic su **OK**.  
  
    ![Selezionare l'ambito di accesso](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_04.jpg)  
  
## <a name="see-also"></a>Vedi anche  
[Controllo degli accessi in base al ruolo](Role-based-Access-Control.md)  
[Gestire Gestione indirizzi IP](Manage-IPAM.md)  
  


