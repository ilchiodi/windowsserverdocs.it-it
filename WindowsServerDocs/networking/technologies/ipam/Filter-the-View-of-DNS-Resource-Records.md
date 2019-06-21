---
title: Filtrare la visualizzazione dei record di risorse DNS
description: Questo argomento fa parte della Guida di gestione di gestione indirizzi IP (IPAM) in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5b80294a-7325-476b-84eb-69f0d051e8b2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cc3f2b8ec6e7c5149ef6351639fbbf8f0def8be8
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283938"
---
# <a name="filter-the-view-of-dns-resource-records"></a>Filtrare la visualizzazione dei record di risorse DNS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per filtrare la visualizzazione di record di risorse DNS nella console di gestione indirizzi IP client.  
  
Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente.  
  
### <a name="to-filter-the-view-of-dns-resource-records"></a>Per filtrare la visualizzazione di record di risorse DNS  
  
1.  In Server Manager, fare clic su  **Gestione indirizzi IP**. Verrà visualizzata la console di gestione indirizzi IP client.  
  
2.  Nel riquadro di spostamento, in **MONITORAGGIO e GESTIONE**, fare clic su **zone DNS**.  Riquadro di spostamento viene suddivisa in un riquadro di spostamento superiore e un riquadro di navigazione inferiore.  
  
3.  Nel riquadro di spostamento inferiore, fare clic su **ricerca diretta**. Tutti gestito tramite Gestione indirizzi IP DNS zone di ricerca diretta vengono visualizzate nei risultati della ricerca del riquadro visualizzato.  
  
4.  Fare clic sulla zona di cui si desidera visualizzare e filtrare i record.  
  
5.  Nel riquadro informazioni fare clic su **visualizzazione corrente**, quindi fare clic su **record di risorse**. I record di risorse per la zona vengono visualizzati nel riquadro informazioni.  
  
6.  Nel riquadro informazioni fare clic su **Aggiungi criteri**.  
  
    ![Aggiungi criteri](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_01.jpg)  
  
7.  Seleziona un criterio dall'elenco a discesa. Ad esempio, se si desidera visualizzare un tipo di record specifici, fare clic su **tipo di Record**.  
  
    ![Seleziona un criterio](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_02.jpg)  
  
8.  Fai clic su **Aggiungi**.  
  
    ![Aggiungere i criteri](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_03.jpg)  
  
9. **Tipo di record** viene aggiunto come un parametro di ricerca. Immettere il testo per il tipo di record che si desidera trovare. Ad esempio, se si desidera visualizzare solo i record SRV, digitare **SRV**.  
  
    ![Specificare il tipo di record che si desidera trovare](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_04.jpg)  
  
10. Premere INVIO. I record risorsa DNS sono filtrati in base ai criteri e cercare una frase che è stato specificato.  
  
    ![Eseguire il filtro](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_05.jpg)  
  
## <a name="see-also"></a>Vedere anche  
[Gestione dei Record risorse DNS](DNS-Resource-Record-Management.md)  
[Gestire Gestione indirizzi IP](Manage-IPAM.md)  
  


