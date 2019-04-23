---
title: Ripristino della foresta Active Directory - backup di un server completo
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adds
ms.openlocfilehash: fec8de8ea1dadb392f6a3bd1c881e8df2266f404
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846522"
---
# <a name="ad-forest-recovery---backing-up-a-full-server"></a>Ripristino della foresta Active Directory - backup di un server completo  

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Un backup completo del server è consigliabile preparare per il ripristino di una foresta perché può essere ripristinato a un hardware diverso o in un'istanza diversa del sistema operativo.  Con Windows Server Backup è possibile eseguire un backup completo del server. 

## <a name="windows-server-backup"></a>Windows Server Backup

Windows Server Backup non è installato per impostazione predefinita. In Windows Server 2016 e Windows Server 2012 R2, installarlo seguendo la procedura seguente.

>[!NOTE]
>Tenere presente che la procedura può variare leggermente tra Windows Server 2016 e Windows Server 2012 R2.

Per istruzioni per installarlo in Windows Server 2008 e Windows Server 2008 R2, vedere [installazione di Windows Server Backup](https://technet.microsoft.com/library/cc771232.aspx).  

### <a name="to-install-windows-server-backup"></a>Per installare Windows Server Backup

1. Aprire **Server Manager** e fare clic su **Aggiungi ruoli e funzionalità**.
2. Nel **Aggiunta guidata ruoli e funzionalità** fare clic su **successivo**.
3. Nel **tipo di installazione** lasciare il valore predefinito **installazione basata su ruoli o basata su funzionalità** e fare clic su **Next**.
4. Nel **selezione del Server** schermata, fare clic su **successivo**.
5. Nel **ruoli predefiniti del Server** dello schermo fare clic su **successivo**.
6. Nel **caratteristiche** schermata, seleziona **Windows Server Backup** e fare clic su **successiva**
   ![installare Backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup2.png)
7. Fare clic su **Installa**.
8. Al termine dell'installazione, fare clic su **Chiudi**.

### <a name="to-perform-a-backup-with-windows-server-backup"></a>Per eseguire un backup con Windows Server Backup

1. Aprire **Server Manager**, fare clic su **Tools**, quindi fare clic su **Windows Server Backup**.
   - In Windows Server 2008 R2 e Windows Server 2008, fare clic su **avviare**, scegliere **strumenti di amministrazione**, quindi fare clic su **Windows Server Backup**.

   ![Installazione di Backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png) 

2. Se viene richiesto, nelle **User Account Control** finestra di dialogo, fornire le credenziali di Backup Operator e quindi fare clic su **OK**.
3. Fare clic su **Backup locale**.
4. Scegliere **Backup unico** dal menu **Azione**.
5. Nella creazione guidata Backup unico, nel **opzioni di Backup** pagina, fare clic su **diverse opzioni**e quindi fare clic su **Avanti**.

   ![Installazione di Backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)

6. Nel **Seleziona configurazione backup** pagina, fare clic su **completo del server (scelta consigliato)** e quindi fare clic su **Avanti**.
7. Nel **impostazione tipo destinazione** pagina, fare clic su **unità locali** o **cartella condivisa remota**e quindi fare clic su **Avanti**.
8. Nel **Seleziona destinazione di Backup** pagina, scegliere il percorso di backup.  Se si seleziona locale unità scegliere un'unità locale o se è stato selezionato remoto condivisione scegliere una condivisione di rete.
9. Nella schermata di conferma, fare clic su **Backup**.

   ![Installazione di Backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup4.png)

10. Al termine fare clic su **Chiudi**.
11. Chiudere Windows Server Backup.

>[!NOTE]
>Se si verifica un errore che informa che non è disponibile alcun percorso di archiviazione di backup, è necessario se escludere uno dei volumi che è stato selezionato o aggiungere un nuovo volume o una condivisione remota.
>Se viene visualizzato un avviso che informa che il volume selezionato è incluso anche nell'elenco di elementi per backup, determinare se visualizzare o meno rimuovere e fare clic su **accettabile**.

## <a name="using-wbadminexe-to-backup-a-windows-server"></a>Uso di Wbadmin.exe per eseguire il backup di windows server

Wbadmin.exe è un'utilità della riga di comando che consente di eseguire il backup e ripristinare il sistema operativo, volumi, file, cartelle e le applicazioni da un prompt dei comandi.

### <a name="to-perform-a-full-server-backup-using-wbadminexe"></a>Per eseguire un backup completo del server con Wbadmin.exe
  
- Aprire un prompt dei comandi con privilegi elevati, digitare il comando seguente e premere INVIO:  

   ```
   wbadmin start backup -backuptarget:<Drive_letter_to store_backup>: -include:<Drive_letter_to_include>:
   ```

   ![Installazione di Backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup5.png)

## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta AD](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)
