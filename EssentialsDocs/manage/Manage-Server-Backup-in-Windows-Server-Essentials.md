---
title: Gestire il Backup dei Server in Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0302d070-c58a-40f2-b56d-7e7842813d02
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 2b0cd926b15d65e5cd4c784681c40df29b18a48f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="manage-server-backup-in-windows-server-essentials"></a>Gestire il Backup dei Server in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Gli argomenti seguenti includono informazioni sulle attività di backup comuni che possono essere eseguite tramite il Dashboard di Windows Server Essentials:  
  
-   [Backup devo scegliere?](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_WhichBackup)  
  
-   [Configurare o personalizzare un backup del server](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Arresta backup del server in corso](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Gestire in remoto i backup](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Disabilitare il backup dei server](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Altre informazioni sulla configurazione del backup del server](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Ripartizionare un disco rigido sul server](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Ripristinare file e cartelle da un backup del server](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
##  <a name="BKMK_WhichBackup"></a>Backup devo scegliere?  
 Scelta di una copia di backup può essere semplice se si dispone di un backup molto recente completato correttamente e si è certi che il backup contenga tutti i dati critici. Se si sta tentando di ripristinare il server o in un computer da un backup precedente, la scelta di un backup ottimale da ripristinare potrebbe richiedere particolare attenzione e, probabilmente, alcuni compromettere.  
  
#### <a name="to-choose-a-backup"></a>Per scegliere un backup  
  
1.  Verificare con il proprietario dei file o cartelle e prendere nota delle date e ore quando sono stati aggiunti o modificati. Utilizzare queste date e ore come punto di partenza.  
  
2.  Nel **scegliere un'opzione di ripristino** pagina il ripristino guidato file e cartelle, fare clic su **ripristinare da un backup selezionato (utenti esperti)**.  
  
3.  A seconda che si voglia ripristinare una versione più recente o precedente dei file o cartelle, selezionare il backup che meglio si adatta le date e ore annotate nel passaggio 1.  
  
4.  Come procedura consigliata, è possibile ripristinare file e cartelle in un percorso alternativo e quindi richiedere al proprietario dei file e cartelle quelli che hanno bisogno di spostare nel percorso originale. Al termine, possono essere eliminate il file e cartelle che rimangono nel percorso alternativo.  
  
##  <a name="BKMK_1"></a>Configurare o personalizzare un backup del server  
 Backup del server non viene configurato automaticamente durante l'installazione. È consigliabile proteggere automaticamente il server e i relativi dati pianificando backup giornalieri. È consigliabile mantenere un piano di backup giornaliero poiché la maggior parte delle organizzazioni non può permettersi di perdere i dati che sono stati creati in diversi giorni. Per ulteriori informazioni, vedere [impostare configurare o personalizzare un backup del server](Set-up-or-customize-server-backup.md).  
  
##  <a name="BKMK_2"></a>Arresta backup del server in corso  
 Se viene avviato un backup del server in un momento regolarmente pianificato o si avvia un backup del server manualmente, è possibile interrompere il backup in corso.  
  
#### <a name="to-stop-a-backup-in-progress"></a>Per arrestare un backup in corso  
  
1.  Aprire il Dashboard.  
  
2.  Nella barra di spostamento, fare clic su **dispositivi**.  
  
3.  Nell'elenco dei computer, fare clic sul server e quindi fare clic su **arresta backup del server** nel **attività** riquadro.  
  
4.  Fare clic su **Sì** per confermare l'azione.  
  
##  <a name="BKMK_3"></a>Gestire in remoto i backup  
 Quando si è fuori sede, è possibile utilizzare accesso Web remoto di Windows Server Essentials per accedere al Dashboard di Windows Server Essentials per gestire il server.  
  
#### <a name="to-use-remote-web-access-to-manage-your-server"></a>Per usare accesso Web remoto per gestire il server  
  
1.  Aprire un web browser.  
  
2.  Nella casella degli indirizzi, digitare il nome di dominio di Windows Server Essentials.  
  
3.  Quando richiesto, immettere il nome utente e password.  
  
4.  Quando si seleziona il nome del server di accesso Web remoto, verrà visualizzata la pagina di accesso per il Dashboard.  
  
5.  Accedere al Dashboard come amministratore e quindi fare clic su **dispositivi**.  
  
 Per ulteriori informazioni su accesso Web remoto, vedere [Panoramica di accesso Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Overview).  
  
##  <a name="BKMK_4"></a>Disabilitare il backup dei server  
 È consigliabile proteggere automaticamente il server e i relativi dati pianificando backup giornalieri. È consigliabile mantenere un piano di backup giornaliero poiché la maggior parte delle organizzazioni non può permettersi di perdere i dati che sono stati creati in diversi giorni.  
  
 Se si dispone già di backup del server configurato e in seguito si desidera utilizzare un'applicazione di terze parti per il backup del server, è possibile disattivare backup server Windows Server Essentials.  
  
#### <a name="to-disable-server-backup"></a>Per disabilitare il backup di server  
  
1.  Accedere al Dashboard di Windows Server Essentials usando un account amministratore e una password.  
  
2.  Fare clic su di **dispositivi** scheda e quindi fare clic sul nome del server.  
  
3.  Nel riquadro attività fare clic su **Personalizza Backup del server**.  
  
    > [!NOTE]
    >  Il **Personalizza Backup del server** attività viene visualizzata dopo aver configurato utilizzando il Server di Backup procedura guidata Configura backup del server. Per ulteriori informazioni sulla configurazione del backup del server, vedere [impostare configurare o personalizzare un backup del server](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1).  
  
4.  Verrà visualizzata la procedura guidata personalizzare Backup Server.  
  
5.  Nel **opzioni di configurazione** pagina, fare clic su **Disattiva Backup Server**. Seguire le istruzioni della procedura guidata.  
  
##  <a name="BKMK_5"></a>Altre informazioni sulla configurazione del backup del server  
 Backup del server non è abilitato durante l'installazione di server.  
  
> [!NOTE]
>  Quando si configura backup server, è consigliabile connettere almeno un disco rigido esterno al server da utilizzare come unità disco rigido di destinazione di backup.  
  
###  <a name="BKMK_Target"></a>Unità di destinazione di backup  
 È possibile utilizzare più unità di archiviazione esterna per i backup e alternare le unità tra percorsi di archiviazione in sede e fuori sede. Ciò può migliorare l'emergenza pianificazione grazie alla possibilità di ripristinare i dati in caso di danni fisici per l'hardware in sede.  
  
 Quando si sceglie un'unità di archiviazione per il backup dei server, considerare quanto segue:  
  
-   Scegliere un'unità che include spazio sufficiente per archiviare i dati. Le unità di archiviazione devono contenere almeno 2,5 volte la capacità di archiviazione dei dati che si desidera eseguire il backup. L'unità devono essere anche dimensioni sufficienti per adeguarsi alla crescita futura dei dati del server.  
  
-   Se l'unità di destinazione di backup include unità offline, la configurazione del backup avrà esito negativo. Per completare la configurazione, quando si seleziona la destinazione di backup, deselezionare la casella di controllo per escludere le unità offline.  
  
-   Se si sceglie un'unità che include backup precedenti come destinazione del backup, la procedura guidata permetterà di scegliere se si desidera conservare i backup precedenti. Se si conservano i backup, la procedura guidata non formattare l'unità.  
  
-   Se si riutilizza un'unità di archiviazione esterna, assicurarsi che l'unità sia vuota o che includa solo dati non è necessario.  
  
-   È consigliabile visitare il sito Web del produttore dell'unità di archiviazione esterna per assicurarsi che l'unità di backup sia supportata nei computer che eseguono Windows Server Essentials.  
  
> [!CAUTION]
>  La configurazione Server Backup guidata formatta le unità di archiviazione quando le configura per il backup.  
  
### <a name="server-backup-schedule"></a>Pianificazione di backup server  
 È consigliabile proteggere automaticamente il server e i relativi dati pianificando backup giornalieri. È consigliabile mantenere un piano di backup giornaliero poiché la maggior parte delle organizzazioni non può permettersi di perdere i dati che sono stati creati in diversi giorni.  
  
 Quando si utilizza Windows Server Essentials configurazione Server Backup guidata, è possibile scegliere di eseguire il backup dei dati del server più volte nel corso della giornata. Poiché la procedura guidata consente di pianificare i backup su base differenziale, il Backup viene eseguito rapidamente e non sono interessata in modo significativo sulle prestazioni del server. Per impostazione predefinita, **Configura Backup Server** pianifica un backup da eseguire ogni giorno alle 12:00 e 11:00 PM. Tuttavia, è possibile modificare la pianificazione di backup in base alle esigenze dell'organizzazione. In alcuni casi si deve valutare l'efficacia del piano di backup e modifica il piano in base alle esigenze.  
  
> [!NOTE]
>  Nell'installazione predefinita di Windows Server Essentials, il server è configurato per eseguire automaticamente una deframmentazione una volta alla settimana. Questo può comportare maggiori di backup normali se si utilizza software di imaging non Microsoft. Se non è necessario deframmentare il server regolarmente, è possibile eseguire la procedura per disattivare la pianificazione della deframmentazione:  
>   
>  1.  Premere il tasto Windows + W per aprire **ricerca**.  
> 2.  Nella casella di testo di ricerca, digitare **Deframmenta**.  
> 3.  Nella sezione dei risultati, fare clic su **deframmenta e ottimizza unità**.  
> 4.  Nel **Ottimizza unità** pagina, selezionare l'unità e quindi fare clic su **modificare le impostazioni**.  
> 5.  Nel **pianificazione ottimizzazione** finestra, deseleziona il **eseguire una pianificazione (scelta consigliata)** casella di controllo, quindi fare clic su **OK** per salvare le modifiche.  
  
### <a name="items-to-be-backed-up"></a>Elementi per il backup  
 Per impostazione predefinita, tutti i file del sistema operativo e le cartelle sono selezionate per il backup. È possibile scegliere di eseguire il backup di tutti i dischi rigidi, i file e cartelle nel server oppure selezionare solo i singoli dischi rigidi, file o cartelle al backup. Per aggiungere o rimuovere gli elementi per il backup, eseguire una delle operazioni seguenti:  
  
-   Per includere un'unità dati nel backup del server, selezionare la casella di controllo adiacente.  
  
-   Per escludere un'unità dati dal backup del server, deselezionare la casella di controllo adiacente.  
  
> [!NOTE]
>  Se si desidera escludere il **del sistema operativo** elemento dal backup, è innanzitutto necessario deselezionare il **Backup del sistema (scelta consigliata)** casella di controllo.  
  
 Per ridurre al minimo la quantità di spazio su disco rigido di backup che utilizzano i backup del server, è consigliabile escludere eventuali cartelle contenenti file non considerati utili o particolarmente importanti.  
  
 Ad esempio, potrebbe essere una cartella contenente programmi televisivi registrati che usa una quantità elevata di spazio su disco rigido. È possibile scegliere di non eseguire il backup di questi file, perché in genere eliminati dopo la visualizzazione comunque. In alternativa, potrebbe essere una cartella che contiene i file temporanei che non si intende conservare.  
  
##  <a name="BKMK_6"></a>Ripartizionare un disco rigido sul server  
 Quando viene rilevata un'unità di disco rigido interno non formattata nel server di Windows Server Essentials, viene generato un avviso di integrità contenente un collegamento per l'aggiunta guidata nuova unità disco rigido. L'aggiunta guidata nuova unità disco rigido descrive le varie opzioni per la formattazione del disco rigido. Al termine della procedura guidata, uno o più, a seconda delle dimensioni dell'unità, dischi rigidi logici formattati verranno create sul disco rigido e formattati come NTFS.  
  
 Se è necessario ripartizionare un'unità disco rigido, seguire queste istruzioni:  
  
#### <a name="to-repartition-a-hard-disk-drive"></a>Per ripartizionare un'unità disco rigido  
  
1.  Nel **Start** schermata, fare clic su **strumenti di amministrazione**e quindi fare doppio clic su **Gestione Computer**.  
  
2.  In Gestione Computer, fare clic su **archiviazione**e quindi fare doppio clic su **Gestione disco**.  
  
3.  Pulsante destro del mouse sull'unità che si desidera partizionare, fare clic su **Elimina Volume**, quindi fare clic su **Sì**.  
  
    > [!NOTE]
    >  Ripetere questo passaggio per ogni partizione sull'unità disco rigido.  
  
4.  Fare doppio clic su di **non allocata** unità disco rigido e quindi fare clic su **nuovo Volume semplice**.  
  
5.  Nella semplice creazione guidata nuovo Volume, creare e formattare un volume di 16 TB (16.000.000 MB) o meno.  
  
    > [!NOTE]
    >  Ripetere questo passaggio fino a quando non viene utilizzato tutto lo spazio non allocato sull'unità disco rigido.  
  
##  <a name="BKMK_7"></a>Ripristinare file e cartelle da un backup del server  
 È possibile selezionare e ripristinare singoli file e cartelle da un backup del server.  
  
#### <a name="to-restore-files-and-folders-from-a-server-backup"></a>Per ripristinare file e cartelle da un backup del server  
  
1.  Aprire il Dashboard, quindi fare clic su di **dispositivi** scheda.  
  
2.  Fare clic sul nome del server, quindi fare clic su **ripristinare file o cartelle per il server** nel **attività** riquadro.  
  
3.  Il ripristino di file o cartelle guidata verrà visualizzata. Seguire le istruzioni della procedura guidata per ripristinare i file o cartelle.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire Backup e ripristino](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usare Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
