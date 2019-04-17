---
title: Gestire le cartelle Server in Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 090cf1b8-7b9b-48b9-ae85-b98477b8d7cc
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e1f3640f21b95acafa850b2204cd52f9c0f324e5
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="manage-server-folders-in-windows-server-essentials"></a>Gestire le cartelle Server in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 Come un amministratore del server, è possibile gestire l'accesso alle cartelle server (noto come cartelle condivise per l'accesso dalla finestra di avvio, accesso Web remoto, l'app My Server per Windows Phone o app My Server per Windows 8) nel server tramite le attività nel **cartelle Server** scheda del Dashboard, consentendo agli utenti diversi livelli di accesso a una serie di file.  
  
 Gli argomenti seguenti forniscono informazioni che consentono di comprendere, creare e gestire le cartelle del server:  
  
-   [Gestire le cartelle del server tramite il Dashboard](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Gestire l'accesso alle cartelle del server](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Aggiungere o spostare una cartella del server](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Aggiungere una cartella del server mancante](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Informazioni sulle cartelle condivise](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Informazioni sulle copie shadow](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Shadow)  
  
##  <a name="BKMK_2"></a>Gestire le cartelle del server tramite il Dashboard  
 Windows Server Essentials consente di eseguire attività amministrative comuni tramite il Dashboard. Il **cartelle Server** pagina del Dashboard contiene quanto segue:  
  
-   Un elenco di cartelle del server, che visualizza:  
  
    -   Il nome della cartella  
  
    -   Una descrizione della cartella  
  
    -   Il percorso della cartella  
  
    -   La quantità di spazio libero disponibile nel percorso di cartella  
  
    -   Brevi informazioni di stato su qualsiasi attività che vengono eseguite nella cartella. il **stato** campo è vuoto se la cartella è integra e se non sono in esecuzione alcuna attività  
  
-   Un riquadro dei dettagli che può fornire informazioni aggiuntive su una cartella selezionata  
  
-   Un riquadro attività che include una serie di attività amministrative correlate alle cartelle.  
  
 La tabella seguente descrive le varie attività cartelle server disponibili nel Dashboard di Windows Server Essentials. La maggior parte delle attività sono specifiche delle cartelle e sono visibili solo quando si seleziona una cartella nell'elenco.  
  
### <a name="server-folder-tasks-on-the-dashboard"></a>Attività relative alle cartelle server nel Dashboard  
  
|Nome attività|Descrizione|  
|---------------|-----------------|  
|Aprire la cartella|Visualizza il contenuto della cartella selezionata in Esplora File (denominato Esplora risorse nelle versioni precedenti di Windows).|  
|Eliminare la cartella|Consente di eliminare una cartella creata dall'utente. Questa attività non è disponibile per le cartelle predefinite che crea installazione del server.|  
|Spostare la cartella|Apre una procedura guidata che consente di spostare una cartella del server in una nuova posizione.|  
|Interrompi condivisione cartella|Interrompe la condivisione della cartella selezionata, ma non lo elimina. Quando la cartella non è più condivisa, non viene visualizzato nel Dashboard. Questa attività non è disponibile per le cartelle predefinite che crea installazione del server.|  
|Visualizzare le proprietà della cartella|Visualizza le proprietà di una cartella selezionata e consente di:<br /><br /> -Modificare il nome delle cartelle creati dall'utente.<br /><br /> -Modificare la descrizione di una cartella selezionata.<br /><br /> -Consente di visualizzare le dimensioni della cartella.<br /><br /> -Aprire la cartella condivisa in Esplora File.<br /><br /> -Specificare le autorizzazioni di accesso di account utente per una cartella selezionata.<br /><br /> -Nascondere una cartella selezionata dalle applicazioni accesso Web remoto e servizio Web.<br /><br /> -Specificare quota della cartella.|  
|Aggiungere una cartella|Consente di creare una nuova cartella del server e assegnare il livello di accesso consentito per ogni account utente.|  
|Informazioni sulle cartelle Server|Apre un argomento della Guida su Internet che descrive l'utilizzo e la funzionalità di cartelle del server.|  
  
##  <a name="BKMK_1"></a>Gestire l'accesso alle cartelle del server  
 Windows Server Essentials consente di archiviare i file che si trovano nei computer client in una posizione centrale tramite le cartelle del server. L'archiviazione dei file in cartelle server assicura che i file sono in una posizione sempre accessibile in modo sicuro da ogni client.  
  
 Uso di cartelle del server per archiviare i file consente di:  
  
-   Eseguire il backup della cartella del server tramite Backup e ripristino Server per la protezione in caso di errore totale del server.  
  
-   Accesso ai file archiviati nella cartella del server da qualsiasi posizione tramite un Browser Internet mediante accesso Web remoto o tramite l'App My Server per Windows Phone e Windows 8.  
  
-   Accesso nuova cartella del server da qualsiasi computer client.  
  
 Puoi gestire l'accesso alle cartelle server nel server tramite le attività nel **cartelle Server** scheda del Dashboard. La tabella seguente elenca le cartelle server create per impostazione predefinita quando si installa Windows Server Essentials o attivare i flussi multimediali sul server.  
  
|Nome cartella del server|Descrizione|  
|------------------------|-----------------|  
|Backup Computer client|Per impostazione predefinita, Windows Server Essentials Crea backup dei computer che sono archiviati in questa cartella client. Le impostazioni per backup Computer Client possono essere modificate dall'amministratore di rete.|  
|Società|Usato per archiviare e accedere ai documenti correlati all'organizzazione dagli utenti di rete.|  
|Backup cronologia file|Per impostazione predefinita, Windows Server Essentials Usa cronologia File per creare backup di file che vengono archiviati in questa cartella. Queste impostazioni di cronologia File possono essere modificate dagli amministratori di rete.|  
|Reindirizzamento cartelle|Usato per archiviare e accedere alle cartelle configurate per il reindirizzamento cartelle per gli utenti della rete.|  
|Utenti|Usato per archiviare e accedere ai file da parte degli utenti di rete. Una cartella specifiche dell'utente viene generata automaticamente nel **utenti** cartella del server per ogni account utente di rete che crei.|  
|Musica|Usato per archiviare e accedere ai file musicali da parte degli utenti di rete. In questa cartella è disponibile quando si attiva la condivisione di file multimediali.|  
|Immagini|Usato per archiviare e accedere ai file di immagine da parte degli utenti di rete. In questa cartella è disponibile quando si attiva la condivisione di file multimediali.|  
|Registrazioni|Usato per archiviare e accedere ai programmi TV registrati per gli utenti della rete. In questa cartella è disponibile quando si attiva la condivisione di file multimediali.|  
|Video|Usato per archiviare e accedere ai file video da parte degli utenti di rete. In questa cartella è disponibile quando si attiva la condivisione di file multimediali.|  
  
 Per nascondere o impostare le autorizzazioni per le cartelle del server o per modificare le proprietà delle cartelle server, vedere le procedure seguenti:  
  
-   [Nascondere le cartelle del server](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Hide)  
  
-   [Impostare le autorizzazioni per le cartelle del server](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Perms)  
  
-   [Visualizzare o modificare le proprietà delle cartelle server](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_10)  
  
###  <a name="BKMK_Hide"></a>Nascondere le cartelle del server  
 Come amministratore di rete, è possibile scegliere di nascondere le cartelle del server e impedirne la visualizzazione nel sito Web accesso Web remoto o nelle applicazioni servizi Web (ad esempio My Server).  
  
> [!NOTE]
>  È necessario essere un amministratore di rete per eseguire questa procedura.  
  
##### <a name="to-hide-server-folders-from-being-displayed-in-remote-web-access"></a>Per nascondere le cartelle del server venga visualizzato in accesso Web remoto  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Fare clic su **archiviazione**, quindi fare clic su **cartelle Server**.  
  
3.  Nella visualizzazione elenco, selezionare la cartella cui si desidera visualizzare o modificare le proprietà del server.  
  
4.  Nel **< ServerFolder\ > attività** riquadro, fare clic su **visualizzare le proprietà della cartella**.  
  
5.  In **< FolderName\ > proprietà**, fare clic su **condivisione**selezionare **Nascondi la cartella dalle applicazioni accesso Web remoto e servizio Web**, quindi fare clic su **applica**.  
  
###  <a name="BKMK_Perms"></a>Impostare le autorizzazioni per le cartelle del server  
 Per eventuali cartelle del server che aggiungere al server tramite il Dashboard, è possibile scegliere tre impostazioni di accesso diversi:  
  
-   **Lettura/scrittura**  
  
     Selezionare questa impostazione se si vuole permettere all'utente di creare, modificare ed eliminare i file nella cartella del server.  
  
-   **Sola lettura**  
  
     Selezionare questa impostazione se si vuole permettere all'utente solo di leggere i file nella cartella del server. Gli utenti con accesso in sola lettura non possono creare, modificare o eliminare i file nella cartella del server.  
  
-   **Nessun accesso**  
  
     Selezionare questa impostazione se non si desidera che questa persona acceda ai file disponibili nella cartella del server.  
  
> [!IMPORTANT]
>  Le autorizzazioni che vengono visualizzate nelle proprietà della cartella rappresentano solo gli utenti che sono gestiti dal Dashboard. Non includono le autorizzazioni utente, ad esempio gruppi o account del servizio, o eventuali autorizzazioni configurate per la cartella tramite altri strumenti nativi oppure gli utenti che non sono stati aggiunti tramite il Dashboard.  
  
> [!NOTE]
>  È necessario essere un amministratore di rete per eseguire questa procedura.  
  
##### <a name="to-set-permissions-to-server-folders-on-the-server"></a>Per impostare le autorizzazioni per le cartelle del server nel server di  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Fare clic su **archiviazione**, quindi fare clic su **cartelle Server**.  
  
3.  Nella visualizzazione elenco, selezionare la cartella cui si desidera visualizzare o modificare le proprietà del server.  
  
4.  Nel **< ServerFolder\ > attività** riquadro, fare clic su **visualizzare le proprietà della cartella**.  
  
5.  In **< FolderName\ > proprietà**, fare clic su **condivisione**, selezionare il livello di accesso utente appropriato per gli account utente elencati e quindi fare clic su **applica**.  
  
> [!NOTE]
>  Per impostazione predefinita, quando si aggiunge un account utente alla rete, viene creata una sottocartella per l'utente per il **utenti** cartella sul server. La sottocartella sono accessibili da un computer di rete da solo l'utente o dell'amministratore. Le autorizzazioni sono impostate per ogni sottocartella **utenti**, quindi nessuna autorizzazione di accesso generale per il primo livello **utenti** cartella.  
  
> [!NOTE]
>  È possibile modificare le autorizzazioni di condivisione per il **backup cronologia File**, **il reindirizzamento delle cartelle**, e **utenti** le cartelle del server. Di conseguenza, le proprietà della cartella di queste cartelle del server non includono un **condivisione** scheda.  
  
###  <a name="BKMK_10"></a>Visualizzare o modificare le proprietà delle cartelle server  
 È possibile modificare il nome di cartella del server, la descrizione e definire gli account utente che hanno accesso a una cartella del server tramite il **visualizzare le proprietà della cartella** attività sul **cartelle Server** scheda del Dashboard.  
  
> [!NOTE]
>  In Windows Server Essentials e Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials installato, è inoltre possibile modificare quota della cartella.  
  
##### <a name="to-view-or-modify-folder-properties"></a>Per visualizzare o modificare le proprietà delle cartelle  
  
1.  Aprire il Dashboard di Windows Server Essentials.  
  
2.  Fare clic su **archiviazione**, quindi fare clic su **cartelle Server**.  
  
3.  Nella visualizzazione elenco, selezionare la cartella cui si desidera visualizzare o modificare le proprietà del server.  
  
4.  Nel **< ServerFolder\ > attività** riquadro, fare clic su **visualizzare le proprietà della cartella**.  
  
5.  In **< Foldername\ > proprietà**via il **generale** , visualizzare o modificare il nome e descrizione della cartella del server.  
  
    > [!NOTE]
    >  In Windows Server Essentials e Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials installato, è inoltre possibile modificare quote delle cartelle che generano un messaggio di avviso quando una cartella del server raggiunge la dimensione specificata.  
  
##  <a name="BKMK_5"></a>Aggiungere o spostare una cartella del server  
 È possibile **aggiungere altre cartelle del server** per archiviare i file sul server, oltre alle cartelle server predefinite che vengono create durante l'installazione. È possibile aggiungere le cartelle del server nel server primario o un server membro che esegue Windows Server Essentials.  
  
 È possibile **spostare una cartella del server** che si trova sul server primario che esegue Windows Server Essentials e viene visualizzato il **cartelle Server** scheda del Dashboard in un'altra unità disco rigido quando necessario tramite lo spostamento di una procedura guidata cartella. È possibile spostare una cartella del server a un altro indirizzo percorso disco rigido se:  
  
-   L'unità disco rigido di dati non ha spazio sufficiente per archiviare i dati.  
  
-   Si desidera modificare il percorso di archiviazione predefinito. Per uno spostamento più rapido, è consigliabile spostare la cartella del server, mentre non include tutti i dati.  
  
-   Si desidera rimuovere il disco rigido esistente senza perdere le cartelle del server che si trovano su di esso.  
  
 Prima di spostare la cartella, verificare quanto segue:  
  
-   Assicurarsi che è stato eseguito il backup del server.  
  
-   Assicurarsi che tutti i backup dei client vengono arrestati e non in corso se si prevede di spostare la cartella Backup Computer Client. Mentre si sposta la cartella Backup Computer Client, il server sarà Impossibile eseguire il backup di tutti i computer client fino a quando non lo spostamento della cartella viene completato.  
  
-   Assicurarsi che il server non esegue operazioni critiche del sistema. È consigliabile che completare eventuali aggiornamenti o spostare i backup che sono in corso prima di avviare una cartella o il processo può richiedere più tempo per completare.  
  
-   Nessuno dei file nella cartella da spostare sono in uso. Non sarà possibile accedere alla cartella server durante lo spostamento.  
  
 Lo spostamento di una cartella da NTFS a ReFS non è supportato se i file nelle cartelle del server implementano le tecnologie seguenti:  
  
-   Flussi di dati alternativi  
  
-   ID oggetto  
  
-   Nomi brevi (nomi in formato 8.3)  
  
-   Compressione  
  
-   Crittografia EFS  
  
-   NTFS transazionale, TxF (introdotto con Windows Vista)  
  
-   File sparse  
  
-   Collegamenti reali  
  
-   Attributi estesi  
  
-   Quote  
  
###  <a name="BKMK_6"></a>Dove aggiungere o spostare una cartella del server  
 In genere, si deve aggiungere o spostare le cartelle del server nelle unità disco rigida che la quantità massima di spazio libero. Se possibile, evitare di aggiungere o spostare una cartella condivisa nell'unità di sistema (ad esempio c:), in quanto potrebbe richiedere immediatamente lo spazio sull'unità necessario è necessario per il sistema operativo e gli aggiornamenti. Inoltre, evitare di aggiungere o spostare le cartelle del server in un'unità disco rigida esterna, perché può essere disconnesso facilmente e di conseguenza, potrebbe non essere in grado di accedere ai file. Al contrario, si consiglia di creare la cartella in un'unità interna.  
  
 Una cartella del server non è possibile aggiungere o spostare ai percorsi seguenti se nessuna di queste posizioni è selezionata per aggiunte o spostamenti, sarà generato un errore:  
  
-   Un disco rigido non formattata con il file system NTFS o ReFS  
  
-   La cartella % windir %  
  
-   Un'unità di rete  
  
-   Una cartella che contiene una cartella condivisa  
  
-   Un disco rigido che si trova in **dispositivi con archivi rimovibili**  
  
-   Una directory radice di un'unità disco rigida (ad esempio C:\\, d:\\., E:\\)  
  
-   Una sottocartella di una cartella condivisa esistente  
  
-   Un server membro che esegue Windows Server Essentials o Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials installato  
  
### <a name="steps-to-add-or-move-a-server-folder"></a>Passaggi per aggiungere o spostare una cartella del server  
  
> [!NOTE]
>  È necessario essere un amministratore del server per completare queste procedure.  
  
##### <a name="to-add-a-server-folder"></a>Per aggiungere una cartella del server  
  
1.  Aprire il Dashboard.  
  
2.  Fare clic su **archiviazione**, quindi fare clic su **cartelle Server**.  
  
3.  In **attività cartelle Server**, fare clic su **aggiungere una cartella**. Verrà avviata l'aggiunta guidata cartella.  
  
4.  Seguire le istruzioni per completare la procedura guidata.  
  
    > [!NOTE]
    >  -   Se si seleziona una cartella specifica tramite il pulsante Sfoglia per specificare il percorso della cartella server, la cartella selezionata viene aggiunto come una cartella del server.  
    > -   È possibile definire quali cartelle del server saranno accessibili tramite accesso Web remoto. Per ulteriori informazioni, vedere [gestire l'accesso alle cartelle del server](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_1).  
  
##### <a name="to-move-a-server-folder"></a>Per spostare una cartella del server  
  
1.  Aprire il Dashboard.  
  
2.  Fare clic su **archiviazione**, quindi fare clic su **cartelle Server**.  
  
3.  Nell'elenco di cartelle del server, selezionare la cartella che si desidera spostare.  
  
4.  Nel riquadro attività fare clic su **spostare la cartella**.  
  
5.  Seguire le istruzioni per completare la procedura guidata.  
  
##  <a name="BKMK_9"></a>Aggiungere una cartella del server mancante  
 Se il server rileva che una cartella predefinita del server? Società, utenti, backup Computer Client, Backup cronologia File o reindirizzamento cartelle? non è più condivisa (per qualche motivo o altro), viene generato un avviso per fornire all'utente per risolvere questo problema. È consigliabile provare a ripristinare la cartella dal backup del server. Tuttavia, se il server non è stato eseguito il, selezionare la cartella mancante e quindi fare clic su **ricrea la cartella mancante** per riconfigurare il percorso della cartella del server.  
  
> [!NOTE]
>  Solo le cartelle predefinite? Società, utenti, backup Computer Client, Backup cronologia File o reindirizzamento cartelle? possono essere ricreati. Non è possibile ricreare server creati dall'utente e le cartelle multimediali server.  
  
 Dopo il ripristino o ricrea la cartella mancante, non è più dovrebbe essere elencato come **mancante**.  
  
 Per informazioni sul ripristino dei file dai backup del server, vedere la sezione informazioni più informazioni sul ripristino di file e cartelle nell'argomento [gestire Backup e ripristino](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md).  
  
##  <a name="BKMK_11"></a>Informazioni sulle cartelle condivise  
 Esistono molti modi diversi, è possibile accedere alle cartelle condivise in Windows Server Essentials da un dispositivo è connesso al server. Per ulteriori informazioni, vedere l'argomento [usare le cartelle condivise](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md).  
  
##  <a name="BKMK_Shadow"></a>Informazioni sulle copie shadow  
 Con server copie Shadow, gli utenti possono visualizzare file e cartelle condivisi come si presentavano in determinati momenti nel passato. Accesso alle versioni precedenti dei file, o copie shadow, è utile perché gli utenti possono:  
  
1.  **Ripristinare file eliminati accidentalmente**. Se si elimina accidentalmente un file, puoi aprire una versione precedente e copiarla in una posizione sicura.  
  
2.  **Ripristino da un file sovrascritto accidentalmente**. Se si sovrascrive accidentalmente un file, è possibile ripristinare una versione precedente del file. (Il numero delle versioni dipende dal numero di snapshot creati.)  
  
3.  **Confrontare diverse versioni di un file durante l'utilizzo**. Quando si desidera verificare le differenze tra le versioni di un file, è possibile utilizzare le versioni precedenti.  
  
 Per usare le copie Shadow, da un computer client, fare doppio clic su una cartella condivisa del server e selezionare **Ripristina versione precedente**.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire l'archiviazione Server](Manage-Server-Storage-in-Windows-Server-Essentials.md)  
  
-   [Usare le cartelle condivise](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)
