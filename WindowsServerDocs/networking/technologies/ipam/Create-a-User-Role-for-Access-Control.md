---
title: Creare un ruolo utente per il controllo di accesso
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
ms.assetid: ae6a42db-a104-401b-a8e6-b85c47d30b46
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 69d7acec19a460b51819bdc30ce40e21089c7bcf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823582"
---
# <a name="create-a-user-role-for-access-control"></a>Creare un ruolo utente per il controllo di accesso

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per creare un nuovo ruolo utente di controllo di accesso nella console di gestione indirizzi IP client.  
  
Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente.  
  
> [!NOTE]  
> Dopo aver creato un ruolo, è possibile creare un criterio di accesso per assegnare il ruolo a un utente specifico o un gruppo di Active Directory. Per altre informazioni, vedere [creare un criterio di accesso](../../technologies/ipam/Create-an-Access-Policy.md).  
  
### <a name="to-create-a-role"></a>Per creare un ruolo  
  
1.  In Server Manager, fare clic su  **Gestione indirizzi IP**. Verrà visualizzata la console di gestione indirizzi IP client.  
  
2.  Nel riquadro di spostamento, fare clic su **controllo di accesso**, nel riquadro di spostamento inferiore, fare clic su **ruoli**.  
  
    ![Ruoli di controllo di accesso](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_01.jpg)  
  
3.  Fare doppio clic su **ruoli**, quindi fare clic su **Aggiungi ruolo utente**.  
  
    ![Aggiungere il ruolo utente](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_02.jpg)  
  
4.  Il **Aggiungi / Modifica ruolo** verrà visualizzata la finestra di dialogo. Nelle **nome**, digitare un nome per il ruolo che rende la funzione ruolo cancellare. Ad esempio, se si desidera creare un ruolo che consente agli amministratori di gestire i record risorsa DNS SRV, è possibile denominare il ruolo **IPAMSrv**. Se necessario, scorrere verso il basso **operazioni** per individuare il tipo di operazioni che si desidera definire per il ruolo. Per questo esempio, scorrere verso il basso **operazioni di gestione dei record risorsa DNS**.  
  
    ![Operazioni di gestione dei record di risorse DNS](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_03.jpg)  
  
5.  Espandere **operazioni di gestione dei record risorsa DNS**, quindi individuare **operazioni sui record SRV**.  
  
    ![Operazioni di record SRV](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_04.jpg)  
  
6.  Espandere e selezionare **operazioni sui record SRV**, quindi fare clic su **OK**.  
  
    ![Selezione delle operazioni dei record SRV](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_05.jpg)  
  
7.  Nella console del client di gestione indirizzi IP, fare clic sul ruolo appena creato. Nelle **visualizzazione dettagli,** vengono visualizzate le operazioni consentite per il ruolo.  
  
    ![Dettagli del nuovo ruolo](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_06.jpg)  
  
## <a name="see-also"></a>Vedere anche  
[Controllo degli accessi in base al ruolo](Role-based-Access-Control.md)  
[Gestire gestione indirizzi IP](Manage-IPAM.md)  
  


