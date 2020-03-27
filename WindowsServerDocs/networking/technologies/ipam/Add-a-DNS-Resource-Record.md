---
title: Aggiungere un record di risorse DNS
description: Questo argomento fa parte della Guida alla gestione di gestione indirizzi IP in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5379373f-a3d9-4f51-b6fc-bf0f6df1d244
ms.author: lizross
author: eross-msft
ms.openlocfilehash: b89502a7ba5ceceea10e1f7cfae9e91f6a7ead59
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316900"
---
# <a name="add-a-dns-resource-record"></a>Aggiungere un record di risorse DNS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Per aggiungere uno o più nuovi record di risorse utilizzando la console di gestione indirizzi IP client, è possibile utilizzare questo argomento.  
  
Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente.  
  
### <a name="to-add-a-dns-resource-record"></a>Per aggiungere un record di risorse DNS  
  
1.  In Server Manager, fare clic su  **Gestione indirizzi IP**. Verrà visualizzata la console di gestione indirizzi IP client.  
  
2.  Nel riquadro di spostamento, in **MONITORAGGIO e GESTIONE**, fare clic su **zone DNS**.  Riquadro di spostamento viene suddivisa in un riquadro di spostamento superiore e un riquadro di navigazione inferiore.  
  
3.  Nel riquadro di spostamento inferiore, fare clic su **ricerca diretta**. Tutti gestito tramite Gestione indirizzi IP DNS zone di ricerca diretta vengono visualizzate nei risultati della ricerca del riquadro visualizzato. Pulsante destro del mouse la zona in cui si desidera aggiungere un record di risorse e quindi fare clic su **record di risorse DNS aggiungere**.  
  
    ![Aggiungere record di risorse DNS](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_01.jpg)
  
4.  Il **aggiungere record di risorse DNS** verrà visualizzata la finestra di dialogo. In **proprietà dei record di risorse**, fare clic su **server DNS** e selezionare il server DNS in cui si desidera aggiungere uno o più nuovi record di risorse. In **record di risorse DNS configurare**, fare clic su **nuovo**.  
  
    ![Configurare i record DNS](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_02.jpg)  
  
5.  Nella finestra di dialogo si espande per mostrare **Nuovo Record di risorse**. Fare clic su **tipo di record di risorse**.  
  
    ![Tipo di record di risorse](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_03.jpg)  
  
6.  Viene visualizzato l'elenco dei tipi di record di risorse. Fare clic sul tipo di record di risorse che si desidera aggiungere.  
  
    ![Selezionare tipo di record da aggiungere](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_04.jpg)  
  
7.  In **Nuovo Record di risorse,** in **nome**, digitare il nome di un record di risorse. In **indirizzo IP**, digitare un indirizzo IP e quindi selezionare le proprietà di record di risorse appropriate per la distribuzione. Fare clic su **aggiungere Record di risorse**.  
  
    ![Aggiungere Record di risorse](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_06.jpg)  
  
8.  Se non si desidera creare nuovi record di risorse aggiuntive, fare clic su **OK**. Se si desidera creare nuovi record di risorse aggiuntive, fare clic su **nuovo**.  
  
    ![Fare clic su OK o nuovi](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_01.jpg)
  
9. Nella finestra di dialogo si espande per mostrare **Nuovo Record di risorse**. Fare clic su **tipo di record di risorse**. Viene visualizzato l'elenco dei tipi di record di risorse. Fare clic sul tipo di record di risorse che si desidera aggiungere.  
  
10. In **Nuovo Record di risorse,** in **nome**, digitare il nome di un record di risorse. In **indirizzo IP**, digitare un indirizzo IP e quindi selezionare le proprietà di record di risorse appropriate per la distribuzione. Fare clic su **aggiungere Record di risorse**.  
  
    ![Aggiungere Record di risorse](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_02.jpg)  
  
11. Se si desidera aggiungere altri record di risorse, ripetere il processo per la creazione di record. Dopo aver finito di creare nuovi record di risorse, fare clic su **Applica**.  
  
    ![Creazione di record di risorsa completo](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_03.jpg)  
  
12. Il **aggiungere Record di risorse** la finestra di dialogo viene visualizzato un riepilogo di record di risorse mentre gestione indirizzi IP consente di creare i record di risorse nel server DNS specificato. Quando i record vengono creati correttamente, il **stato** del record è **successo**.  
  
    ![Stato di aggiunta di record](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_04.jpg)  
  
13. Fare clic su **OK**.  
  
## <a name="see-also"></a>Vedi anche  
[Gestione dei record di risorse DNS](DNS-Resource-Record-Management.md)  
[Gestire Gestione indirizzi IP](Manage-IPAM.md)  
  


