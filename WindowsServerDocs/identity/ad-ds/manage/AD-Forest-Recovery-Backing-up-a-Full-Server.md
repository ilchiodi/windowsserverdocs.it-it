---
title: Ripristino della foresta di Active Directory-backup di un server completo
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adds
ms.openlocfilehash: 4377c1d993b4f6d30cf8ca8a7d149b741d7f8d2f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369356"
---
# <a name="ad-forest-recovery---backing-up-a-full-server"></a>Ripristino della foresta di Active Directory-backup di un server completo  

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

È consigliabile eseguire un backup completo del server per prepararsi al ripristino di una foresta, perché può essere ripristinato in un hardware diverso o in un'istanza diversa del sistema operativo.  Utilizzando Windows Server Backup è possibile eseguire un backup completo del server. 

## <a name="windows-server-backup"></a>Windows Server Backup

Per impostazione predefinita, Windows Server Backup non è installato. In Windows Server 2016 e Windows Server 2012 R2 installarlo attenendosi alla procedura descritta di seguito.

>[!NOTE]
>Tenere presente che la procedura può variare leggermente tra Windows Server 2016 e Windows Server 2012 R2.

Per la procedura di installazione in Windows Server 2008 e Windows Server 2008 R2, vedere [installazione di Windows Server Backup](https://technet.microsoft.com/library/cc771232.aspx).  

### <a name="to-install-windows-server-backup"></a>Per installare Windows Server Backup

1. Aprire **Server Manager** e fare clic su **Aggiungi ruoli e funzionalità**.
2. Nell' **Aggiunta guidata ruoli e funzionalità** fare clic su **Avanti**.
3. Nella schermata **tipo di installazione** lasciare l'installazione predefinita basata su **ruoli o basata su funzionalità** e fare clic su **Avanti**.
4. Nella schermata **Selezione server** fare clic su **Avanti**.
5. Nella schermata **ruoli server** fare clic su **Avanti**.
6. Nella schermata **funzionalità** selezionare **Windows Server Backup** e fare clic su **Avanti**
    @ no__t-4Install backup @ no__t-5
7. Fare clic su **Installa**.
8. Al termine dell'installazione, fare clic su **Chiudi**.

### <a name="to-perform-a-backup-with-windows-server-backup"></a>Per eseguire un backup con Windows Server Backup

1. Aprire **Server Manager**, fare clic su **strumenti**e quindi su **Windows Server Backup**.
   - In Windows Server 2008 R2 e Windows Server 2008, fare clic su **Start**, scegliere **strumenti di amministrazione**e quindi fare clic su **Windows Server Backup**.

   ![Installare il backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png) 

2. Se richiesto, nella finestra di dialogo **controllo account utente** specificare le credenziali dell'operatore di backup e quindi fare clic su **OK**.
3. Fare clic su **backup locale**.
4. Scegliere **Backup unico** dal menu **Azione**.
5. Nella pagina **Opzioni di backup** della creazione guidata backup unico fare clic su **opzioni diverse**e quindi su **Avanti**.

   ![Installare il backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)

6. Nella pagina **Selezione configurazione di backup** fare clic su **server completo (scelta consigliata)** , quindi fare clic su **Avanti**.
7. Nella pagina **impostazione tipo di destinazione** fare clic **su unità locali** o su **cartella condivisa remota**, quindi fare clic su **Avanti**.
8. Nella pagina **Selezione destinazione di backup** scegliere il percorso di backup.  Se si seleziona unità locale scegliere un'unità locale o se è stata selezionata l'opzione condivisione remota, scegliere una condivisione di rete.
9. Nella schermata di conferma fare clic su **backup**.

   ![Installare il backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup4.png)

10. Al termine, fare clic su **Chiudi**.
11. Chiudere Windows Server Backup.

>[!NOTE]
>Se viene visualizzato un errore che informa che non è disponibile alcun percorso di archiviazione di backup, sarà necessario escludere uno dei volumi selezionati o aggiungere un nuovo volume o una condivisione remota.
>Se viene visualizzato un avviso che informa che il volume selezionato è incluso anche nell'elenco di elementi di cui eseguire il backup, determinare se rimuovere o meno, quindi fare clic su **OK**.

## <a name="using-wbadminexe-to-backup-a-windows-server"></a>Utilizzo di Wbadmin. exe per eseguire il backup di un server Windows

Wbadmin. exe è un'utilità da riga di comando che consente di eseguire il backup e il ripristino del sistema operativo, di volumi, file, cartelle e applicazioni da un prompt dei comandi.

### <a name="to-perform-a-full-server-backup-using-wbadminexe"></a>Per eseguire un backup completo del server utilizzando WBADMIN. exe
  
- Aprire un prompt dei comandi con privilegi elevati, digitare il comando seguente e premere INVIO:  

   ```
   wbadmin start backup -backuptarget:<Drive_letter_to store_backup>: -include:<Drive_letter_to_include>:
   ```

   ![Installare il backup](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup5.png)

## <a name="next-steps"></a>Passaggi successivi

- [Guida al ripristino della foresta di Active Directory](AD-Forest-Recovery-Guide.md)
- [Ripristino della foresta di Active Directory - Procedure](AD-Forest-Recovery-Procedures.md)
