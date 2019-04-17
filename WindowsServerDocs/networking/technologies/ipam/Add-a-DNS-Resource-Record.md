---
title: Aggiungere un Record di risorse DNS
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
ms.assetid: 5379373f-a3d9-4f51-b6fc-bf0f6df1d244
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e14a59e9f172b20e85a34d2299e3733a796adafc
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="add-a-dns-resource-record"></a>Aggiungere un Record di risorse DNS

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per aggiungere uno o più record di risorse DNS nuovo tramite la console client di gestione indirizzi IP.  
  
Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per eseguire questa procedura.  
  
### <a name="to-add-a-dns-resource-record"></a>Per aggiungere un record di risorse DNS  
  
1.  In Server Manager, fare clic su **gestione indirizzi IP**. Verrà visualizzata la console del client gestione indirizzi IP.  
  
2.  Nel riquadro di spostamento, in **monitoraggio e gestione**, fare clic su **zone DNS**.  Riquadro di spostamento divide in un riquadro di spostamento superiore e un riquadro di spostamento inferiore.  
  
3.  Nel riquadro di spostamento inferiore, fare clic su **ricerca diretta**. Tutti gestiti da Gestione indirizzi IP DNS zone di ricerca diretta vengono visualizzate nei risultati della ricerca del riquadro visualizzato. Pulsante destro del mouse la zona in cui si desidera aggiungere un record di risorse e quindi fare clic su **record di risorse DNS aggiungere**.  
  
    ![Aggiungere record di risorse DNS](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_01.jpg)
  
4.  Il **aggiungere record di risorse DNS** apre la finestra di dialogo. In **proprietà dei record di risorse**, fare clic su **server DNS** e selezionare il server DNS in cui si desidera aggiungere uno o più nuovi record di risorse. In **record di risorse DNS configurare**, fare clic su **New**.  
  
    ![Configurare i record di risorse DNS](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_02.jpg)  
  
5.  Nella finestra di dialogo si espande per mostrare **nuovo Record di risorse**. Fare clic su **tipo record di risorse**.  
  
    ![Tipo di record di risorse](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_03.jpg)  
  
6.  Viene visualizzato l'elenco dei tipi di record di risorse. Fare clic sul tipo di record di risorse che si desidera aggiungere.  
  
    ![Selezionare tipo di record da aggiungere](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_04.jpg)  
  
7.  In **nuovo Record di risorse,** in **nome**, digitare il nome di un record di risorse. In **indirizzo IP**, digitare un indirizzo IP e quindi selezionare le proprietà di record di risorse che sono appropriate per la distribuzione. Fare clic su **aggiungere Record di risorse**.  
  
    ![Aggiungere Record di risorse](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_06.jpg)  
  
8.  Se non si desidera creare nuovi record di risorse aggiuntive, fare clic su **OK**. Se si desidera creare nuovi record di risorse aggiuntive, fare clic su **New**.  
  
    ![Fare clic su OK o nuovi](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_01.jpg)
  
9. Nella finestra di dialogo si espande per mostrare **nuovo Record di risorse**. Fare clic su **tipo record di risorse**. Viene visualizzato l'elenco dei tipi di record di risorse. Fare clic sul tipo di record di risorse che si desidera aggiungere.  
  
10. In **nuovo Record di risorse,** in **nome**, digitare il nome di un record di risorse. In **indirizzo IP**, digitare un indirizzo IP e quindi selezionare le proprietà di record di risorse che sono appropriate per la distribuzione. Fare clic su **aggiungere Record di risorse**.  
  
    ![Aggiungere Record di risorse](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_02.jpg)  
  
11. Se si desidera aggiungere altri record di risorse, ripetere il processo per la creazione di record. Dopo aver creare nuovi record di risorse, fare clic su **applica**.  
  
    ![Creazione di record di risorsa completo](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_03.jpg)  
  
12. Il **aggiungere Record di risorse** la finestra di dialogo viene visualizzato un riepilogo di record di risorse mentre gestione indirizzi IP consente di creare i record di risorse nel server DNS specificato. Quando i record vengono creati correttamente, il **stato** del record è **successo**.  
  
    ![Stato di aggiunta di record](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_04.jpg)  
  
13. Fare clic su **OK**.  
  
## <a name="see-also"></a>Vedere anche  
[Gestione Record di risorse DNS](DNS-Resource-Record-Management.md)  
[Gestire gestione indirizzi IP](Manage-IPAM.md)  
  


