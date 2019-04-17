---
title: Personalizzare un Backup del Server
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 19b2559c-6090-45af-9a08-2eefc28473c8
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d18dca276bccdf672664a5a3c2bd28e0221fff94
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="customize-server-backup"></a>Personalizzare un Backup del Server

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

## <a name="turn-off-server-backup-by-default"></a>Disattivare il Backup del Server per impostazione predefinita  
 È possibile scegliere di disattivare Backup del Server per impostazione predefinita. È necessario impostare il valore di **HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Server\ServerBackup\ProviderDisabled** su 1 per abilitare questa opzione.  
  
 Quando questa chiave è impostata, l'interfaccia utente di Backup Server non verrà esposto tramite Dashboard o finestra di avvio. In questo modo è possibile utilizzare le applicazioni di terze parti per il Backup di Server.  
  
#### <a name="to-add-serverbackupproviderdisabled-registry-key-and-set-the-value-to-1"></a>Per aggiungere ServerBackup\ProviderDisabled? chiave del Registro di sistema, impostare il valore su 1  
  
1.  Nel server, fare clic su **avviare**, fare clic su **eseguire**, tipo **regedit** nel **aprire** textbox, quindi fare clic su **OK**.  
  
2.  Nel riquadro di spostamento, espandere **HKEY_LOCAL_MACHINE**, espandere **SOFTWARE**, espandere **Microsoft**, espandere **Windows Server**, quindi espandere **ServerBackup**.  
  
3.  Fare doppio clic su **ServerBackup**, fare clic su **New**, quindi fare clic su **valore DWARD**.  
  
4.  Per nome, immettere **ProviderDisabled**.  
  
5.  Fare clic sul nome, selezionare **modifica**, immettere **1** per i dati valore e quindi fare clic su **OK**.  
  
## <a name="turn-on-server-backup"></a>Attiva Backup del Server  
 È possibile attivare il Backup di Server se è stato disattivato creando **ProviderDisabled** chiave del Registro di sistema (come descritto in precedenza in questo documento).  
  
 È necessario eliminare la chiave **HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Server\ServerBackup\ProviderDisabled** per poter abilitare il backup di server predefinito, modificare il tipo di avvio del servizio Backup Server Windows Server e riavviare il server.  
  
#### <a name="to-delete-serverbackupproviderdisabled-registry-key"></a>Per eliminare ServerBackup\ProviderDisabled? chiave del Registro di sistema  
  
1.  Nel server, sposta il mouse nell'angolo superiore destro dello schermo e fare clic su **ricerca**.  
  
2.  Nella casella di ricerca, digitare **regedit**, quindi fare clic su di **Regedit** applicazione.  
  
3.  Nel riquadro di spostamento, espandere **HKEY_LOCAL_MACHINE**, espandere **SOFTWARE**, espandere **Microsoft**, espandere **Windows Server**, quindi espandere **ServerBackup**.  
  
4.  Fare doppio clic su **ProviderDisabled**, quindi fare clic su **eliminare**.  
  
#### <a name="change-the-start-type-of-windows-server-server-backup-service"></a>Modificare il tipo di avvio del servizio di Backup Server Windows Server  
  
1.  Nel server, sposta il mouse nell'angolo superiore destro dello schermo e fare clic su **ricerca**.  
  
2.  Nella casella di ricerca, digitare **services.msc**, quindi fare clic su **servizi** applicazione.  
  
3.  Nel riquadro servizi, fare doppio clic su di **servizio Backup Server Windows Server**e fare clic su **proprietà**.  
  
4.  In **generale** selezionare **automatica** per **tipo di avvio**.  
  
5.  Fare clic su **OK** per chiudere la finestra di dialogo.  
  
#### <a name="restart-the-server"></a>Riavviare il server  
  
1.  Nel server, spostare il mouse nell'angolo in alto a destra dello schermo, fare clic su **impostazioni**, fare clic su **alimentazione**, quindi scegliere Riavvia.