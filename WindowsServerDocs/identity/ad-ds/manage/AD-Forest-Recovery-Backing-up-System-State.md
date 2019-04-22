---
title: Ripristino della foresta Active Directory - backup di un server completo
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 9238cb27-0020-42f7-90d6-fcebf7e3c0bc
ms.technology: identity-adds
ms.openlocfilehash: a5306960bb2dca3849bdb4fc7304781af3f25335
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815872"
---
# <a name="ad-forest-recovery---backing-up-the-system-state-data"></a>Ripristino della foresta Active Directory - backup dei dati dello stato del sistema  

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Utilizzare la procedura seguente per eseguire un backup dello stato del sistema in un controller di dominio con Windows Server Backup o wbadmin.exe.  

## <a name="to-perform-a-system-state-backup-using-windows-server-backup"></a>Per eseguire un backup dello stato del sistema tramite Windows Server Backup

1. Aprire **Server Manager**, fare clic su **Tools**, quindi fare clic su **Windows Server Backup**.
   - In Windows Server 2008 R2 e Windows Server 2008, fare clic su **avviare**, scegliere **strumenti di amministrazione**, quindi fare clic su **Windows Server Backup**. 

   ![Installazione di Backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png)

2. Se viene richiesto, nelle **User Account Control** finestra di dialogo, fornire le credenziali di Backup Operator e quindi fare clic su **OK**.
3. Fare clic su **Backup locale**.
4. Scegliere **Backup unico** dal menu **Azione**.
5. Nella creazione guidata Backup unico, nel **opzioni di Backup** pagina, fare clic su **diverse opzioni**e quindi fare clic su **Avanti**.

   ![Installazione di Backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)

6. Nella **Seleziona configurazione backup** fare clic su **personalizzata)** e quindi fare clic su **Avanti**.
7. Nel **Seleziona elementi per il Backup** schermata, fare clic su **Aggiunta elementi** e selezionare **dello stato del sistema** e fare clic su **Ok**.
   - In Windows Server 2008 R2 e Windows Server 2008, selezionare i volumi da includere nel backup. Se si seleziona il **abilitare il ripristino di sistema** casella di controllo, vengono selezionati tutti i volumi critici. 

   ![Installazione di Backup](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup.png)  

8. Nel **impostazione tipo destinazione** pagina, fare clic su **unità locali** o **cartella condivisa remota**e quindi fare clic su **Avanti**.  Se esegue il backup in una cartella condivisa remota, eseguire le operazioni seguenti:  
   - Digitare il percorso alla cartella condivisa.
   - Sotto **controllo di accesso**, selezionare **non ereditano** oppure **eredita** determinano l'accesso in base al backup e quindi fare clic su **Avanti**.  
   - Nel **fornire le credenziali utente per il Backup** finestra di dialogo, specificare il nome utente e la password per un utente con accesso in scrittura alla cartella condivisa e quindi fare clic su **OK**.

9. Per Windows Server 2008 R2 e Windows Server 2008, nelle **specificare opzioni avanzate** pagina, selezionare **backup VSS copia** e quindi fare clic su **Avanti**.
10. Nel **Seleziona destinazione di Backup** pagina, scegliere il percorso di backup.  Se si seleziona locale unità scegliere un'unità locale o se è stato selezionato remoto condivisione scegliere una condivisione di rete.
11. Nella schermata di conferma, fare clic su **Backup**.
12. Al termine fare clic su **Chiudi**.
13. Chiudere Windows Server Backup.

## <a name="to-perform-a-system-state-backup-using-wbadminexe"></a>Per eseguire un backup dello stato del sistema usando Wbadmin.exe

Aprire un prompt dei comandi con privilegi elevati, digitare il comando seguente e premere INVIO:  
  
   ```
   wbadmin start systemstatebackup -backuptarget:<targetDrive>:
   ```

   ![Installazione di Backup](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup2.png)  

## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta AD](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)
