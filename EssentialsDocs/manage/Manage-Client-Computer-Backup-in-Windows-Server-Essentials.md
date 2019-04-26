---
title: Gestire il backup dei computer client in Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839272"
---
# <a name="manage-client-computer-backup-in-windows-server-essentials"></a>Gestire il backup dei computer client in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Questo articolo include informazioni sulle attività di backup comuni per i computer client che è possibile eseguire tramite il Dashboard di Windows Server Essentials e Dashboard e contiene le sezioni seguenti:  
  
-   [Come funziona la riparazione guidata di Database di Backup](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Comprendere le impostazioni di backup di computer](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Configurare il backup per un computer client](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Modifica dell'ora del backup pianificato per l'esecuzione](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Modificare i criteri di conservazione dei backup di computer](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Ripristinare le impostazioni predefinite del backup](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Usare gli strumenti di ripristino](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Ripristinare il database di backup](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Disabilitare il backup per un computer](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Eseguire l'attività di pulizia backup](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Visualizzare gli avvisi sulla barra delle applicazioni in un computer client](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Visualizzare lo stato di backup dalla finestra di avvio](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [Interrompe un backup in corso dalla finestra di avvio](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [Avviare un backup dalla finestra di avvio](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [Funzionamento del backup di computer](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_15)  
  
-   [Suggerimenti utili per evitare la perdita di dati a causa del danneggiamento del database di backup del client](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_16)  
  
-   [Ripristinare file o cartelle da un backup del computer client](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_17)  
  
-   [Informazioni su Cronologia File](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_FileHistory)  
  
##  <a name="BKMK_1"></a> Come funziona la riparazione guidata di Database di Backup  
 Se Windows Server Essentials rileva errori nel database di backup, invia una notifica relativa all'integrità e lo stato dell'avviso cambia in rosso, che indica una condizione critica.  
  
 Nel Dashboard di Windows Server Essentials fare clic sull'icona dello stato dell'avviso per visualizzare la notifica relativa all'errore del database di backup. La notifica include un pulsante **Ripristina** , che avvia la procedura guidata Ripristino database di backup. Per il completamento della procedura guidata potrebbero essere necessarie alcune ore.  
  
 La procedura guidata esamina il database di backup per il computer selezionato e tenta di ripristinare il database nel caso in cui siano rilevati errori. In alcuni casi la procedura guidata non è in grado di ripristinare l'intero database di backup. Prima di avviare la procedura guidata, verificare che tutte le unità disco rigido esterne usate nel server siano connesse al server, siano attivate e funzionino correttamente. È possibile che l'errore del database di backup venga corretto automaticamente tramite la riconnessione di un'unità disco rigido mancante.  
  
> [!CAUTION]
>  È consigliabile eseguire il backup del database prima di tentarne il ripristino. In base al tipo di errori rilevati nel database di backup, è possibile che la procedura guidata non sia in grado di recuperare alcuni backup. Alcuni o tutti i backup potrebbero andare persi in modo definitivo.  
  
##  <a name="BKMK_2"></a> Comprendere le impostazioni di backup di computer  
 Dopo la configurazione del backup per i computer client, sarà possibile specificare un intervallo di tempo diverso per l'esecuzione del backup. Analogamente sarà possibile specificare un periodo di conservazione più lungo o più breve rispetto al valore predefinito.  
  
 L'opzione **Ripristina predefiniti** permette di ripristinare i valori predefiniti, specificati durante la configurazione iniziale del backup, per la finestra di backup e i criteri di conservazione.  
  
 Di seguito sono indicati i valori predefiniti:  
  
|Impostazione di backup|Impostazione predefinita|Descrizione|  
|--------------------|-------------|-----------------|  
|Ora di inizio|18:00|Specifica l'ora di inizio del backup giornaliero. È consigliabile impostarla su un orario in cui gli utenti non usano i computer.|  
|Ora di fine|9.00|Specifica l'ora in cui il backup deve essere completato. Se il backup non richiede la durata specificata, userà solo il tempo necessario per il backup corretto del computer.|  
|Mantieni backup giornalieri|5 giorni|Specifica il numero di giorni per cui saranno conservati i backup giornalieri. Ad esempio, se si usa l'impostazione predefinita, saranno conservati cinque backup giornalieri. A partire dal sesto giorno e in ogni giorno successivo sarà eliminato il file di backup giornaliero meno recente.|  
|Mantieni backup settimanali|4 settimane|Specifica il numero di settimane per cui sarà conservato il backup settimanale più recente. Ad esempio, se si usa l'impostazione predefinita, saranno conservati quattro backup settimanali. A partire dalla quinta settimana e in ogni settimana successiva sarà eliminato il backup settimanale meno recente.|  
|Mantieni backup mensili|6 months|Specifica il numero di mesi per cui sarà conservato il backup mensile più recente. Ad esempio, se si usa l'impostazione predefinita, saranno conservati sei mesi di backup. A partire dal settimo mese e in ogni mese successivo sarà eliminato il backup mensile meno recente.|  
  
##  <a name="BKMK_3"></a> Configurare il backup per un computer client  
 Se il backup è disabilitato, sarà possibile configurare il backup per il computer dal dashboard. Quando si configura il backup per un computer, è possibile scegliere di eseguire il backup completo del computer oppure selezionare i volumi e le cartelle di cui eseguire un backup.  
  
> [!NOTE]
>  Per la configurazione del backup è necessario che il computer sia online.  
  
> [!IMPORTANT]
>   Windows Server Essentials non supporta il backup e ripristino dei dischi dinamici nei computer client.  
  
#### <a name="to-set-up-backup-for-a-client-computer"></a>Per configurare il backup per un computer client  
  
1.  Aprire il **Dashboard**, quindi fare clic sulla scheda **Dispositivi** .  
  
2.  Fare clic sul nome del computer client per cui si vuole configurare il backup, quindi nel riquadro **Attività** fare clic su **Imposta backup del computer**.  
  
    > [!NOTE]
    >  Se il backup è già configurato per il computer client, l'opzione **Personalizza backup del computer** sarà elencata nel riquadro **Attività** invece dell'opzione **Imposta backup del computer**.  
  
3.  Nella procedura guidata Imposta backup del computer è possibile scegliere di eseguire il backup di tutte le cartelle oppure selezionare determinate cartelle per il backup. Seguire le istruzioni contenute nella procedura guidata.  
  
4.  Dopo avere configurato il backup per il computer, fare clic su **Chiudi** .  
  
### <a name="critical-system-files"></a>File di sistema critici  
 Quando si installa il sistema operativo Windows, il programma di installazione crea cartelle nell'unità di sistema in cui inserisce i file necessari per l'avvio e l'esecuzione del sistema. I file di sistema critici includono i file con estensioni dll, exe, ocx e sys. Alcuni di questi file sono tipi di carattere True Type. Inoltre, i file di stato di sistema, ad esempio il sistema s del Registro di sistema, sono necessari per il sistema operativo eseguire correttamente.  
  
### <a name="find-the-file-you-are-looking-for"></a>Individuare un file specifico  
 È possibile ripristinare tutte le cartelle di un computer, più file e cartelle o un singolo file o cartella da un backup esistente.  
  
 Dopo avere selezionato il backup da usare per il ripristino, il file di backup sarà letto e tutti i file e le cartelle saranno visualizzati. È possibile eseguire il drill-down fino a una file specifico o una cartella da ripristinare facendo doppio clic sulla cartella di livello principale, quindi eseguendo il drill-down nella gerarchia di cartelle fino a individuare il file da ripristinare.  
  
### <a name="why-am-i-unable-to-select-some-items"></a>Perché non è possibile selezionare alcuni elementi?  
 La casella di controllo nel menu di selezione della pagina **Seleziona elementi per il backup** può indicare stati diversi per ogni cartella. Se la casella di controllo è:  
  
-   **Selezionata**, la cartella associata e i contenuti della cartella saranno selezionati per il backup.  
  
-   **Deselezionata**, la cartella associata e i contenuti della cartella saranno esclusi dal backup.  
  
-   **Piena**, la cartella associata è selezionata per il backup, ma uno o più elementi della cartella sono esclusi dal backup.  
  
 Se non è possibile selezionare una cartella specifica:  
  
-   Il volume potrebbe essere configurato per il backup, ma potrebbe essere offline. Ciò accade spesso per le unità USB rimovibili. I volumi offline sono visualizzati con testo di colore grigio.  
  
-   Si può eseguire il backup di dati solo da un'unità locale formattata come file system NTFS. Le unità formattate come file system FAT (incluso FAT32) o ReFS non saranno visualizzate nell'elenco di unità di cui eseguire un backup.  
  
> [!IMPORTANT]
>  Il servizio Copia Shadow del volume (VSS, Volume Shadow Copy Service) non supporta la creazione di una copia shadow di una macchina virtuale e del volume host nello stesso set istantanee. VSS supporta la creazione di snapshot dei volumi in un disco rigido virtuale (VHD, Virtual Hard Disk), se il backup del volume virtuale è necessario. Per altre informazioni, vedere [Eseguire la manutenzione e il backup di dischi rigidi virtuali](https://go.microsoft.com/fwlink/p/?LinkId=256577).  
  
##  <a name="BKMK_4"></a> Modifica dell'ora del backup pianificato per l'esecuzione  
 È consigliabile pianificare il processo di backup in un orario in cui il minor numero possibile di persona usa i computer in rete. Di solito si tratta della sera tardi o delle prime ore della mattina. L'orario predefinito per il backup è compreso tra le 18:00 e le 9:00. Il server tenta di eseguire il backup dei computer client solo durante l'intervallo di tempo pianificato.  
  
 Se tuttavia l'organizzazione specifica è operativa durante questi orari, è consigliabile modificare le impostazioni predefinite.  
  
#### <a name="to-change-the-time-that-backup-is-scheduled-to-run"></a>Per modificare l'orario pianificato per l'esecuzione del backup  
  
1.  Aprire il **Dashboard** del server, quindi fare clic su **Dispositivi**.  
  
2.  In **Attività dispositivo** fare clic su **Personalizza impostazioni per backup computer e cronologia file**.  
  
    > [!NOTE]
    >  In Windows Server Essentials, questa attività è stata rinominata **attività di backup computer Client**.  
  
3.  In **Impostazioni e strumenti per il backup del computer client**nella scheda **Backup computer** è possibile modificare gli orari di inizio e di fine in base alle esigenze specifiche.  
  
4.  Fare clic su **Applica** e quindi su **OK**.  
  
##  <a name="BKMK_5"></a> Modificare i criteri di conservazione dei backup di computer  
 I backup giornalieri di tutti i computer si accumulano sul server nel corso del tempo. Per semplificare la gestione di questi backup, Windows Server Essentials permette di gestire i database di backup dei computer. È possibile configurare il numero di backup da mantenere per tutti i computer.  
  
 I criteri di conservazione dei backup determinano il periodo di tempo per cui un backup sarà conservato prima dell'eliminazione durante il processo di pulizia dei backup. La pulizia dei backup è eseguita ogni sabato alle 23:59. Elimina tutti i backup non inclusi nei criteri di conservazione dei backup. Le impostazioni predefinite per i criteri di conservazione dei backup sono le seguenti:  
  
-   **Conservazione dei backup giornalieri per 5 giorni.** Il primo backup della giornata è conservato come backup giornaliero. Dopo 5 giorni il backup giornaliero meno recente sarà eliminato nel processo di pulizia.  
  
-   **Conservazione dei backup settimanali per 4 settimane.** Il primo backup della settimana è conservato come backup settimanale. Dopo 4 settimane il backup settimanale meno recente sarà eliminato nel processo di pulizia.  
  
-   **Conservazione dei backup mensili per 6 mesi.** Il primo backup del mese è conservato come backup mensile. Dopo 6 mesi il backup mensile meno recente sarà eliminato nel processo di pulizia.  
  
#### <a name="to-change-the-computer-backup-retention-policy"></a>Per modificare i criteri di conservazione dei backup del computer  
  
1.  Aprire il **Dashboard**.  
  
2.  Fare clic su **Dispositivi**.  
  
3.  Nel riquadro **Attività dispositivo** fare clic su **Personalizza impostazioni per backup computer e cronologia file**.  
  
    > [!NOTE]
    >  In Windows Server Essentials, questa attività è stata rinominata **attività di backup Computer Client**.  
  
4.  In **Impostazioni e strumenti per il backup del computer client**nella sezione **Criteri di gestione backup computer client** modificare i criteri di conservazione in base alle esigenze specifiche.  
  
5.  Al termine, fare clic su **OK** per applicare le modifiche e chiudere la finestra di dialogo. Il criterio di conservazione aggiornato sarà applicato alla successiva esecuzione della pulizia dei backup.  
  
    > [!NOTE]
    >  Il criterio di conservazione aggiornato sarà applicato a tutti i computer client in rete configurati per il backup.  
  
##  <a name="BKMK_6"></a> Ripristinare le impostazioni predefinite del backup  
 Dopo la configurazione del backup per i computer client, è possibile che l'amministratore di rete abbia specificato un intervallo di tempo diverso. Analogamente, è possibile che l'amministratore abbia specificato un periodo di conservazione del backup più lungo o più breve rispetto al valore predefinito. Il pulsante **Ripristina valori predefiniti** permette di ripristinare i valori per l'intervallo di backup e i criteri di conservazione, usando le impostazioni specificate durante la configurazione iniziale del backup.  
  
 Di seguito sono indicati i valori predefiniti:  
  
-   Ora di inizio backup: 18:00  
  
-   Ora di fine: 9.00  
  
-   Numero di giorni per cui saranno conservati i backup giornalieri: 5 giorni  
  
-   Numero di settimane per cui sarà conservato il backup settimanale più recente: 4 settimane  
  
-   Numero di mesi per cui sarà conservato il backup mensile più recente: 6 months  
  
#### <a name="to-reset-computer-backup-to-the-default-settings"></a>Per ripristinare le impostazioni predefinite di Backup computer  
  
1.  Aprire il **Dashboard**, quindi aprire la pagina **Dispositivi** .  
  
2.  In **Attività dispositivo** fare clic su **Personalizza impostazioni per backup computer e cronologia file**.  
  
    > [!NOTE]
    >  In Windows Server Essentials, fare clic su **attività di backup computer Client**.  
  
3.  Nella scheda **Backup computer** della pagina **Impostazioni e strumenti per il backup del computer client** fare clic su **Ripristina valori predefiniti**.  
  
    > [!NOTE]
    >  È consigliabile annotare le impostazioni personalizzate per la pianificazione e per i criteri di conservazione, poiché saranno eliminate quando si ripristinano le impostazioni predefinite.  
  
4.  Fare clic su **Applica** e quindi su **OK**.  
  
##  <a name="BKMK_7"></a> Usare gli strumenti di ripristino  
 **Ripristinare il backup:** Se il database di backup dei computer risulta danneggiato o inutilizzabile per qualche motivo, è possibile tentare di ripristinare il database usando la procedura guidata Ripristino database di backup. La procedura guidata analizza i file di backup per determinare se sono presenti eventuali problemi che possono essere risolti tramite ripristino. La procedura guidata tenta quindi di risolvere i problemi rilevati.  
  
> [!WARNING]
>  Se non è possibile risolvere un problema, è possibile che la procedura guidata elimini in modo definitivo un file di backup del computer.  
  
 **Ripristino del computer:** È possibile creare un'unità flash USB avviabile da usare per ripristinare un computer da un backup esistente. È necessario usare un'unità flash USB da 1 GB o superiore. Dopo la creazione dell'unità flash USB avviabile, è necessario inserirla nel computer da ripristinare, quindi effettuare l'avvio sull'unità flash USB per avviare il processo di ripristino completo del sistema.  
  
##  <a name="BKMK_8"></a> Ripristinare il database di backup  
 Se si riceve un avviso relativo a problemi nel database di backup del computer, è possibile tentare di ripristinarlo.  
  
#### <a name="to-repair-the-backup-database"></a>Per ripristinare il database di backup  
  
1.  Aprire il **Dashboard**.  
  
2.  Fare clic su **Dispositivi**.  
  
3.  Nel riquadro **Attività dispositivo** fare clic su **Personalizza impostazioni per backup computer e cronologia file**.  
  
    > [!NOTE]
    >  In Windows Server Essentials, fare clic su **attività di backup computer Client**.  
  
4.  In **Impostazioni e strumenti per il backup del computer client** fare clic sulla scheda **Strumenti**.  
  
5.  Nella sezione **Ripristina backup** fare clic su **Ripristina ora**. Sarà visualizzata la procedura guidata Ripristino database di backup.  
  
6.  Seguire le istruzioni visualizzate nella procedura guidata Ripristino database di backup.  
  
7.  In base alle dimensioni del database di backup, il ripristino del database può richiedere alcune ore. Fare clic su **Chiudi**, quindi su **OK** per chiudere la pagina **Personalizza impostazioni per backup computer e cronologia file**.  
  
    > [!NOTE]
    >  In Windows Server Essentials, fare clic su **le impostazioni di backup computer Client e gli strumenti**.  
  
#### <a name="to-review-the-results-of-the-backup-database-repair"></a>Per esaminare i risultati del ripristino del database di backup  
  
1.  Aprire il **Dashboard**.  
  
2.  Fare clic su **Dispositivi**.  
  
3.  Nel riquadro **Attività dispositivo** fare clic su **Personalizza impostazioni per backup computer e cronologia file**.  
  
    > [!NOTE]
    >  In Windows Server Essentials, fare clic su **attività di backup computer Client**.  
  
4.  In **Impostazioni e strumenti per il backup del computer client** fare clic sulla scheda **Strumenti**.  
  
5.  I risultati saranno visualizzati nella sezione **Ripristina backup** .  
  
##  <a name="BKMK_9"></a> Disabilitare il backup per un computer  
 Usare il dashboard per disabilitare rapidamente i backup per i computer in rete.  
  
#### <a name="to-disable-backup-for-a-computer"></a>Per disabilitare il backup per un computer  
  
1.  Aprire il **Dashboard**.  
  
2.  Fare clic sulla scheda **Dispositivi**.  
  
3.  Fare clic sul nome del computer per cui si vuole disabilitare il backup.  
  
4.  Nel riquadro attività fare clic su **Personalizza backup del computer**. Sarà visualizzata la procedura guidata per la personalizzazione del backup.  
  
5.  Fare clic su **Disabilita backup del computer**, quindi specificare se si vogliono mantenere o eliminare i file di backup esistenti.  
  
6.  Fare clic su **Salva modifiche**, quindi su **Chiudi**.  
  
 Per informazioni su come abilitare il backup per un computer dopo la disabilitazione del backup, vedere [Set up backup for a client computer](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_3).  
  
##  <a name="BKMK_10"></a> Eseguire l'attività di pulizia backup  
 L'esecuzione dell'attività di pulizia del backup del computer client è pianificata ogni sabato alle 23:59, al termine di tutti i backup. L'attività di pulizia elimina i backup dal database di backup del computer client, in base ai criteri di conservazione dei backup. Le impostazioni predefinite per i criteri di conservazione dei backup sono le seguenti:  
  
-   Numero di giorni per cui saranno conservati i backup giornalieri: 5 giorni  
  
-   Numero di settimane per cui sarà conservato il backup settimanale più recente: 4 settimane  
  
-   Numero di mesi per cui sarà conservato il backup mensile più recente: 6 months  
  
 Per informazioni sulla modifica dei criteri di conservazione dei backup, vedere [Modificare i criteri di conservazione dei backup del computer](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_5).  
  
#### <a name="to-run-the-client-backup-database-cleanup-task"></a>Per eseguire l'attività di pulizia del database di backup del computer client  
  
1.  Accedere al server con l'account utente e la password per Administrator.  
  
2.  Nel server aprire l'**Utilità di pianificazione**. È possibile accedere all'Utilità di pianificazione tramite la console **Strumenti di amministrazione**.  
  
3.  Nell'Utilità di pianificazione espandere **Libreria Utilità di pianificazione**, quindi **Microsoft**, **Windows** e infine fare clic su **Windows Server Essentials**.  
  
4.  Fare clic sull'attività **Pulizia backup** e quindi su **Esegui** nel riquadro **Azioni**. Lo stato cambierà in **In esecuzione** fino al completamento dell'attività.  
  
##  <a name="BKMK_11"></a> Visualizzare gli avvisi sulla barra delle applicazioni in un computer client  
 È possibile ricevere una notifica di backup nella barra delle applicazioni nel computer per i motivi seguenti:  
  
-   Avvio di un backup.  
  
-   Fine di un backup.  
  
-   Nessun backup riuscito recente. Il numero di giorni trascorsi dall'ultimo backup riuscito è incluso nella notifica.  
  
-    Solo Windows Server Essentials: Aggiunta di un nuovo volume al computer. L'amministratore di rete deve eseguire la procedura guidata per la personalizzazione del backup per includere o escludere il volume nei backup futuri.  
  
##  <a name="BKMK_12"></a> Visualizzare lo stato di backup dalla finestra di avvio  
 Usare la finestra di avvio per visualizzare rapidamente lo stato del backup per il computer.  
  
> [!TIP]
>  Per un'indicazione rapida sullo stato, spostare il cursore sull'icona della finestra di avvio nell'area di notifica del desktop. Lo stato del backup è visualizzato nella descrizione comando.  
  
#### <a name="to-view-status-of-a-backup-for-your-computer"></a>Per visualizzare lo stato di un backup del computer  
  
1.  Accedere alla **Finestra di avvio** con il proprio nome utente e la propria password.  
  
2.  Fare clic su **Backup**.  
  
3.  Nella finestra di dialogo **Proprietà backup** lo stato del backup sarà visualizzato nella sezione **Stato backup** .  
  
    -   **Nessun backup precedente**: il backup non è stato ancora eseguito o la cronologia dei backup è stata eliminata.  
  
    -   **Backup in sospeso**: un backup in attesa del completamento di un altro processo sul server.  
  
    -   **Connessione in corso**: il backup si sta connettendo al server.  
  
    -   **Impossibile connettersi**: il backup non riesce a connettersi al servizio Provider backup computer client Windows Server.  
  
        > [!NOTE]
        >  In Windows Server Essentials, questo servizio è stato rinominato servizio gestione di Computer Client di Windows Server Essentials.  
  
    -   **Backup in corso**: mostra la percentuale di completamento del backup.  
  
    -   **Backup annullato**: mostra la data e l'ora di inizio del backup in caso di annullamento del backup da parte dell'utente o dell'amministratore di rete.  
  
    -   **Backup completato**: mostra la data e l'ora di inizio del backup in caso di completamento corretto del backup.  
  
    -   **Backup incompleto**: mostra la data e l'ora di inizio del backup in caso di mancato completamento del backup.  
  
    -   **Backup non riuscito**: mostra la data e l'ora di inizio del backup in caso di mancato completamento del backup.  
  
4.  Fare clic su **OK** per chiudere la finestra di dialogo **Proprietà backup**.  
  
##  <a name="BKMK_13"></a> Interrompe un backup in corso dalla finestra di avvio  
 È possibile arrestare facilmente un backup in corso.  
  
#### <a name="to-stop-a-backup-in-progress"></a>Per arrestare un backup in corso  
  
1.  Aprire la **Finestra di avvio**.  
  
    > [!NOTE]
    >  Se richiesto, accedere alla **Finestra di avvio** specificando nome utente e password.  
  
2.  Fare clic su **Backup**.  
  
3.  Nella finestra di dialogo **Proprietà backup** il pulsante **Avvia backup** nella sezione **Stato backup** cambia in **Interrompi backup** durante l'esecuzione del backup. Fare clic su **Interrompi backup**, quindi su **Sì** per confermare. L'esecuzione del backup continuerà fino a quando non si fa clic su **Sì**.  
  
4.  Quando si interrompe un backup in corso, lo stato cambierà in **Backup annullato** e saranno indicate la data e l'ora di inizio del backup. Se invece lo stato visualizzato è **Riuscito**, **Incompleto**o **Non riuscito**, l'ultimo backup è già stato completato.  
  
5.  Fare clic su **OK** per chiudere la finestra di dialogo **Proprietà backup**.  
  
##  <a name="BKMK_14"></a> Avviare un backup dalla finestra di avvio  
 In alcuni casi è possibile che si voglia eseguire il backup di file e cartelle prima dell'ora pianificata per l'esecuzione normale del backup definita sul server. La finestra di avvio permette di avviare manualmente il backup del computer.  
  
#### <a name="to-start-a-backup"></a>Per avviare un backup  
  
1.  Aprire la **Finestra di avvio**.  
  
    > [!NOTE]
    >  Se richiesto, accedere alla **Finestra di avvio** specificando nome utente e password.  
  
2.  Fare clic su **Backup**.  
  
3.  Nella finestra di dialogo **Proprietà backup** fare clic su **Avvia backup** nella sezione **Stato backup**, quindi fare clic su **OK**.  
  
4.  Digitare una descrizione per il backup, quindi fare clic su **OK**. Lo stato cambierà in **Avvio backup**, quindi in **Backup in corso**, con una percentuale di completamento.  
  
5.  Dopo l'avvio del backup, il pulsante cambia in **Interrompi backup**. Se si vuole arrestare un backup in corso, fare clic su **Interrompi backup**, quindi su **Sì**. Quando si interrompe un backup in corso, lo stato cambierà in **Backup annullato** e saranno indicate la data e l'ora di inizio del backup.  
  
6.  In caso di completamento corretto del backup, lo stato cambierà in **Backup completato** e saranno indicate la data e l'ora di inizio del backup completato.  
  
7.  Fare clic su **OK** per chiudere la finestra di dialogo **Proprietà backup**.  
  
##  <a name="BKMK_15"></a> Funzionamento del backup di computer  
 Ogni giorno, durante il backup, sono eseguite le operazioni seguenti:  
  
-   Viene eseguito il backup in sequenza dei computer di rete.  
  
-   Un backup in corso sarà completato anche se si chiude la finestra del backup.  
  
-   Vengono installati gli aggiornamenti di Windows e Windows Server Essentials viene riavviato, se necessario.  
  
-   Alla domenica viene eseguita la pulizia dei backup.  
  
> [!IMPORTANT]
>  Il servizio Copia Shadow del volume (VSS, Volume Shadow Copy Service) non supporta la creazione di una copia shadow di una macchina virtuale e del volume host nello stesso set istantanee. VSS supporta la creazione di snapshot dei volumi in un disco rigido virtuale (VHD, Virtual Hard Disk), se il backup del volume virtuale è necessario. Per altre informazioni, vedere [Eseguire la manutenzione e il backup di dischi rigidi virtuali](https://go.microsoft.com/fwlink/p/?LinkId=256577).  
  
##  <a name="BKMK_16"></a> Suggerimenti utili per evitare la perdita di dati a causa del danneggiamento del database di backup del client  
 In caso di danneggiamento del database di backup del client, è possibile che vadano persi dati critici.  
  
 Per evitare la perdita di dati a causa del danneggiamento del database di backup del client, prendere in considerazione quanto segue:  
  
-   Abilitare un metodo aggiuntivo per il backup del database di backup dei client. Ad esempio, a seconda della versione di Windows Server Essentials in esecuzione, usare Server Backup, Backup Online o Backup di Microsoft Azure per il backup del database.  
  
    > [!IMPORTANT]
    >  Assicurarsi di selezionare l'opzione per il backup di tutti i file della cartella **Backup computer client**.  
  
-   Se occorre ripristinare il database, assicurarsi di ripristinare tutti i file della cartella **Backup computer client** . In caso contrario, è possibile che il database risulti incoerente.  
  
-   Prima di pulire o ripristinare il database di backup dei client, arrestare tutti i backup dei client attualmente in corso.  
  
     Se si esegue Windows Server Essentials, è consigliabile arrestare anche il servizio Backup Computer Client di Windows Server e il servizio Provider Backup Computer di Windows Server Client.  
  
     Se si esegue Windows Server Essentials, è consigliabile arrestare anche il servizio Backup Computer Windows Server.  
  
     Al termine dell'operazione di ripristino, riavviare il servizio.  
  
##  <a name="BKMK_17"></a> Ripristinare file o cartelle da un backup del computer client  
 È possibile individuare e ripristinare singoli file e cartelle da un backup.  
  
> [!NOTE]
>  Non è possibile ripristinare file e cartelle in un computer client usando il dashboard sul server. È necessario aprire il dashboard in un computer client per completare l'attività.  
  
> [!IMPORTANT]
>  Non è possibile ripristinare file e cartelle in un disco rigido configurato come disco dinamico.  
  
#### <a name="to-restore-files-and-folders"></a>Per ripristinare file e cartelle  
  
1.  Aprire il **Dashboard** in un computer client.  
  
2.  Fare clic sulla scheda **Dispositivi**.  
  
3.  Fare clic sul computer che include i file e le cartelle da ripristinare, quindi fare clic su **Ripristino file o cartelle del computer** nel riquadro **Attività** . Verrà visualizzata la procedura guidata Ripristino file o cartelle.  
  
4.  Seguire le istruzioni contenute nella procedura guidata.  
  
##  <a name="BKMK_FileHistory"></a> Informazioni su Cronologia File  
 La funzionalità Cronologia file di Windows Server Essentials esegue automaticamente il backup di file disponibili nelle cartelle Raccolte, Contatti, Desktop e Preferiti dei computer di rete che dispongono di funzionalità di Cronologia file. In caso di perdita, danneggiamento o eliminazione dei file originali, sarà possibile ripristinarli. È anche possibile trovare versioni diverse dei file da un punto specifico nel tempo. Nel corso del tempo si avrà a disposizione una cronologia completa dei file.  
  
 In Windows Server Essentials, è possibile personalizzare le impostazioni di cronologia File dal **cronologia File** pagina della **le impostazioni di backup computer Client e gli strumenti**.  
  
 In Windows Server Essentials, è possibile personalizzare le impostazioni di cronologia File passando il **gli utenti** scheda e quindi selezionando il **modificare l'impostazione cronologia File** attività.  
  
 Nella pagina Cronologia file sono disponibili le opzioni seguenti:  
  
|Impostazione di backup|Impostazione predefinita|Descrizione|  
|--------------------|-------------|-----------------|  
|Attiva / Disattiva|Attivato|La funzionalità Cronologia file è attivata per impostazione predefinita quando si installa Windows Server Essentials.|  
|Dati di backup|Documenti e desktop|Sono disponibili tre impostazioni preconfigurate, che permettono di specificare diverse soluzioni di backup. È possibile scegliere una delle opzioni seguenti:<br /><br /> -Documenti e Desktop<br /><br /> -Tutte le raccolte, Desktop, contatti e Preferiti<br /><br /> -Tutti i dati in raccolte, Desktop, contatti e Preferiti, esclusi i dati nella musica, Video e immagini|  
|Frequenza di backup|Ogni ora|Specifica la frequenza con cui Cronologia file crea un backup dei dati selezionati. È possibile scegliere tra diverse opzioni, ad esempio ogni 10 minuti oppure ogni giorno.|  
|Conserva copie per|1 anno|Specifica il periodo di tempo per cui Cronologia file conserverà una copia di un backup.|  
  
 Per informazioni sui problemi relativi a Cronologia file, vedere [Risoluzione dei problemi di Cronologia file](../support/Troubleshoot-File-History-in-Windows-Server-Essentials.md).  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire il Backup e ripristino](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usare Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
