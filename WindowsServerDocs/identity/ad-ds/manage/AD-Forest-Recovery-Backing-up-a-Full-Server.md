---
title: Ripristino della foresta Active Directory - backup di un intero server
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adfs
ms.openlocfilehash: b1af97c2eb23d65c2d106906bc0f5bb1f10b23ec
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---backing-up-a-full-server"></a>Ripristino della foresta Active Directory - backup di un intero server  

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Un backup completo del server è consigliabile preparare per il ripristino di un insieme di strutture, perché può essere ripristinato per diversi tipi di hardware o un'istanza diversa del sistema operativo.  Con Windows Server Backup è possibile eseguire un backup completo del server. 

## <a name="windows-server-backup"></a>Windows Server Backup
Windows Server Backup non è installato per impostazione predefinita. In Windows Server 2016 e Windows Server 2012 R2, installarlo seguendo la procedura seguente.

>[!NOTE]
>Tenere presente che i passaggi possono variare leggermente tra Windows Server 2016 e Windows Server 2012 R2.

Per eseguire l'installazione in Windows Server 2008 e Windows Server 2008 R2, vedere [l'installazione di Windows Server Backup](https://technet.microsoft.com/library/cc771232.aspx).  

### <a name="to-install-windows-server-backup"></a>Per installare Windows Server Backup
1. Apri **Server Manager** e fare clic su **Aggiungi ruoli e funzionalità**.
2. Nel **Aggiunta guidata ruoli e funzionalità** fare clic su **Avanti**.
3. Nel **tipo di installazione** schermata, lasciare il valore predefinito **installazione basata su ruoli o basata su funzionalità** e fare clic su **Avanti**.
4. Nel **selezione dei Server** schermata, fare clic su **Avanti**.
5. Nel **ruoli Server** fare clic su schermo **Avanti**.
6. Nel **funzionalità** schermo, seleziona **Windows Server Backup** e fare clic su **Avanti**<ph x="4">
! [</ph> Installare Backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup2.png)
7. Fare clic su **installare**.
8. Una volta completata l'installazione, fare clic su **Chiudi**.


### <a name="to-perform-a-backup-with-windows-server-backup"></a>Per eseguire un backup con Windows Server Backup

1. Apri **Server Manager**, fare clic su **strumenti**, quindi fare clic su **Windows Server Backup**.
    - In Windows Server 2008 R2 e Windows Server 2008, fare clic su **Start**, scegliere **strumenti di amministrazione**, quindi fare clic su **Windows Server Backup**. 
![Installare Backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png) 
2. Se viene richiesto, nel **controllo dell'Account utente** nella finestra di dialogo delle credenziali di Backup Operators e quindi fare clic su **OK**.
3. Fare clic su **Backup locale**.
4. Nel **azione** menu, fare clic su **Backup unico**.
5. Nella creazione guidata Backup unico, nel **le opzioni di Backup** pagina, fare clic su **diverse opzioni**, quindi fare clic su **Avanti**.
![Installare Backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)
6. Nel **la configurazione del backup seleziona** pagina, fare clic su **server completo (consigliato)**e quindi fare clic su **Avanti**.
7. Nel **specificare il tipo di destinazione** pagina, fare clic su **unità locali** o **cartella condivisa remota**, quindi fare clic su **Avanti**.
8. Nel **Seleziona destinazione di Backup**, selezionare il percorso di backup.  Se si è selezionato locale unità scegliere un'unità locale o se è stato selezionato remoto condivisione scegliere una condivisione di rete.
9. Nella schermata di conferma, fare clic su **Backup**.
![Installare Backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup4.png)
10. Una volta completata questa operazione, fare clic su **Chiudi**.
11. Chiudere Windows Server Backup.

>[!NOTE]
>Se ricevi un errore che informa che non è disponibile alcun percorso di archiviazione del backup, è necessario escludere uno dei volumi in cui è stato selezionato o aggiungere un nuovo volume o una condivisione remota.
>Se viene visualizzato un avviso che informa che il volume selezionato è anche incluso nell'elenco di elementi per il backup, determinare se rimuovere e fare clic su o meno **Ok**.

## <a name="using-wbadminexe-to-backup-a-windows-server"></a>Utilizzando Wbadmin.exe per eseguire il backup di un server windows
Wbadmin.exe è un'utilità della riga di comando che consente di eseguire il backup e ripristinare il sistema operativo, volumi, file, cartelle e le applicazioni da un prompt dei comandi.

#### <a name="to-perform-a-full-server-backup-using-wbadminexe"></a>Per eseguire un backup completo del server utilizzando Wbadmin.exe  
  
1.  Aprire un prompt dei comandi con privilegi elevati, digitare il comando seguente e premere INVIO:  

        wbadmin start backup -backuptarget:<Drive_letter_to store_backup>: -include:<Drive_letter_to_include>:

![Installare Backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup5.png)
## <a name="next-steps"></a>Passaggi successivi

- [Guida di ripristino della foresta Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)
