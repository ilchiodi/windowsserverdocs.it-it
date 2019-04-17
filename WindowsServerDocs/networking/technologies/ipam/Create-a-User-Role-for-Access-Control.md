---
title: Creare un ruolo utente per il controllo degli accessi
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
ms.openlocfilehash: fa0ed71d399ad638a648946952fe170d93f69ceb
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-user-role-for-access-control"></a>Creare un ruolo utente per il controllo degli accessi

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per creare un nuovo ruolo utente il controllo degli accessi nella console del client di gestione indirizzi IP.  
  
Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per eseguire questa procedura.  
  
> [!NOTE]  
> Dopo aver creato un ruolo, è possibile creare un criterio di accesso per assegnare il ruolo a un utente specifico o un gruppo di Active Directory. Per ulteriori informazioni, vedere [creare un criterio di accesso](../../technologies/ipam/Create-an-Access-Policy.md).  
  
### <a name="to-create-a-role"></a>Per creare un ruolo  
  
1.  In Server Manager, fare clic su **gestione indirizzi IP**. Verrà visualizzata la console del client gestione indirizzi IP.  
  
2.  Nel riquadro di spostamento, fare clic su **il controllo degli accessi**, nel riquadro di spostamento inferiore, fare clic su **ruoli**.  
  
    ![Ruoli di controllo di accesso](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_01.jpg)  
  
3.  Fare doppio clic su **ruoli**, quindi fare clic su **Aggiungi ruolo utente**.  
  
    ![Aggiungere il ruolo utente](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_02.jpg)  
  
4.  Il **Aggiungi o Modifica ruolo** apre la finestra di dialogo. In **nome**, digitare un nome per il ruolo che consente di cancellare la funzione di ruolo. Ad esempio, se si desidera creare un ruolo che consente agli amministratori di gestire i record DNS SRV, è possibile denominare il ruolo **IPAMSrv**. Se necessario, scorrere verso il basso **Operations** per individuare il tipo di operazioni che si desidera definire per il ruolo. Per questo esempio, scorrere verso il basso **operazioni di gestione dei record di risorse DNS**.  
  
    ![Operazioni di gestione dei record di risorse DNS](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_03.jpg)  
  
5.  Espandere **operazioni di gestione dei record di risorse DNS**e quindi individuare **operazioni sui record SRV**.  
  
    ![Operazioni di record SRV](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_04.jpg)  
  
6.  Espandere e selezionare **operazioni sui record SRV**, quindi fare clic su **OK**.  
  
    ![Selezionare operazioni sui record SRV](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_05.jpg)  
  
7.  Nella console del client di gestione indirizzi IP, fare clic sul ruolo appena creato. In **visualizzazione dei dettagli,** vengono visualizzate le operazioni consentite per il ruolo.  
  
    ![Nuovi dettagli di ruolo](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_06.jpg)  
  
## <a name="see-also"></a>Vedere anche  
[Controllo degli accessi in base al ruolo](Role-based-Access-Control.md)  
[Gestire gestione indirizzi IP](Manage-IPAM.md)  
  


