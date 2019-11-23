---
title: Installare il ruolo del server del controller di rete utilizzando Server Manager
description: In questo argomento vengono fornite istruzioni su come installare il ruolo del server del controller di rete utilizzando Server Manager in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 3a6e4352-ff62-4290-b8a4-5c83740070fc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8b656bbd823a10f1e36d1757bb53c4565d4e828c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405841"
---
# <a name="install-the-network-controller-server-role-using-server-manager"></a>Installare il ruolo del server del controller di rete utilizzando Server Manager

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

In questo argomento vengono fornite istruzioni su come installare il ruolo del server del controller di rete utilizzando Server Manager.

>[!IMPORTANT]
>Non distribuire il ruolo del server del controller di rete negli host fisici. Per distribuire il controller di rete, è necessario installare il ruolo del server del controller di rete in una macchina virtuale Hyper-V \(VM\) installata in un host Hyper-V. Dopo aver installato il controller di rete in macchine virtuali in tre host Hyper\-V diversi, è necessario abilitare gli host Hyper\-V per Software Defined Networking \(SDN\) aggiungendo gli host al controller di rete usando il comando di Windows PowerShell **New-NetworkControllerServer**. In questo modo si Abilita il funzionamento del software SDN Load Balancer. Per ulteriori informazioni, vedere [New-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).
  
Dopo aver installato il controller di rete, è necessario utilizzare i comandi di Windows PowerShell per la configurazione del controller di rete aggiuntivo. Per ulteriori informazioni, vedere [deploy Network controller using Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
### <a name="to-install-network-controller"></a>Per installare il controller di rete  
  
1.  In Server Manager fare clic su **Gestione**e quindi su **Aggiungi ruoli e funzionalità**. Si apre la procedura guidata Aggiungi ruoli e funzionalità. Fai clic su **Next**.  
  
2.  In **Selezione tipo di installazione**, Mantieni l'impostazione predefinita e fai clic su **Avanti**.  
  
3.  In **Selezione server di destinazione**scegliere il server in cui si desidera installare il controller di rete e quindi fare clic su **Avanti**.  
  
4.  In **Selezione ruoli server**, in **ruoli**, fare clic su **controller di rete**.  
  
    ![Ruolo del server del controller di rete](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
5.  Verrà visualizzata la finestra di dialogo **Aggiungi le funzionalità necessarie per il controller di rete** . Fare clic su **Aggiungi funzionalità**.  
  
    ![Aggiungere funzionalità per il controller di rete](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_06.jpg)  
  
6.  In **Selezione ruoli server**fare clic su **Avanti**.  
  
    ![Fare clic su Avanti](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
7.  In **Selezione funzionalità**fare clic su **Avanti**.  
  
8.  Nel **controller di rete** fare clic su **Avanti**.  
  
9. In **Conferma selezioni**per l'installazione rivedere le scelte effettuate. Per l'installazione del controller di rete è necessario riavviare il computer dopo l'esecuzione della procedura guidata. Per questo motivo, fare clic su **Riavvia automaticamente il server di destinazione, se necessario**. Verrà visualizzata la finestra di dialogo **Aggiunta guidata ruoli e funzionalità** . fare clic su **Sì**.  
  
    ![Aggiunta guidata ruoli e funzionalità](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_11.jpg)  
  
10. In **Conferma selezioni per l'installazione**, fare clic su **installare**.  
  
11. Il ruolo del server del controller di rete viene installato nel server di destinazione e quindi il server viene riavviato.  
  
12. Dopo il riavvio del computer, accedere al computer e verificare l'installazione del controller di rete visualizzando Server Manager.  
  
    ![Server Manager](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/nc_013.jpg)  
  
## <a name="see-also"></a>Vedi anche  
[Controller di rete](Network-Controller.md)  
  


