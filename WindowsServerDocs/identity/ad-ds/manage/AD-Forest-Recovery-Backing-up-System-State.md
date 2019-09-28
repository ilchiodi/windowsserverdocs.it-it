---
title: Ripristino della foresta di Active Directory-backup di un server completo
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 9238cb27-0020-42f7-90d6-fcebf7e3c0bc
ms.technology: identity-adds
ms.openlocfilehash: 14aa7abc19573b76ebc144cb6dea5f510b45e269
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369354"
---
# <a name="ad-forest-recovery---backing-up-the-system-state-data"></a>Ripristino della foresta di Active Directory: backup dei dati sullo stato del sistema  

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Utilizzare la procedura seguente per eseguire un backup dello stato del sistema in un controller di dominio utilizzando Windows Server Backup o Wbadmin. exe.  

## <a name="to-perform-a-system-state-backup-using-windows-server-backup"></a>Per eseguire un backup dello stato del sistema utilizzando Windows Server Backup

1. Aprire **Server Manager**, fare clic su **strumenti**e quindi su **Windows Server Backup**.
   - In Windows Server 2008 R2 e Windows Server 2008, fare clic su **Start**, scegliere **strumenti di amministrazione**e quindi fare clic su **Windows Server Backup**. 

   ![Installare il backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png)

2. Se richiesto, nella finestra di dialogo **controllo account utente** specificare le credenziali dell'operatore di backup e quindi fare clic su **OK**.
3. Fare clic su **backup locale**.
4. Scegliere **Backup unico** dal menu **Azione**.
5. Nella pagina **Opzioni di backup** della creazione guidata backup unico fare clic su **opzioni diverse**e quindi su **Avanti**.

   ![Installare il backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)

6. Nella pagina **Selezione configurazione di backup** fare clic su **personalizzata)** , quindi fare clic su **Avanti**.
7. Nella schermata **Seleziona elementi per backup** fare clic su **Aggiungi elementi** e selezionare **stato del sistema** , quindi fare clic su **OK**.
   - In Windows Server 2008 R2 e Windows Server 2008 selezionare i volumi da includere nel backup. Se si seleziona la casella di controllo **Abilita ripristino di sistema** , vengono selezionati tutti i volumi critici. 

   ![Installare il backup](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup.png)  

8. Nella pagina **impostazione tipo di destinazione** fare clic **su unità locali** o su **cartella condivisa remota**, quindi fare clic su **Avanti**.  Se si esegue il backup in una cartella condivisa remota, eseguire le operazioni seguenti:  
   - Digitare il percorso della cartella condivisa.
   - In **controllo di accesso**selezionare **non ereditare** o **eredita** per determinare l'accesso al backup, quindi fare clic su **Avanti**.  
   - Nella finestra di dialogo **specificare le credenziali utente per il backup** specificare il nome utente e la password di un utente che dispone dell'accesso in scrittura alla cartella condivisa e quindi fare clic su **OK**.

9. Per Windows Server 2008 R2 e Windows Server 2008, nella pagina **specifica opzione avanzata** selezionare copia di **backup VSS** , quindi fare clic su **Avanti**.
10. Nella pagina **Selezione destinazione di backup** scegliere il percorso di backup.  Se si seleziona unità locale scegliere un'unità locale o se è stata selezionata l'opzione condivisione remota, scegliere una condivisione di rete.
11. Nella schermata di conferma fare clic su **backup**.
12. Al termine, fare clic su **Chiudi**.
13. Chiudere Windows Server Backup.

## <a name="to-perform-a-system-state-backup-using-wbadminexe"></a>Per eseguire un backup dello stato del sistema utilizzando WBADMIN. exe

Aprire un prompt dei comandi con privilegi elevati, digitare il comando seguente e premere INVIO:  
  
   ```
   wbadmin start systemstatebackup -backuptarget:<targetDrive>:
   ```

   ![Installare il backup](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup2.png)  

## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta di Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta di Active Directory - Procedure](AD-Forest-Recovery-Procedures.md)
