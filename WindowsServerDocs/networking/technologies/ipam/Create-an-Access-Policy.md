---
title: Creare un criterio di accesso
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
ms.assetid: 854bd064-2f86-4678-a940-a04b3e48ae10
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c8a97cd145a695bc8755f9111291e5c8bba2e572
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881902"
---
# <a name="create-an-access-policy"></a>Creare un criterio di accesso

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare questo argomento per creare un criterio di accesso nella console di gestione indirizzi IP client.  
  
Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente.  
  
> [!NOTE]  
> È possibile creare un criterio di accesso per un utente specifico o per un gruppo di utenti in Active Directory. Quando si crea un criterio di accesso, è necessario selezionare un ruolo di gestione indirizzi IP predefinito o un ruolo personalizzato che è stato creato. Per altre informazioni sui ruoli personalizzati, vedere [creare un ruolo utente per il controllo di accesso](../../technologies/ipam/Create-a-User-Role-for-Access-Control.md).  
  
### <a name="to-create-an-access-policy"></a>Per creare un criterio di accesso  
  
1.  In Server Manager, fare clic su  **Gestione indirizzi IP**. Verrà visualizzata la console di gestione indirizzi IP client.  
  
2.  Nel riquadro di spostamento, fare clic su **controllo di accesso**. Nel riquadro di spostamento inferiore, fare doppio clic su **criteri di accesso**, quindi fare clic su **Aggiungi criterio di accesso**.  
  
    ![Aggiungere criteri di accesso](../../media/Create-an-Access-Policy/ipam_CreateAP_01.jpg)  
  
3.  Il **aggiungere i criteri di accesso** verrà visualizzata la finestra di dialogo. Nelle **delle impostazioni utente**, fare clic su **Add**.  
  
    ![Aggiungere criteri di accesso](../../media/Create-an-Access-Policy/ipam_CreateAP_02.jpg)  
  
4.  Il **Seleziona utente o gruppo** verrà visualizzata la finestra di dialogo. Fare clic su **posizioni**.  
  
    ![Percorsi utente o gruppo](../../media/Create-an-Access-Policy/ipam_CreateAP_03.jpg)  
  
5.  Il **posizioni** verrà visualizzata la finestra di dialogo. Passare al percorso che contiene l'account utente, selezionare il percorso e quindi fare clic su **OK**. Il **posizioni** chiude la finestra di dialogo.  
  
    ![Selezionare il percorso](../../media/Create-an-Access-Policy/ipam_CreateAP_04.jpg)  
  
6.  Nel **Seleziona utente o gruppo** nella finestra di dialogo **immettere il nome dell'oggetto da selezionare**, digitare il nome dell'account utente per il quale si desidera creare un criterio di accesso. Fare clic su **OK**.  
  
7.  In **aggiungere i criteri di accesso**, nel **impostazioni utente**, **alias utente** ora contiene l'account utente a cui si applica il criterio. Nelle **le impostazioni di accesso**, fare clic su **New**.  
  
    ![Nuova impostazione di accesso](../../media/Create-an-Access-Policy/ipam_CreateAP_05.jpg)  
  
8.  Nelle **aggiungere i criteri di accesso**, **le impostazioni di accesso** cambia in **nuova impostazione**.  
  
    ![Nome della finestra di dialogo Modifica alla nuova impostazione](../../media/Create-an-Access-Policy/ipam_CreateAP_06.jpg)  
  
9. Fare clic su **selezionare un ruolo** per espandere l'elenco dei ruoli. Selezionare uno dei ruoli predefiniti o, se è stato creato nuovi ruoli, selezionare uno dei ruoli di cui è stato creato. Ad esempio, se è stato creato il ruolo IPAMSrv da applicare all'utente, fare clic su **IPAMSrv**.  
  
    ![Selezionare un ruolo](../../media/Create-an-Access-Policy/ipam_CreateAP_07.jpg)  
  
10. Fare clic su **Aggiungi impostazione**.  
  
    ![Aggiungi nuova impostazione](../../media/Create-an-Access-Policy/ipam_CreateAP_08.jpg)  
  
11. Il ruolo viene aggiunto ai criteri di accesso. Per creare i criteri di accesso aggiuntivi, fare clic su **applica**, quindi ripetere questi passaggi per ogni criterio che si desidera creare. Se non si desidera creare criteri aggiuntivi, fare clic su **OK**.  
  
    ![Fare clic su Applica o su OK](../../media/Create-an-Access-Policy/ipam_CreateAP_09.jpg)  
  
12. Nel riquadro di visualizzazione IPAM client console verificare che venga creato il nuovo criterio di accesso.  
  
    ![Visualizzare il nuovo criterio di accesso](../../media/Create-an-Access-Policy/ipam_CreateAP_09a.jpg)  
  
## <a name="see-also"></a>Vedere anche  
[Controllo degli accessi in base al ruolo](Role-based-Access-Control.md)  
[Gestire gestione indirizzi IP](Manage-IPAM.md)  
  


