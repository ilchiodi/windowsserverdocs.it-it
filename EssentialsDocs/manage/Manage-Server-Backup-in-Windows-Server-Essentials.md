---
title: Gestire il backup dei server in Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
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
ms.openlocfilehash: 7e40a4675cf77d55a3047b41e0ab852fd7cd9de9
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371214"
---
# <a name="manage-server-backup-in-windows-server-essentials"></a>Gestire il backup dei server in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Gli argomenti seguenti includono informazioni sulle attività di backup comuni che possono essere eseguite tramite il Dashboard di Windows Server Essentials:  
  
-   [Quale backup scegliere?](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_WhichBackup)  
  
-   [Configurare o personalizzare un backup del server](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Arresto del backup del server in corso](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Gestire i backup in modalità remota](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Disabilitare il backup del server](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Altre informazioni sulla configurazione del backup del server](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Ripartizionare un disco rigido sul server](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Ripristinare file e cartelle da un backup del server](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
##  <a name="BKMK_WhichBackup"></a>Quale backup scegliere?  
 La scelta del backup può essere semplice se si dispone di un backup molto recente completato correttamente e si è certi che il backup contenga tutti i dati critici di interesse. Se si sta tentando di eseguire il ripristino nel server o di ripristinare un computer da un backup precedente, la scelta di un backup ottimale da ripristinare potrebbe richiedere particolare attenzione e, probabilmente, qualche compromesso.  
  
#### <a name="to-choose-a-backup"></a>Per scegliere un backup  
  
1.  Contattare il proprietario dei file o delle cartelle e annotare la data e l'ora in cui i file o le cartelle sono stati aggiunti o modificati. Utilizzare queste date e ore come punto di partenza.  
  
2.  Nella pagina **Scegliere un'opzione di ripristino** nella procedura guidata Ripristino file o cartelle fare clic su **Ripristina da un backup selezionato (utenti esperti)** .  
  
3.  A seconda che si voglia ripristinare una versione meno recente o più recente dei file o delle cartelle, selezionare il backup più adatto alle date e alle ore annotate nel passaggio 1.  
  
4.  È consigliabile ripristinare i file e le cartelle in un percorso alternativo, quindi richiedere al proprietario dei file e delle cartelle di spostare i file necessari nel percorso originale. Al termine, sarà possibile eliminare i file e le cartelle rimanenti nel percorso alternativo.  
  
##  <a name="BKMK_1"></a>Configurare o personalizzare il backup del server  
 Il backup dei server non è configurato automaticamente durante l'installazione. È consigliabile proteggere automaticamente il server e i relativi dati pianificando backup giornalieri. Si consiglia di mantenere un piano di backup giornaliero poiché la maggior parte delle organizzazioni non può permettersi di perdere i dati creati in diversi giorni. Per altre informazioni, vedere [Configurare o personalizzare un backup del server](Set-up-or-customize-server-backup.md).  
  
##  <a name="BKMK_2"></a>Arresto del backup del server in corso  
 È possibile interrompere il backup in corso, sia che si tratti di un backup pianificato o di un backup avviato manualmente.  
  
#### <a name="to-stop-a-backup-in-progress"></a>Per arrestare un backup in corso  
  
1.  Aprire il Dashboard.  
  
2.  Sulla barra di spostamento fare clic su **Dispositivi**.  
  
3.  Nell'elenco di computer fare clic sul server, quindi su **Arresta backup del server** nel riquadro **Attività**.  
  
4.  Fare clic su **Sì** per confermare l'azione.  
  
##  <a name="BKMK_3"></a>Gestire i backup in modalità remota  
 Se non ci si trova in ufficio, è possibile usare Accesso Web remoto di Windows Server Essentials per accedere al Dashboard di Windows Server Essentials per gestire il server.  
  
#### <a name="to-use-remote-web-access-to-manage-your-server"></a>Per usare Accesso Web remoto per gestire il server  
  
1. Aprire un Web browser.  
  
2. Nella casella degli indirizzi digitare il nome del dominio di Windows Server Essentials.  
  
3. Quando richiesto, immettere nome utente e password.  
  
4. Quando si fa clic sul nome del server in Accesso Web remoto, viene visualizzata la pagina di accesso per il dashboard.  
  
5. Accedere al dashboard come amministratore, quindi fare clic su **Dispositivi**.  
  
   Per ulteriori informazioni sulla Accesso Web remota, vedere [remote accesso Web Overview](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Overview).  
  
##  <a name="BKMK_4"></a>Disabilitare il backup del server  
 È consigliabile proteggere automaticamente il server e i relativi dati pianificando backup giornalieri. Si consiglia di mantenere un piano di backup giornaliero poiché la maggior parte delle organizzazioni non può permettersi di perdere i dati creati in diversi giorni.  
  
 Se il backup dei server è già stato configurato e in un secondo momento si vuole usare un'applicazione di terze parti per eseguire il backup del server, sarà possibile disabilitare il backup dei server di Windows Server Essentials.  
  
#### <a name="to-disable-server-backup"></a>Per disabilitare il backup dei server  
  
1.  Accedere al Dashboard di Windows Server Essentials usando un account e una password di amministratore.  
  
2.  Fare clic sulla scheda **Dispositivi**, quindi fare clic sul nome del server.  
  
3.  Nel riquadro attività fare clic su **Personalizza backup del server**.  
  
    > [!NOTE]
    >  L'attività **Personalizza backup del server** sarà visualizzata dopo la configurazione del backup dei server tramite la procedura guidata Configura backup server. Per altre informazioni sulla configurazione del backup dei server, vedere [Configurare o personalizzare un backup del server](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1).  
  
4.  Sarà visualizzata la procedura guidata Configura backup server.  
  
5.  Nella pagina **Opzioni di configurazione** fare clic su **Disattiva Backup server**. Seguire le istruzioni fornite nella procedura guidata.  
  
##  <a name="BKMK_5"></a>Altre informazioni sulla configurazione del backup del server  
 Il backup dei server non è abilitato durante la configurazione del server.  
  
> [!NOTE]
>  Quando si configura il backup dei server, è consigliabile connettere almeno un disco rigido esterno al server, in modo da usarlo come disco rigido di destinazione di backup.  
  
###  <a name="BKMK_Target"></a>Unità di destinazione di backup  
 È possibile usare più unità di archiviazione esterne per i backup e alternare le unità tra percorsi di archiviazione in sede e fuori sede. Ciò può migliorare la pianificazione della preparazione alle emergenze, semplificando il recupero dei dati in caso di danni fisici all'hardware in sede.  
  
 Quando si sceglie un'unità di archiviazione per il backup dei server, occorre considerare quanto segue:  
  
-   Scegliere un'unità che include spazio sufficiente per l'archiviazione dei dati. Le unità di archiviazione devono includere almeno 2,5 volte la capacità di archiviazione dei dati di cui si deve eseguire il backup. Le unità devono essere anche di dimensioni sufficienti per adeguarsi alla crescita futura dei dati del server.  
  
-   Se l'unità di destinazione del backup include unità offline, la configurazione del backup avrà esito negativo. Per completare la configurazione, quando si seleziona la destinazione del backup, deselezionare la casella di controllo per escludere le unità offline.  
  
-   Se si sceglie un'unità che include backup precedenti come destinazione del backup, la procedura guidata permetterà di scegliere se conservare i backup precedenti. Se si conservano i backup, l'unità non viene formattata.  
  
-   Se si riutilizza un'unità di archiviazione esterna, assicurarsi che l'unità sia vuota o che includa solo dati non necessari.  
  
-   È consigliabile visitare il sito Web del produttore dell'unità di archiviazione esterna per assicurarsi che l'unità di backup sia supportata nei computer che eseguono Windows Server Essentials.  
  
> [!CAUTION]
>  La procedura guidata Configura backup server formatta le unità di archiviazione quando le configura per il backup.  
  
### <a name="server-backup-schedule"></a>Pianificazione del backup del server  
 È consigliabile proteggere automaticamente il server e i relativi dati pianificando backup giornalieri. Si consiglia di mantenere un piano di backup giornaliero poiché la maggior parte delle organizzazioni non può permettersi di perdere i dati creati in diversi giorni.  
  
 Quando si usa la procedura guidata Configura backup server di Windows Server Essentials, sarà possibile scegliere di eseguire il backup dei dati del server più volte durante il giorno. Poiché la procedura guidata pianifica backup su base differenziale, l'esecuzione del backup risulta veloce e non influisce in modo significativo sulle prestazioni del server. Per impostazione predefinita, **Configura backup server** pianifica l'esecuzione del backup ogni giorno alle 12:00 e alle 23:00. È tuttavia possibile modificare la pianificazione dei backup in base alle esigenze dell'organizzazione. È consigliabile rivalutare periodicamente l'efficacia del piano di backup adottato ed eventualmente modificarlo in base alla necessità.  
  
> [!NOTE]
>  Nell'installazione predefinita di Windows Server Essentials il server è configurato per l'esecuzione automatica di una deframmentazione una volta alla settimana. Ciò può generare backup di dimensioni superiori al normale, se si usa un software di imaging non Microsoft. Se non è necessario deframmentare il server regolarmente, è possibile eseguire questa procedura per disattivare la pianificazione della deframmentazione:  
> 
> 1. Premere il tasto WINDOWS+W per aprire la **Ricerca**.  
>    2. Nella casella di testo di ricerca digitare **Defragment**.  
>    3. Nella sezione dei risultati fare clic su **Deframmenta e ottimizza unità**.  
>    4. Nella pagina **Ottimizza unità** selezionare un'unità, quindi fare clic su **Cambia impostazioni**.  
>    5. Nella finestra **Pianificazione ottimizzazione** deselezionare la casella di controllo **Esegui in base a una pianificazione (scelta consigliata)** , quindi fare clic su **OK** per salvare la modifica.  
  
### <a name="items-to-be-backed-up"></a>Elementi di cui eseguire il backup  
 Per impostazione predefinita, tutti i file e le cartelle del sistema operativo sono selezionati per il backup. È possibile scegliere di eseguire il backup di tutti i dischi rigidi, i file e le cartelle sul server oppure selezionare solo alcuni elementi per il backup. Per aggiungere o rimuovere elementi per il backup, eseguire una delle operazioni seguenti:  
  
-   Per includere un'unità dati nel backup del server, selezionare la casella di controllo adiacente.  
  
-   Per escludere un'unità dati dal backup del server, deselezionare la casella di controllo adiacente.  
  
> [!NOTE]
>  Se si vuole escludere l'elemento **Sistema operativo** dal backup, è prima di tutto necessario deselezionare la casella di controllo **Backup del sistema (scelta consigliata)** .  
  
 Per ridurre al minimo la quantità di spazio su disco rigido di backup usato dai backup del server, è consigliabile escludere eventuali cartelle contenenti file non considerati utili o particolarmente importanti.  
  
 Ad esempio, è possibile che sia presente una cartella contenente programmi televisivi registrati che usa una quantità elevata di spazio su disco rigido. Si può scegliere di non eseguire il backup di questi file, perché comunque verranno in genere eliminati dopo la visualizzazione. Oppure è possibile che sia presente una cartella contenente file temporanei che non si vuole conservare.  
  
##  <a name="BKMK_6"></a>Ripartizionare un disco rigido sul server  
 In caso di rilevamento di unità disco rigido non formattata nel server di Windows Server Essentials, sarà generato un avviso di integrità contenente un collegamento alla procedura guidata Aggiungi nuova unità disco rigido. La procedura guidata Aggiungi nuova unità disco rigido fornisce informazioni dettagliate sulle varie opzioni per la formattazione del disco rigido. Al termine della procedura guidata, uno o più dischi rigidi logici formattati, in base alla dimensione dell'unità, saranno creati nel disco rigido e formattati come NTFS.  
  
 Se è necessario ripartizionare un'unità disco rigido, seguire le istruzioni seguenti:  
  
#### <a name="to-repartition-a-hard-disk-drive"></a>Per ripartizionare un'unità disco rigido  
  
1.  Nella schermata **Start** fare clic su **Strumenti di amministrazione** e quindi fare doppio clic su **Gestione computer**.  
  
2.  In Gestione computer fare clic su **Archiviazione**, quindi fare doppio clic su **Gestione disco**.  
  
3.  Fare clic con il pulsante destro del mouse sull'unità da ripartizionare, scegliere **Elimina volume**, quindi fare clic su **Sì**.  
  
    > [!NOTE]
    >  Ripetere questo passaggio per ogni partizione sull'unità disco rigido.  
  
4.  Fare clic con il pulsante destro del mouse sull'unità disco rigido **Non allocata**, quindi scegliere **Nuovo volume semplice**.  
  
5.  Nella Creazione guidata nuovo volume semplice creare e formattare un volume di 16 TB (16.000.000 MB) o meno.  
  
    > [!NOTE]
    >  Ripetere questo passaggio finché non sarà usato tutto lo spazio non allocato sull'unità disco rigido.  
  
##  <a name="BKMK_7"></a>Ripristinare file e cartelle da un backup del server  
 È possibile individuare e ripristinare singoli file e cartelle da un backup del server.  
  
#### <a name="to-restore-files-and-folders-from-a-server-backup"></a>Per ripristinare file e cartelle da un backup del server  
  
1.  Aprire il dashboard, quindi fare clic sulla scheda **Dispositivi**.  
  
2.  Fare clic sul nome del server, quindi su **Ripristino file o cartelle del server** nel riquadro **Attività**.  
  
3.  Verrà visualizzata la procedura guidata Ripristino file o cartelle. Seguire le istruzioni della procedura guidata per ripristinare i file o le cartelle.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire il backup e il ripristino](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usare Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
