---
title: Installare il ruolo di Server di Controller di rete con Server Manager
description: Questo argomento fornisce istruzioni su come installare il ruolo del server Controller di rete con Server Manager in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 3a6e4352-ff62-4290-b8a4-5c83740070fc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 699e2ca5c4ec33099d0ad948523b6f587ad118e4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859062"
---
# <a name="install-the-network-controller-server-role-using-server-manager"></a>Installare il ruolo di Server di Controller di rete con Server Manager

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Questo argomento fornisce istruzioni su come installare il ruolo del server Controller di rete con Server Manager.

>[!IMPORTANT]
>Non distribuire il ruolo del server Controller di rete in host fisici. Per distribuire Controller di rete, è necessario installare il ruolo del server Controller di rete in una macchina virtuale Hyper-V \(VM\) che viene installato in un host Hyper-V. Dopo aver installato il Controller di rete nelle macchine virtuali in tre diverse Hyper\-host V, è necessario abilitare il Hyper\-host V per Software Defined Networking \(SDN\) mediante l'aggiunta di host al Controller di rete usando il comando di Windows PowerShell **New-NetworkControllerServer**. In questo modo, si sta abilitando il bilanciamento del carico Software SDN alla funzione. Per altre informazioni, vedere [New-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).
  
Dopo aver installato il Controller di rete, è necessario utilizzare i comandi di Windows PowerShell per la configurazione di Controller di rete aggiuntiva. Per altre informazioni, vedere [distribuire Controller di rete tramite Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
### <a name="to-install-network-controller"></a>Per installare Controller di rete  
  
1.  In Server Manager fare clic su **Gestione**e quindi su **Aggiungi ruoli e funzionalità**. Verrà visualizzata la procedura guidata Aggiungi ruoli e funzionalità. Fare clic su **Avanti**.  
  
2.  Nelle **Select Installation Type**, mantenere l'impostazione predefinita e fare clic su **successivo**.  
  
3.  Nelle **selezione Server di destinazione**, scegliere il server in cui si desidera installare Controller di rete e quindi fare clic su **successivo**.  
  
4.  Nelle **Selezione ruoli Server**, nel **ruoli**, fare clic su **Controller di rete**.  
  
    ![Ruolo server di Controller di rete](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
5.  Il **aggiungere le funzionalità necessarie per il Controller di rete** verrà visualizzata la finestra di dialogo. Fare clic su **aggiungere funzionalità**.  
  
    ![Aggiunta di funzionalità per il Controller di rete](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_06.jpg)  
  
6.  Nelle **Selezione ruoli Server**, fare clic su **successivo**.  
  
    ![Fare clic su Avanti](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
7.  Nelle **Selezione funzionalità**, fare clic su **successivo**.  
  
8.  Nelle **Controller di rete** fare clic su **successivo**.  
  
9. Nelle **Conferma selezioni per l'installazione**, rivedere le scelte effettuate. Installazione del Controller di rete richiede il riavvio del computer dopo l'esecuzione della procedura guidata. Per questo motivo, fare clic su **riavvia automaticamente il server di destinazione se necessario**. Il **Aggiunta guidata ruoli e funzionalità** verrà visualizzata la finestra di dialogo. Scegliere **Sì**.  
  
    ![Aggiunta guidata ruoli e funzionalità](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_11.jpg)  
  
10. In **Conferma selezioni per l'installazione**, fare clic su **installare**.  
  
11. Installa il ruolo del server Controller di rete nel server di destinazione e quindi il riavvio del server.  
  
12. Dopo il riavvio del computer, accedere al computer e verificare l'installazione di Controller di rete per la visualizzazione dei Server di gestione.  
  
    ![Server Manager](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/nc_013.jpg)  
  
## <a name="see-also"></a>Vedere anche  
[Controller di rete](Network-Controller.md)  
  


