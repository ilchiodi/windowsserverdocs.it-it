---
title: Visualizzare i record di risorse DNS per una zona DNS
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
ms.assetid: 375feefc-949e-47c3-9e61-35b79e021966
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 786c1ee8fd673bd17465ab9586dd1e0bcfd7971c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="view-dns-resource-records-for-a-dns-zone"></a>Visualizzare i record di risorse DNS per una zona DNS

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per visualizzare i record di risorse DNS per una zona DNS nella console del client di gestione indirizzi IP.  
  
Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per eseguire questa procedura.  
  
### <a name="to-view-dns-resource-records-for-a-zone"></a>Per visualizzare i record di risorse DNS per una zona  
  
1.  In Server Manager, fare clic su **gestione indirizzi IP**. Verrà visualizzata la console del client gestione indirizzi IP.  
  
2.  Nel riquadro di spostamento, in **monitoraggio e gestione**, fare clic su **zone DNS**.  Riquadro di spostamento divide in un riquadro di spostamento superiore e un riquadro di spostamento inferiore.  
  
3.  Nel riquadro di spostamento inferiore, fare clic su **ricerca diretta**, quindi espandere l'elenco di dominio e fuso orario per individuare e selezionare l'area in cui si desidera visualizzare. Ad esempio, se si dispone di una zona denominata Dublino, fare clic su **dublin**.  
  
    ![Selezionare l'area in cui che si desidera visualizzare](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01a.jpg)  

  
4.  Nel riquadro di visualizzazione, la visualizzazione predefinita è dei server DNS per la zona. Per modificare la visualizzazione, fare clic su **visualizzazione corrente**, quindi fare clic su **record di risorse**.  
  
    ![Passare alla visualizzazione di record di risorse](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_Zone_RR_02.jpg)  
  
5.  Vengono visualizzati i record di risorse DNS per la zona. Per filtrare i record, digitare il testo da trovare **filtro**.  
  
    ![Digitare il testo per filtrare i record](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01c.jpg)  
  
6.  Per filtrare i record di risorse dal tipo di record, l'ambito di accesso o altri criteri, fare clic su **aggiungere criteri**, effettuare le selezioni dall'elenco dei criteri e fare clic su **Aggiungi**.  
  
    ![Utilizzare i criteri per filtrare i record](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01d.jpg)  
  
## <a name="see-also"></a>Vedere anche  
[Gestire le Zone DNS](DNS-Zone-Management.md)  
[Gestire gestione indirizzi IP](Manage-IPAM.md)  
  


