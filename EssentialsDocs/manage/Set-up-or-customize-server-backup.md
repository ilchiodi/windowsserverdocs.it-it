---
title: Configurare o personalizzare un backup del server
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 441c2d6c-435a-42cb-90f2-6d680d279d34
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 727dd74e4bddc52f735969f216914b9d76d1f413
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="set-up-or-customize-server-backup"></a>Configurare o personalizzare un backup del server

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Backup del server non viene configurato automaticamente durante l'installazione. È consigliabile proteggere automaticamente il server e i relativi dati pianificando backup giornalieri. È consigliabile mantenere un piano di backup giornaliero poiché la maggior parte delle organizzazioni non può permettersi di perdere i dati che sono stati creati in diversi giorni.  
  
 Vedere le sezioni seguenti per configurare o personalizzare un backup del server:  
  
-   [Consente di impostare o modificare le impostazioni di backup server](Set-up-or-customize-server-backup.md#BKMK_1)  
  
-   [Pianificazione di backup server](Set-up-or-customize-server-backup.md#BKMK_2)  
  
-   [Unità di destinazione del backup](Set-up-or-customize-server-backup.md#BKMK_Target)  
  
-   [Elementi per il backup](Set-up-or-customize-server-backup.md#BKMK_4)  
  
##  <a name="BKMK_1"></a>Consente di impostare o modificare le impostazioni di backup server  
  
#### <a name="to-set-up-or-change-server-backup-settings"></a>Per impostare o modificare le impostazioni di backup server  
  
1.  Aprire il **Dashboard**, quindi fare clic sul **dispositivi** scheda.  
  
2.  Nella visualizzazione elenco, fare clic su server per selezionarlo.  
  
3.  Nel riquadro attività fare clic su **imposta backup del server**.  
  
    > [!NOTE]
    >  Se si desidera modificare le impostazioni di backup esistenti, fare clic su **Personalizza Backup del server**.  
  
4.  Seguire le istruzioni della procedura guidata.  
  
    > [!NOTE]
    >  Se si avvia la procedura guidata prima di collegare il disco rigido esterno al server, fare clic su **Aggiorna elenco** nel **selezionare la destinazione di backup** pagina dopo aver collegato il disco rigido.  
  
> [!NOTE]
>  Nell'installazione predefinita di Windows Server Essentials, il server è configurato per eseguire automaticamente una deframmentazione una volta alla settimana. Questo può comportare maggiori di backup normali se si utilizza software di imaging non Microsoft. Se non è necessario deframmentare il server regolarmente, è possibile eseguire la procedura per disattivare la pianificazione della deframmentazione:  
>   
>  1.  Premere il tasto Windows + W per aprire **ricerca**.  
> 2.  Nella casella di testo di ricerca, digitare **Deframmenta**.  
> 3.  Nella sezione dei risultati, fare clic su **deframmenta e ottimizza unità**.  
> 4.  Nel **Ottimizza unità** pagina, selezionare l'unità e quindi fare clic su **modificare le impostazioni**.  
> 5.  Nel **pianificazione ottimizzazione** finestra, deseleziona il **eseguire una pianificazione (scelta consigliata)** casella di controllo, quindi fare clic su **OK** per salvare le modifiche.  
  
##  <a name="BKMK_2"></a>Pianificazione di backup server  
 Quando si utilizza la configurazione guidata Backup Server o la procedura guidata personalizzare Backup Server, è possibile scegliere di eseguire il backup dei dati del server più volte nel corso della giornata. Poiché le procedure guidate pianificano i backup incrementale, i backup eseguiti rapidamente e non sono interessata in modo significativo sulle prestazioni del server. Per impostazione predefinita, le procedure guidate di pianificano un backup da eseguire ogni giorno alle 12:00 e 11:00 PM. Tuttavia, è possibile modificare la pianificazione di backup in base alle esigenze dell'organizzazione. In alcuni casi si deve valutare l'efficacia del piano di backup e modifica il piano in base alle esigenze.  
  
##  <a name="BKMK_Target"></a>Unità di destinazione del backup  
 È possibile utilizzare più unità di archiviazione esterna per i backup e alternare le unità tra percorsi di archiviazione in sede e fuori sede. Ciò può migliorare l'emergenza pianificazione grazie alla possibilità di ripristinare i dati in caso di danni fisici per l'hardware in sede.  
  
 Quando si sceglie un'unità di archiviazione per il backup dei server, considerare quanto segue:  
  
-   Scegliere un'unità che include spazio sufficiente per archiviare i dati. Le unità di archiviazione devono contenere almeno 2,5 volte la capacità di archiviazione dei dati che si desidera eseguire il backup. L'unità devono essere anche dimensioni sufficienti per adeguarsi alla crescita futura dei dati del server.  
  
-   Quando si utilizza un'unità di archiviazione esterna, assicurarsi che l'unità sia vuota o che includa solo dati non è necessario.  
  
    > [!CAUTION]
    >  La configurazione Server Backup guidata formatta le unità di archiviazione quando le configura per il backup.  
  
-   Se l'unità di destinazione del backup include unità offline, la configurazione del backup avrà esito negativo. Per completare la configurazione, quando si seleziona la destinazione del backup, deselezionare la casella di controllo per escludere le unità offline.  
  
-   Se si sceglie un'unità che include backup precedenti come destinazione di backup, la procedura guidata permetterà di scegliere se si desidera conservare i backup precedenti. Se si conservano i backup, la procedura guidata non formattare l'unità.  
  
-   È consigliabile visitare il sito Web del produttore dell'unità di archiviazione esterna per assicurarsi che l'unità di backup sia supportata nei computer che eseguono Windows Server Essentials.  
  
-   L'unità non può contenere una partizione di sistema interfaccia EFI (Extensible Firmware Interface). Se una partizione EFI è presente in un'unità USB, si presuppone che il disco è un disco di avvio. Se si è certi che non è necessario t i dati sul disco, è possibile riformattare il disco e usarlo per i backup.  
  
    > [!CAUTION]
    >  Quando si riformatta il disco verranno eliminati tutti i dati.  
  
    #### <a name="to-remove-an-efi-system-partition-from-a-usb-disk"></a>Per rimuovere una partizione di sistema EFI da un disco USB  
  
    1.  Nel Pannello di controllo, aprire **sistema e sicurezza**.  
  
    2.  In **strumenti di amministrazione**, fare clic su **creare e formattare le partizioni del disco rigido**.  
  
    3.  Eliminare tutti i volumi nel disco USB oppure eliminare semplicemente la partizione EFI. La configurazione guidata Backup Server elimina tutti i volumi.  
  
-   L'unità non può contenere cartelle condivise del server. Prima di poter utilizzare il disco come unità di destinazione del backup, è necessario interrompere la condivisione in cartelle condivise del server. È possibile interrompere la condivisione dal Dashboard o in Esplora File.  
  
    #### <a name="to-stop-sharing-on-a-server-folder-from-the-dashboard"></a>Per interrompere la condivisione in una cartella del server dal Dashboard  
  
    1.  Nel Dashboard, fare clic su **archiviazione**, quindi fare clic su **cartelle Server**.  
  
    2.  Selezionare la cartella che si desidera interrompere la condivisione e quindi nel riquadro attività fare clic su **arrestare**.  
  
> [!NOTE]
>  Se un backup non riesce perché l'unità di backup è insufficiente, la lettera di unità per l'unità di destinazione del backup viene rimosso dal database di Windows Server Essentials e Dashboard visualizza l'unità. Se si desidera utilizzare l'unità di backup in futuro, è necessario riassegnare la lettera di unità con uno strumento nativo.  
>   
>  **Per riassegnare una lettera di unità per un volume esistente**  
>   
>  1.  Nel Pannello di controllo, aprire **sistema e sicurezza**.  
> 2.  In **strumenti di amministrazione**, fare clic su **creare e formattare le partizioni del disco rigido**.  
> 3.  L'unità di mouse e scegliere **Cambia lettera di unità e percorso**.  
> 4.  Fare clic su **aggiungere**.  
> 5.  Nella finestra di dialogo Aggiungi lettera o percorso unità, selezionare una lettera di unità da assegnare. (È possibile riassegnare la stessa lettera di unità). Fare clic su **OK**.  
>   
>      L'unità verrà visualizzata immediatamente nel Dashboard.  
  
##  <a name="BKMK_4"></a>Elementi per il backup  
 È possibile scegliere di eseguire il backup di tutte le unità, file e cartelle nel server oppure selezionare solo singole unità, file o cartelle al backup.  
  
 Quando si aggiungere o rimuovere un'unità, oppure aggiungere o rimuovere i file e cartelle condivise, è consigliabile rivedere la configurazione di backup del server per assicurarsi che questi elementi vengono aggiunti o rimossi dalla configurazione del backup. Per aggiungere o rimuovere gli elementi per il backup, eseguire una delle operazioni seguenti:  
  
-   Per includere un'unità dati nel backup del server, selezionare la casella di controllo adiacente.  
  
-   Per escludere un'unità dati dal backup del server, deselezionare la casella di controllo adiacente.  
  
    > [!NOTE]
    >  Se si desidera escludere il **del sistema operativo** elemento dal backup, è innanzitutto necessario deselezionare il **Backup del sistema (scelta consigliata)** casella di controllo.  
  
 Per ridurre al minimo la quantità di memoria nel server che utilizzano i backup del server, è consigliabile escludere eventuali cartelle contenenti file non considerati utili o particolarmente importanti.  
  
 Ad esempio, potrebbe essere una cartella contenente programmi televisivi registrati che usa una quantità elevata di spazio su disco rigido. È possibile scegliere di non eseguire il backup di questi file, perché in genere eliminati dopo la visualizzazione comunque. Oppure potrebbe essere una cartella che contiene i file temporanei che non si intende conservare.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire il Backup dei Server](Manage-Server-Backup-in-Windows-Server-Essentials.md)  
  
-   [Gestire Backup e ripristino](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usare Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
