---
title: Creare un ruolo utente per il controllo di accesso
description: Questo argomento fa parte della Guida alla gestione di gestione indirizzi IP in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ipam
ms.topic: article
ms.assetid: ae6a42db-a104-401b-a8e6-b85c47d30b46
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 90ba50189b0f42f1f581032b7dc8b52b8c3fca4d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814764"
---
# <a name="create-a-user-role-for-access-control"></a>Creare un ruolo utente per il controllo di accesso

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile usare questo argomento per creare un nuovo ruolo utente di controllo di accesso nella console del client di gestione indirizzi IP.  
  
Per eseguire questa procedura è necessaria almeno l'appartenenza al gruppo **Administrators** oppure a un gruppo equivalente.  
  
> [!NOTE]  
> Dopo aver creato un ruolo, è possibile creare un criterio di accesso per assegnare il ruolo a un utente o a un gruppo di Active Directory specifico. Per altre informazioni, vedere [creare un criterio di accesso](../../technologies/ipam/Create-an-Access-Policy.md).  
  
### <a name="to-create-a-role"></a>Per creare un ruolo  
  
1.  In Server Manager, fare clic su  **Gestione indirizzi IP**. Verrà visualizzata la console di gestione indirizzi IP client.  
  
2.  Nel riquadro di spostamento fare clic su **controllo di accesso**, quindi nel riquadro di spostamento inferiore fare clic su **ruoli**.  
  
    ![Ruoli di controllo di accesso](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_01.jpg)  
  
3.  Fare clic con il pulsante destro del mouse su **ruoli**, quindi scegliere **Aggiungi ruolo utente**.  
  
    ![Aggiungi ruolo utente](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_02.jpg)  
  
4.  Verrà visualizzata la finestra di dialogo **Aggiungi o modifica ruolo** . In **nome**Digitare un nome per il ruolo che rende chiaro la funzione del ruolo. Se, ad esempio, si desidera creare un ruolo che consenta agli amministratori di gestire i record di risorse SRV DNS, è possibile denominare il ruolo **IPAMSrv**. Se necessario, scorrere le **operazioni** per individuare il tipo di operazioni che si desidera definire per il ruolo. Per questo esempio, scorrere fino a **operazioni di gestione dei record di risorse DNS**.  
  
    ![Operazioni di gestione dei record di risorse DNS](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_03.jpg)  
  
5.  Espandere **operazioni di gestione dei record di risorse DNS**, quindi individuare **le operazioni di record SRV**.  
  
    ![Operazioni di record SRV](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_04.jpg)  
  
6.  Espandere e selezionare **operazioni di record SRV**, quindi fare clic su **OK**.  
  
    ![Selezionare le operazioni di record SRV](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_05.jpg)  
  
7.  Nella console del client di gestione indirizzi IP, fare clic sul ruolo appena creato. In **visualizzazione dettagli** vengono visualizzate le operazioni consentite per il ruolo.  
  
    ![Dettagli nuovo ruolo](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_06.jpg)  
  
## <a name="see-also"></a>Vedi anche  
[Controllo degli accessi in base al ruolo](Role-based-Access-Control.md)  
[Gestire Gestione indirizzi IP](Manage-IPAM.md)  
  


