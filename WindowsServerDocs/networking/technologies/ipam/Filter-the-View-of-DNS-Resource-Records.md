---
title: Filtrare la visualizzazione dei record di risorse DNS
description: Questo argomento fa parte della Guida alla gestione di gestione indirizzi IP in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5b80294a-7325-476b-84eb-69f0d051e8b2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 541645a274481bb8b044c37df572d7746c5da30e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405667"
---
# <a name="filter-the-view-of-dns-resource-records"></a>Filtrare la visualizzazione dei record di risorse DNS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile usare questo argomento per filtrare la visualizzazione dei record di risorse DNS nella console del client di gestione indirizzi IP.  
  
Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente.  
  
### <a name="to-filter-the-view-of-dns-resource-records"></a>Per filtrare la visualizzazione dei record di risorse DNS  
  
1.  In Server Manager, fare clic su  **Gestione indirizzi IP**. Verrà visualizzata la console di gestione indirizzi IP client.  
  
2.  Nel riquadro di spostamento, in **MONITORAGGIO e GESTIONE**, fare clic su **zone DNS**.  Riquadro di spostamento viene suddivisa in un riquadro di spostamento superiore e un riquadro di navigazione inferiore.  
  
3.  Nel riquadro di spostamento inferiore, fare clic su **ricerca diretta**. Tutti gestito tramite Gestione indirizzi IP DNS zone di ricerca diretta vengono visualizzate nei risultati della ricerca del riquadro visualizzato.  
  
4.  Fare clic sulla zona di cui si desidera visualizzare i record e filtrare.  
  
5.  Nel riquadro informazioni fare clic su **visualizzazione corrente**, quindi fare clic su **record di risorse**. I record di risorse per la zona vengono visualizzati nel riquadro informazioni.  
  
6.  Nel riquadro informazioni fare clic su **Aggiungi criteri**.  
  
    ![Aggiungi criteri](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_01.jpg)  
  
7.  Selezionare un criterio dall'elenco a discesa. Se ad esempio si desidera visualizzare un tipo di record specifico, fare clic su **tipo di record**.  
  
    ![Selezionare un criterio](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_02.jpg)  
  
8.  Fai clic su **Aggiungi**.  
  
    ![Aggiungere i criteri](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_03.jpg)  
  
9. Il **tipo di record** viene aggiunto come parametro di ricerca. Immettere il testo per il tipo di record che si desidera trovare. Se ad esempio si desidera visualizzare solo i record SRV, digitare **SRV**.  
  
    ![Specificare il tipo di record che si desidera trovare](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_04.jpg)  
  
10. Premere INVIO. I record di risorse DNS vengono filtrati in base ai criteri e alla frase di ricerca specificati.  
  
    ![Eseguire il filtro](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_05.jpg)  
  
## <a name="see-also"></a>Vedere anche  
[Gestione dei record di risorse DNS](DNS-Resource-Record-Management.md)  
[Gestire Gestione indirizzi IP](Manage-IPAM.md)  
  


