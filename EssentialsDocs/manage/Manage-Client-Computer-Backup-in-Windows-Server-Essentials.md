---
title: Gestire il Backup dei Computer Client in Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b4776e8-9504-4b98-ae80-11da797d9819
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d184c97e47f04b9d7434aaeb0d328761bcfac1c0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="manage-client-computer-backup-in-windows-server-essentials"></a>Gestire il Backup dei Computer Client in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Questo argomento include informazioni sulle attività di backup comuni per i computer client, è possibile eseguire tramite il Dashboard di Windows Server Essentials che include le sezioni seguenti:  
  
-   [Come funziona la riparazione guidata del Database di Backup](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Comprendere le impostazioni di backup del computer](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Configurare il backup per un computer client](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Modifica dell'ora di backup pianificato per l'esecuzione](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Modificare i criteri di conservazione dei backup del computer](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Ripristinare le impostazioni predefinite del backup](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Utilizzare gli strumenti di ripristino](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Ripristinare il database di backup](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Disabilitare il backup per un computer](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Eseguire l'attività pulizia dei backup](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Visualizzare gli avvisi nella barra delle applicazioni in un computer client](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Visualizzare lo stato di backup dalla finestra di avvio](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [Interrompere un backup in corso dalla finestra di avvio](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [Avviare un backup dalla finestra di avvio](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [Funzionamento di backup del computer](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_15)  
  
-   [Suggerimenti utili per evitare la perdita di dati a causa del danneggiamento del database di backup del client](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_16)  
  
-   [Ripristinare file o cartelle da un backup del computer client](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_17)  
  
-   [Informazioni su Cronologia File](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_FileHistory)  
  
##  <a name="BKMK_1"></a>Come funziona la riparazione guidata del Database di Backup  
 Se Windows Server Essentials rileva errori nel database di backup, invia una notifica relativa all'integrità e lo stato dell'avviso cambia in rosso, che indica una condizione critica.  
  
 Nel Dashboard di Windows Server Essentials, fare clic sull'icona dello stato degli avvisi per visualizzare la notifica di errore di database di backup. La notifica include un **riparazione** pulsante, che avvia la riparazione guidata del Database di Backup. La procedura guidata potrebbe richiedere diverse ore per terminare.  
  
 La procedura guidata esamina il database di backup per il computer selezionato e tenta di ripristinare il database, se vengono rilevati errori. In alcuni casi, la procedura guidata non può ripristinare l'intero database di backup. Prima di iniziare la procedura guidata, verificare che tutti i dischi rigidi esterni che usi nel server sono connessi al server, attivato e funzioni correttamente. L'errore di database di backup può essere corretto automaticamente tramite la riconnessione di un disco rigido mancano.  
  
> [!CAUTION]
>  Eseguire il backup del database prima di tentare di ripristinarlo. A seconda del tipo di errori trovati nel database di backup, la procedura guidata potrebbe non essere in grado di recuperare alcuni backup. Alcuni o tutti i backup potrebbero andare persi definitivamente.  
  
##  <a name="BKMK_2"></a>Comprendere le impostazioni di backup del computer  
 Dopo la configurazione del backup per i computer client, è possibile specificare un intervallo di tempo per l'esecuzione del backup diverso. Analogamente, è possibile specificare un periodo di conservazione più lungo o più breve rispetto al valore predefinito.  
  
 Il **Ripristina valori predefiniti** opzione consente di reimpostare i criteri di finestra e conservazione dei backup per le impostazioni predefinite fornite durante la configurazione iniziale del backup.  
  
 I valori predefiniti sono:  
  
|Impostazione del backup|Impostazione predefinita|Descrizione|  
|--------------------|-------------|-----------------|  
|Ora di inizio|6:00 PM|Specifica l'ora di inizio del backup giornaliero. È consigliabile impostarla su un orario quando gli utenti non usano i computer.|  
|Ora di fine|9:00 AM|Specifica il tempo che deve essere completato il backup. Se il backup non richiede la durata specificata, viene utilizzato solo il tempo necessario per eseguire correttamente il backup del computer.|  
|Mantieni backup giornalieri|5 giorni|Specifica il numero di giorni per cui saranno conservati i backup giornalieri. Ad esempio, Usa l'impostazione predefinita, saranno conservati i backup giornalieri 5. Il giorno 6 e successivamente ogni giorno, il file di backup giornaliero meno recente viene eliminato.|  
|Mantieni backup settimanali|4 settimane|Specifica il numero di settimane per cui viene mantenuto l'ultimo backup della settimana. Ad esempio, Usa le impostazioni predefinite, saranno conservati i backup settimanali 4. 5 settimana e in seguito ogni settimana, il backup settimana meno recente viene eliminato.|  
|Mantieni backup mensili|6 mesi|Specifica il numero di mesi per cui viene mantenuto l'ultimo backup del mese. Ad esempio, Usa l'impostazione predefinita, saranno conservati sei mesi di backup. Il 7 e ogni mese in seguito, il backup mensile meno recente viene eliminato.|  
  
##  <a name="BKMK_3"></a>Configurare il backup per un computer client  
 Se il backup è disabilitato, è possibile impostare il backup per il computer dal Dashboard. Quando si imposta il backup per un computer, è possibile scegliere di eseguire il backup completo del computer oppure selezionare i volumi e le cartelle che si desidera eseguire il backup.  
  
> [!NOTE]
>  Il computer deve essere online per poter configurare il backup.  
  
> [!IMPORTANT]
>   Windows Server Essentials non supporta il backup e ripristino dei dischi dinamici nei computer client.  
  
#### <a name="to-set-up-backup-for-a-client-computer"></a>Per configurare il backup per un computer client  
  
1.  Aprire il **Dashboard**, quindi fare clic sul **dispositivi** scheda.  
  
2.  Fare clic sul nome del computer client che si desidera configurare il backup e quindi nel **attività** riquadro, fare clic su **imposta backup del computer**.  
  
    > [!NOTE]
    >  Se il backup è già configurato per il computer client, **Personalizza Backup del computer** è elencato nel **attività** riquadro anziché **imposta backup del computer**.  
  
3.  Nel Set di Backup guidato di, è possibile eseguire il backup di tutte le cartelle oppure selezionare determinate cartelle che si desidera eseguire il backup. Seguire le istruzioni della procedura guidata.  
  
4.  Fare clic su **Chiudi** quando backup è impostato per il computer.  
  
### <a name="critical-system-files"></a>File di sistema critici  
 Quando si installa il sistema operativo Windows, il programma di installazione crea cartelle nell'unità di sistema in cui inserisce i file di sistema necessari per avviare ed eseguire. File di sistema critici includono i file con estensione dll, .exe, ocx e le estensioni di file sys. Alcuni di questi file sono tipi di carattere True Type. Inoltre, i file di stato del sistema, ad esempio il sistema s del Registro di sistema, sono necessari per il sistema operativo a funzionare correttamente.  
  
### <a name="find-the-file-you-are-looking-for"></a>Trovare il file che si sta cercando  
 È possibile ripristinare tutte le cartelle per un computer, più file e cartelle, o un singolo file o cartella da un backup esistente.  
  
 Dopo aver selezionato il backup che si desidera utilizzare per il ripristino, viene letto il file di backup e vengono visualizzate tutti i file e cartelle. È possibile eseguire il drill down file specifico o una cartella da ripristinare facendo doppio clic la cartella di livello superiore, e quindi il drill-down tramite la gerarchia di cartelle fino a individuare il file che si sta cercando.  
  
### <a name="why-am-i-unable-to-select-some-items"></a>Perché non è possibile selezionare alcuni elementi?  
 La casella di controllo nel menu di selezione di **selezionare gli elementi da eseguire il backup di pagina** può indicare stati diversi per ogni cartella. Quando la casella di controllo è:  
  
-   **Selezionato**, la cartella associata e i contenuti della cartella sono selezionati per il backup.  
  
-   **Deselezionate**, la cartella associata e i contenuti della cartella saranno esclusi dal backup.  
  
-   **Tinta unita**, la cartella associata è selezionata per il backup, ma uno o più elementi all'interno di tale cartella sono esclusi dal backup.  
  
 Se non è possibile selezionare una cartella specifica:  
  
-   Il volume potrebbe essere configurato per il backup, ma potrebbe essere offline. Questa situazione è comune per le unità USB rimovibili. I volumi offline sono visualizzati in grigio.  
  
-   È possibile eseguire il backup solo dei dati da un'unità locale formattata come file system NTFS. Unità formattate come FAT (incluso FAT32) o file System (Refs) non vengono visualizzati nell'elenco di unità per eseguire il backup.  
  
> [!IMPORTANT]
>  Servizio Copia Shadow del volume (VSS) non supporta la creazione di una copia shadow di una macchina virtuale e il volume host nello stesso set di snapshot. VSS supporta la creazione di snapshot dei volumi in un disco rigido virtuale (VHD), se il backup del volume virtuale è necessario. Per ulteriori informazioni, vedere [eseguire la manutenzione e il backup di dischi rigidi virtuali](https://go.microsoft.com/fwlink/p/?LinkId=256577).  
  
##  <a name="BKMK_4"></a>Modifica dell'ora di backup pianificato per l'esecuzione  
 Il processo di backup deve essere pianificato in un periodo di tempo quando utilizza il minor numero possibile di persone i computer in rete. Si tratta in genere durante la sera tardi o ore mattino. Il periodo predefinito per il backup è 6:00 e 9:00 AM. Il server tenta di eseguire il backup dei computer client solo durante l'intervallo di tempo pianificato.  
  
 Tuttavia, se l'azienda è operativa durante questi orari, puoi modificare queste impostazioni predefinite.  
  
#### <a name="to-change-the-time-that-backup-is-scheduled-to-run"></a>Per modificare l'ora che il backup pianificato per l'esecuzione  
  
1.  Aprire il server **Dashboard**, quindi fare clic su **dispositivi**.  
  
2.  In **attività dispositivi**, fare clic su **le impostazioni di personalizzare il Backup del Computer e cronologia File**.  
  
    > [!NOTE]
    >  In Windows Server Essentials, questa attività è stata rinominata come **attività di backup computer Client**.  
  
3.  In **strumenti e impostazioni di backup computer Client**via il **Backup Computer** scheda, è possibile modificare gli orari di inizio e fine in base alle esigenze.  
  
4.  Fare clic su **applica**, quindi fare clic su **OK**.  
  
##  <a name="BKMK_5"></a>Modificare i criteri di conservazione dei backup del computer  
 I backup giornalieri di tutti i computer si accumulano sul server nel corso del tempo. Per gestire questi backup, Windows Server Essentials consente di gestire il database di backup dei computer. È possibile configurare il numero di backup da mantenere per tutti i computer.  
  
 I criteri di conservazione dei backup determinano per quanto tempo un backup sarà conservato prima dell'eliminazione durante il processo di pulizia dei backup. Pulizia dei backup viene eseguito ore 11:59 ogni sabato. Elimina tutti i backup che non rientrano i criteri di conservazione dei backup. Le impostazioni predefinite per i criteri di conservazione dei backup sono:  
  
-   **Mantieni backup giornalieri per 5 giorni.** Il primo backup del giorno è conservato come backup giornaliero. Dopo 5 giorni, il backup giornaliero meno recente sarà eliminato nel processo di pulizia.  
  
-   **Conservazione dei backup settimanali per 4 settimane.** Il primo backup della settimana è conservato come backup settimanale. Dopo 4 settimane, il backup settimana meno recente sarà eliminato nel processo di pulizia.  
  
-   **Mantieni backup mensili per 6 mesi.** Il primo backup del mese è conservato come backup mensile. Dopo 6 mesi il backup mensile meno recente sarà eliminato nel processo di pulizia  
  
#### <a name="to-change-the-computer-backup-retention-policy"></a>Per modificare i criteri di conservazione dei backup del computer  
  
1.  Aprire il **Dashboard**.  
  
2.  Fare clic su **dispositivi**.  
  
3.  Nel **attività dispositivi** riquadro, fare clic su **le impostazioni di personalizzare il Backup del Computer e cronologia File**.  
  
    > [!NOTE]
    >  In Windows Server Essentials, questa attività è stata rinominata come **attività di backup Computer Client**.  
  
4.  In **strumenti e impostazioni di backup computer Client**, nel **criteri di conservazione dei backup computer Client** sezione, apportare le modifiche al criterio di conservazione adatto alle proprie esigenze.  
  
5.  Al termine, fare clic su **OK** per rendere effettive le modifiche e chiudere la finestra di dialogo. I criteri di conservazione aggiornato verranno applicati alla successiva pulizia dei backup.  
  
    > [!NOTE]
    >  I criteri di conservazione aggiornato si applicano a tutti i computer client nella rete che sono configurati per il backup.  
  
##  <a name="BKMK_6"></a>Ripristinare le impostazioni predefinite del backup  
 Dopo la configurazione del backup per i computer client, è possibile l'amministratore di rete abbia specificato un intervallo di tempo diverso. Analogamente, l'amministratore può abbia specificato un periodo di conservazione più lungo o più breve rispetto al valore predefinito. Il **Ripristina valori predefiniti** pulsante consente di reimpostare i criteri di finestra e conservazione dei backup per le impostazioni predefinite fornite durante la configurazione iniziale del backup.  
  
 I valori predefiniti sono:  
  
-   Ora di inizio backup: 6:00 PM  
  
-   Ora di fine: 9.00  
  
-   Numero di giorni per cui saranno conservati i backup giornalieri: 5 giorni  
  
-   Numero di settimane per cui l'ultimo backup della settimana è conservato: 4 settimane  
  
-   Numero di mesi per cui viene mantenuto l'ultimo backup del mese: 6 mesi  
  
#### <a name="to-reset-computer-backup-to-the-default-settings"></a>Per ripristinare le impostazioni predefinite del Backup di Computer  
  
1.  Aprire il **Dashboard**e quindi aprire il **dispositivi** pagina.  
  
2.  In **attività dispositivi**, fare clic su **le impostazioni di personalizzare il Backup del Computer e cronologia File**.  
  
    > [!NOTE]
    >  In Windows Server Essentials, fare clic su **attività di backup computer Client**.  
  
3.  Nel **Backup Computer** scheda della finestra di **strumenti e impostazioni di backup e i computer Client** pagina, fare clic su **Ripristina valori predefiniti**.  
  
    > [!NOTE]
    >  È possibile scrivere le personalizzate per la pianificazione e le impostazioni dei criteri di conservazione poiché saranno eliminate quando si ripristinano le impostazioni predefinite.  
  
4.  Fare clic su **applica**, quindi fare clic su **OK**.  
  
##  <a name="BKMK_7"></a>Utilizzare gli strumenti di ripristino  
 **Ripristinare il backup:** se il database di backup dei computer risulta danneggiato o inutilizzabile per qualche motivo, è possibile tentare di riparare il database utilizzando la riparazione guidata del Database di Backup. La procedura guidata analizza i file di backup per determinare se sono presenti eventuali problemi che possono essere ripristinati. La procedura guidata tenta quindi di risolvere eventuali problemi rilevati.  
  
> [!WARNING]
>  Se non è possibile risolvere un problema, la procedura guidata può eliminare definitivamente un file di backup del computer.  
  
 **Ripristino del computer:** è possibile creare un'unità flash USB avviabile da usare per ripristinare un computer da un backup esistente. È necessario utilizzare un'unità flash USB da 1 GB o superiori. Dopo aver creato l'unità flash USB avviabile, inserire nel computer che si desidera ripristinare e quindi avviare l'unità flash USB per avviare il processo di ripristino di sistema completo.  
  
##  <a name="BKMK_8"></a>Ripristinare il database di backup  
 Se si riceve un avviso che informa che il database di backup computer presenta problemi, è possibile tentare di ripristinarlo.  
  
#### <a name="to-repair-the-backup-database"></a>Per ripristinare il database di backup  
  
1.  Aprire il **Dashboard**.  
  
2.  Fare clic su **dispositivi**.  
  
3.  Nel **attività dispositivi** riquadro, fare clic su **le impostazioni di personalizzare il Backup del Computer e cronologia File.**  
  
    > [!NOTE]
    >  In Windows Server Essentials, fare clic su **attività di backup computer Client**.  
  
4.  In **strumenti e impostazioni di backup computer Client**, fare clic su di **strumenti** scheda.  
  
5.  Nel **ripristinare i backup** fare clic su **ripristinare ora**. Apre la riparazione guidata del Database di Backup.  
  
6.  Seguire le istruzioni in riparazione guidata del Database di Backup.  
  
7.  A seconda delle dimensioni è il database di backup, il ripristino del database può richiedere diverse ore. Fare clic su **chiudere**, quindi fare clic su **OK** per chiudere la **le impostazioni di personalizzare il Backup del Computer e cronologia File** pagina.  
  
    > [!NOTE]
    >  In Windows Server Essentials, fare clic su **strumenti e impostazioni di backup computer Client**.  
  
#### <a name="to-review-the-results-of-the-backup-database-repair"></a>Per esaminare i risultati della riparazione del database di backup  
  
1.  Aprire il **Dashboard**.  
  
2.  Fare clic su **dispositivi**.  
  
3.  Nel **attività dispositivi** riquadro, fare clic su **le impostazioni di personalizzare il Backup del Computer e cronologia File.**  
  
    > [!NOTE]
    >  In Windows Server Essentials, fare clic su **attività di backup computer Client**.  
  
4.  In **strumenti e impostazioni di backup computer Client**, fare clic su di **strumenti** scheda.  
  
5.  I risultati vengono visualizzati nel **ripristinare i backup** sezione.  
  
##  <a name="BKMK_9"></a>Disabilitare il backup per un computer  
 Usare il Dashboard per disabilitare rapidamente i backup per i computer nella rete.  
  
#### <a name="to-disable-backup-for-a-computer"></a>Per disabilitare il backup per un computer  
  
1.  Aprire il **Dashboard**.  
  
2.  Fare clic su di **dispositivi** scheda.  
  
3.  Fare clic sul nome del computer per cui si desidera disabilitare il backup.  
  
4.  Nel riquadro attività fare clic su **Personalizza Backup del computer**. Verrà visualizzata la procedura guidata Backup personalizzare.  
  
5.  Fare clic su **disabilitare il backup del computer**, quindi selezionare se si desidera mantenere o eliminare i file di backup esistenti.  
  
6.  Fare clic su **salvare le modifiche**, quindi fare clic su **Chiudi**.  
  
 Per informazioni su come abilitare il backup per un computer dopo la disabilitazione del backup, vedere [configurare il backup per un computer client](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_3).  
  
##  <a name="BKMK_10"></a>Eseguire l'attività pulizia dei backup  
 L'attività di pulizia dei backup computer client è pianificata per l'esecuzione ore 11:59 ogni sabato al termine di tutti i backup. L'attività di pulizia Elimina i backup del database di backup computer client in base al criterio di conservazione dei backup. Le impostazioni predefinite per i criteri di conservazione dei backup sono:  
  
-   Numero di giorni per cui saranno conservati i backup giornalieri: 5 giorni  
  
-   Numero di settimane per cui l'ultimo backup della settimana è conservato: 4 settimane  
  
-   Numero di mesi per cui viene mantenuto l'ultimo backup del mese: 6 mesi  
  
 Per informazioni sulla modifica dei criteri di conservazione dei backup, vedere [modificare i criteri di conservazione dei backup computer](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_5).  
  
#### <a name="to-run-the-client-backup-database-cleanup-task"></a>L'esecuzione di attività di pulizia del database di backup del client  
  
1.  Accedere al server con l'account dell'utente amministratore e la password.  
  
2.  Nel server, aprire **utilità di pianificazione**. È possibile accedere all'utilità di pianificazione dal **strumenti di amministrazione** console.  
  
3.  Nell'utilità di pianificazione, espandere **libreria utilità di pianificazione**, espandere **Microsoft**, espandere **Windows**, quindi fare clic su **Windows Server Essentials**.  
  
4.  Fare clic su di **pulizia dei Backup** attività, quindi fare clic su **eseguire** nel **azioni** riquadro. Lo stato cambierà in **esegue** fino al completamento dell'attività.  
  
##  <a name="BKMK_11"></a>Visualizzare gli avvisi nella barra delle applicazioni in un computer client  
 È possibile ricevere una notifica di backup nella barra delle applicazioni nel computer in uso per i motivi seguenti:  
  
-   Avvio di un backup.  
  
-   Fine di un backup.  
  
-   Non è disponibile un backup recente completato correttamente. Il numero di giorni trascorsi dall'ultimo backup riuscito è incluso nell'avviso.  
  
-    Solo Windows Server Essentials: viene aggiunto un nuovo volume al computer. La persona che gestisce la rete deve eseguire la procedura guidata Backup personalizzare da includere o escludere il volume nei backup futuri.  
  
##  <a name="BKMK_12"></a>Visualizzare lo stato di backup dalla finestra di avvio  
 Utilizzare la finestra di avvio per visualizzare lo stato del backup rapido per il computer.  
  
> [!TIP]
>  Per un riepilogo sullo stato, spostare il cursore sull'icona finestra di avvio nell'area di notifica del desktop. Nella descrizione comando viene visualizzato lo stato del backup.  
  
#### <a name="to-view-status-of-a-backup-for-your-computer"></a>Per visualizzare lo stato di un backup del computer  
  
1.  Accedere al **finestra di avvio** con il nome utente e password.  
  
2.  Fare clic su **Backup**.  
  
3.  Nel **proprietà Backup** della finestra di dialogo di **eseguire il Backup dello stato** sezione, lo stato del backup viene visualizzato.  
  
    -   **Nessun backup precedente**: il Backup non è ancora eseguito o la cronologia dei backup è stata eliminata.  
  
    -   **Backup in sospeso**: una copia di backup è in attesa di un altro processo sul server per completare.  
  
    -   **Connessione**: Backup si sta connettendo al server.  
  
    -   **Non riesci a connetterti**: Backup non può connettersi al servizio Provider di Windows Server Client Computer Backup.  
  
        > [!NOTE]
        >  In Windows Server Essentials, questo servizio è stato rinominato servizio gestione di Computer Client di Windows Server Essentials.  
  
    -   **Backup in corso**: Visualizza la percentuale di completamento del backup.  
  
    -   **Backup annullato**: Visualizza la data e ora di inizio del backup, se l'utente o dell'amministratore di rete annullamento del backup.  
  
    -   **Backup completato**: Visualizza la data e ora di inizio del backup, se il backup è stata completata correttamente.  
  
    -   **Backup incompleto**: Visualizza la data e ora di inizio del backup, se il backup non completato correttamente.  
  
    -   **Backup non riuscito**: Visualizza la data e ora di inizio del backup, se il backup non completato correttamente.  
  
4.  Fare clic su **OK** per chiudere la **proprietà Backup** la finestra di dialogo.  
  
##  <a name="BKMK_13"></a>Interrompere un backup in corso dalla finestra di avvio  
 È possibile arrestare facilmente un backup in corso.  
  
#### <a name="to-stop-a-backup-in-progress"></a>Per arrestare un backup in corso  
  
1.  Aprire il **finestra di avvio**.  
  
    > [!NOTE]
    >  Se richiesto, accedere al **finestra di avvio** con il nome utente e password.  
  
2.  Fare clic su **Backup**.  
  
3.  Nel **proprietà Backup** della finestra di dialogo di **eseguire il Backup dello stato** sezione, il **Avvia backup** pulsante Cambia in **backup Stop** quando il backup è in corso. Fare clic su **backup Stop**, quindi fare clic su **Sì** per confermare. L'operazione di backup continuerà fino a quando non si fa clic su **Sì**.  
  
4.  Quando si arresta un backup in corso, lo stato cambierà in **Backup annullato** con la data e l'ora di inizio del backup. Se, invece, viene visualizzato lo stato **riuscite**, **incompleto**, o **Failed**, l'ultimo backup è già stato completato.  
  
5.  Fare clic su **OK** per chiudere la **proprietà Backup** la finestra di dialogo.  
  
##  <a name="BKMK_14"></a>Avviare un backup dalla finestra di avvio  
 È possibile che quando si desidera eseguire il backup dei file e cartelle prima l'ora di backup regolari configurate nel server. La finestra di avvio consente di avviare manualmente il backup del computer.  
  
#### <a name="to-start-a-backup"></a>Per avviare un backup  
  
1.  Aprire il **finestra di avvio**.  
  
    > [!NOTE]
    >  Se richiesto, accedere al **finestra di avvio** con il nome utente e password.  
  
2.  Fare clic su **Backup**.  
  
3.  Nel **proprietà Backup** della finestra di dialogo di **eseguire il Backup dello stato** fare clic su **Avvia backup**e quindi fare clic su **OK**.  
  
4.  Digitare una descrizione per il backup e quindi fare clic su **OK**. Lo stato cambierà in **avvio del backup**e quindi a **Backup in corso** con una percentuale di completamento.  
  
5.  Dopo l'avvio del backup, il pulsante Cambia in **backup Stop**. Se è in corso il backup e si desidera arrestarlo, fare clic su **backup Stop**, quindi fare clic su **Sì**. Quando si arresta un backup in corso, lo stato cambierà in **Backup annullato** con la data e ora in cui è stato avviato il backup.  
  
6.  Quando il backup viene completata correttamente, lo stato cambierà in **Backup ha avuto esito positivo** con la data e l'ora di inizio del backup completato.  
  
7.  Fare clic su **OK** per chiudere la **proprietà Backup** la finestra di dialogo.  
  
##  <a name="BKMK_15"></a>Funzionamento di backup del computer  
 Si verifica quanto segue durante il backup ogni giorno:  
  
-   Computer di rete viene eseguito il backup in sequenza.  
  
-   Termine di un backup in corso, anche se si chiude la finestra del backup.  
  
-   Vengono installati gli aggiornamenti di Windows e Windows Server Essentials viene riavviato (se necessario).  
  
-   Alla domenica viene eseguita pulizia dei Backup.  
  
> [!IMPORTANT]
>  Servizio Copia Shadow del volume (VSS) non supporta la creazione di una copia shadow di una macchina virtuale e il volume host nello stesso set di snapshot. VSS supporta la creazione di snapshot dei volumi in un disco rigido virtuale (VHD), se il backup del volume virtuale è necessario. Per ulteriori informazioni, vedere [eseguire la manutenzione e il backup di dischi rigidi virtuali](https://go.microsoft.com/fwlink/p/?LinkId=256577).  
  
##  <a name="BKMK_16"></a>Suggerimenti utili per evitare la perdita di dati a causa del danneggiamento del database di backup del client  
 Se il database di backup del client risulta danneggiato, è possibile perdere dati importanti.  
  
 Per evitare la perdita di dati a causa del danneggiamento del database di backup del client, considerare quanto segue:  
  
-   Abilitare un metodo aggiuntivo di un backup dei database di backup del client. Ad esempio, a seconda della versione di Windows Server Essentials è in esecuzione, usare Server Backup, Backup in linea o Microsoft Azure Backup per il backup del database.  
  
    > [!IMPORTANT]
    >  Assicurarsi che si desidera eseguire il backup di tutti i file di **Backup computer Client** cartella.  
  
-   Nel caso in cui è necessario ripristinare il database, assicurarsi di ripristinare tutti i file di **Backup computer Client** cartella. Se non si ripristina tutti i file, il database risulti incoerente.  
  
-   Prima di pulire o ripristinare il database di backup del client, assicurati di interrompere tutti i backup dei client che sono attualmente in corso.  
  
     Se si esegue Windows Server Essentials, è consigliabile arrestare anche il servizio Backup Computer Client di Windows Server e il servizio Provider Backup Computer di Windows Server Client.  
  
     Se si esegue Windows Server Essentials, è consigliabile arrestare anche il servizio di Backup Computer Windows Server.  
  
     Dopo aver completato l'operazione di ripristino, riavviare il servizio.  
  
##  <a name="BKMK_17"></a>Ripristinare file o cartelle da un backup del computer client  
 È possibile selezionare e ripristinare singoli file e cartelle da un backup.  
  
> [!NOTE]
>  È possibile ripristinare file e cartelle in un computer client tramite il Dashboard sul server. È necessario aprire il Dashboard in un computer client per completare l'attività.  
  
> [!IMPORTANT]
>  È possibile ripristinare file e cartelle in un disco rigido che è stato configurato per essere un disco dinamico.  
  
#### <a name="to-restore-files-and-folders"></a>Per ripristinare file e cartelle  
  
1.  Aprire il **Dashboard** in un computer client.  
  
2.  Fare clic su di **dispositivi** scheda.  
  
3.  Fare clic sul computer per cui si desidera ripristinare file e cartelle, quindi fare clic su **ripristinare file o cartelle per il computer** nel **attività** riquadro. Il ripristino di file o cartelle guidata verrà visualizzata.  
  
4.  Seguire le istruzioni della procedura guidata.  
  
##  <a name="BKMK_FileHistory"></a>Informazioni su Cronologia File  
 La funzionalità cronologia File di Windows Server Essentials esegue automaticamente il backup di file presenti nelle cartelle raccolte, contatti, Desktop e Preferiti dei computer di rete che dispongono di funzionalità di cronologia File. Se gli originali vengono persi, danneggiati o eliminati, è possibile ripristinarli. È anche possibile trovare versioni diverse dei file da un punto specifico nel tempo. Nel corso del tempo si avrà una cronologia completa dei file.  
  
 In Windows Server Essentials, è possibile personalizzare le impostazioni di cronologia File dal **cronologia File** pagina di **strumenti e impostazioni di backup computer Client**.  
  
 In Windows Server Essentials, è possibile personalizzare le impostazioni di cronologia File passando al **utenti** scheda e quindi seleziona il **modificare l'impostazione di cronologia File** attività.  
  
 Nella pagina Cronologia File fornisce le seguenti opzioni:  
  
|Impostazione del backup|Impostazione predefinita|Descrizione|  
|--------------------|-------------|-----------------|  
|Attiva / disattiva|In|Cronologia file è attivata per impostazione predefinita quando si installa Windows Server Essentials.|  
|Dati di backup|Documenti e Desktop|Sono disponibili tre impostazioni preconfigurate, che consentono di specificare una serie di soluzioni di backup. È possibile scegliere una delle opzioni seguenti:<br /><br /> -Documenti e Desktop<br /><br /> -Tutte le raccolte, Desktop, contatti e Preferiti<br /><br /> -Tutti i dati in raccolte, Desktop, contatti e Preferiti, esclusi i dati di musica, Video e immagini|  
|Frequenza di backup|Ogni ora|Specifica la frequenza con cui cronologia File crea un backup dei dati selezionati. È possibile scegliere tra diverse opzioni che vanno da ogni 10 minuti oppure ogni giorno.|  
|Conserva copie per|1 anno|Specifica la quantità di tempo per cui cronologia File conserverà una copia di un backup.|  
  
 Per informazioni sui problemi relativi a cronologia file, vedere [risoluzione dei problemi di cronologia File](../support/Troubleshoot-File-History-in-Windows-Server-Essentials.md).  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire Backup e ripristino](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usare Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
