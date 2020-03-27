---
title: Creare una zona DNS
description: Questo argomento fa parte della Guida alla gestione di gestione indirizzi IP in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a030ff51-a815-4fc4-b26d-aae41c3e4ce5
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a9762d15d0b95954623bbefdec38696885676975
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312631"
---
# <a name="create-a-dns-zone"></a>Creare una zona DNS

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile usare questo argomento per creare una zona DNS usando la console client di gestione indirizzi IP.  
  
Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente.  
  
### <a name="to-create-a-dns-zone"></a>Per creare una zona DNS  
  
1.  In Server Manager, fare clic su  **Gestione indirizzi IP**. Verrà visualizzata la console di gestione indirizzi IP client.  
  
2.  Nel riquadro di spostamento, in **monitoraggio e gestione**, fare clic su **server DNS e DHCP**. Nel riquadro informazioni fare clic su **tipo di server**, quindi fare clic su **DNS**. Tutti i server DNS gestiti da Gestione indirizzi IP sono elencati nei risultati della ricerca.  
  
3.  Individuare il server in cui si desidera aggiungere una zona e fare clic con il pulsante destro del mouse sul server.  Fare clic su **Crea zona DNS**.  
  
    ![Crea zona DNS](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_01a.jpg)  
  
4.  Verrà visualizzata la finestra di dialogo **Crea zona DNS** . In **Proprietà generali**selezionare una categoria di zona, un tipo di zona e immettere un nome in **nome zona**. Selezionare anche i valori appropriati per la distribuzione in **proprietà avanzate**, quindi fare clic su **OK**.  
  
    ![Proprietà avanzate](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_02a.jpg)  
  
## <a name="see-also"></a>Vedi anche  
[Gestione delle zone DNS](DNS-Zone-Management.md)  
[Gestire Gestione indirizzi IP](Manage-IPAM.md)  
  


