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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831932"
---
# <a name="set-up-or-customize-server-backup"></a>Configurare o personalizzare un backup del server

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Il backup dei server non è configurato automaticamente durante l'installazione. È consigliabile proteggere automaticamente il server e i relativi dati pianificando backup giornalieri. Si consiglia di mantenere un piano di backup giornaliero poiché la maggior parte delle organizzazioni non può permettersi di perdere i dati creati in diversi giorni.  
  
 Vedere le sezioni seguenti per impostare o personalizzare il backup del server:  
  
-   [Consente di impostare o modificare le impostazioni di backup di server](Set-up-or-customize-server-backup.md#BKMK_1)  
  
-   [Pianificazione del backup server](Set-up-or-customize-server-backup.md#BKMK_2)  
  
-   [Unità di destinazione di backup](Set-up-or-customize-server-backup.md#BKMK_Target)  
  
-   [Elementi da sottoporre a backup](Set-up-or-customize-server-backup.md#BKMK_4)  
  
##  <a name="BKMK_1"></a> Consente di impostare o modificare le impostazioni di backup di server  
  
#### <a name="to-set-up-or-change-server-backup-settings"></a>Per configurare o modificare le impostazioni del backup del server  
  
1.  Aprire il **Dashboard**, quindi fare clic sulla scheda **Dispositivi** .  
  
2.  Nella visualizzazione elenco fare clic sul server per selezionarlo.  
  
3.  Nel riquadro attività fare clic su **Imposta backup del server**.  
  
    > [!NOTE]
    >  Per modificare le impostazioni di backup esistenti, fare clic su **Personalizza backup del server**.  
  
4.  Seguire le istruzioni contenute nella procedura guidata.  
  
    > [!NOTE]
    >  Se si avvia la procedura guidata prima di collegare il disco rigido esterno al server, fare clic su **Aggiorna elenco** nella pagina **Seleziona la destinazione di backup** dopo aver collegato il disco rigido.  
  
> [!NOTE]
>  Nell'installazione predefinita di Windows Server Essentials, il server è configurato per eseguire automaticamente una deframmentazione una volta alla settimana. Ciò può generare backup di dimensioni superiori al normale, se si usa un software di imaging non Microsoft. Se non è necessario deframmentare il server regolarmente, è possibile eseguire questa procedura per disattivare la pianificazione della deframmentazione:  
>   
>  1.  Premere il tasto WINDOWS+W per aprire la **Ricerca**.  
> 2.  Nella casella di testo di ricerca digitare **Defragment**.  
> 3.  Nella sezione dei risultati fare clic su **Deframmenta e ottimizza unità**.  
> 4.  Nella pagina **Ottimizza unità** selezionare un'unità, quindi fare clic su **Cambia impostazioni**.  
> 5.  Nella finestra **Pianificazione ottimizzazione** deselezionare la casella di controllo **Esegui in base a una pianificazione (scelta consigliata)** , quindi fare clic su **OK** per salvare la modifica.  
  
##  <a name="BKMK_2"></a> Pianificazione del backup server  
 Quando si usa la procedura guidata Configura backup server o Personalizza Backup server, è possibile scegliere di eseguire il backup dei dati del server più volte al giorno. Dato che i backup pianificati dalle procedure guidate sono di tipo incrementale, vengono eseguiti rapidamente e non influiscono in modo significativo sulle prestazioni del server. Per impostazione predefinita, le procedure guidate pianificano l'esecuzione di un backup ogni giorno alle 12:00 e alle 23:00. È tuttavia possibile modificare la pianificazione dei backup in base alle esigenze dell'organizzazione. È consigliabile rivalutare periodicamente l'efficacia del piano di backup adottato ed eventualmente modificarlo in base alla necessità.  
  
##  <a name="BKMK_Target"></a> Unità di destinazione di backup  
 È possibile usare più unità di archiviazione esterne per i backup e alternare le unità tra percorsi di archiviazione in sede e fuori sede. Ciò può migliorare la pianificazione della preparazione alle emergenze, semplificando il recupero dei dati in caso di danni fisici all'hardware in sede.  
  
 Quando si sceglie un'unità di archiviazione per il backup dei server, occorre considerare quanto segue:  
  
-   Scegliere un'unità che include spazio sufficiente per l'archiviazione dei dati. Le unità di archiviazione devono includere almeno 2,5 volte la capacità di archiviazione dei dati di cui si deve eseguire il backup. Le unità devono essere anche di dimensioni sufficienti per adeguarsi alla crescita futura dei dati del server.  
  
-   Se si usa un'unità di archiviazione esterna, assicurarsi che l'unità sia vuota o che includa solo dati non necessari.  
  
    > [!CAUTION]
    >  La procedura guidata Configura backup server formatta le unità di archiviazione quando le configura per il backup.  
  
-   Se l'unità di destinazione del backup include unità offline, la configurazione del backup avrà esito negativo. Per completare la configurazione, quando si seleziona la destinazione del backup, deselezionare la casella di controllo per escludere le unità offline.  
  
-   Se si sceglie un'unità che include backup precedenti come destinazione del backup, la procedura guidata permetterà di scegliere se conservare i backup precedenti. Se si conservano i backup, l'unità non viene formattata.  
  
-   È consigliabile visitare il sito Web del produttore dell'unità di archiviazione esterna per assicurarsi che l'unità di backup sia supportata nei computer che eseguono Windows Server Essentials.  
  
-   L'unità non può contenere una partizione di sistema EFI (Extensible Firmware Interface). Se in un'unità USB è presente una partizione EFI, il disco è considerato un disco di avvio. Se si è certi che non si necessità di t i dati sul disco, è possibile riformattare il disco e usarlo per i backup.  
  
    > [!CAUTION]
    >  Tutti i dati vengono eliminati quando si riformatta il disco.  
  
    #### <a name="to-remove-an-efi-system-partition-from-a-usb-disk"></a>Per rimuovere una partizione di sistema EFI da un disco USB  
  
    1.  Nel Panello di controllo aprire **Sistema e sicurezza**.  
  
    2.  In **Strumenti di amministrazione**fare clic su **Crea e formatta le partizioni del disco rigido**.  
  
    3.  Eliminare tutti i volumi nel disco USB oppure eliminare semplicemente la partizione EFI. La procedura guidata Configura backup server elimina tutti i volumi.  
  
-   L'unità non può contenere cartelle condivise del server. Per poter usare il disco come unità di destinazione di backup, è necessario interrompere la condivisione delle cartelle condivise del server. È possibile interrompere la condivisione dal dashboard o in Esplora file.  
  
    #### <a name="to-stop-sharing-on-a-server-folder-from-the-dashboard"></a>Per interrompere la condivisione in una cartella del server dal dashboard  
  
    1.  Nel dashboard fare clic su **Archiviazione**, quindi su **Cartelle server**.  
  
    2.  Selezionare la cartella di cui si vuole interrompere la condivisione, quindi fare clic su **Interrompi** nel riquadro attività.  
  
> [!NOTE]
>  Se un backup non riesce perché l'unità di backup è disponibile spazio sufficiente, la lettera di unità per l'unità di destinazione di backup viene rimosso dal database di Windows Server Essentials e il Dashboard non viene visualizzata l'unità. Per usare l'unità di backup in futuro, è necessario riassegnare la lettera di unità con uno strumento nativo.  
>   
>  **Per riassegnare una lettera di unità per un volume esistente**  
>   
>  1.  Nel Panello di controllo aprire **Sistema e sicurezza**.  
> 2.  In **Strumenti di amministrazione**fare clic su **Crea e formatta le partizioni del disco rigido**.  
> 3.  Fare clic con il pulsante destro del mouse sull'unità e scegliere **Cambia lettera e percorso di unità**.  
> 4.  Fai clic su **Aggiungi**.  
> 5.  Nella finestra di dialogo Aggiungi lettera o percorso di unità, selezionare una lettera di unità da assegnare (è possibile riassegnare la stessa lettera di unità). Fare quindi clic su **OK**.  
>   
>      L'unità verrà visualizzata immediatamente nel dashboard.  
  
##  <a name="BKMK_4"></a> Elementi da sottoporre a backup  
 È possibile scegliere di eseguire il backup di tutte le unità, i file e le cartelle sul server oppure selezionare solo singole unità, file o cartelle di cui eseguire il backup.  
  
 Quando si aggiunge o rimuove un'unità oppure file e cartelle condivise, è consigliabile rivedere la configurazione di backup del server per assicurarsi che questi elementi vengono aggiunti o rimossi dalla configurazione del backup. Per aggiungere o rimuovere elementi per il backup, eseguire una delle operazioni seguenti:  
  
-   Per includere un'unità dati nel backup del server, selezionare la casella di controllo adiacente.  
  
-   Per escludere un'unità dati dal backup del server, deselezionare la casella di controllo adiacente.  
  
    > [!NOTE]
    >  Se si vuole escludere l'elemento **Sistema operativo** dal backup, è prima di tutto necessario deselezionare la casella di controllo **Backup del sistema (scelta consigliata)** .  
  
 Per ridurre al minimo la quantità di spazio di archiviazione server usata per i backup del server, è possibile escludere eventuali cartelle contenenti file non considerati utili o particolarmente importanti.  
  
 Ad esempio, è possibile che sia presente una cartella contenente programmi televisivi registrati che usa una quantità elevata di spazio su disco rigido. Si può scegliere di non eseguire il backup di questi file, perché comunque verranno in genere eliminati dopo la visualizzazione. È anche possibile che sia presente una cartella contenente file temporanei che non si intende conservare.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire il Backup dei Server](Manage-Server-Backup-in-Windows-Server-Essentials.md)  
  
-   [Gestire il Backup e ripristino](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usare Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
