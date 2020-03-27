---
title: Creare un criterio di accesso
description: Questo argomento fa parte della Guida alla gestione di gestione indirizzi IP in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 854bd064-2f86-4678-a940-a04b3e48ae10
ms.author: lizross
author: eross-msft
ms.openlocfilehash: b5ce4c6dae668372521e5b8e0d5168a94cbb86e2
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312648"
---
# <a name="create-an-access-policy"></a>Creare un criterio di accesso

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile usare questo argomento per creare un criterio di accesso nella console del client di gestione indirizzi IP.  
  
Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente.  
  
> [!NOTE]  
> È possibile creare criteri di accesso per un utente specifico o per un gruppo di utenti in Active Directory. Quando si crea un criterio di accesso, è necessario selezionare un ruolo predefinito di gestione indirizzi IP o un ruolo personalizzato che è stato creato. Per altre informazioni sui ruoli personalizzati, vedere [creare un ruolo utente per il controllo di accesso](../../technologies/ipam/Create-a-User-Role-for-Access-Control.md).  
  
### <a name="to-create-an-access-policy"></a>Per creare un criterio di accesso  
  
1.  In Server Manager, fare clic su  **Gestione indirizzi IP**. Verrà visualizzata la console di gestione indirizzi IP client.  
  
2.  Nel riquadro di spostamento fare clic su **controllo di accesso**. Nel riquadro di spostamento inferiore fare clic con il pulsante destro del mouse su **criteri di accesso**e quindi scegliere **Aggiungi criteri di accesso**.  
  
    ![Aggiungi criteri di accesso](../../media/Create-an-Access-Policy/ipam_CreateAP_01.jpg)  
  
3.  Verrà visualizzata la finestra di dialogo **Aggiungi criteri di accesso** . In **impostazioni utente**fare clic su **Aggiungi**.  
  
    ![Aggiungi criteri di accesso](../../media/Create-an-Access-Policy/ipam_CreateAP_02.jpg)  
  
4.  Verrà visualizzata la finestra di dialogo **Seleziona utente o gruppo** . Fare clic su **percorsi**.  
  
    ![Percorsi utente o gruppo](../../media/Create-an-Access-Policy/ipam_CreateAP_03.jpg)  
  
5.  Verrà visualizzata la finestra di dialogo **percorsi** . Individuare il percorso che contiene l'account utente, selezionare il percorso, quindi fare clic su **OK**. La finestra di dialogo **percorsi** verrà chiusa.  
  
    ![Selezionare il percorso](../../media/Create-an-Access-Policy/ipam_CreateAP_04.jpg)  
  
6.  Nella finestra di dialogo **Seleziona utente o gruppo** , in **immettere il nome dell'oggetto da selezionare**, digitare il nome dell'account utente per il quale si desidera creare un criterio di accesso. Fare clic su **OK**.  
  
7.  In **Aggiungi criteri di accesso**, in **impostazioni utente**, l' **alias utente** contiene ora l'account utente a cui si applicano i criteri. In **Impostazioni accesso**fare clic su **nuovo**.  
  
    ![Nuova impostazione di accesso](../../media/Create-an-Access-Policy/ipam_CreateAP_05.jpg)  
  
8.  In **Aggiungi criteri di accesso**accedere a **Impostazioni** modifiche a **nuova impostazione**.  
  
    ![Modifica del nome della finestra di dialogo in nuova impostazione](../../media/Create-an-Access-Policy/ipam_CreateAP_06.jpg)  
  
9. Fare clic su **Seleziona ruolo** per espandere l'elenco dei ruoli. Selezionare uno dei ruoli predefiniti o, se sono stati creati nuovi ruoli, selezionare uno dei ruoli creati. Se, ad esempio, è stato creato il ruolo IPAMSrv da applicare all'utente, fare clic su **IPAMSrv**.  
  
    ![Selezione ruolo](../../media/Create-an-Access-Policy/ipam_CreateAP_07.jpg)  
  
10. Fare clic su **Aggiungi impostazione**.  
  
    ![Aggiungi nuova impostazione](../../media/Create-an-Access-Policy/ipam_CreateAP_08.jpg)  
  
11. Il ruolo viene aggiunto ai criteri di accesso. Per creare criteri di accesso aggiuntivi, fare clic su **applica**, quindi ripetere questi passaggi per ogni criterio che si desidera creare. Se non si desidera creare criteri aggiuntivi, fare clic su **OK**.  
  
    ![Fare clic su Applica o OK](../../media/Create-an-Access-Policy/ipam_CreateAP_09.jpg)  
  
12. Nel riquadro di visualizzazione della console client di gestione indirizzi IP, verificare che il nuovo criterio di accesso sia stato creato.  
  
    ![Visualizzare i nuovi criteri di accesso](../../media/Create-an-Access-Policy/ipam_CreateAP_09a.jpg)  
  
## <a name="see-also"></a>Vedi anche  
[Controllo degli accessi in base al ruolo](Role-based-Access-Control.md)  
[Gestire Gestione indirizzi IP](Manage-IPAM.md)  
  


