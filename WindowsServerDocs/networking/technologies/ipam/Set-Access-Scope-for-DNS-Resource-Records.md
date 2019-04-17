---
title: Imposta ambito di accesso per record di risorse DNS
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
ms.assetid: a96a8752-5678-49c5-b069-d2cce8042a51
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 06c33a633975497e80863cc8d42b14a0f9ac8193
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="set-access-scope-for-dns-resource-records"></a>Imposta ambito di accesso per record di risorse DNS

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Per impostare l'ambito di accesso per un record di risorse DNS tramite la console di gestione indirizzi IP client, è possibile utilizzare questo argomento.  
  
Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per eseguire questa procedura.  
  
### <a name="to-set-access-scope-for-dns-resource-records"></a>Per impostare l'ambito di accesso per record di risorse DNS  
  
1.  In Server Manager, fare clic su **gestione indirizzi IP**. Verrà visualizzata la console del client gestione indirizzi IP.  
  
2.  Nel riquadro di spostamento, fare clic su **zone DNS**.  Nel riquadro di spostamento inferiore, espandere **ricerca diretta** e individuare e selezionare la zona che contiene i record di risorse il cui ambito di accesso che si desidera modificare.  
  
3.  Nel riquadro di visualizzazione, individuare e selezionare i record di risorse il cui ambito di accesso che si desidera modificare.  
  
    ![Selezionare i record di risorse](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_02.jpg)  
  
4.  Fare doppio clic su record di risorse DNS selezionato, quindi fare clic su **Imposta ambito di accesso**.  
  
    ![Imposta ambito di accesso](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_03.jpg)  
  
5.  Il **Imposta ambito di accesso** apre la finestra di dialogo. Se richiesto per la distribuzione, fare clic per deselezionare **eredita ambito di accesso da padre**. In **selezionare l'ambito di accesso**, selezionare un elemento e quindi fare clic su **OK**.  
  
    ![Selezionare l'ambito di accesso](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_04.jpg)  
  
## <a name="see-also"></a>Vedere anche  
[Controllo degli accessi in base al ruolo](Role-based-Access-Control.md)  
[Gestire gestione indirizzi IP](Manage-IPAM.md)  
  


