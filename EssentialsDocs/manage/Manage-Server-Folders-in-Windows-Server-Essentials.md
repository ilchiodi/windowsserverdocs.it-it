---
title: Gestire le cartelle server in Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
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
ms.openlocfilehash: 0c46d19f4f172786e0ffe7f3b9dd7ac1d8f4fcf0
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/13/2020
ms.locfileid: "79322233"
---
# <a name="manage-server-folders-in-windows-server-essentials"></a>Gestire le cartelle server in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
  
 In qualità di amministratore del server, è possibile gestire l'accesso a qualsiasi cartella del server (nota come cartella condivisa quando si accede dalla finestra di avvio, dall'Accesso Web remota, dall'app My Server per Windows Phone o dall'app My Server per Windows 8) nel server usando le attività nella scheda **cartelle server** del dashboard, consentendo agli utenti di variare i livelli di accesso a un'ampia gamma di file.  
  
 Gli argomenti seguenti includono informazioni che permettono di comprendere, creare e gestire le cartelle del server:  
  
-   [Gestire le cartelle del server tramite il dashboard](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Gestire l'accesso alle cartelle del server](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Aggiungere o spostare una cartella del server](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Aggiungere una cartella del server mancante](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Informazioni sulle cartelle condivise](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Informazioni sulle copie shadow](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Shadow)  
  
##  <a name="BKMK_2"></a>Gestire le cartelle del server tramite il dashboard  
 Windows Server Essentials permette di eseguire le attività amministrative comuni tramite il dashboard. La pagina **Cartelle server** del dashboard offre le informazioni seguenti:  
  
- Un elenco di cartelle del server, che include:  
  
  -   Il nome della cartella.  
  
  -   Una descrizione della cartella.  
  
  -   La posizione della cartella.  
  
  -   La quantità di spazio disponibile presente nel percorso della cartella.  
  
  -   Brevi informazioni sullo stato per ogni attività in esecuzione nella cartella. Il campo **Stato** sarà vuoto se la cartella è integra e se non sono in esecuzione attività.  
  
- Un riquadro dei dettagli che offre informazioni aggiuntive su una cartella selezionata.  
  
- Un riquadro attività che include un insieme di attività amministrative correlate alle cartelle.  
  
  La tabella seguente illustra le diverse attività relative alle cartelle del server disponibili nel Dashboard di Windows Server Essentials. La maggior parte delle attività è specifica per la cartella e le attività saranno visualizzate solo quando si seleziona una cartella nell'elenco.  
  
### <a name="server-folder-tasks-on-the-dashboard"></a>Attività relative alle cartelle del server nel dashboard  
  
|Nome dell'attività|Descrizione|  
|---------------|-----------------|  
|Apri la cartella|Mostra i contenuti della cartella selezionata in Esplora file (denominato Esplora risorse nelle versioni precedenti di Windows).|  
|Elimina la cartella|Permette di eliminare una cartella creata dagli utenti. Questa attività non è disponibile per le cartelle predefinite create dall'installazione del server.|  
|Sposta la cartella|Apre una procedura guidata che semplifica lo spostamento di una cartella del server in un nuovo percorso.|  
|Interrompi condivisione cartella|Interrompe la condivisione della cartella selezionata, ma non la elimina. Se la cartella non è più condivisa, non è più visualizzata nel dashboard. Questa attività non è disponibile per le cartelle predefinite create dall'installazione del server.|  
|Visualizza le proprietà della cartella|Visualizza le proprietà di una cartella selezionata e permette di eseguire le operazioni seguenti:<br /><br /> -Modificare il nome delle cartelle create dall'utente.<br /><br /> -Modificare la descrizione di una cartella selezionata.<br /><br /> -Visualizzare le dimensioni della cartella.<br /><br /> -Aprire la cartella selezionata in Esplora file.<br /><br /> -Specificare le autorizzazioni di accesso all'account utente per una cartella selezionata.<br /><br /> -Nascondi una cartella selezionata dalle applicazioni remote Accesso Web e servizio Web.<br /><br /> -Specifica la quota della cartella.|  
|Aggiungi una cartella|Permette di creare una nuova cartella del server e di assegnare il livello di accesso consentito per ogni account utente.|  
|Informazioni sulle cartelle server|Apre un argomento della Guida su Internet che descrive l'uso e le funzionalità delle cartelle del server.|  
  
##  <a name="BKMK_1"></a>Gestire l'accesso alle cartelle del server  
 Windows Server Essentials permette di archiviare i file disponibili nei computer client in una posizione centrale tramite le cartelle del server. L'archiviazione dei file nelle cartelle del server permette di assicurare che i file si trovino in una posizione sempre accessibile in modo sicuro da ogni client.  
  
 L'uso delle cartelle del server per l'archiviazione dei file permette di ottenere i risultati seguenti:  
  
- Eseguire il backup della cartella del server tramite il backup e il ripristino del server per semplificare la protezione da un errore completo del server.  
  
- Accedere a file archiviati nella cartella del server da qualsiasi posizione tramite un browser Internet mediante Accesso Web remoto oppure tramite le app My Server per Windows Phone e Windows 8.  
  
- Accedere a una nuova cartella del server da qualsiasi computer client.  
  
  È possibile gestire l'accesso a qualsiasi cartella del server disponibile sul server tramite le attività nella scheda **Cartelle server** del dashboard. Nella tabella seguente sono elencate le cartelle del server create per impostazione predefinita quando si installa Windows Server Essentials o si attivano i flussi multimediali sul server.  
  
|Nome della cartella del server|Descrizione|  
|------------------------|-----------------|  
|Backup computer client|Per impostazione predefinita, Windows Server Essentials crea backup dei computer client, archiviati in questa cartella. Le impostazioni per Backup computer client possono essere modificate dall'amministratore di rete.|  
|Società|Permette l'archiviazione e l'accesso ai documenti correlati all'organizzazione da parte degli utenti di rete.|  
|Backup Cronologia file|Per impostazione predefinita, Windows Server Essentials usa Cronologia file per creare backup dei file archiviati in questa cartella. Queste impostazioni di Cronologia file possono essere modificate dagli amministratori di rete.|  
|Reindirizzamento cartelle|Permette l'archiviazione e l'accesso a cartelle configurate per il reindirizzamento da parte degli utenti di rete.|  
|Utenti|Permette l'archiviazione e l'accesso ai file da parte degli utenti di rete. Una cartella specifica per l'utente è generata automaticamente nella cartella del server **Utenti** per ogni account utente di rete creato.|  
|Musica|Permette l'archiviazione e l'accesso a file musicali da parte degli utenti di rete. Questa cartella è disponibile quando si attiva la condivisione dei file multimediali.|  
|Immagini|Permette l'archiviazione e l'accesso a file di immagine da parte degli utenti di rete. Questa cartella è disponibile quando si attiva la condivisione dei file multimediali.|  
|Registrazioni|Permette l'archiviazione e l'accesso ai programmi televisivi registrati da parte degli utenti di rete. Questa cartella è disponibile quando si attiva la condivisione dei file multimediali.|  
|Videos|Permette l'archiviazione e l'accesso a file video da parte degli utenti di rete. Questa cartella è disponibile quando si attiva la condivisione dei file multimediali.|  
  
 Per nascondere o impostare autorizzazioni per le cartelle del server o per modificare le proprietà delle cartelle del server, vedere le procedure seguenti:  
  
-   [Nascondi cartelle server](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Hide)  
  
-   [Impostare le autorizzazioni per le cartelle del server](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Perms)  
  
-   [Visualizzare o modificare le proprietà delle cartelle del server](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_10)  
  
###  <a name="BKMK_Hide"></a>Nascondi cartelle server  
 Gli amministratori di rete possono scegliere di nascondere le cartelle del server e impedirne la visualizzazione nel sito Web Accesso Web remoto o nelle applicazioni Servizi Web, ad esempio My Server.  
  
> [!NOTE]
>  Per eseguire questa procedura, è necessario essere un amministratore di rete.  
  
##### <a name="to-hide-server-folders-from-being-displayed-in-remote-web-access"></a>Per nascondere le cartelle del server dalla visualizzazione in Accesso Web remoto  
  
1.  Aprire il dashboard di Windows Server Essentials.  
  
2.  Fare clic su **ARCHIVIAZIONE**, quindi su **Cartelle server**.  
  
3.  Nella visualizzazione elenco selezionare la cartella del server di cui si vogliono visualizzare o modificare le proprietà.  
  
4.  Nel riquadro **attività < cartellaserver\>** fare clic su **Visualizza proprietà cartella**.  
  
5.  In **< foldername\> proprietà**fare clic su **condivisione**, selezionare **Nascondi la cartella dalle applicazioni remote accesso Web e servizio Web**, quindi fare clic su **applica**.  
  
###  <a name="BKMK_Perms"></a>Impostare le autorizzazioni per le cartelle del server  
 È possibile scegliere tre impostazioni di accesso diverse per eventuali cartelle del server aggiunte al server tramite il dashboard:  
  
-   **Lettura/scrittura**  
  
     Selezionare questa impostazione se si vuole permettere all'utente di creare, modificare ed eliminare eventuali file disponibili nella cartella del server.  
  
-   **Sola lettura**  
  
     Selezionare questa impostazione se si vuole permettere all'utente solo di leggere i file disponibili nella cartella del server. Gli utenti con accesso di sola lettura non possono creare, modificare o eliminare eventuali file disponibili nella cartella del server.  
  
-   **Nessun accesso**  
  
     Selezionare questa impostazione se non si vuole che l'utente acceda ai file disponibili nella cartella del server.  
  
> [!IMPORTANT]
>  Le autorizzazioni visualizzate nelle proprietà della cartella rappresentano solo gli utenti gestiti dal dashboard. Non includono le autorizzazioni utente, ad esempio gruppi o account del servizio, eventuali autorizzazioni configurate per la cartella tramite altri strumenti nativi oppure gli utenti non aggiunti tramite il dashboard.  
  
> [!NOTE]
>  Per eseguire questa procedura, è necessario essere un amministratore di rete.  
  
##### <a name="to-set-permissions-to-server-folders-on-the-server"></a>Per impostare le autorizzazioni per le cartelle del server nel server  
  
1.  Aprire il dashboard di Windows Server Essentials.  
  
2.  Fare clic su **ARCHIVIAZIONE**, quindi su **Cartelle server**.  
  
3.  Nella visualizzazione elenco selezionare la cartella del server di cui si vogliono visualizzare o modificare le proprietà.  
  
4.  Nel riquadro **attività < cartellaserver\>** fare clic su **Visualizza proprietà cartella**.  
  
5.  In **< foldername\> proprietà**fare clic su **condivisione**e selezionare il livello di accesso utente appropriato per gli account utente elencati, quindi fare clic su **applica**.  
  
> [!NOTE]
>  Per impostazione predefinita, quando si aggiunge un account utente alla rete, nella cartella **Utenti** sul server sarà creata una sottocartella per l'utente. Solo l'utente o l'amministratore potranno accedere alla sottocartella da un computer di rete. Le autorizzazioni sono configurate per ogni sottocartella disponibile in **Utenti**. Non sono quindi disponibili autorizzazioni di accesso generali per la cartella **Utenti** di primo livello.  
  
> [!NOTE]
>  Non è possibile modificare le autorizzazioni di condivisione per le cartelle del server **Backup Cronologia file**, **Reindirizzamento cartelle** e **Utenti**. Le proprietà di queste cartelle del server non includono quindi la scheda **Condivisione**.  
  
###  <a name="BKMK_10"></a>Visualizzare o modificare le proprietà delle cartelle del server  
 È possibile modificare il nome e la descrizione delle cartelle del server e definire gli account utente autorizzati ad accedere a una cartella del server tramite l'attività **Visualizza le proprietà della cartella** nella scheda **Cartelle server** del dashboard.  
  
> [!NOTE]
>  In Windows Server Essentials e Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials installato è anche possibile modificare la quota della cartella.  
  
##### <a name="to-view-or-modify-folder-properties"></a>Per visualizzare o modificare le proprietà delle cartelle  
  
1.  Aprire il dashboard di Windows Server Essentials.  
  
2.  Fare clic su **ARCHIVIAZIONE**, quindi su **Cartelle server**.  
  
3.  Nella visualizzazione elenco selezionare la cartella del server di cui si vogliono visualizzare o modificare le proprietà.  
  
4.  Nel riquadro **attività < cartellaserver\>** fare clic su **Visualizza proprietà cartella**.  
  
5.  In **< foldername\> proprietà**, nella scheda **generale** , visualizzare o modificare il nome e la descrizione della cartella del server.  
  
    > [!NOTE]
    >  In Windows Server Essentials e Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials installato è anche possibile modificare la quota della cartella che genera un messaggio di avviso quando una cartella del server raggiunge le dimensioni specificate.  
  
##  <a name="BKMK_5"></a>Aggiungere o spostare una cartella del server  
 È possibile **aggiungere altre cartelle del server** per archiviare i file sul server, oltre alle cartelle del server predefinite create durante la procedura di installazione. Si possono aggiungere cartelle del server nel server primario o in un server membro che esegue Windows Server Essentials.  
  
 Se necessario, è possibile **spostare una cartella del server** disponibile nel server primario che esegue Windows Server Essentials e visualizzata nella scheda **Cartelle server** del dashboard in un altro disco rigido usando la procedura guidata Sposta una cartella. È possibile spostare una cartella del server in un altro indirizzo del percorso del disco rigido se si verificano le condizioni seguenti:  
  
- Lo spazio disponibile nel disco rigido di dati non è più sufficiente per l'archiviazione dei dati.  
  
- Si vuole modificare il percorso di archiviazione predefinito. Per uno spostamento più rapido, prendere in considerazione lo spostamento della cartella del server quando ancora non include dati.  
  
- Si vuole rimuovere il disco rigido esistente senza perdere le cartelle del server incluse.  
  
  Prima di spostare la cartella, verificare quanto segue:  
  
- Verificare che il backup del server sia stato eseguito.  
  
- Se si prevede di spostare la cartella Backup computer client, verificare che tutti i backup dei client siano stati interrotti e non siano in esecuzione. Durante lo spostamento della cartella Backup computer client, il server non sarà in grado di eseguire il backup dei computer client fino al termine dello spostamento della cartella.  
  
- Verificare che il server non stia eseguendo operazioni critiche per il sistema. È consigliabile completare eventuali aggiornamenti o backup in corso prima di avviare lo spostamento di una cartella. In caso contrario, il completamento del processo potrebbe richiedere più tempo.  
  
- Nessuno dei file della cartella da spostare deve essere in uso. Non sarà possibile accedere alla cartella del server durante lo spostamento.  
  
  Lo spostamento di una cartella da NTFS a ReFS non è supportato se i file nelle cartelle del server implementano le tecnologie seguenti:  
  
- Flussi di dati alternativi  
  
- ID oggetto  
  
- Nomi brevi (nomi in formato 8.3)  
  
- Compressione  
  
- Crittografia EFS  
  
- NTFS transazionale, TxF (introdotto con Windows Vista)  
  
- File sparse  
  
- Collegamenti reali  
  
- Attributi estesi  
  
- Quote  
  
###  <a name="BKMK_6"></a>Dove aggiungere o spostare una cartella del server  
 In genere è consigliabile aggiungere o spostare le cartelle del server nel disco rigido in cui è disponibile la quantità massima di spazio. Se possibile, evitare di aggiungere o spostare una cartella condivisa nell'unità di sistema, ad esempio C:, poiché ciò potrebbe ridurre lo spazio sull'unità necessario per il sistema operativo e i relativi aggiornamenti. Evitare anche di aggiungere o spostare le cartelle del server in un disco rigido esterno, poiché queste unità possono essere disconnesse con facilità ed è quindi possibile che non si riesca ad accedere ai file. È invece consigliabile creare la cartella in un'unità interna.  
  
 Non è possibile aggiungere o spostare una cartella del server nei percorsi seguenti. Se si seleziona uno dei percorsi seguenti per aggiunte o spostamenti, sarà generato un errore:  
  
-   Un disco rigido non formattata con il file system NTFS o ReFS  
  
-   La cartella %windir%  
  
-   Un'unità di rete mappata  
  
-   Una cartella contenente una cartella condivisa  
  
-   Un disco rigido che si trova in **Dispositivi con archivi rimovibili**  
  
-   Una directory radice di un disco rigido (ad esempio C:\\, D:\\, E:\\)  
  
-   Una sottocartella di una cartella condivisa esistente  
  
-   Un server membro che esegue Windows Server Essentials o Windows Server 2012 R2 con il ruolo esperienza Windows Server Essentials installato  
  
### <a name="steps-to-add-or-move-a-server-folder"></a>Procedure per l'aggiunta o lo spostamento di una cartella del server  
  
> [!NOTE]
>  Per completare queste procedure, è necessario essere un amministratore del server.  
  
##### <a name="to-add-a-server-folder"></a>Per aggiungere una cartella del server  
  
1. Aprire il Dashboard.  
  
2. Fare clic su **ARCHIVIAZIONE**, quindi su **Cartelle server**.  
  
3. In **Attività cartelle server** fare clic su **Aggiungi una cartella**. Sarà visualizzata la procedura guidata Aggiungi cartella.  
  
4. Seguire le istruzioni per completare la procedura guidata.  
  
   > [!NOTE]
   > - Se si cerca una cartella specifica tramite il pulsante Sfoglia per specificare il percorso della cartella del server, la cartella selezionata sarà aggiunta come cartella del server.  
   >   -   È possibile definire le cartelle del server che saranno accessibili tramite il servizio Accesso Web remoto. Per altre informazioni, vedere [Gestire l'accesso alle cartelle del server](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_1).  
  
##### <a name="to-move-a-server-folder"></a>Per spostare una cartella del server  
  
1.  Aprire il Dashboard.  
  
2.  Fare clic su **ARCHIVIAZIONE**, quindi su **Cartelle server**.  
  
3.  Nell'elenco di cartelle del server selezionare la cartella da spostare.  
  
4.  Nel riquadro attività fare clic su **Sposta la cartella**.  
  
5.  Seguire le istruzioni per completare la procedura guidata.  
  
##  <a name="BKMK_9"></a>Aggiungere una cartella del server mancante  
 Quando il server rileva la presenza di una cartella predefinita del server? Società, utenti, backup computer client, backup cronologia file o Reindirizzamento cartelle? non è più condiviso (per qualche motivo o un altro), viene generato un avviso per consentire all'utente di risolvere il problema. È consigliabile provare a ripristinare la cartella dal backup del server. Se tuttavia non è stato eseguito il backup del server, selezionare la cartella mancante e quindi fare clic su **Ricrea la cartella mancante** per riconfigurare il percorso della cartella del server.  
  
> [!NOTE]
>  Solo le cartelle predefinite? È possibile ricreare la società, gli utenti, i backup dei computer client, il backup di cronologia file o il reindirizzamento cartelle? Le cartelle del server create dagli utenti e le cartelle multimediali del server non possono essere create di nuovo.  
  
 Dopo il ripristino o la ricreazione, la cartella mancante non dovrebbe essere più elencata come **Mancante**.  
  
 Per informazioni sul ripristino dei file dai backup del server, vedere la sezione ulteriori informazioni sul ripristino di file e cartelle nell'argomento [gestire il backup e il ripristino](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md).  
  
##  <a name="BKMK_11"></a>Informazioni sulle cartelle condivise  
 È possibile accedere in molti modi diversi alle cartelle condivise in Windows Server Essentials da un dispositivo connesso al server. Per ulteriori informazioni, vedere l'argomento [utilizzo di cartelle condivise](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md).  
  
##  <a name="BKMK_Shadow"></a>Informazioni sulle copie shadow  
 Le copie shadow del server permettono agli utenti di visualizzare i file e le cartelle condivisi disponibili in determinati momenti passati. L'accesso a versioni precedenti dei file ovvero alle copie shadow è utile poiché permette agli utenti di eseguire le operazioni seguenti:  
  
1. **Ripristinare file eliminati accidentalmente**. Se un file è stato eliminato accidentalmente, è possibile aprire una versione precedente e copiarla in una posizione sicura.  
  
2. **Ripristinare un file sovrascritto accidentalmente**. Se si sovrascrive accidentalmente un file, è possibile eseguire il ripristino di una versione precedente del file (il numero delle versioni dipende da quanti snapshot sono stati creati).  
  
3. **Confrontare diverse versioni di un file durante l'utilizzo**. È possibile utilizzare le versioni precedenti quando si desidera verificare le modifiche apportate tra le diverse versioni di un file.  
  
   Per usare le copie shadow, in un computer client fare clic con il pulsante destro del mouse su una cartella condivisa del server, quindi scegliere **Ripristina versione precedente**.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestisci archiviazione server](Manage-Server-Storage-in-Windows-Server-Essentials.md)  
  
-   [Usa cartelle condivise](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)
