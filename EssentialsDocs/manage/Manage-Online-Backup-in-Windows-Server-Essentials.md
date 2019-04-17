---
title: Gestire il Backup Online in Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95a9f593-fad7-4335-bd4d-c7bb8c033efb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 37d59d500a2de1e2b98c848e7484ae13639d09b7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="manage-online-backup-in-windows-server-essentials"></a>Gestire il Backup Online in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Dopo aver integrato con Microsoft Azure Backup, il **Backup in linea** nel Dashboard di Windows Server Essentials viene visualizzata la pagina di gestione. Il **Backup in linea** pagina consente di eseguire comuni attività amministrative. Le funzionalità nella pagina di Backup in linea includono:  
  
-   Le pagine di sottosezioni seguenti:  
  
    -   **Backup in linea** dopo aver registrato il server per il backup online, in questa sezione Visualizza stato corrente del backup, lo stato dell'archiviazione e informazioni sull'account.  
  
    -   **Cartelle protette** questa sezione sono elencate tutte le cartelle condivise e tutte le cartelle Cronologia File nel server e qualsiasi altra cartella che si è scelto di eseguire il backup in Azure. L'elenco include il nome della cartella, percorso della cartella e stato per ogni cartella condivisa.  
  
    -   **Cronologia backup** in questa sezione Visualizza un elenco dei recenti backup online. L'elenco include il tipo di operazione, l'ora e lo stato per ogni backup.  
  
-   Un riquadro dei dettagli con altre informazioni su un processo di backup selezionato o ripristino.  
  
-   Un riquadro attività che include una serie di attività amministrative che è possibile eseguire.  
  
 Come procedura consigliata, è necessario eseguire il backup dei dati aziendali e utente più importanti. Ad esempio, eseguire il backup le cartelle del server che contengono file di dati critici. È inoltre consigliabile eseguire il backup della cronologia File per i computer di rete che contengono informazioni critiche.  
  
 Quando si scelgono i dati di backup online, valutare i criteri di frequenza e la conservazione dei backup che si desidera implementare. Quindi esaminare il budget e determinare la quantità di spazio di archiviazione che è possibile acquisire. Valutare i costi e il volume di archiviazione rispetto alle esigenze e quindi configurare il backup online per proteggere la maggior parte dei dati importanti possibile. Per informazioni sui prezzi, vedere [dettagli dei prezzi per Azure Backup](https://azure.microsoft.com/pricing/details/backup/).  
  
 Le sezioni seguenti descrivono varie attività di backup online che possono essere visualizzate nel Dashboard di Windows Server Essentials.  
  
## <a name="online-backup-tasks-in-the-dashboard"></a>Attività di backup online nel Dashboard di  
  
-   [Caricare un certificato per l'insieme di credenziali di Backup di Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Configurare il backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Avviare un backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Ripristinare file e cartelle da un backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Registrare il server per il backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Annullare la registrazione server](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
###  <a name="BKMK_1"></a>Caricare un certificato per l'insieme di credenziali di Backup di Azure  
 Prima di poter utilizzare Azure Backup per i backup online in Windows Server Essentials, è necessario caricare un certificato pubblico da registrare nell'insieme di credenziali di backup. Il certificato viene utilizzato per autenticare la distribuzione di Backup di Azure (agente) che agisce per conto di proprietario della sottoscrizione dei Microsoft Online Services per gestire le risorse associate alla sottoscrizione.  
  
> [!NOTE]
>  Prima di caricare il certificato, è necessario completare le procedure descritte in [iscriversi servizio Backup di Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16).  
  
##### <a name="to-upload-a-certificate-to-use-with-the-azure-backup-service"></a>Per caricare un certificato da utilizzare con il servizio di Backup di Azure  
  
1.  Accedere al Dashboard di Windows Server Essentials con un account amministratore.  
  
2.  Nel Dashboard **Home** pagina, fare clic su **BACKUP in linea**.  
  
3.  Nel **BACKUP in linea** area, fare clic su **certificato caricamento insieme di credenziali di Backup di Azure**.  
  
     Che viene aperto **servizi di ripristino** nel portale di gestione di Azure. Se t già eseguito l'accesso di Azure, è necessario accedere con l'account Microsoft.  
  
4.  Fare clic sul nome dell'archivio di backup che userai per i backup online per aprire il **avvio rapido** pagina per l'insieme di credenziali di backup.  
  
5.  Fare clic su **gestire certificato**.  
  
6.  Nel **Gestisci certificato** la finestra di dialogo, incollare il percorso del certificato pubblico generato da Windows Server Essentials. Per ottenere il percorso del certificato pubblico, procedere come segue:  
  
    1.  Nel Dashboard di Windows Server Essentials, fare clic su di **Backup in linea** scheda.  
  
    2.  Nel **Backup in linea** pagina, copiare il percorso del certificato generato.  
  
    3.  Passare al portale di gestione di Azure, quindi il **Gestisci certificato** finestra di dialogo incollare il percorso in cui caricare il certificato pubblico generato.  
  
    > [!NOTE]
    >  È inoltre possibile utilizzare il proprio certificato pubblico. Per sapere quale certificato è obbligatorio, scegliere il **avvio rapido** pagina, fare clic sul **Acquisisci certificato** collegamento.  
  
    > [!NOTE]
    >   Azure richiede un certificato di x. 509 v2 con una chiave pubblica. Per ulteriori informazioni, vedere [gestire i certificati di insieme di credenziali](https://msdn.microsoft.com/library/azure/dn169036.aspx).  
  
7.  Dopo aver selezionato il certificato, fare clic su **OK** (segno di spunta).  
  
8.  Vengono restituiti il **avvio rapido** pagina. Fare clic su **Dashboard**e verificare che il certificato è stato caricato correttamente. Dopo il certificato è stato caricato correttamente, il dashboard Visualizza la data di scadenza e l'identificazione personale del certificato.  
  
 Dopo aver completato questa procedura, eseguire le operazioni seguenti:  
  
1.  Registrare il server con il servizio di Backup di Azure. Per ulteriori informazioni, vedere [registrare il server per il backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5).  
  
2.  Configurare il backup online del server. Per ulteriori informazioni, vedere [Configura backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
###  <a name="BKMK_2"></a>Configurare il backup online  
 Dopo aver registrato il server con Backup di Azure, è possibile configurare le impostazioni di backup online in Windows Server Essentials.  
  
##### <a name="to-configure-online-backup"></a>Per configurare il backup online  
  
1.  Dalla registrazione guidata Server o il **Online Backup** pagina del Dashboard di Windows Server Essentials, fare clic su **Configura backup online**. Verrà visualizzata la configurazione guidata Backup Online.  
  
2.  Nel **Configura Backup Online** pagina della procedura guidata, selezionare la casella di controllo per ogni cartella del server che si desidera eseguire il backup in Azure Backup. Deselezionare la casella di controllo per ogni cartella del server che non si desidera includere nel backup. Per aggiungere cartelle che non vengono visualizzate nell'elenco, fare clic su **Aggiungi cartelle**. Dopo aver completato la selezione, fare clic su **Avanti**.  
  
    > [!NOTE]
    >  È possibile selezionare una cartella in cui non si trova nel server locale o in una cartella che si trova su un'unità formattata come ReFS.  
  
3.  Nel **aggiungere backup cronologia File** pagina della procedura guidata, è possibile scegliere di includere i backup di cronologia File per i computer di rete che supportano la funzionalità cronologia File. Fare clic su **Avanti** per continuare.  
  
4.  Nel **specificare la pianificazione di Backup** pagina della procedura guidata, configurare le opzioni seguenti e quindi fare clic su **Avanti** per continuare:  
  
    -   Selezionare o accettare l'ora del giorno in cui eseguire il backup online. Il periodo predefinito è 10:00 PM. È anche possibile specificare un tempo necessario per eseguire un secondo backup.  
  
    -   Selezionare o accettare i giorni in cui eseguire il backup online. Per impostazione predefinita, i backup online viene eseguito dal lunedì al venerdì.  
  
5.  Nel **specificare i criteri di conservazione dei Backup** pagina della procedura guidata, selezionare il numero di giorni per cui si desidera mantenere i backup online, quindi fare clic su **Avanti**. Il valore predefinito è 7 giorni. È anche possibile scegliere di mantenere i backup online per 15 o 30 giorni.  
  
    > [!NOTE]
    >   Backup di Azure mantiene sempre i backup più recente. Se la destinazione di backup non dispone di spazio sufficiente archiviare il backup, il processo di backup avrà esito negativo. Per evitare questa situazione, acquistare altro spazio di archiviazione oppure abbreviare il periodo di conservazione dei dati. Per informazioni sui prezzi, vedere [dettagli dei prezzi](https://azure.microsoft.com/pricing/details/backup/) per Microsoft Azure Backup.  
  
6.  Nel **scegliere il consumo di larghezza di banda** pagina della procedura guidata, se si desidera limitare la quantità di larghezza di banda Internet allocata al backup in linea, selezionare **l'utilizzo della larghezza di banda Internet consentire**. Utilizzare le opzioni nella pagina per specificare quanta larghezza di banda Internet per consentire il backup online da utilizzare durante il processo di ore e ore non lavorative. Definire quindi gli orari e i giorni lavorativi.  
  
    > [!NOTE]
    >   Backup di Azure consente di personalizzare come il software di integrazione utilizza la larghezza di banda di rete quando si esegue il backup o il ripristino delle informazioni. Usa una tecnica comune nota come limitazione, è possibile controllare la quantità di larghezza di banda di rete è disponibile per il backup e i processi di ripristino durante il giorno specifico e intervalli di tempo. Dopo aver selezionato il **l'utilizzo della larghezza di banda Internet consentire** casella di controllo della procedura guidata Configura Backup Online, è possibile specificare i giorni lavorativi e le ore lavorative durante cui applicare il limite di larghezza di banda delle ore di lavoro. Ore di lavoro non viene usato il limite affatto in altri casi. Gli intervalli validi della larghezza di banda variano da 256 Kbps a senza per entrambi i limiti.  
  
7.  Al termine della configurazione del backup online, fare clic su **Chiudi**.  
  
    > [!TIP]
    >  Dopo la configurazione del backup, il **Online Backup** pagina Mostra lo stato del backup online più recente e la quantità di spazio di archiviazione utilizzato per l'insieme di credenziali di backup. Per visualizzare lo stato di backup precedente, fare clic su **cronologia Backup**.  
  
###  <a name="BKMK_3"></a>Avviare un backup online  
  
> [!NOTE]
>  Prima di avviare un backup in linea, è necessario [registrare il server per il backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)e quindi [Configura backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
##### <a name="to-start-an-online-backup-immediately"></a>Per avviare immediatamente un backup in linea  
  
1.  Accedere al Dashboard come amministratore.  
  
2.  Sulla barra di spostamento, fare clic su **BACKUP in linea**.  
  
3.  Nel **attività di Backup Online** riquadro, fare clic su **Avvia backup adesso**.  
  
###  <a name="BKMK_4"></a>Ripristinare file e cartelle da un backup online  
 Il ripristino guidato file e cartelle in modo semplificato il processo di individuazione, selezione e ripristino di file e cartelle in cui vengono eseguito il backup in linea. Avanzando la procedura guidata, verranno eseguite le attività seguenti:  
  
1.  **Scegliere un backup online da cui ripristinare file e cartelle**  
  
     Quando si esegue il ripristino guidato file e cartelle, la prima cosa che è necessario eseguire è specificare se si desidera ripristinare i file da un backup online del server che è attualmente connessi o da un backup di un server diverso.  
  
2.  **Scegliere un server da cui ripristinare file e cartelle**  
  
     Se si sceglie di ripristinare file e cartelle da un server diverso, selezionare il server che si desidera ripristinare da nell'elenco dei server disponibili, quindi fare clic su **Avanti**.  
  
3.  **Scegliere un volume che contiene i file e cartelle da ripristinare**  
  
     I backup online contengono i volumi che corrispondono ai volumi del server di origine di backup. Dopo aver selezionato il volume, selezionare la data e ora del backup da cui si desidera ripristinare e quindi fare clic su **Avanti**.  
  
4.  **Selezionare i file e cartelle da ripristinare**  
  
     Nel **selezionare gli elementi da ripristinare** pagina della procedura guidata, è possibile aprire i vari percorsi delle cartelle e selezionare la casella di controllo per ogni file o cartella che si desidera ripristinare. Dopo aver selezionato i file, fare clic su **Avanti**.  
  
5.  **Specificare il percorso di destinazione per ripristinato i file e cartelle**  
  
     Il **specificare opzioni di ripristino** pagina della procedura guidata consente di specificare dove ripristinare file e cartelle. Sono disponibili due opzioni:  
  
    -   **Ripristina nel percorso originale**. Selezionare questa opzione per ripristinare file e cartelle nel percorso esatto da cui ha avuto origine il backup.  
  
    -   **Ripristino in un percorso diverso**.  Scegliere questa opzione per specificare un percorso diverso nel server in cui ripristinare i file.  
  
         Nell'installazione predefinita, se esiste già un file con lo stesso nome di un file di ripristino nel percorso di destinazione, la procedura guidata crea una copia del file originale. Inoltre, l'elenco di controllo di accesso (ACL) viene aggiornata per riflettere le autorizzazioni di file corrente. È possibile fare clic su **avanzate** per personalizzare le impostazioni predefinite.  
  
6.  **Verificare le informazioni di ripristino**  
  
     Il **confermare di informazioni per il ripristino** pagina fornisce un riepilogo delle istruzioni ripristino specificate. Per procedere con il ripristino di file, è necessario digitare la passphrase corretta per il tuo account di backup online.  
  
###  <a name="BKMK_5"></a>Registrare il server per il backup  
 Per eseguire il backup o il ripristino di file, cartelle e cronologia file del server di Windows Server Essentials per Azure Backup, è necessario registrare il server con il servizio di Backup di Microsoft Azure.  
  
> [!NOTE]
>  Prima di registrare il server, è necessario caricare un certificato da usare con l'insieme di credenziali di backup in cui verranno archiviati i backup online. Per ulteriori informazioni, vedere [caricare un certificato per l'insieme di credenziali di Backup di Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1).  
  
##### <a name="to-register-your-server-with-azure-backup"></a>Per registrare il server con Backup di Azure  
  
1.  Accedere al server come amministratore e aprire il Dashboard.  
  
2.  Sulla barra di spostamento, fare clic su **BACKUP in linea**.  
  
3.  Nel **Backup in linea** sottosezione, fare clic su **registrare**.  
  
4.  Scegliere il certificato che si desidera utilizzare con l'insieme di credenziali di backup per i backup online. Il certificato predefinito è selezionato per impostazione predefinita. Se si desidera scegliere un certificato diverso, utilizzare **Sfoglia**. Fare clic su **Avanti**.  
  
5.  Seguire le istruzioni della procedura guidata per creare una passphrase e quindi completare la registrazione.  
  
###  <a name="BKMK_6"></a>Annullare la registrazione server  
  
> [!CAUTION]
>  Se si annulla la registrazione del server di Windows Server Essentials dal servizio Microsoft Azure Backup, Backup di Azure non è più possibile il backup del server. Inoltre, i dati server caricati in precedenza verranno cancellati. Per riprendere i backup online, è necessario registrare nuovamente il server.  
  
##### <a name="to-unregister-your-server-with-azure-backup"></a>Per annullare la registrazione del server con Backup di Azure  
  
1.  Accedere al [portale di gestione di Azure](https://manage.windowsazure.com).  
  
2.  Fare clic su **servizi di ripristino**.  
  
3.  In **servizi di ripristino**, fare clic sul nome dell'archivio di backup.  
  
4.  Dal **avvio rapido** pagina per l'insieme di credenziali, fare clic su **server**.  
  
5.  Selezionare il server, fare clic su **eliminare**, quindi fare clic su **Sì** alla richiesta di conferma.  
  
## <a name="related-tasks"></a>Attività correlate  
  
-   [Modificare i criteri di backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Visualizzare informazioni sull'utilizzo di archiviazione dei backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Includere una nuova cartella nel backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Rimuovere o escludere i backup di cronologia file dai criteri di backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Disabilitare o riabilitare backup del server online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Interrompere un backup del server online in corso](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [Visualizza lo stato del backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [Visualizzare e gestire gli avvisi di backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [Ripristinare le impostazioni predefinite di backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_15)  
  
-   [Registrarsi per il servizio di Backup di Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16)  
  
-   [Integrare Backup di Azure con Windows Server Essentials](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_17)  
  
-   [Proteggere le cartelle per il backup online in Windows Server Essentials](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_18)  
  
-   [Cronologia dei backup online in Windows Server Essentials](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_19)  
  
###  <a name="BKMK_7"></a>Modificare i criteri di backup online  
 È facile apportare modifiche ai criteri di backup online tramite il Dashboard di Windows Server Essentials.  
  
##### <a name="to-change-the-online-backup-policy"></a>Per modificare i criteri di backup online  
  
1.  Accedere al Dashboard come amministratore.  
  
2.  Sulla barra di spostamento, fare clic su **Backup in linea**.  
  
3.  Nel **attività di Backup Online** riquadro, fare clic su **Configura backup online**.  
  
4.  Seguire le istruzioni della procedura guidata per personalizzare i criteri di backup online.  
  
 Per ulteriori informazioni sulle impostazioni che è possibile personalizzare, vedere [Configura backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
###  <a name="BKMK_8"></a>Visualizzare informazioni sull'utilizzo di archiviazione dei backup online  
  
##### <a name="to-view-the-amount-of-storage-space-that-online-backup-uses"></a>Per visualizzare la quantità di spazio di archiviazione che utilizza il Backup in linea  
  
1.  Accedere al Dashboard come amministratore.  
  
2.  Sulla barra di spostamento, fare clic su **BACKUP in linea**.  
  
3.  Nel **Backup in linea** scheda, il **lo stato dell'archiviazione** viene visualizzato nel riquadro informazioni.  
  
###  <a name="BKMK_9"></a>Includere una nuova cartella nel backup online  
  
##### <a name="to-include-a-new-folder-in-the-online-backup-policy"></a>Per includere una nuova cartella nei criteri di backup online  
  
1.  Accedere al Dashboard come amministratore.  
  
2.  Sulla barra di spostamento, fare clic su **Backup in linea**.  
  
3.  Nel **attività di Backup Online** riquadro, fare clic su **Configura backup online**.  
  
4.  Nel **modificare o rimuovere i criteri di Backup** pagina, fare clic su **personalizzare i criteri di Backup Online**.  
  
5.  Nel **Configura Backup Online** pagina, se la cartella da includere non viene visualizzata nell'elenco, fare clic su **aggiungere cartelle**.  
  
6.  Nel **aggiungere cartelle** finestra, individuare e selezionare la cartella che si desidera includere nel backup online, quindi fare clic su **OK**.  
  
7.  Nel **Configura Backup Online** selezionare altre cartelle da includere nel modo desiderato e quindi fare clic su **Avanti**.  
  
8.  Seguire le istruzioni della procedura guidata per completare la personalizzazione di criteri di backup online.  
  
 Per ulteriori informazioni sulle altre impostazioni che è possibile personalizzare, vedere [Configura backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
###  <a name="BKMK_10"></a>Rimuovere o escludere i backup di cronologia file dai criteri di backup online  
  
##### <a name="to-remove-or-exclude-a-folder-from-the-online-backup-policy"></a>Per rimuovere o escludere una cartella dai criteri di backup online  
  
1.  Accedere al Dashboard come amministratore.  
  
2.  Sulla barra di spostamento, fare clic su **Backup in linea**.  
  
3.  Fare clic su di **cartelle protette** scheda. Riquadro dei dettagli visualizza un elenco delle cartelle con il relativo stato di backup online.  
  
4.  Selezionare la cartella che si desidera escludere dai criteri di backup online e quindi nel riquadro attività fare clic su **rimuovere la cartella dal backup in linea**.  
  
###  <a name="BKMK_11"></a>Disabilitare o riabilitare backup del server online  
 Per istruzioni su come usare Azure Backup per eseguire il backup o ripristino dei dati del server, vedere [registrare il server per il backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5).  
  
 Per istruzioni su come interrompere l'uso di Azure Backup per eseguire il backup o ripristino dei dati del server, vedere [server Unregister](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_6).  
  
###  <a name="BKMK_12"></a>Interrompere un backup del server online in corso  
  
##### <a name="to-stop-an-online-server-backup-in-progress"></a>Per interrompere un backup del server online in corso  
  
1.  Accedere al Dashboard come amministratore.  
  
2.  Sulla barra di spostamento, fare clic su **BACKUP in linea**.  
  
3.  Nel **attività di Backup Online** riquadro, fare clic su **interrotto il backup**.  
  
     Dopo aver interrotto il backup, lo stato **Canceled** viene visualizzato per il backup nel **cronologia dei Backup** elenco.  
  
###  <a name="BKMK_13"></a>Visualizza lo stato del backup online  
  
##### <a name="to-view-the-backup-status"></a>Per visualizzare lo stato del backup  
  
1.  Accedere al Dashboard come amministratore.  
  
2.  Sulla barra di spostamento, fare clic su **BACKUP in linea**.  
  
3.  Fare clic su di **cronologia Backup** scheda. La visualizzazione elenco Mostra lo stato per ogni processo di backup. Selezionare un processo di backup per visualizzare ulteriori dettagli su tale processo.  
  
###  <a name="BKMK_14"></a>Visualizzare e gestire gli avvisi di backup online  
 Come molti altri avvisi nel Visualizzatore avvisi vengono visualizzati avvisi per Azure Backup.  
  
##### <a name="to-view-online-backup-alerts-in-the-alert-viewer"></a>Per visualizzare gli avvisi di backup online nel Visualizzatore avvisi  
  
1.  Aprire il Dashboard.  
  
2.  Eseguire una delle operazioni seguenti:  
  
      Windows Server Essentials: Nel riquadro di spostamento fare clic sull'icona avvisi \ (potrebbero essere Informational\, avviso o critico). Verrà aperto il Visualizzatore avvisi.  
  
      Windows Server Essentials: Sul **Home** pagina, fare clic su di **il monitoraggio dello stato** scheda.  
  
3.  Esaminare l'elenco degli avvisi per i problemi relativi al Backup di Azure.  
  
 Per ulteriori informazioni sull'uso del Visualizzatore avvisi o il monitoraggio dello stato della scheda per gestire gli avvisi, vedere [Manage System Health](Manage-System-Health-in-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_15"></a>Ripristinare le impostazioni predefinite di backup online  
 Windows Server Essentials fornisce una procedura guidata che consente di configurare le impostazioni per il backup online. Se si desidera ripristinare le impostazioni predefinite, eseguire il **Configura backup online** attività e scegliere il **rimuovere i criteri di backup online** opzione. Eseguire quindi il **Configura backup online** nuovamente l'attività. I dati caricati in precedenza rimangano invariati.  
  
###  <a name="BKMK_16"></a>Registrarsi per il servizio di Backup di Azure  
 Per preparare l'integrazione di Microsoft Azure Backup con Windows Server Essentials, potrai accedere al portale di gestione di Azure con il tuo account Microsoft Online Services e quindi creare un insieme di credenziali di backup per archiviare i backup online in Azure. È quindi dovrai scaricare il modulo di integrazione di Backup di Azure e utilizzare il file scaricato per installare il Backup di Azure Add-In nel server Windows Server Essentials. Se non hai un account Microsoft, è possibile iscriversi per una valutazione gratuita.  
  
 Per eseguire il programma di installazione, completare le attività seguenti:  
  
1.  Iscriversi per un account Microsoft Online Services e l'anteprima di Backup.  
  
2.  Creare un insieme di credenziali di backup per archiviare i backup online.  
  
3.  Scaricare Azure Backup Agent.  
  
4.  Installare il Backup di Azure Add-In nel server.  
  
####  <a name="BKMK_SignupforaMicrosoftOnlineServiceAccount"></a>Iscriversi a un account Microsoft Online Services e l'anteprima di Backup  
  
1.  Accedere al Dashboard di Windows Server Essentials.  
  
2.  Nel Dashboard **Home** pagina, fare clic sul **componenti aggiuntivi** categoria, fare clic su **integrazione con Backup di Azure**, quindi fare clic su **fare clic per iscriversi per Azure Backup**.  
  
3.  In Azure **servizi di ripristino** pagina il **Backup (anteprima)** sezione, esaminare i dettagli.  
  
4.  Se non hai una sottoscrizione di Azure, fare clic su **versione di valutazione gratuita**e quindi segui le istruzioni per ottenere una sottoscrizione di Azure.  
  
     Se hai già una sottoscrizione di Azure, fare clic su **portale** nell'angolo superiore destro della pagina web per passare al portale di gestione di Azure.  
  
5.  Nella pagina portale di gestione di Azure, vedrai **servizi di ripristino** nel riquadro a sinistra. Che s in cui si ll gestire il backup archivi tale archivio i backup online da Windows Server Essentials.  
  
####  <a name="BKMK_Createabackupvaulttostoreonlinebackups"></a>Creare un insieme di credenziali di backup per archiviare i backup online  
  
1.  Accedere al [portale di gestione di Azure](https://manage.windowsazure.com)dal web browser nel server Windows Server Essentials.  
  
2.  Nel portale di gestione di Azure, fare clic su **New**, fare clic su **servizi dati**, fare clic su **servizi di ripristino**, fare clic su **insieme di credenziali di Backup**, quindi fare clic su **creazione rapida**.  
  
3.  Immettere un nome per l'insieme di credenziali di backup, scegliere l'area in cui si desidera archiviare i backup, quindi fare clic su **creazione rapida**.  
  
     Il **servizi di ripristino** area si apre con il nuovo insieme di credenziali di backup visualizzato.  
  
####  <a name="BKMK_DownloadtheWindowsAzureBackupAgent"></a>Scaricare Azure Backup Agent  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Nel Dashboard **Home** pagina, fare clic su di **iniziare** scheda, fare clic sul **componenti aggiuntivi** categoria, fare clic su **integrazione con Backup di Azure**e quindi fare clic su **fare clic per scaricare il modulo di integrazione di Azure Backup**.  
  
####  <a name="BKMK_InstalltheWindowsAzureBackupAddIn"></a>Installare il Backup di Azure Add-In nel server di  
  
1.  Accedere al server utilizzando un account amministratore e quindi eseguire il **Onlinebackupaddin** file scaricato nel passaggio precedente.  
  
2.  Il **condizioni di licenza Software** vengono visualizzati. Se si accettano i termini e condizioni, fare clic su **accetta** per continuare l'installazione.  
  
3.  Nel **installare il componente aggiuntivo** pagina, fare clic su **installare il componente aggiuntivo**.  
  
    > [!NOTE]
    >  Potrebbe essere necessario riavviare il server per installare il software prerequisito.  
  
     Il **installazione** verrà visualizzata la pagina. Un indicatore di stato viene visualizzato quando l'installazione viene avviata e Mostra lo stato di avanzamento dell'installazione. Una volta completato l'installazione, si riceverà un messaggio che il Backup di Azure Add-in è stato installato correttamente.  
  
4.  Fare clic su **fine **.  
  
5.  Chiudere e riaprire il Dashboard.  
  
     Una nuova scheda, **Backup in linea**, viene aggiunto al Dashboard. In questa scheda è possibile registrare il server, configurare le impostazioni di backup e aprire **servizi di ripristino** nel portale di gestione di Azure per gestire gli insiemi di credenziali di backup per i server.  
  
 Dopo aver completato questi passaggi, eseguire le operazioni seguenti:  
  
1.  In Windows Server Essentials, caricare un certificato da usare per i backup online. Per ulteriori informazioni, vedere [caricare un certificato per l'insieme di credenziali di Backup di Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1).  
  
2.  Registrare il server con l'insieme di credenziali di Backup di Azure. Per ulteriori informazioni, vedere [registrare il server per il backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5).  
  
3.  Configurare il backup online del server. Per ulteriori informazioni, vedere [Configura backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2).  
  
###  <a name="BKMK_17"></a>Integrare Backup di Azure con Windows Server Essentials  
 Il modulo di integrazione di Microsoft Azure Backup è una nuova funzionalità di Windows Server Essentials che consente di crittografare ed eseguirne il backup di file e cartelle dal server a un sistema di archiviazione ospitati Azure fornito da Microsoft. Usando il Backup di Azure per crittografare ed eseguire il backup dei dati nel server, è possibile prevenire la perdita irreversibile di dati aziendali critici a causa di incendi, inondazioni, furti o altre calamità. Quando si utilizza Azure Backup per eseguire il backup dei dati del server, le informazioni vengono crittografate con una passphrase prima di essere caricate in un Data Center sicuro su Internet. Per accedere ai dati da un online backup, è necessario disporre di un server che viene autenticato da un certificato e devono fornire la passphrase.  
  
 Dopo aver integrato e registrato il server con Backup di Azure, è possibile configurare le impostazioni di backup online per eseguire regolarmente i backup pianificati. È anche possibile avviare un backup online in qualsiasi momento facendo il **Avvia backup adesso** attività nel Dashboard di Backup Online.  
  
 Configurare il backup online è necessario completare questi passaggi:  
  
1.  [Iscriversi servizio Backup di Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16) - dopo l'accesso per il servizio di Backup, creare un insieme di credenziali di backup, scarica e installa il modulo di integrazione di Azure Backup.  
  
2.  [Caricare un certificato per l'insieme di credenziali di Backup di Azure](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
3.  [Registrare il server per il backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
4.  [Configurare il backup online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
> [!NOTE]
>   Backup di Azure Usa la passphrase per crittografare file e cartelle per il backup online. Modifica la passphrase di crittografia sostituirà la passphrase specificata al momento della registrazione di server. La passphrase accetta solo caratteri con codifica ASCII.  
  
###  <a name="BKMK_18"></a>Proteggere le cartelle per il backup online in Windows Server Essentials  
 Il **cartelle protette** secondarie della sezione Backup Online del Dashboard visualizza un elenco di tutte le cartelle condivise sul server. Nella tabella seguente vengono descritte le informazioni che sono incluso nell'elenco.  
  
|Colonna|Descrizione|  
|------------|-----------------|  
|**Nome cartella:**|Il nome della cartella che è incluso nel backup online.<br /><br /> Per aggiungere o escludere una cartella, eseguire il **Configura backup online** attività.|  
|**Percorso di cartella:**|Il percorso della cartella.|  
|**Stato:**|Esistono tre tipi di stato **Protected**, **non protetti**, e **sconosciuto**.|  
  
###  <a name="BKMK_19"></a>Cronologia dei backup online in Windows Server Essentials  
 Il **cronologia Backup** secondarie della sezione Backup Online del Dashboard visualizza un elenco dei recenti backup online. È possibile utilizzare i backup corretti per ripristinare file e cartelle. Nella tabella seguente vengono descritte le informazioni che sono incluso nell'elenco.  
  
|Colonna|Descrizione|  
|------------|-----------------|  
|**Operazione:**|Esistono due tipi di operazioni - **Backup** e **ripristinare**.|  
|**Ora:**|Questo è il tempo registrato per lo stato più recente.|  
|**Stato:**|Sono disponibili cinque tipi di stato **successo**, **In corso**, **Canceled**, **avviso**, e **non riuscito**.|  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire Backup e ripristino](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gestire Microsoft Online Services](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usare Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
