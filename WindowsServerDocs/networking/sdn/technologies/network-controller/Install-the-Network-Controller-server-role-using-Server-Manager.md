---
title: Installare il ruolo di Server di Controller di rete con Server Manager
description: In questo argomento vengono fornite istruzioni su come installare il ruolo server di Controller di rete tramite Server Manager in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 3a6e4352-ff62-4290-b8a4-5c83740070fc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 15cb1ef3bad7038cc97784504807b44b4920def6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-network-controller-server-role-using-server-manager"></a>Installare il ruolo di Server di Controller di rete con Server Manager

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

In questo argomento vengono fornite istruzioni su come installare il ruolo server di Controller di rete tramite Server Manager.

>[!IMPORTANT]
>Non distribuire il ruolo server di Controller di rete in host fisici. Per distribuire Controller di rete, è necessario installare il ruolo server di Controller di rete in una macchina virtuale Hyper-V \(VM\) che viene installato in un host Hyper-V. Dopo aver installato il Controller di rete nelle macchine virtuali in tre host Hyper\ V diversi, è necessario attivare gli host Hyper\-V per la rete definita dal Software \(SDN\) mediante l'aggiunta di host al Controller di rete utilizzando il comando di Windows PowerShell **New-NetworkControllerServer**. In questo modo, si desidera abilitare il bilanciamento del carico Software SDN alla funzione. Per ulteriori informazioni, vedere [New-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).
  
Dopo l'installazione di Controller di rete, è necessario utilizzare i comandi di Windows PowerShell per la configurazione di Controller di rete aggiuntive. Per ulteriori informazioni, vedere [distribuire Controller di rete tramite Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
### <a name="to-install-network-controller"></a>Per installare Controller di rete  
  
1.  In Server Manager, fare clic su **Gestisci**, quindi fare clic su **Aggiungi ruoli e funzionalità**. Apre la procedura guidata Aggiungi ruoli e funzionalità. Fare clic su **Avanti**.  
  
2.  In **Selezione tipo di installazione**, mantenere le impostazioni predefinite e fare clic su **Avanti**.  
  
3.  In **selezione Server di destinazione**, scegliere il server in cui si desidera installare Controller di rete e quindi fare clic su **Avanti**.  
  
4.  In **Selezione ruoli Server**, in **ruoli**, fare clic su **Controller di rete**.  
  
    ![Ruolo server di Controller di rete](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
5.  Il **Aggiungi funzionalità necessarie per il Controller di rete** apre la finestra di dialogo. Fare clic su **aggiungere funzionalità**.  
  
    ![Aggiungere funzionalità per i Controller di rete](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_06.jpg)  
  
6.  In **Selezione ruoli Server**, fare clic su **Avanti**.  
  
    ![Fare clic su Avanti](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_07.jpg)  
  
7.  In **Selezione funzionalità**, fare clic su **Avanti**.  
  
8.  In **Controller di rete** fare clic su **Avanti**.  
  
9. In **Conferma selezioni per l'installazione**, esaminare le impostazioni selezionate. Installazione di Controller di rete è necessario riavviare il computer dopo l'esecuzione della procedura guidata. Per questo motivo, fare clic su **riavvia automaticamente il server di destinazione se necessario**. Il **Aggiunta guidata ruoli e funzionalità** apre la finestra di dialogo. Fare clic su **Sì**.  
  
    ![Aggiunta guidata ruoli e funzionalità](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/netc_install_11.jpg)  
  
10. In **Conferma selezioni per l'installazione**, fare clic su **installare**.  
  
11. Installa il ruolo server di Controller di rete nel server di destinazione e quindi il server viene riavviato.  
  
12. Dopo il riavvio del computer, accedere al computer e verificare l'installazione di Controller di rete per la visualizzazione di Server Manager.  
  
    ![Server Manager](../../../media/Install-the-Network-Controller-server-role-using-Server-Manager/nc_013.jpg)  
  
## <a name="see-also"></a>Vedere anche  
[Controller di rete](Network-Controller.md)  
  


