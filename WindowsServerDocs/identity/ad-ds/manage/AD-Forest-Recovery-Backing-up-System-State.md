---
title: Ripristino della foresta Active Directory - backup di un intero server
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 9238cb27-0020-42f7-90d6-fcebf7e3c0bc
ms.technology: identity-adfs
ms.openlocfilehash: a86d61536f8b426e1a5258c661d4e53da63d4162
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---backing-up-the-system-state-data"></a>Ripristino della foresta Active Directory - eseguire il backup dello stato del sistema  

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2
 
Utilizzare la procedura seguente per eseguire un backup dello stato del sistema su un controller di dominio tramite Windows Server Backup o wbadmin.exe.  
  
## <a name="to-perform-a-system-state-backup-using-windows-server-backup"></a>Per eseguire un backup dello stato del sistema tramite Windows Server Backup  
1. Apri **Server Manager**, fare clic su **strumenti**, quindi fare clic su **Windows Server Backup**.
    - In Windows Server 2008 R2 e Windows Server 2008, fare clic su **Start**, scegliere **strumenti di amministrazione**, quindi fare clic su **Windows Server Backup**. 
![Installare Backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png) 
2. Se viene richiesto, nel **controllo dell'Account utente** nella finestra di dialogo delle credenziali di Backup Operators e quindi fare clic su **OK**.
3. Fare clic su **Backup locale**.
4. Nel **azione** menu, fare clic su **Backup unico**.
5. Nella creazione guidata Backup unico, nel **le opzioni di Backup** pagina, fare clic su **diverse opzioni**, quindi fare clic su **Avanti**.
![Installare Backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)
6. Nel **la configurazione del backup seleziona** pagina, fare clic su **personalizzata)**e quindi fare clic su **Avanti**.
7. Nel **selezione elementi per Backup** schermata, fare clic su **Aggiungi elementi** e seleziona **dello stato del sistema** e fare clic su **Ok**.
    - In Windows Server 2008 R2 e Windows Server 2008, selezionare i volumi da includere nel backup. Se si seleziona il **abilitare il ripristino di sistema** casella di controllo sono selezionati tutti i volumi critici. 
![Installare Backup](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup.png)  
8. Nel **specificare il tipo di destinazione** pagina, fare clic su **unità locali** o **cartella condivisa remota**, quindi fare clic su **Avanti**.  Se esegue il backup in una cartella condivisa remota, eseguire le operazioni seguenti:  
  
 1.  Digitare il percorso alla cartella condivisa.  
 2.  In **il controllo degli accessi**selezionare **non ereditano** o **eredita** per determinare l'accesso per il backup e quindi fare clic su **Avanti**.  
 3.  Nel **fornire le credenziali utente per il Backup** la finestra di dialogo, fornire il nome utente e la password per un utente che ha accesso in scrittura alla cartella condivisa, quindi fare clic su **OK**.
9. Per Windows Server 2008 R2 e Windows Server 2008, nel **specificare opzioni avanzate** selezionare **backup di copia VSS** e quindi fare clic su **Avanti**.
10. Nel **Seleziona destinazione di Backup**, selezionare il percorso di backup.  Se si è selezionato locale unità scegliere un'unità locale o se è stato selezionato remoto condivisione scegliere una condivisione di rete.
11. Nella schermata di conferma, fare clic su **Backup**.
12. Una volta completata questa operazione, fare clic su **Chiudi**.
13. Chiudere Windows Server Backup.

  
## <a name="to-perform-a-system-state-backup-using-wbadminexe"></a>Per eseguire un backup dello stato del sistema utilizzando Wbadmin.exe  
  
1.  Aprire un prompt dei comandi con privilegi elevati, digitare il comando seguente e premere INVIO:  
  
    ```  
    wbadmin start systemstatebackup -backuptarget:<targetDrive>:
    ```  
![Installare Backup](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup2.png)  

## <a name="next-steps"></a>Passaggi successivi

- [Guida di ripristino della foresta Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)
