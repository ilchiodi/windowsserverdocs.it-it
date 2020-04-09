---
title: Ripristinare un server che esegue Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 27bf6f24-30c4-4935-9b24-069eb43e22f4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 89220092746b29a2bd45a906c93aa85abe045319
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852634"
---
# <a name="restore-or-repair-your-server-running-windows-server-essentials"></a>Ripristinare un server che esegue Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 In questo argomento vengono fornite informazioni generali e procedure di supporto per il ripristino o il ripristino di un server che esegue Windows Server Essentials e sono incluse le sezioni seguenti:  
  
-   [Panoramica dei ripristini del sistema server](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Overview)  
  
-   [Ripristinare o ripristinare l'unità di sistema](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore)  
  
-   [Ripristino di file e cartelle nel server](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders)  
  
##  <a name="overview-of-server-system-restores"></a><a name="BKMK_Overview"></a>Panoramica dei ripristini del sistema server  
 Lo stato del server al momento del ripristino incide sul metodo di ripristino disponibile e sulla completezza del ripristino stesso.  
  
 I motivi più comuni per cui si esegue il ripristino di un server sono:  
  
- Sul server è presente un virus impossibile da inoculare o eliminare.  
  
- Le impostazioni di configurazione del server sono danneggiate e non è possibile avviare il server.  
  
- È stata sostituita l'unità di sistema.  
  
- Il server viene disattivato e si vuole eseguirne il ripristino in un nuovo server.  
  
  È possibile ripristinare il server da un backup oppure ripristinare le impostazioni predefinite del server.  
  
- [Ripristino del server da un backup](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFromBackup)  
  
- [Ripristino delle impostazioni predefinite del server](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_FactoryReset)  
  
###  <a name="restoring-the-server-from-a-backup"></a><a name="BKMK_RestoreFromBackup"></a>Ripristino del server da un backup  
 In questa sezione vengono fornite indicazioni sul tipo di backup da scegliere.  
  
 Se è disponibile un backup, la scelta migliore per il ripristino del server consiste nell'utilizzare i supporti di installazione del produttore per eseguire il ripristino da un backup esterno. Verranno ripristinate le impostazioni e le cartelle del server dal backup specificato. È necessario solo configurare le impostazioni e ripristinare i dati creati dopo il backup.  
  
 Quando si sceglie di ripristinare il server da un backup precedente, è necessario scegliere il backup specifico da ripristinare e disporre di un file di backup valido in un'unità disco rigido esterna connessa direttamente al server:  
  
- **Se si dispone di un backup molto recente completato correttamente** e si è certi che il backup contenga tutti i dati critici di interesse, la scelta è semplice. Sarà sufficiente ricreare i dati creati dopo l'ultimo backup completato correttamente e riconfigurare le impostazioni modificate dopo tale backup.  
  
- **Se si esegue il ripristino del server a causa di un virus**, selezionare un backup creato sicuramente prima della ricezione del virus. Può essere necessario selezionare un backup di diversi giorni prima.  
  
- **Se si riconfigura il server a causa di impostazioni di configurazione errate**, selezionare un backup creato sicuramente prima della modifica alle impostazioni di configurazione all'origine dell'errore sul server.  
  
  Quando si esegue il ripristino da un backup, il processo esatto e le azioni successive dipendono dal numero di dischi rigidi sul server e dal fatto che l'unità di sistema venga sostituita o meno:  
  
- **Se il server ha un solo disco rigido e l'unità non viene sostituita**, le informazioni di partizione dell'unità rimangono intatte quando si ripristina il server. Il volume di sistema viene ripristinato e i dati sul volume rimanente vengono mantenuti.  
  
- **Se il server ha un solo disco rigido e l'unità non viene sostituita**, il volume di sistema viene ripristinato e sarà necessario ripristinare manualmente le cartelle nel volume dati. È necessario creare tutte le eventuali cartelle condivise non predefinite perché non vengono create durante la ricreazione dell'archiviazione server.  
  
- **Se il server ha più dischi rigidi e l'unità 0 (contenente il volume di sistema) non viene sostituita**, le informazioni di partizione dell'unità rimangono intatte quando si ripristina il server. Il volume di sistema viene ripristinato e tutti i dati sui volumi rimanenti vengono mantenuti.  
  
- **Se il server dispone di più dischi rigidi e l'unità 0 (contenente il volume di sistema) viene sostituita**, viene ripristinato il volume di sistema e sarà necessario ripristinare manualmente le cartelle condivise precedentemente archiviate nell'unità 0.  
  
###  <a name="resetting-the-server-to-factory-default-settings"></a><a name="BKMK_FactoryReset"></a>Ripristino delle impostazioni predefinite del server  
 Se non si dispone di un backup da cui eseguire il ripristino o se per qualche ragione è preferibile o necessario eseguire un ripristino completo del sistema senza ripristinare la configurazione del server precedente, è possibile eseguire il ripristino delle impostazioni predefinite del server mediante il supporto di installazione o ripristino fornito dal produttore dell'hardware del server.  
  
 Quando si ripristinano le impostazioni predefinite del server, vengono eliminate tutte le impostazioni esistenti e le applicazioni installate nel server ed è necessario configurare nuovamente il server. Dopo un ripristino delle impostazioni di fabbrica, il server viene riavviato.  
  
 Quando si esegue un ripristino delle impostazioni di fabbrica, è possibile decidere di conservare o eliminare i dati, con gli effetti seguenti:  
  
-   Se si sceglie di conservare tutti i propri dati, vengono eliminati tutti i dati sul volume di sistema, ma vengono mantenuti i dati sugli altri volumi.  
  
    > [!CAUTION]
    >  Se le impostazioni del disco non corrispondono alle impostazioni predefinite, tutti i dati presenti su un disco verranno eliminati. Se è stato sostituito il disco di sistema, il nuovo disco deve essere più grande del volume di sistema del disco originale.  
    >   
    >  Se le informazioni di partizione per un'unità di sistema sono illeggibili o se si sostituisce l'unità di sistema, tutti i dati sull'unità di sistema vengono rimossi, anche se si imposta la conservazione dei dati.  
  
-   Se si intende rimuovere le autorizzazioni o reimpiegare il server, scegliere di eliminare tutti i dati. Oltre alla configurazione e altre impostazioni server e ai dati nel volume di sistema, vengono eliminati anche tutti gli altri dati e vengono riformattati tutti i dischi rigidi sul server.  
  
> [!NOTE]
>  Se Spazi di archiviazione è configurato nel server, prima di eseguire il ripristino delle impostazioni di fabbrica è consigliabile usare la sezione **Avanzate** della console **Gestione spazi di archiviazione** per rimuovere manualmente tutti gli spazi di archiviazione.  
  
 Dopo un ripristino delle impostazioni di fabbrica, è necessario eseguire le attività seguenti:  
  
-   **Riconfigurare il server.** Nel server usare la procedura guidata Configura server per immettere nuovamente le impostazioni di configurazione. Per configurare un server di Windows Server Essentials gestito in remoto da un computer client, aprire un Web browser e digitare **http://** _< nomeserver\>_ nella barra degli indirizzi.  
  
-   **Riconnettere i computer client al server.** Se un computer è stato precedentemente connesso al server, è necessario disinstallare il software connettore di Windows Server Essentials dal computer prima di connettere di nuovo il computer al server. Per altre informazioni, vedere gli argomenti relativi alla [disinstallazione del software Connettore](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13) e alla [connessione di computer al server](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  
  
##  <a name="restore-or-repair-the-system-drive"></a><a name="BKMK_Restore"></a>Ripristinare o ripristinare l'unità di sistema  
 Il primo passaggio per il ripristino del server consiste nel ripristinare l'unità di sistema del server. Dopo avere ripristinato l'unità di sistema, si effettuerà quanto necessario per ripristinare le unità dati sul server e le eventuali condivisioni interrotte con il ripristino.  
  
 Per eseguire il ripristino, è possibile procedere in tre modi:  
  
-   [Ripristinare il server mediante il supporto di installazione](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_1). Per eseguire il ripristino da un backup, usare il supporto di installazione fornito dal produttore del server.  
  
-   **Usare il supporto di installazione per ripristinare le impostazioni predefinite del server**. Per informazioni su come eseguire questa operazione sul server, vedere la documentazione fornita dal produttore del server.  
  
-   [Ripristinare il server da un computer client mediante il DVD di ripristino](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_2). Se è necessario ripristinare un server amministrato in remoto che esegue Windows Server Essentials, è necessario eseguire il ripristino da un computer client usando il DVD di ripristino del produttore del server.  
  
###  <a name="restore-or-repair-your-server-using-installation-media"></a><a name="BKMK_Restore_1"></a>Ripristinare o ripristinare il server utilizzando il supporto di installazione  
 Nella procedura seguente viene descritto come ripristinare l'unità di sistema del server da un backup tramite il supporto di installazione di Windows Server Essentials. Per informazioni su come usare il supporto di installazione per ripristinare le impostazioni predefinite, vedere la documentazione fornita dal produttore del server.  
  
> [!NOTE]
>  Se il server USA spazi di archiviazione e si esegue il ripristino dei dati in un nuovo server, è necessario ripristinare prima l'unità di sistema e quindi accedere al dashboard di Windows Server Essentials, configurare gli spazi di archiviazione in modo analogo al server precedente e quindi ripristinare il volumi di dati.  
  
##### <a name="to-restore-the-server-system-drive-from-a-backup-using-installation-media"></a>Per ripristinare l'unità di sistema del server da un backup mediante il supporto di installazione  
  
1.  Inserire il DVD di installazione di Windows Server Essentials nell'unità DVD del server, riavviare il server e quindi premere un tasto qualsiasi per avviare dal DVD.  
  
    > [!NOTE]
    >  Se il processo di ripristino non viene avviato automaticamente, verificare le impostazioni del BIOS per il server per assicurarsi che l'unità DVD sia visualizzata per prima nel menu di avvio.  
  
     -Oppure-  
  
     Se il produttore ha precaricato il supporto di installazione sul server, premere F8 durante l'avvio per avviare il sistema dal supporto di installazione.  
  
2.  Terminato il caricamento dei file di Windows Server, scegliere la lingua e le altre preferenze e fare clic su **Avanti**.  
  
3.  Nella pagina successiva della procedura guidata fare clic su **Ripristina il computer**.  
  
    > [!CAUTION]
    >  Non scegliere l'opzione **Installa**. Tale opzione determina l'esecuzione di un'installazione di sistema completa che elimina tutte le impostazioni di configurazione e tutti i dati sull'unità di sistema.  
  
4.  Nella pagina **Scegli un'opzione** fare clic su **Risoluzione dei problemi**.  
  
5.  Nella pagina **Ripristino immagine del sistema** selezionare il sistema corrente? **Windows Server Essentials** o **Windows Server Essentials**.  
  
     Viene avviata la procedura guidata Effettua re-imaging computer.  
  
6.  Nella **Selezionare un backup dell'immagine del sistema** è possibile scegliere di usare il backup più recente oppure selezionarne uno precedente. Verrà ripristinato lo stato in cui il sistema si trovava al momento del backup scelto per il ripristino del server. I dati aggiunti o le modifiche apportate alle impostazioni dopo il salvataggio del backup devono essere ricreati.  
  
     Selezionare una delle opzioni seguenti e quindi fare clic su **Avanti**:  
  
    -   **Usa l'immagine del sistema più recente (scelta consigliata)** .  
  
    -   **Seleziona un'immagine del sistema**  
  
    > [!NOTE]
    >  Se si dispone di un backup molto recente completato correttamente e si è certi che il backup contenga tutti i dati critici di interesse, la scelta è semplice. Sarà sufficiente ricreare i dati creati dopo l'ultimo backup completato correttamente e riconfigurare le impostazioni modificate dopo tale backup.  
    >   
    >  Se si esegue il ripristino del server a causa di un virus, selezionare un backup creato sicuramente prima della ricezione del virus. Può essere necessario selezionare un backup di diversi giorni prima.  
    >   
    >  Se si riconfigura il server a causa di impostazioni di configurazione errate, selezionare un backup creato sicuramente prima della modifica alle impostazioni di configurazione all'origine dell'errore sul server.  
  
7.  Seguire le istruzioni della procedura guidata per completare il ripristino di sistema.  
  
8.  Al termine del ripristino del server, rimuovere il DVD di installazione, se è stato usato, quindi riavviare il server.  
  
> [!NOTE]
>  Per ripristinare e condividere le cartelle sul server, può essere necessario eseguire alcuni altri passaggi. Per altre informazioni, vedere [Ripristinare file e cartelle sul server](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders).  
  
###  <a name="restore-or-reset-your-server-from-a-client-computer-using-the-recovery-dvd"></a><a name="BKMK_Restore_2"></a>Ripristinare o reimpostare il server da un computer client usando il DVD di ripristino  
 In Windows Server Essentials è possibile avviare il server da un'unità flash USB avviabile creata e quindi ripristinare il server da un computer client usando il DVD di ripristino ricevuto dal produttore del server. Il computer client deve trovarsi nella stessa rete del server. Questo metodo non è disponibile in Windows Server Essentials.  
  
 La procedura che segue indica le operazioni generali per il ripristino di un server. I passaggi sono applicabili sia per il ripristino da un backup, sia per il ripristino delle impostazioni predefinite. Per istruzioni più dettagliate, vedere la documentazione fornita dal produttore del server.  
  
##### <a name="to-restore-or-reset-the-server-from-a-client-computer-using-the-recovery-dvd"></a>Per ripristinare il server da un computer client mediante il DVD di ripristino  
  
1.  Inserire i supporti di ripristino del server di Windows Server Essentials ricevuti dal produttore del server in un computer client.  
  
     Viene avviata la procedura guidata Ripristina server in uso.  
  
2.  Seguire le istruzioni della procedura guidata per creare un'unità flash USB avviabile che verrà usata per avviare il server in modalità di ripristino.  
  
3.  Terminata la preparazione dell'unità flash USB avviabile, inserirla nel server e avviare il server in modalità di ripristino. Per informazioni su come avviare il server in modalità di ripristino, consultare la documentazione fornita dal produttore dell'hardware del server.  
  
     Dopo avere avviato il server in modalità di ripristino, la procedura guidata Ripristina server in uso individua il server e quindi stabilisce una connessione.  
  
4.  Seguire le istruzioni della procedura guidata per eseguire il ripristino del server.  
  
> [!NOTE]
>  Questo metodo di ripristino del server ignora i dispositivi di archiviazione esterni collegati al server durante il ripristino. Se si desidera cancellare i dati in un dispositivo di archiviazione esterno, è necessario procedere manualmente.  
  
> [!NOTE]
>  Se sono state create altre cartelle condivise nel server, è possibile che queste non vengano riconosciute dal server dopo il ripristino dei dati dal backup. È necessario condividere nuovamente tali cartelle. Per altre informazioni, vedere [Ripristinare file e cartelle sul server](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders).  
  
##  <a name="restore-files-and-folders-on-the-server"></a><a name="BKMK_RestoreFilesAndFolders"></a>Ripristino di file e cartelle nel server  
 A seconda del metodo usato per ripristinare il server e del tipo di archiviazione in uso sul server, potrebbe essere necessario ripristinare i volumi di dati dopo il ripristino dell'unità del sistema. In alcuni casi potrebbe essere necessario condividere nuovamente le cartelle esistenti per fare in modo che il server le riconosca.  
  
 Di seguito sono riportati alcuni esempi dei casi in cui è necessario ripristinare file e cartelle:  
  
-   [Ripristinare file e cartelle da un backup del server](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesFromBackup). Se è stato sostituito il disco di sistema o se le informazioni di partizione del disco di sistema sono illeggibili, è possibile ripristinare il sistema, ma non i dati da altri volumi sul disco. Per ripristinare file e cartelle da altri volumi di dati, è necessario usare il ripristino guidato di file e cartelle.  
  
-   [Restore shared folders on the server](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_ConfigreSharedFolders). Se sono state create altre cartelle condivise sul server, dopo il ripristino dell'unità di sistema dal backup, le cartelle condivise si trovano ancora nella partizione dati o sono state ripristinate in tale partizione, ma potrebbero non essere riconosciute dal server. È necessario condividere nuovamente tali cartelle.  
  
###  <a name="restore-files-and-folders-from-a-server-backup"></a><a name="BKMK_RestoreFilesFromBackup"></a>Ripristinare file e cartelle da un backup del server  
 La procedura guidata Ripristino file o cartelle consente di proteggere i dati se il disco rigido smette di funzionare o i file vengano accidentalmente eliminati. Con il backup di Windows Server Essentials, è possibile creare una copia di tutti i dati nel disco rigido e archiviare i dati in un dispositivo di archiviazione esterno. Se i dati originali sul disco rigido vengono accidentalmente cancellati o sovrascritti o diventano inaccessibili a causa di un malfunzionamento, è possibile eseguirne il ripristino dal backup. La procedura guidata Ripristino file o cartelle consente di ripristinare un file o una cartella, più file o cartelle oppure un'intera unità disco rigido da un backup esistente.  
  
 Dopo un ripristino di sistema, può essere necessario eseguire la procedura guidata Ripristino file o cartelle per ripristinare i file e le cartelle non mantenuti durante il ripristino. Se ad esempio è stato sostituito il disco di sistema o se le informazioni di partizione del disco di sistema sono illeggibili, non è possibile ripristinare i dati da altri volumi sul disco di sistema.  
  
> [!NOTE]
>  Non è possibile usare la procedura guidata Ripristino file o cartelle per ripristinare l'intera unità di sistema. Per informazioni su come ripristinare l'intero sistema, vedere [Ripristinare il server mediante il supporto di installazione](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_1) oppure [Ripristinare il server da un computer client mediante il DVD di ripristino](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_2).  
  
##### <a name="to-restore-files-and-folders-from-a-server-backup"></a>Per ripristinare file e cartelle da un backup del server  
  
1.  Aprire il dashboard di Windows Server Essentials e quindi fare clic sulla scheda **dispositivi** .  
  
2.  Fare clic sul nome del server, quindi su **Ripristino file o cartelle del server** nel riquadro **Attività**.  
  
     Verrà avviata la procedura guidata Ripristino file o cartelle.  
  
3.  Seguire le istruzioni della procedura guidata per ripristinare i file o le cartelle.  
  
> [!WARNING]
>  Per altre informazioni sul backup e il ripristino di file e cartelle, vedere [Manage backup and Restore](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md).  
  
###  <a name="restore-shared-folders-on-the-server"></a><a name="BKMK_ConfigreSharedFolders"></a>Ripristinare le cartelle condivise nel server  
 Dopo aver ripristinato l'unità di sistema del server, se le cartelle condivise si trovano ancora nella partizione dati o sono state ripristinate nella partizione dati, potrebbe essere necessario configurare nuovamente le cartelle condivise affinché il server riconosca le cartelle. La procedura seguente descrive come aggiungere cartelle condivise in precedenza.  
  
##### <a name="to-add-an-existing-folder-to-the-server-shared-folders"></a>Per aggiungere una cartella esistente alle cartelle condivise del  
  
1.  In Esplora file individuare la cartella condivisa sul disco rigido.  
  
2.  Fare clic con il pulsante destro del mouse sulla cartella condivisa, scegliere **Proprietà**, fare clic sulla scheda **Condivisione** e prendere nota delle autorizzazioni per la cartella.  
  
3.  Accedere al dashboard di Windows Server Essentials, fare clic sulla scheda **archiviazione** e quindi fare clic su **Aggiungi una cartella** nel riquadro **attività cartelle server** .  
  
     Verrà avviata la procedura guidata Aggiungi cartella.  
  
4.  Digitare un nome per la condivisione nella casella **Nome**.  
  
5.  Fare clic su **Sfoglia**, passare a *< unità\>\\< nomeserver\>* \ServerFolders (ad esempio *d:\Contoso\ServerFolders*), selezionare la cartella che si desidera condividere, quindi fare clic su **OK**.  
  
6.  Fare clic su **Avanti**.  
  
7.  Specificare le autorizzazioni annotate nel passaggio 2, quindi fare clic su **Aggiungi cartella**.  
  
    > [!IMPORTANT]
    >  Queste autorizzazioni sostituiranno tutte le autorizzazioni esistenti che non sono state aggiunte alla cartella usando il dashboard di Windows Server Essentials.  
  
> [!IMPORTANT]
>  Dopo avere aggiunto le cartelle all'elenco di cartelle condivise, verificare che siano incluse nel backup del server. Per informazioni sull'aggiunta di cartelle al backup del server, vedere [Configurare o personalizzare un backup del server](Set-up-or-customize-server-backup.md).  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire il backup e il ripristino](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usare Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
