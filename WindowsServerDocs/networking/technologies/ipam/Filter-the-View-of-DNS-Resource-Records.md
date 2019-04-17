---
title: Filtrare la visualizzazione dei record di risorse DNS
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
ms.assetid: 5b80294a-7325-476b-84eb-69f0d051e8b2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 35c0e822daa9f2c8c49ae7e6f2f40ec0411cb6fa
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="filter-the-view-of-dns-resource-records"></a>Filtrare la visualizzazione dei record di risorse DNS

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per filtrare la visualizzazione di record di risorse DNS nella console del client di gestione indirizzi IP.  
  
Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per eseguire questa procedura.  
  
### <a name="to-filter-the-view-of-dns-resource-records"></a>Per filtrare la visualizzazione dei record di risorse DNS  
  
1.  In Server Manager, fare clic su **gestione indirizzi IP**. Verrà visualizzata la console del client gestione indirizzi IP.  
  
2.  Nel riquadro di spostamento, in **monitoraggio e gestione**, fare clic su **zone DNS**.  Riquadro di spostamento divide in un riquadro di spostamento superiore e un riquadro di spostamento inferiore.  
  
3.  Nel riquadro di spostamento inferiore, fare clic su **ricerca diretta**. Tutti gestiti da Gestione indirizzi IP DNS zone di ricerca diretta vengono visualizzate nei risultati della ricerca del riquadro visualizzato.  
  
4.  Fare clic sulla zona cui si desidera visualizzare e filtrare i record.  
  
5.  Nel riquadro informazioni fare clic su **visualizzazione corrente**, quindi fare clic su **record di risorse**. I record di risorse per la zona vengono visualizzati nel riquadro di visualizzazione.  
  
6.  Nel riquadro informazioni fare clic su **aggiungere criteri**.  
  
    ![Aggiungere criteri](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_01.jpg)  
  
7.  Selezionare un criterio dall'elenco a discesa. Ad esempio, se si desidera visualizzare un tipo di record specifici, fare clic su **tipo di Record**.  
  
    ![Selezionare un criterio](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_02.jpg)  
  
8.  Fare clic su **aggiungere**.  
  
    ![Aggiungere i criteri](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_03.jpg)  
  
9. **Tipo di record** viene aggiunto come parametro di ricerca. Immettere il testo per il tipo di record che si desidera trovare. Ad esempio, se si desidera visualizzare solo i record SRV, digitare **SRV**.  
  
    ![Specificare il tipo di record che si desidera trovare](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_04.jpg)  
  
10. Premere INVIO. I record di risorse DNS vengono filtrati in base ai criteri e cercare la frase specificata.  
  
    ![Eseguire il filtro](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_05.jpg)  
  
## <a name="see-also"></a>Vedere anche  
[Gestione Record di risorse DNS](DNS-Resource-Record-Management.md)  
[Gestire gestione indirizzi IP](Manage-IPAM.md)  
  


