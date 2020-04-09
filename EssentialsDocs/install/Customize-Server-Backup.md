---
title: Personalizzazione del backup del server
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 19b2559c-6090-45af-9a08-2eefc28473c8
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cf39aaedeb266d903e928a589132fff764fed60b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80818134"
---
# <a name="customize-server-backup"></a>Personalizzazione del backup del server

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

## <a name="turn-off-server-backup-by-default"></a>Disattivazione predefinita di Backup server  
 È possibile scegliere di disattivare Backup server per impostazione predefinita. Per abilitare questa opzione, impostare il valore di **HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Server\ServerBackup\ProviderDisabled** su 1.  
  
 Quando si imposta questa chiave, l'interfaccia utente di Backup server non viene visualizzata nel dashboard e nella finestra di avvio. In questo modo è possibile utilizzare applicazioni di terzi per eseguire il backup del server.  
  
#### <a name="to-add-serverbackupproviderdisabled-registry-key-and-set-the-value-to-1"></a>Per aggiungere ServerBackup\ProviderDisabled? chiave del registro di sistema e impostazione del valore su 1  
  
1.  Nel server, fare clic su **Start** e poi su **Esegui**, digitare **regedit** nella casella di testo **Apri** poi selezionare **OK**.  
  
2.  Nel riquadro di spostamento, espandere **HKEY_LOCAL_MACHINE**, **SOFTWARE**, **Microsoft**, **Windows Server** e infine **ServerBackup**.  
  
3.  Fare clic con il pulsante destro del mouse su **ServerBackup**, selezionare **Nuovo** e quindi **Valore DWARD**.  
  
4.  Immettere **ProviderDisabled** come nome.  
  
5.  Fare clic con il pulsante destro del mouse sul nome, scegliere **Modifica**, immettere **1** per il valore e fare clic su **OK**.  
  
## <a name="turn-on-server-backup"></a>Attivazione di Backup server  
 Se Backup server era stato disattivato, è possibile attivarlo creando la chiave del Registro di sistema **ProviderDisabled** (come descritto in precedenza in questo documento).  
  
 È necessario eliminare la chiave **HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Server\ServerBackup\ProviderDisabled** per poter abilitare il backup del server per impostazione predefinita, modificare il tipo di avvio del servizio Backup server Windows Server e riavviare il server.  
  
#### <a name="to-delete-serverbackupproviderdisabled-registry-key"></a>Per eliminare ServerBackup\ProviderDisabled? chiave del registro di sistema  
  
1.  Sul server, spostare il puntatore del mouse verso l'angolo superiore destro dello schermo e fare clic su **Trova**.  
  
2.  Nella casella di ricerca digitare **regedit** e quindi fare clic sull'applicazione **Regedit**.  
  
3.  Nel riquadro di spostamento, espandere **HKEY_LOCAL_MACHINE**, **SOFTWARE**, **Microsoft**, **Windows Server** e infine **ServerBackup**.  
  
4.  Fare clic con il pulsante destro del mouse su **ProviderDisabled** e selezionare **Elimina**.  
  
#### <a name="change-the-start-type-of-windows-server-server-backup-service"></a>Modifica del tipo di avvio del servizio Backup server Windows Server  
  
1.  Sul server, spostare il puntatore del mouse verso l'angolo superiore destro dello schermo e fare clic su **Trova**.  
  
2.  Nella casella di ricerca digitare **services.msc** e quindi fare clic sull'applicazione **Services**.  
  
3.  Nel riquadro dei servizi, fare clic con il pulsante destro del mouse su **Servizio Backup server Windows Server** e selezionare **Proprietà**.  
  
4.  Nella scheda **Generale**, selezionare **Automatico** per **Tipo di avvio**.  
  
5.  Fare clic su **OK** per chiudere la finestra di dialogo.  
  
#### <a name="restart-the-server"></a>Riavvio del server  
  
1.  Sul server, spostare il puntatore del mouse verso l'angolo superiore destro dello schermo e fare clic su **Impostazioni**, **Arresta** e infine su Riavvia.