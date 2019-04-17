---
title: Ripristinare o riparare il server che esegue Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 27bf6f24-30c4-4935-9b24-069eb43e22f4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e1ebc539928d13b0d34dfe5a0ee57ce6e98088e9
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="restore-or-repair-your-server-running-windows-server-essentials"></a>Ripristinare o riparare il server che esegue Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 In questo argomento fornisce una panoramica e procedure di supporto per il ripristino di un server che esegue Windows Server Essentials e include le sezioni seguenti:  
  
-   [Panoramica del ripristino di sistema in un server](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Overview)  
  
-   [Ripristinare o riparare l'unità del sistema](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore)  
  
-   [Ripristinare file e cartelle nel server di](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders)  
  
##  <a name="BKMK_Overview"></a>Panoramica del ripristino di sistema in un server  
 Lo stato del server quando si esegue un ripristino riguarda il metodo di ripristino disponibile e sulla completezza del ripristino è possibile eseguire.  
  
 I motivi più comuni per il ripristino di un server sono:  
  
-   Impossibile inoculato o eliminare un virus nel server.  
  
-   Le impostazioni di configurazione del server sono danneggiate e non è possibile avviare il server.  
  
-   È stata sostituita l'unità del sistema.  
  
-   Il server viene disattivato e si desidera ripristinare un nuovo server.  
  
 È possibile ripristinare il server da un backup o il server è possibile ripristinare le impostazioni predefinite.  
  
-   [Ripristino del server da un backup](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFromBackup)  
  
-   [Il server il ripristino delle impostazioni predefinite](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_FactoryReset)  
  
###  <a name="BKMK_RestoreFromBackup"></a>Ripristino del server da un backup  
 In questa sezione vengono fornite indicazioni sul tipo di backup da scegliere.  
  
 Se è disponibile un backup, la scelta migliore per il ripristino del server è possibile utilizzare il supporto di installazione s produttore per ripristinare da un backup esterno. Il verranno ripristinate le impostazioni del server e le cartelle dal backup che preferisci. È necessario solo configurare le impostazioni e ripristinare i dati creati dopo il backup.  
  
 Quando si sceglie di ripristinare il server tramite il ripristino da un backup precedente, è necessario scegliere il backup specifico da ripristinare e disporre di un file di backup valido in un'unità disco rigido esterna connessa direttamente al server:  
  
-   **Se si dispone di un backup molto recente completato del server**, e si è certi che il backup contenga tutti i dati critici, la scelta è piuttosto semplice. Sarà sufficiente ricreare i dati creati dopo l'ultimo buona backup e riconfigurare le impostazioni modificate dopo il backup.  
  
-   **Se si sta ripristinando il server a causa di un virus**, selezionare un backup creato sicuramente prima della ricezione del virus. Potrebbe essere necessario tornare a selezionare un backup di diversi giorni.  
  
-   **Se si sta ripristinando il server a causa di impostazioni di configurazione errate**, selezionare un backup creato sicuramente prima l'impostazione di modifica che causa il problema nel server di configurazione.  
  
 Quando si ripristina da un backup, il processo esatto e le azioni successive dipendono dal numero di dischi rigidi sul server e se viene sostituita l'unità di sistema:  
  
-   **Se il server dispone di una singola unità disco rigida e l'unità non viene sostituita**, le informazioni di partizione dell'unità rimangono intatte quando si ripristina il server. Il volume di sistema viene ripristinato e i dati sul volume rimanente vengono mantenuti.  
  
-   **Se il server dispone di una singola unità disco rigida e l'unità viene sostituita**, il volume di sistema viene ripristinato e sarà necessario ripristinare manualmente le cartelle per il volume di dati. Le cartelle condivise non predefinite devono essere creati poiché non vengono creati quando lo spazio di archiviazione server viene ricreato.  
  
-   **Se il server ha più dischi rigidi e unità 0 (contenente il volume di sistema) non viene sostituita**, le informazioni di partizione dell'unità rimangono intatte quando si ripristina il server. Il volume di sistema viene ripristinato e i dati in tutti i volumi rimanenti vengono mantenuti.  
  
-   **Se il server ha più dischi rigidi e unità 0 (contenente il volume di sistema) viene sostituita**, il volume di sistema viene ripristinato e sarà necessario ripristinare manualmente le cartelle condivise precedentemente archiviate nell'unità 0.  
  
###  <a name="BKMK_FactoryReset"></a>Il server il ripristino delle impostazioni predefinite  
 Se non hai una copia di backup che è possibile ripristinare o se per qualche ragione è preferibile o necessario eseguire un ripristino completo del sistema senza ripristinare la configurazione del server precedente, è possibile eseguire un ripristino che consente di ripristinare il server delle impostazioni predefinite tramite installazione o supporti di ripristino dal produttore dell'hardware del server.  
  
 Quando si ripristina il server per il ripristino delle impostazioni predefinite, tutte le impostazioni esistenti e le applicazioni installate nel server vengono eliminate, ed è necessario configurare nuovamente il server. Dopo la reimpostazione di una factory, il riavvio del server.  
  
 Quando si esegue un ripristino delle impostazioni di fabbrica, è possibile scegliere di mantenere i dati o l'eliminazione, con gli effetti seguenti:  
  
-   Se si sceglie di mantenere tutti i dati, tutti i dati volume di sistema viene eliminato, ma vengono mantenuti i dati sugli altri volumi.  
  
    > [!CAUTION]
    >  Se le impostazioni del disco non corrispondono le impostazioni predefinite, verranno eliminati tutti i dati su un disco. Se è stato sostituito il disco di sistema, il nuovo disco deve essere maggiore di volume di sistema s disco originale.  
    >   
    >  Se le informazioni di partizione per un'unità di sistema sono illeggibili o se si sostituisce l'unità del sistema, tutti i dati nell'unità di sistema vengono rimossi, anche se si sceglie di conservare tutti i dati.  
  
-   Se si intende rimuovere le autorizzazioni o reimpiegare il server, è possibile scegliere di eliminare tutti i dati. Oltre alla configurazione del server, altre impostazioni e i dati nel volume di sistema, tutti gli altri dati viene eliminato e vengono riformattati tutti i dischi rigidi sul server.  
  
> [!NOTE]
>  Se gli spazi di archiviazione è configurato nel server, prima di eseguire un ripristino delle impostazioni di fabbrica, è necessario utilizzare il **avanzate** sezione il **gestire spazi di archiviazione** console per rimuovere manualmente tutti gli spazi di archiviazione.  
  
 Dopo la reimpostazione di una factory, sarà necessario eseguire le attività seguenti:  
  
-   **Riconfigurare il server.** Nel server, utilizzare la configurazione guidata Server per immettere nuovamente le impostazioni di configurazione. Per configurare un server gestito in remoto di Windows Server Essentials da un computer client, aprire un web browser e digitare **http://***< YourServerName\ >* nella barra degli indirizzi.  
  
-   **Riconnettere i computer client al server.** Se un computer precedentemente connesso al server, è necessario disinstallare il software connettore Windows Server Essentials dal computer prima connettere il computer al server. Per ulteriori informazioni, vedere [disinstallare il software connettore](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13) e [connettere i computer al server](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  
  
##  <a name="BKMK_Restore"></a>Ripristinare o riparare l'unità del sistema  
 Il primo passaggio per il ripristino del server è per il ripristino delle unità di sistema del server. Dopo avere ripristinato l'unità del sistema, si effettuerà quanto necessario per ripristinare le unità dati sul server e le eventuali condivisioni interrotte con il ripristino.  
  
 Tre metodi disponibili per eseguire il ripristino:  
  
-   [Ripristinare o riparare il server mediante il supporto di installazione](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_1). Utilizzare il supporto di installazione dal produttore del server per ripristinare da un backup.  
  
-   **Utilizzare il supporto di installazione per ripristinare il server di impostazioni predefinite**. Per scoprire come eseguire questa operazione sul server, vedere la documentazione fornita dal produttore del server.  
  
-   [Ripristinare o reimpostare il server da un computer client mediante il DVD di ripristino](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_2). Se è necessario ripristinare un server amministrato da postazione remota che esegue Windows Server Essentials, è necessario eseguire il ripristino da un computer client mediante il DVD di ripristino fornito dal produttore del server.  
  
###  <a name="BKMK_Restore_1"></a>Ripristinare o riparare il server mediante il supporto di installazione  
 La procedura seguente viene descritto come ripristinare l'unità di sistema del server da un backup utilizzando il supporto di installazione di Windows Server Essentials. (Per scoprire come usare il supporto di installazione per ripristinare le impostazioni predefinite, vedere la documentazione fornita dal produttore del server).  
  
> [!NOTE]
>  Se il server usa spazi di archiviazione e ripristinare i dati in un nuovo server, è necessario ripristinare l'unità di sistema prima di tutto e quindi accedere al Dashboard di Windows Server Essentials, configurare spazi di archiviazione in modo analogo, come il server precedente e quindi ripristinare i volumi di dati.  
  
##### <a name="to-restore-the-server-system-drive-from-a-backup-using-installation-media"></a>Per ripristinare l'unità di sistema di server da un backup con supporto di installazione  
  
1.  Inserire il DVD di installazione di Windows Server Essentials nell'unità DVD del server, riavviare il server e quindi premere un tasto qualsiasi per avviare dal DVD.  
  
    > [!NOTE]
    >  Se il processo di ripristino non viene avviata automaticamente, controllare le impostazioni del BIOS per il server per assicurarsi che l'unità DVD visualizzata per prima nel menu di avvio.  
  
     - O -  
  
     Se il produttore ha precaricato il supporto di installazione sul server, premere F8 durante l'avvio per avviare dal supporto di installazione.  
  
2.  Dopo il Server Windows file caricare, scegli la lingua e altre preferenze e quindi fare clic su **Avanti**.  
  
3.  Nella pagina successiva della procedura guidata, fare clic su **Ripristina il computer**.  
  
    > [!CAUTION]
    >  Non scegliere il **installa** opzione. Tale opzione semplificherà un'installazione completa del sistema che elimina tutte le impostazioni di configurazione e tutti i dati nell'unità di sistema.  
  
4.  Nel **Scegli un'opzione** pagina, fare clic su **risoluzione dei problemi**.  
  
5.  Nel **Ripristino immagine del sistema** pagina, selezionare il sistema corrente? entrambi **Windows Server Essentials** o **Windows Server Essentials**.  
  
     Apre la procedura guidata effettua re-imaging del Computer.  
  
6.  Nel **selezionare il backup di un'immagine di sistema** pagina, è possibile scegliere di utilizzare il backup più recente oppure è possibile selezionare un backup precedente. Il sistema verrà ripristinato lo stato in cui si trovava al momento del backup scelto per il ripristino del server. Dati che è stato aggiunto o le modifiche alle impostazioni che sono state apportate dopo il salvataggio del backup devono essere ricreate.  
  
     Selezionare una delle opzioni seguenti e quindi fare clic su **Avanti**:  
  
    -   **Utilizzare l'immagine di sistema più recente (scelta consigliata)**  
  
    -   **Selezionare un'immagine del sistema**  
  
    > [!NOTE]
    >  Se si dispone di un backup molto recente completato del server, e si è certi che il backup contenga tutti i dati critici, la scelta è piuttosto semplice. Sarà sufficiente ricreare i dati creati dopo l'ultimo buona backup e riconfigurare le impostazioni modificate dopo il backup.  
    >   
    >  Se si desidera ripristinare il server a causa di un virus, selezionare un backup creato sicuramente prima della ricezione del virus. Potrebbe essere necessario tornare a selezionare un backup di diversi giorni.  
    >   
    >  Se si desidera ripristinare il server a causa di impostazioni di configurazione errate, selezionare un backup creato sicuramente prima la modifica di impostazione di configurazione che causa il problema sul server.  
  
7.  Seguire le istruzioni della procedura guidata per completare il ripristino di sistema.  
  
8.  Dopo che il server è stato ripristinato correttamente, rimuovere il DVD di installazione se è stato utilizzato uno e quindi riavviare il server.  
  
> [!NOTE]
>  Per ripristinare e condividere le cartelle nel server, è necessario eseguire alcuni passaggi aggiuntivi. Per ulteriori informazioni, vedere [ripristinare file e cartelle sul server](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders).  
  
###  <a name="BKMK_Restore_2"></a>Ripristinare o reimpostare il server da un computer client mediante il DVD di ripristino  
 In Windows Server Essentials, è possibile avviare il server da un'unità flash USB avviabile creata e quindi ripristinare il server da un computer client mediante il DVD che hai ricevuto dal produttore del server di ripristino. Il computer client deve essere sulla stessa rete del server. Questo metodo non è disponibile in Windows Server Essentials.  
  
 La procedura seguente descrive i passaggi generali per l'esecuzione di ripristino di un server. I passaggi sono applicabili sia per il ripristino da un backup o il ripristino delle impostazioni predefinite. Per istruzioni più dettagliate, vedere la documentazione fornita dal produttore del server.  
  
##### <a name="to-restore-or-reset-the-server-from-a-client-computer-using-the-recovery-dvd"></a>Per ripristinare o reimpostare il server da un computer client mediante il DVD di ripristino  
  
1.  Inserire il supporto di ripristino fornito dal produttore del server in un computer client Windows Server Essentials.  
  
     Apre la procedura guidata Ripristina il Server.  
  
2.  Seguire le istruzioni della procedura guidata per creare un'unità flash USB avviabile che userai per avviare il server in modalità di ripristino.  
  
3.  Dopo la procedura guidata Ripristina il Server prepara l'unità flash USB avviabile, Inserisci l'unità flash nel server e quindi avviare il server in modalità di ripristino. Per informazioni su come avviare il server in modalità di ripristino, vedere la documentazione dal produttore dell'hardware del server.  
  
     Dopo avere avviato il server in modalità di ripristino, la procedura guidata Ripristina il Server individua i server e quindi stabilisce una connessione.  
  
4.  Seguire le istruzioni della procedura guidata per completare il ripristino del server.  
  
> [!NOTE]
>  Questo metodo di ripristino del server ignora i dispositivi di archiviazione esterni collegati al server durante il ripristino. Se si desidera cancellare i dati in un dispositivo di archiviazione esterno, è necessario farlo manualmente.  
  
> [!NOTE]
>  Se hai creato le cartelle condivise aggiuntive nel server, dopo aver ripristinato i dati dal backup, le cartelle condivise aggiuntive potrebbero non essere riconosciute dal server. È necessario condividere nuovamente tali cartelle. Per ulteriori informazioni, vedere [ripristinare file e cartelle sul server](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesAndFolders).  
  
##  <a name="BKMK_RestoreFilesAndFolders"></a>Ripristinare file e cartelle nel server di  
 A seconda del metodo che è stato usato per il ripristino del server e Usa il tipo di server di archiviazione, potrebbe essere necessario ripristinare i volumi di dati dopo il ripristino delle unità di sistema. In alcuni casi, potrebbe essere necessario condividere nuovamente le cartelle esistenti in modo che il server le riconosca.  
  
 Ecco alcuni esempi di quando potrebbe essere necessario ripristinare file e cartelle:  
  
-   [Ripristinare file e cartelle da un backup del server](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_RestoreFilesFromBackup). Se è stato sostituito il disco di sistema o se le informazioni di partizione del disco di sistema sono illeggibili, è possibile ripristinare il sistema, ma non è possibile ripristinare dati da altri volumi sul disco. Per ripristinare file e cartelle da altri volumi di dati, è necessario utilizzare il ripristino guidato file e cartelle.  
  
-   [Ripristinare le cartelle condivise sul server](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_ConfigreSharedFolders). Se hai creato le cartelle condivise aggiuntive nel server, dopo il ripristino delle unità di sistema dal backup, le cartelle condivise sono ancora nella partizione dati o sono state ripristinate in tale partizione, ma potrebbero non essere riconosciute dal server. È necessario condividere nuovamente tali cartelle.  
  
###  <a name="BKMK_RestoreFilesFromBackup"></a>Ripristinare file e cartelle da un backup del server  
 Il ripristino di file e cartelle guidata consente di proteggere i dati se il disco rigido smette di funzionare o i file vengano accidentalmente eliminati. Con Windows Server Essentials Backup, è possibile creare una copia di tutti i dati sul disco rigido e archiviare i dati in un dispositivo di archiviazione esterno. Se i dati originali sul disco rigido vengono accidentalmente cancellati sovrascritti o diventano inaccessibili a causa di un malfunzionamento, è possibile ripristinare i dati dal backup. Il ripristino di file o cartelle guidata consente di ripristinare un singolo file o cartella, più file o cartelle o un'unità disco rigido da un backup esistente.  
  
 Dopo un ripristino del sistema, si potrebbe essere necessario utilizzare il ripristino guidato file e cartelle per ripristinare file e cartelle non mantenuti durante il ripristino. Ad esempio, se è stato sostituito il disco di sistema o se le informazioni di partizione del disco di sistema sono illeggibili, è possibile ripristinare dati da altri volumi sul disco di sistema.  
  
> [!NOTE]
>  È possibile utilizzare il ripristino guidato file e cartelle per ripristinare l'unità di sistema completo. Per informazioni su come ripristinare il sistema completo, vedere [ripristinare o riparare il server mediante il supporto di installazione](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_1) o [ripristinare o reimpostare il server da un computer client mediante il DVD di ripristino](Restore-or-repair-your-server-running-Windows-Server-Essentials.md#BKMK_Restore_2).  
  
##### <a name="to-restore-files-and-folders-from-a-server-backup"></a>Per ripristinare file e cartelle da un backup del server  
  
1.  Aprire il Dashboard di Windows Server Essentials, quindi fare clic su di **dispositivi** scheda.  
  
2.  Fare clic sul nome del server, quindi fare clic su **ripristinare file o cartelle per il server** nel **attività** riquadro.  
  
     Il ripristino di file e cartelle guidata viene aperto.  
  
3.  Seguire le istruzioni della procedura guidata per ripristinare i file o cartelle.  
  
> [!WARNING]
>  Per ulteriori informazioni sul backup e ripristino di file e cartelle, vedere [gestire Backup e ripristino](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_ConfigreSharedFolders"></a>Ripristinare le cartelle condivise nel server di  
 Dopo avere ripristinato l'unità del sistema server s, se le cartelle condivise sono ancora nella partizione dati o sono state ripristinate in tale partizione, si potrebbe essere necessario configurare nuovamente le cartelle condivise in modo che il server di riconoscere le cartelle. La procedura seguente viene descritto come aggiungere cartelle condivise che è state condivise prima.  
  
##### <a name="to-add-an-existing-folder-to-the-server-shared-folders"></a>Per aggiungere una cartella esistente nel server di cartelle condivise  
  
1.  In Esplora File, individuare la cartella condivisa sul disco rigido.  
  
2.  Fare clic sulla cartella condivisa, fare clic su **proprietà**, fare clic su di **condivisione** scheda e quindi annotare le autorizzazioni della cartella.  
  
3.  Accedere al Dashboard di Windows Server Essentials, fare clic su di **archiviazione** scheda e quindi fare clic su **aggiungere una cartella** nel **attività cartelle Server** riquadro.  
  
     L'aggiunta guidata cartella apre.  
  
4.  Digitare un nome per la condivisione nel **nome** casella.  
  
5.  Fare clic su **Sfoglia**, passare a *< drive\ > \ < ServerName\ >*\ServerFolders (ad esempio *d:\Contoso\ServerFolders*), selezionare la cartella che si desidera condividere e quindi fare clic su **OK**.  
  
6.  Fare clic su **Avanti**.  
  
7.  Specificare le autorizzazioni annotate nel passaggio 2, quindi fare clic su **Aggiungi cartella**.  
  
    > [!IMPORTANT]
    >  Queste autorizzazioni sostituiranno tutte le autorizzazioni esistenti che non sono state aggiunte alla cartella mediante il Dashboard di Windows Server Essentials.  
  
> [!IMPORTANT]
>  Dopo aver aggiunto le cartelle all'elenco di cartelle condivise, assicurarsi che le cartelle sono inclusi nel backup del server. Per informazioni sull'aggiunta di cartelle al backup del server, vedere [impostare configurare o personalizzare un backup del server](Set-up-or-customize-server-backup.md).  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire Backup e ripristino](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [Usare Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
