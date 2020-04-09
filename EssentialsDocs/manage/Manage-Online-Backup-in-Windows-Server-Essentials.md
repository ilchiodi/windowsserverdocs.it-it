---
title: Gestire il backup online in Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 95a9f593-fad7-4335-bd4d-c7bb8c033efb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 5c7db7f2e39ec0f46f2b4d36537a0db94980c108
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852774"
---
# <a name="manage-online-backup-in-windows-server-essentials"></a>Gestire il backup online in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Dopo l'integrazione con Backup di Microsoft Azure, nel dashboard di Windows Server Essentials viene visualizzata la pagina Gestione **backup online** . La pagina **Backup online** consente di eseguire comuni attività amministrative. Le funzionalità di questa pagina includono:  
  
- Le pagine di sottosezioni seguenti:  
  
  -   **Backup online** Dopo la registrazione del server per il backup online, questa sezione visualizza lo stato corrente del backup, lo stato dell'archiviazione e informazioni sull'account.  
  
  -   **Cartelle protette** Questa sezione elenca tutte le cartelle condivise e tutte le cartelle di cronologia file nel server e qualsiasi altra cartella di cui si è scelto di eseguire il backup in Azure. L'elenco include il nome, il percorso e lo stato di ogni cartella condivisa.  
  
  -   **Cronologia backup** Questa sezione visualizza un elenco dei recenti backup online. L'elenco include il tipo di operazione, l'ora e lo stato di ogni backup.  
  
- Un riquadro dei dettagli che include informazioni aggiuntive su un processo di backup o di ripristino selezionato.  
  
- Un riquadro attività che include una serie di attività amministrative che è possibile eseguire.  
  
  Come procedura consigliata, eseguire il backup dei dati aziendali e utente più importanti, ad esempio le cartelle del server che contengono file di dati critici. È inoltre consigliabile eseguire il backup della Cronologia file per i computer di rete che contengono informazioni critiche.  
  
  Quando si scelgono i dati di cui eseguire il backup, considerare la frequenza di backup e i criteri di conservazione da implementare. Esaminare quindi il budget disponibile e determinare la quantità di spazio di archiviazione che è possibile acquisire. Valutare i costi e il volume dello spazio di archiviazione rispetto alle esigenze e quindi configurare il backup online in modo da proteggere quanti più dati importanti possibile. Per informazioni sui prezzi, vedere i [dettagli dei prezzi per Azure Backup](https://azure.microsoft.com/pricing/details/backup/).  
  
  Le sezioni seguenti descrivono varie attività di backup online che possono essere visualizzate nel dashboard di Windows Server Essentials.  
  
## <a name="online-backup-tasks-in-the-dashboard"></a>Attività di backup nel dashboard  
  
-   [Caricare un certificato nell'insieme di credenziali di backup di Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Configurare il backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Avviare un backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Ripristinare file e cartelle da un backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Registra questo server per il backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Annulla registrazione server](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
###  <a name="upload-a-certificate-to-the-azure-backup-vault"></a><a name="BKMK_1"></a>Caricare un certificato nell'insieme di credenziali di backup di Azure  
 Prima di poter usare backup di Azure per i backup online in Windows Server Essentials, è necessario caricare un certificato pubblico da registrare con l'insieme di credenziali per il backup. Il certificato viene usato per autenticare la distribuzione di backup di Azure (agente), che agisce per conto del proprietario della sottoscrizione dei Microsoft Online Services per gestire le risorse associate alla sottoscrizione.  
  
> [!NOTE]
>  Prima di caricare il certificato, è necessario completare le procedure riportate in [Iscriversi al servizio Backup di Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16).  
  
##### <a name="to-upload-a-certificate-to-use-with-the-azure-backup-service"></a>Per caricare un certificato da usare con il servizio Backup di Azure  
  
1. Accedere al dashboard di Windows Server Essentials con un account di amministratore.  
  
2. Nella pagina **Home** del dashboard fare clic su **BACKUP ONLINE**.  
  
3. Nell'area **BACKUP ONLINE** fare clic su **Carica certificato nell'insieme di credenziali di Backup di Azure**.  
  
    Verrà aperto **servizi di ripristino** nel portale di gestione di Azure. Se non è già stato effettuato l'accesso ad Azure, è necessario eseguire l'accesso con l'account Microsoft.  
  
4. Fare clic sul nome dell'insieme di credenziali per il backup da usare per i backup online per aprire la pagina **Avvio rapido** corrispondente.  
  
5. Fare clic su **Gestisci certificato**.  
  
6. Nella finestra di dialogo **Gestisci certificato** incollare il percorso del certificato pubblico generato da Windows Server Essentials. Per ottenere il percorso del certificato pubblico, procedere come segue:  
  
   1.  Nel dashboard di Windows Server Essentials fare clic sulla scheda **Backup online**.  
  
   2.  Nella pagina **Backup online** copiare il percorso del certificato generato.  
  
   3.  Passare alla portale di gestione di Azure, quindi nella finestra di dialogo **Gestisci certificato** incollare il percorso per caricare il certificato pubblico generato.  
  
   > [!NOTE]
   >  È anche possibile usare il proprio certificato pubblico. Per identificare il certificato necessario, nella pagina **Avvio rapido** fare clic sul collegamento **Acquisisci certificato**.  
  
   > [!NOTE]
   >   Azure richiede un certificato x. 509 V2 con una chiave pubblica. Per altre informazioni, vedere [Gestire i certificati di insiemi di credenziali](https://msdn.microsoft.com/library/azure/dn169036.aspx).  
  
7. Dopo aver selezionato il certificato, fare clic su **OK** (segno di spunta).  
  
8. Verrà visualizzata nuovamente la pagina **Avvio rapido**. Fare clic su **Dashboard** e verificare se il certificato è stato caricato correttamente. Dopo il caricamento del certificato, il dashboard visualizza l'identificazione personale del certificato e la data di scadenza.  
  
   Dopo aver completato la procedura, eseguire le operazioni seguenti:  
  
9. Registrare il server con il servizio backup di Azure. Per altre informazioni, vedere [Registrare il server corrente per il backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5).  
  
10. Configurare il backup online del server. Per altre informazioni, vedere [Configurare il backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
###  <a name="configure-online-backup"></a><a name="BKMK_2"></a>Configurare il backup online  
 Dopo aver registrato il server con backup di Azure, è possibile configurare le impostazioni di backup online in Windows Server Essentials.  
  
##### <a name="to-configure-online-backup"></a>Per configurare il backup online  
  
1.  In Registrazione guidata server o nella pagina **Backup online** del dashboard di Windows Server Essentials, fare clic su **Configura backup online**. Verrà visualizzata la Configurazione guidata backup online.  
  
2.  Nella pagina **Configura backup online** selezionare la casella di controllo per ogni cartella del server di cui si vuole eseguire il backup in backup di Azure. Deselezionare la casella di controllo per ogni cartella del server che non si vuole includere nel backup. Per aggiungere cartelle che non sono riportate nell'elenco, fare clic su **Aggiungi cartelle**. Dopo aver completato la selezione, fare clic su **Avanti**.  
  
    > [!NOTE]
    >  Non è possibile selezionare una cartella non situata nel server locale o in una cartella che non si trova nell'unità formattata come ReFS.  
  
3.  Nella pagina **Aggiungi backup Cronologia file** della procedura guidata è possibile selezionare di includere i backup di Cronologia file per i computer di rete che supportano questa funzionalità. Quindi, fare clic su **Avanti** per continuare.  
  
4.  Nella pagina **Specificare la pianificazione del backup** della procedura guidata configurare quanto segue e fare clic su **Avanti** per continuare:  
  
    -   Selezionare o accettare l'ora del giorno in cui eseguire il backup online. L'ora predefinita è 22:00. È anche possibile specificare un'ora in cui eseguire un secondo backup.  
  
    -   Selezionare o accettare i giorni in cui eseguire il backup online. Per impostazione predefinita, il backup online viene eseguito dal lunedì al venerdì.  
  
5.  Nella pagina **Specificare i criteri di conservazione del backup** della procedura guidata selezionare il numero di giorni in cui mantenere i backup online, quindi fare clic su **Avanti**. L'impostazione predefinita è 7 giorni. È anche possibile scegliere di mantenere i backup online per 15 o 30 giorni.  
  
    > [!NOTE]
    >   Backup di Azure mantiene sempre il backup più recente. Se lo spazio disponibile nella destinazione di backup non è sufficiente per archiviare il backup, la procedura non riesce. Per evitare questa situazione, acquistare altro spazio di archiviazione oppure abbreviare il periodo di conservazione. Per informazioni sui prezzi, vedere [i dettagli sui prezzi](https://azure.microsoft.com/pricing/details/backup/) per backup di Microsoft Azure.  
  
6.  Nella pagina **Scegliere l'utilizzo della larghezza di banda** della procedura guidata, se si vuole limitare la larghezza di banda Internet allocata al backup online, selezionare **Abilita la limitazione all'utilizzo della larghezza di banda per le operazioni di backup**. Usare le opzioni di questa pagina per specificare quanta larghezza di banda Internet consentire per l'uso del backup online durante gli orari lavorativi e non lavorativi. Definire quindi gli orari e i giorni di ufficio.  
  
    > [!NOTE]
    >   Backup di Azure consente di personalizzare il modo in cui il software di integrazione usa la larghezza di banda di rete durante il backup o il ripristino delle informazioni. Utilizzando una tecnica comunemente nota come limitazione, è possibile controllare la quantità di larghezza di banda di rete disponibile per l'utilizzo da parte dei processi di backup e ripristino durante determinati intervalli di giorno e ora. Dopo aver selezionato la casella di controllo **Abilita la limitazione all'utilizzo della larghezza di banda per le operazioni di backup** nella Configurazione guidata backup online, è possibile specificare i giorni lavorativi e le ore lavorative durante cui applicare il limite della larghezza di banda delle ore lavorative. Per tutti gli altri orari, viene usato il limite delle ore non lavorative. Gli intervalli validi della larghezza di banda variano da 256 Kbps a Senza limiti per entrambi i limiti.  
  
7.  Al termine della configurazione del backup online, fare clic su **Chiudi**.  
  
    > [!TIP]
    >  Dopo la configurazione del backup, nella pagina **Backup online** viene visualizzato lo stato del backup online più recente e la quantità di spazio di archiviazione usata dall'insieme di credenziali per il backup. Per visualizzare lo stato dei backup precedenti, fare clic su **Cronologia backup**.  
  
###  <a name="start-an-online-backup"></a><a name="BKMK_3"></a>Avviare un backup online  
  
> [!NOTE]
>  Prima di avviare un backup online, è necessario [Registrare il server per il backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5) e quindi [Configurare il backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
##### <a name="to-start-an-online-backup-immediately"></a>Per avviare immediatamente un backup online  
  
1.  Accedere al dashboard come amministratore.  
  
2.  Sulla barra di spostamento fare clic su **BACKUP ONLINE**.  
  
3.  Nel riquadro **Attività di backup online** fare clic su **Avvia backup adesso**.  
  
###  <a name="restore-files-and-folders-from-an-online-backup"></a><a name="BKMK_4"></a>Ripristinare file e cartelle da un backup online  
 La procedura guidata Ripristino file e cartelle illustra il processo di individuazione, selezione e ripristino di file e cartelle di cui è stato eseguito il backup online. Avanzando nella procedura guidata, verranno eseguite le attività seguenti:  
  
1.  **Scegliere un backup online da cui ripristinare file e cartelle**  
  
     Quando si esegue la procedura guidata Ripristino file e cartelle, per prima cosa è necessario specificare se si vogliono ripristinare i file da un backup online del server a cui si è connessi o da uno diverso.  
  
2.  **Scegliere un server da cui ripristinare file e cartelle**  
  
     Se si sceglie di ripristinare file e cartelle da un server diverso, selezionare uno dei server disponibili nell'elenco e quindi fare clic su **Avanti**.  
  
3.  **Scegliere un volume contenente i file e le cartelle da ripristinare**  
  
     I backup online contengono i volumi che corrispondono ai volumi del server dell'origine di backup. Dopo aver selezionato il volume, selezionare la data e l'ora del backup da cui eseguire il ripristino e quindi fare clic su **Avanti**.  
  
4.  **Selezionare i file e le cartelle da ripristinare**  
  
     Nella pagina **Selezionare gli elementi da ripristinare** è possibile aprire i vari percorsi delle cartelle e selezionare la casella di controllo corrispondente a ogni file o cartella da ripristinare. Dopo aver selezionato i file, fare clic su **Avanti**.  
  
5.  **Specificare il percorso di destinazione per i file e le cartelle ripristinati**  
  
     La pagina **Specificare le opzioni di ripristino** della procedura guidata consente di specificare dove ripristinare i file e le cartelle. Sono disponibili due opzioni:  
  
    -   **Ripristina nel percorso originale**. Selezionare questa opzione per ripristinare i file e le cartelle nel percorso esatto da cui ha avuto origine il backup.  
  
    -   **Ripristina in un percorso diverso**.  Scegliere questa opzione per specificare un percorso diverso nel server in cui ripristinare i file.  
  
         Nell'installazione predefinita, se nel percorso di destinazione esiste già un file con lo stesso nome del file di ripristino, la procedura guidata crea una copia del file originale. Inoltre, l'elenco di controllo di accesso viene aggiornato in base alle attuali autorizzazioni per i file. È possibile fare clic su **Avanzate** per personalizzare le impostazioni predefinite.  
  
6.  **Confermare le informazioni di ripristino**  
  
     La pagina **Confermare le informazioni per il ripristino** contiene un riepilogo delle istruzioni specificate per il ripristino. Per procedere con il ripristino dei file, è necessario digitare la passphrase corretta per l'account di backup online.  
  
###  <a name="register-this-server-for-backup"></a><a name="BKMK_5"></a>Registra questo server per il backup  
 Per eseguire il backup o il ripristino di file, cartelle e cronologia file nel server di Windows Server Essentials in backup di Azure, è prima necessario registrare il server con il servizio Backup di Microsoft Azure.  
  
> [!NOTE]
>  Prima di registrare il server, è necessario caricare un certificato da usare con l'insieme di credenziali per il backup in cui verranno archiviati i backup online. Per altre informazioni, vedere [Caricare un certificato nell'insieme di credenziali di Backup di Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1).  
  
##### <a name="to-register-your-server-with-azure-backup"></a>Per registrare il server con Backup di Azure  
  
1.  Accedere al server come amministratore e aprire il dashboard.  
  
2.  Sulla barra di spostamento fare clic su **BACKUP ONLINE**.  
  
3.  Nella sottosezione **Backup online** fare clic su **Registra**.  
  
4.  Scegliere il certificato da usare con l'insieme di credenziali per i backup online. Per impostazione predefinita, viene selezionato il certificato predefinito. Per scegliere un certificato diverso, usare **Sfoglia**. Fare quindi clic su **Avanti**.  
  
5.  Seguire le istruzioni della procedura guidata per creare una passphrase e quindi completare la registrazione.  
  
###  <a name="unregister-server"></a><a name="BKMK_6"></a>Annulla registrazione server  
  
> [!CAUTION]
>  Se si annulla la registrazione del server di Windows Server Essentials dal servizio Backup di Microsoft Azure, backup di Azure non può più eseguire il backup del server. Inoltre, i dati del server caricati in precedenza verranno cancellati. Per riprendere i backup online, è necessario registrare di nuovo il server.  
  
##### <a name="to-unregister-your-server-with-azure-backup"></a>Per annullare la registrazione del server con Backup di Azure  
  
1.  Accedere al [portale di gestione di Azure](https://manage.windowsazure.com).  
  
2.  Fare clic su **Servizi di ripristino**.  
  
3.  In **Servizi di ripristino** fare clic sull'insieme di credenziali per il backup.  
  
4.  Nella pagina **Avvio rapido** relativa all'insieme di credenziali fare clic su **Server**.  
  
5.  Selezionare il server, fare clic su **Elimina** e quindi su **Sì** per la richiesta di conferma.  
  
## <a name="related-tasks"></a>Attività correlate  
  
-   [Modificare i criteri di backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Visualizza l'utilizzo dell'archiviazione dei backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Includi una nuova cartella nel backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Rimuovere o escludere i backup della cronologia file dai criteri di backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Disabilitare o riabilitare il backup del server online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Arresto di un backup del server online in corso](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [Visualizza lo stato del backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [Visualizzare e gestire gli avvisi di backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [Ripristinare le impostazioni predefinite del backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_15)  
  
-   [Iscriversi al servizio backup di Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16)  
  
-   [Integrare backup di Azure con Windows Server Essentials](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_17)  
  
-   [Proteggere le cartelle per il backup online in Windows Server Essentials](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_18)  
  
-   [Cronologia dei backup online in Windows Server Essentials](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_19)  
  
###  <a name="change-the-online-backup-policy"></a><a name="BKMK_7"></a>Modificare i criteri di backup online  
 Il dashboard di Windows Server Essentials consente di cambiare facilmente i criteri di backup online.  
  
##### <a name="to-change-the-online-backup-policy"></a>Per cambiare i criteri di backup online  
  
1. Accedere al dashboard come amministratore.  
  
2. Sulla barra di spostamento fare clic su **Backup online**.  
  
3. Nel riquadro **Attività di backup online** fare clic su **Configura backup online**.  
  
4. Seguire le istruzioni della procedura guidata per personalizzare i criteri di backup online.  
  
   Per informazioni più dettagliate sulle impostazioni che è possibile personalizzare, vedere [Configurare il backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
###  <a name="view-online-backup-storage-usage"></a><a name="BKMK_8"></a>Visualizza l'utilizzo dell'archiviazione dei backup online  
  
##### <a name="to-view-the-amount-of-storage-space-that-online-backup-uses"></a>Per visualizzare la quantità di spazio di archiviazione usata dai backup online  
  
1.  Accedere al dashboard come amministratore.  
  
2.  Sulla barra di spostamento fare clic su **BACKUP ONLINE**.  
  
3.  Nel riquadro di informazioni della scheda **Backup online** viene visualizzata l'opzione **Stato archiviazione**.  
  
###  <a name="include-a-new-folder-in-online-backup"></a><a name="BKMK_9"></a>Includi una nuova cartella nel backup online  
  
##### <a name="to-include-a-new-folder-in-the-online-backup-policy"></a>Per includere una nuova cartella nei criteri di backup online  
  
1. Accedere al dashboard come amministratore.  
  
2. Sulla barra di spostamento fare clic su **Backup online**.  
  
3. Nel riquadro **Attività di backup online** fare clic su **Configura backup online**.  
  
4. Nella pagina **Cambia o rimuovi i criteri di backup** fare clic su **Personalizza i criteri di backup online**.  
  
5. Nella pagina **Configura backup online**, se la cartella da includere non è riportata nell'elenco, fare clic su **Aggiungi cartelle**.  
  
6. Nella finestra **Aggiungi cartelle** individuare e selezionare la cartella da includere nel backup online e quindi fare clic su **OK**.  
  
7. Nella pagina **Configura backup online** selezionare altre cartella da includere, quindi fare clic su **Avanti**.  
  
8. Seguire le istruzioni della procedura guidata per completare la personalizzazione dei criteri di backup online.  
  
   Per informazioni più dettagliate sulle altre informazioni che è possibile personalizzare, vedere [Configurare il backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
###  <a name="remove-or-exclude-file-history-backups-from-the-online-backup-policy"></a><a name="BKMK_10"></a>Rimuovere o escludere i backup della cronologia file dai criteri di backup online  
  
##### <a name="to-remove-or-exclude-a-folder-from-the-online-backup-policy"></a>Per rimuovere o escludere una cartella dai criteri di backup online  
  
1.  Accedere al dashboard come amministratore.  
  
2.  Sulla barra di spostamento fare clic su **Backup online**.  
  
3.  Fare clic sulla scheda **cartelle protette** . Il riquadro dei dettagli Visualizza un elenco di cartelle con il relativo stato di backup online.  
  
4.  Selezionare la cartelle da escludere dai criteri di backup online e quindi, nel riquadro attività, fare clic su **Rimuovi la cartella dal backup online**.  
  
###  <a name="disable-or-re-enable-online-server-backup"></a><a name="BKMK_11"></a>Disabilitare o riabilitare il backup del server online  
 Per istruzioni su come usare backup di Azure per eseguire il backup o il ripristino dei dati del server, vedere [registrare il server per il backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5).  
  
 Per istruzioni su come interrompere l'uso di backup di Azure per eseguire il backup o il ripristino dei dati del server, vedere [annullare la registrazione del server](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_6).  
  
###  <a name="stop-an-online-server-backup-in-progress"></a><a name="BKMK_12"></a>Arresto di un backup del server online in corso  
  
##### <a name="to-stop-an-online-server-backup-in-progress"></a>Per interrompere un backup del server online in corso  
  
1.  Accedere al dashboard come amministratore.  
  
2.  Sulla barra di spostamento fare clic su **BACKUP ONLINE**.  
  
3.  Nella pagina **Attività di backup online** fare clic su **Interrompi il backup**.  
  
     Dopo aver interrotto il backup, nell'elenco **Cronologia backup** viene visualizzato lo stato **Annullato**.  
  
###  <a name="view-online-backup-status"></a><a name="BKMK_13"></a>Visualizza lo stato del backup online  
  
##### <a name="to-view-the-backup-status"></a>Per visualizzare lo stato del backup  
  
1.  Accedere al dashboard come amministratore.  
  
2.  Sulla barra di spostamento fare clic su **BACKUP ONLINE**.  
  
3.  Fare clic sulla scheda **cronologia backup** . Nella visualizzazione elenco viene visualizzato lo stato di ogni processo di backup. Selezionare un processo di backup per visualizzare altri dettagli.  
  
###  <a name="view-and-manage-online-backup-alerts"></a><a name="BKMK_14"></a>Visualizzare e gestire gli avvisi di backup online  
 Come molti altri avvisi, gli avvisi per backup di Azure vengono visualizzati nel Visualizzatore avvisi.  
  
##### <a name="to-view-online-backup-alerts-in-the-alert-viewer"></a>Per esaminare gli avvisi del backup online nel Visualizzatore avvisi  
  
1. Aprire il Dashboard.  
  
2. Esegui una delle operazioni seguenti:  
  
     Windows Server Essentials: nel riquadro di spostamento fare clic sull'icona Avvisi \(può essere di importanza critica, di avviso o informativa\). Verrà aperto il Visualizzatore avvisi.  
  
     Windows Server Essentials: nella **Home** page fare clic sulla scheda **monitoraggio dello stato** .  
  
3. Esaminare l'elenco di avvisi per i problemi relativi a backup di Azure.  
  
   Per ulteriori informazioni sull'utilizzo del Visualizzatore avvisi o della scheda monitoraggio stato per la gestione degli avvisi, vedere [Manage System Health](Manage-System-Health-in-Windows-Server-Essentials.md).  
  
###  <a name="reset-online-backup-to-default-settings"></a><a name="BKMK_15"></a>Ripristinare le impostazioni predefinite del backup online  
 Windows Server Essentials include una procedura guidata che consente di configurare le impostazioni del backup online. Per ripristinare le impostazioni predefinite, eseguire l'attività **Configura backup online** e scegliere l'opzione **Rimuovi i criteri di backup online**. Quindi, eseguire di nuovo l'attività **Configura backup online**. I dati caricati in precedenza rimangono invariati.  
  
###  <a name="sign-up-for-azure-backup-service"></a><a name="BKMK_16"></a>Iscriversi al servizio backup di Azure  
 Per preparare l'integrazione di Backup di Microsoft Azure con Windows Server Essentials, accedere al portale di gestione di Azure con l'account Microsoft Online Services, quindi creare un insieme di credenziali per il backup in cui archiviare i backup online in Azure. Sarà quindi possibile scaricare il modulo di integrazione di backup di Azure e usare il file scaricato per installare il componente aggiuntivo backup di Azure nel server Windows Server Essentials. Se non si ha un account Microsoft, è possibile iscriversi per una valutazione gratuita.  
  
 Per eseguire questa configurazione, completare le attività seguenti:  
  
1.  Iscriversi per ottenere un account Microsoft Online Services e l'anteprima di backup.  
  
2.  Creare un insieme di credenziali per il backup in cui archiviare i backup online.  
  
3.  Scaricare l'agente di backup di Azure.  
  
4.  Installare il componente aggiuntivo backup di Azure nel server.  
  
####  <a name="sign-up-for-a-microsoft-online-services-account-and-the-backup-preview"></a><a name="BKMK_SignupforaMicrosoftOnlineServiceAccount"></a>Iscriversi per un account di Microsoft Online Services e l'anteprima di backup  
  
1.  Accedere al dashboard di Windows Server Essentials.  
  
2.  Nella pagina **Home** del dashboard fare clic sulla categoria **COMPONENTI AGGIUNTIVI**, fare clic su **Integrazione con Backup di Azure** e quindi scegliere **Fare clic per iscriversi a Backup di Azure**.  
  
3.  Nella pagina **servizi di ripristino** di Azure, nella sezione **backup (anteprima)** , esaminare i dettagli.  
  
4.  Se non si ha una sottoscrizione di Azure, fare clic su **versione di valutazione gratuita**e quindi seguire le istruzioni per ottenere una sottoscrizione di Azure.  
  
     Se si ha già una sottoscrizione di Azure, fare clic su **portale** nell'angolo superiore destro della pagina Web per passare alla portale di gestione di Azure.  
  
5.  Nella pagina portale di gestione di Azure verranno visualizzati i **servizi di ripristino** nel riquadro sinistro. Qui sarà possibile gestire gli insiemi di credenziali per il backup in cui archiviare i backup online da Windows Server Essentials.  
  
####  <a name="create-a-backup-vault-to-store-online-backups"></a><a name="BKMK_Createabackupvaulttostoreonlinebackups"></a>Creare un insieme di credenziali per il backup in cui archiviare i backup online  
  
1.  Accedere al [portale di gestione di Azure](https://manage.windowsazure.com)dal Web browser nel server Windows Server Essentials.  
  
2.  Nel portale di gestione di Azure fare clic **su nuovo**, su **servizi dati**, su **servizi di ripristino**, su insieme di credenziali per il **backup**e quindi su **creazione rapida**.  
  
3.  Immettere un nome per l'insieme di credenziali per il backup, scegliere l'area in cui archiviare i backup e quindi fare clic su **Creazione rapida**.  
  
     L'area **Servizi di ripristino** si aprirà con il nuovo insieme di credenziali per il backup visualizzato.  
  
####  <a name="download-the-azure-backup-agent"></a><a name="BKMK_DownloadtheWindowsAzureBackupAgent"></a>Scaricare l'agente di backup di Azure  
  
1.  Aprire il dashboard di Windows Server Essentials.  
  
2.  Nella pagina **Home** del dashboard fare clic sulla scheda **Attività iniziali**, sulla categoria **COMPONENTI AGGIUNTIVI**, su **Integrazione con Backup di Azure** e quindi su **Fare clic per scaricare il modulo di integrazione con Backup di Azure**.  
  
####  <a name="install-the-azure-backup-add-in-on-the-server"></a><a name="BKMK_InstalltheWindowsAzureBackupAddIn"></a>Installare il componente aggiuntivo backup di Azure nel server  
  
1. Accedere al server con un account amministratore, quindi eseguire il file **OnlineBackupAddin.wssx** scaricato nel passaggio precedente.  
  
2. Verranno visualizzate le **Condizioni di licenza software**. Se si accettano i termini e le condizioni, fare clic su **Accetto** per continuare l'installazione.  
  
3. Nella pagina **Installa componente aggiuntivo** fare clic su **Installa componente aggiuntivo**.  
  
   > [!NOTE]
   >  Può essere necessario riavviare il server per installare il software prerequisito.  
  
    Verrà visualizzata la pagina **Installazione**. All'avvio dell'installazione viene visualizzato un indicatore di avanzamento che mostra lo stato dell'installazione. Al termine dell'installazione, verrà visualizzato un messaggio che indica che il componente aggiuntivo di backup di Azure è stato installato correttamente.  
  
4. Fare clic su **Fine**.  
  
5. Chiudere e riaprire il dashboard.  
  
    Nel dashboard è stata aggiunta una nuova scheda, **Backup online**. Da questa scheda è possibile registrare il server, configurare le impostazioni di backup e aprire **servizi di ripristino** nel portale di gestione di Azure per gestire gli insiemi di credenziali per il backup per i server.  
  
   Al termine, eseguire le operazioni seguenti:  
  
6. In Windows Server Essentials, caricare un certificato da usare per i backup online. Per altre informazioni, vedere [Caricare un certificato nell'insieme di credenziali di Backup di Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1).  
  
7. Registrare il server con l'insieme di credenziali di backup di Azure. Per altre informazioni, vedere [Registrare il server corrente per il backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5).  
  
8. Configurare il backup online del server. Per altre informazioni, vedere [Configurare il backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
###  <a name="integrate-azure-backup-with-windows-server-essentials"></a><a name="BKMK_17"></a>Integrare backup di Azure con Windows Server Essentials  
 Il modulo di integrazione Backup di Microsoft Azure è una nuova funzionalità di Windows Server Essentials che consente di crittografare ed eseguire il backup di file e cartelle dal server a un sistema di archiviazione ospitato da Azure fornito da Microsoft. Con backup di Azure per crittografare ed eseguire il backup dei dati nel server, è possibile prevenire la perdita irreversibile di dati aziendali critici a causa di incendi, inondazioni, furti o altre calamità. Quando si usa backup di Azure per eseguire il backup dei dati del server, le informazioni vengono crittografate usando la passphrase prima di essere caricate in un data center sicuro su Internet. Per accedere ai dati da un backup online, è necessario avere un server autenticato da un certificato e specificare la passphrase.  
  
 Dopo l'integrazione e la registrazione del server con backup di Azure, è possibile configurare le impostazioni di backup online per eseguire regolarmente i backup pianificati. È anche possibile avviare un backup online in qualsiasi momento facendo clic sull'attività **Avvia backup adesso** nel dashboard Backup online.  
  
 Per configurare il backup online, è necessario completare questi passaggi:  
  
1.  [Iscriversi al servizio backup di Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16) : dopo aver effettuato l'iscrizione al servizio backup, si creerà un insieme di credenziali per il backup, si scaricherà e installerà il modulo di integrazione di backup di Azure.  
  
2.  [Caricare un certificato nell'insieme di credenziali di backup di Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
3.  [Registra questo server per il backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
4.  [Configurare il backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
> [!NOTE]
>   Backup di Azure usa la passphrase per crittografare i file e le cartelle per il backup online. Se si modifica la passphrase di crittografia, verrà sostituita anche la passphrase specificata durante la registrazione del server. La passphrase accetta solo caratteri con codifica ASCII.  
  
###  <a name="protect-folders-for-online-backup-in-windows-server-essentials"></a><a name="BKMK_18"></a>Proteggere le cartelle per il backup online in Windows Server Essentials  
 La sottosezione **Cartelle protette** della sezione Backup online del dashboard visualizza un elenco di tutte le cartelle condivise presenti nel server. La tabella seguente descrive le informazioni incluse nell'elenco.  
  
|Colonna|Descrizione|  
|------------|-----------------|  
|**Nome cartella:**|nome della cartella inclusa nel backup online.<br /><br /> Per aggiungere o escludere una cartella, eseguire l'attività **Configura backup online**.|  
|**Percorso cartella:**|percorso della cartella.|  
|**Stato:**|Sono disponibili tre tipi di stato: **protetto**, **non protetto**e **sconosciuto**.|  
  
###  <a name="online-backup-history-in-windows-server-essentials"></a><a name="BKMK_19"></a>Cronologia dei backup online in Windows Server Essentials  
 La sottosezione **Cronologia backup** della sezione Backup online del dashboard visualizza un elenco dei recenti backup online. È possibile usare i backup corretti per ripristinare file e cartelle. La tabella seguente descrive le informazioni incluse nell'elenco.  
  
|Colonna|Descrizione|  
|------------|-----------------|  
|**Operazione**|le operazioni possono essere di due tipi, **Backup** e **Ripristino**.|  
|**Tempo**|l'ora registrata per lo stato più recente.|  
|**Stato:**|Sono disponibili cinque tipi di stato: **esito positivo**, **in corso**, **annullato**, **avviso**e non **riuscito**.|  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire il backup e il ripristino](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gestire i Microsoft Online Services](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usare Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
