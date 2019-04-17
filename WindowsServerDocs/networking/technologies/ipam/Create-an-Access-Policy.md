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
ms.openlocfilehash: ac8229952c7e038f9af8fc4f9287b1821db887ac
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="create-an-access-policy"></a>Creare un criterio di accesso

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare questo argomento per creare un criterio di accesso nella console del client di gestione indirizzi IP.  
  
Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per eseguire questa procedura.  
  
> [!NOTE]  
> È possibile creare un criterio di accesso per un utente specifico o per un gruppo di utenti in Active Directory. Quando si crea un criterio di accesso, è necessario selezionare un ruolo incorporato di gestione indirizzi IP o un ruolo personalizzato creato. Per ulteriori informazioni sui ruoli personalizzati, vedere [creare un ruolo utente per il controllo degli accessi](../../technologies/ipam/Create-a-User-Role-for-Access-Control.md).  
  
### <a name="to-create-an-access-policy"></a>Per creare un criterio di accesso  
  
1.  In Server Manager, fare clic su **gestione indirizzi IP**. Verrà visualizzata la console del client gestione indirizzi IP.  
  
2.  Nel riquadro di spostamento, fare clic su **il controllo degli accessi**. Nel riquadro di spostamento inferiore, fare doppio clic su **criteri di accesso**, quindi fare clic su **Aggiungi criteri di accesso**.  
  
    ![Aggiungere criteri di accesso](../../media/Create-an-Access-Policy/ipam_CreateAP_01.jpg)  
  
3.  Il **Aggiungi criteri di accesso** apre la finestra di dialogo. In **le impostazioni utente**, fare clic su **Aggiungi**.  
  
    ![Aggiungere criteri di accesso](../../media/Create-an-Access-Policy/ipam_CreateAP_02.jpg)  
  
4.  Il **Seleziona utente o gruppo** apre la finestra di dialogo. Fare clic su **percorsi**.  
  
    ![Percorsi utente o gruppo](../../media/Create-an-Access-Policy/ipam_CreateAP_03.jpg)  
  
5.  Il **percorsi** apre la finestra di dialogo. Selezionare il percorso che contiene l'account utente, selezionare il percorso e quindi fare clic su **OK**. Il **percorsi** chiude la finestra di dialogo.  
  
    ![Seleziona percorso](../../media/Create-an-Access-Policy/ipam_CreateAP_04.jpg)  
  
6.  Nel **Seleziona utente o gruppo** della finestra di dialogo **immettere il nome dell'oggetto da selezionare**, digitare il nome dell'account utente per cui si desidera creare un criterio di accesso. Fare clic su **OK**.  
  
7.  In **Aggiungi criteri di accesso**, in **le impostazioni utente**, **alias utente** ora contiene l'account utente a cui si applica il criterio. In **le impostazioni di accesso**, fare clic su **New**.  
  
    ![Nuova impostazione di accesso](../../media/Create-an-Access-Policy/ipam_CreateAP_05.jpg)  
  
8.  In **Aggiungi criteri di accesso**, **le impostazioni di accesso** diventa **nuova impostazione**.  
  
    ![Nome della finestra di dialogo Modifica all'impostazione di nuovo](../../media/Create-an-Access-Policy/ipam_CreateAP_06.jpg)  
  
9. Fare clic su **ruolo seleziona** per espandere l'elenco dei ruoli. Selezionare uno dei ruoli predefiniti oppure, se hai creato nuovi ruoli, selezionare uno dei ruoli che è stato creato. Ad esempio, se è stato creato il ruolo IPAMSrv da applicare all'utente, fare clic su **IPAMSrv**.  
  
    ![Selezionare ruolo](../../media/Create-an-Access-Policy/ipam_CreateAP_07.jpg)  
  
10. Fare clic su **Aggiungi impostazione**.  
  
    ![Aggiungere una nuova impostazione](../../media/Create-an-Access-Policy/ipam_CreateAP_08.jpg)  
  
11. Il ruolo viene aggiunto al criterio di accesso. Per creare criteri di accesso aggiuntive, fare clic su **applica**, quindi ripetere questi passaggi per ogni criterio che si desidera creare. Se non si desidera creare criteri aggiuntivi, fare clic su **OK**.  
  
    ![Fare clic su Applica o OK](../../media/Create-an-Access-Policy/ipam_CreateAP_09.jpg)  
  
12. Nel riquadro visualizzazione delle console client di gestione indirizzi IP, verificare che venga creato il nuovo criterio di accesso.  
  
    ![Visualizzare il nuovo criterio di accesso](../../media/Create-an-Access-Policy/ipam_CreateAP_09a.jpg)  
  
## <a name="see-also"></a>Vedere anche  
[Controllo degli accessi in base al ruolo](Role-based-Access-Control.md)  
[Gestire gestione indirizzi IP](Manage-IPAM.md)  
  


