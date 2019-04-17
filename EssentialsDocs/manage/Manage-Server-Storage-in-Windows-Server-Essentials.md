---
title: Gestire l'archiviazione Server in Windows Server Essentials
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1836682e-c7bb-4dd5-a2b5-6ff032693574
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e0f65dfd25afbd584764d33904ba82e4da4c5443
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="manage-server-storage-in-windows-server-essentials"></a>Gestire l'archiviazione Server in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
   
 Windows Server Essentials consente di gestire tutta l'archiviazione server (inclusi i dischi rigidi e spazi di archiviazione) dal **unità disco rigido** pagine di **archiviazione** scheda del Dashboard.  
  
 Le sezioni seguenti forniscono informazioni che consentono di aumentare le risorse di archiviazione di server, comprendere e usare spazi di archiviazione e gestire i dischi rigidi:  
  
-   [Gestire i dischi rigidi tramite il Dashboard](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Aumentare spazio di archiviazione nel server di](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Eseguire controlli e operazioni di ripristino sulle unità disco rigido](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_Check)  
  
-   [Dischi rigidi in formato](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Aggiungere un nuovo disco rigido](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Panoramica di spazi di archiviazione](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Creare uno spazio di archiviazione](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_6)  
  
##  <a name="BKMK_1"></a>Gestire i dischi rigidi tramite il Dashboard  
 Windows Server Essentials consente di gestire tutti i dischi rigidi connessi al server tramite il Dashboard. Nel Dashboard **archiviazione** scheda **unità disco rigido** Visualizza tutti i dischi rigidi disponibili nel server per archiviare i backup di dati e server. Il server monitora lo spazio disponibile in ogni disco rigido e visualizza un avviso se diventa insufficiente spazio su disco rigido. Il **i dischi rigidi** scheda Visualizza le informazioni seguenti:  
  
-   Il nome di ogni disco rigido  
  
-   La capacità di ogni disco rigido  
  
-   La quantità di spazio utilizzato in ogni disco rigido  
  
-   La quantità di spazio libero su ciascuna unità disco rigido  
  
-   Lo stato di ogni disco rigido; Se lo stato è vuoto significa che il disco rigido funzioni correttamente  
  
-   Riquadro dei dettagli, che visualizza tutte le informazioni dello stack di archiviazione (per pool di archiviazione, spazio di archiviazione e disco rigido) se il disco rigido selezionato si trova in uno spazio di archiviazione (invece di un disco fisico)  
  
 Nella tabella seguente sono elencate le attività di gestione di unità disco rigido che sono disponibili nel Dashboard e le relative descrizioni. Alcune delle attività vengono visualizzate solo quando viene selezionato un disco rigido.  
  
### <a name="available-hard-drive-management-tasks"></a>Attività di gestione disponibile sul disco rigido  
  
|Nome attività|Descrizione|  
|---------------|-----------------|  
|**Visualizzare le proprietà dell'unità disco rigido**|Apre il *Nomediscorigido***proprietà** pagina. Questa attività viene visualizzata quando viene selezionato il disco rigido. Il **generale** scheda della finestra di *Nomediscorigido* pagina delle proprietà include le attività aggiuntive seguenti:<br /><br /> -   **Pulitura unità**: consente di eseguire la pulizia dei file non usati nel disco rigido (questa attività è disponibile solo in Windows Server Essentials).<br />-   **Controlla e Ripristina**: verifica la presenza di errori del file system e cerca di correggere gli errori rilevati automaticamente.<br /><br /> Il **copie Shadow** scheda della finestra di *Nomediscorigido***proprietà** pagina consente di abilitare le copie shadow. Questa scheda Mostra anche la volta successiva che le copie shadow è pianificata l'esecuzione.|  
|**Gestione spazi di archiviazione**|**Nota:** per Windows Server Essentials, questa attività viene visualizzata solo quando è presente uno spazio di archiviazione.<br /><br /> Apre il **spazi di archiviazione** Pannello di controllo da cui è possibile creare e gestire i pool di archiviazione e spazi di archiviazione.|  
|**Creare uno spazio di archiviazione**|Apre la creazione guidata spazio di archiviazione, che consente di usare uno o più dischi rigidi per aumentare la capacità di un pool di archiviazione.|  
|**Aumentare la capacità del pool di archiviazione**|**Nota:** questa attività è visibile solo se il disco rigido selezionato si trova in uno spazio di archiviazione.<br /><br /> Apre l'aumenta capacità di una procedura guidata Pool di archiviazione, che consente di usare uno o più dischi rigidi per aumentare la capacità di un pool di archiviazione.|  
  
##  <a name="BKMK_2"></a>Aumentare spazio di archiviazione nel server di  
 Per aumentare lo spazio di archiviazione nel server, è possibile aggiungere un ulteriore disco rigido interno al server. Per aggiungere l'ulteriore disco rigido interno, è necessario arrestare il server, aggiungere il disco rigido interno e quindi riavviare il server. Non devi arrestare il server se il disco rigido è collegato al controller SCSI. In tal caso, il disco rigido può essere collegato mentre il server è in esecuzione.  
  
 A seconda del fatto che il disco rigido da aggiungere è formattato, eseguire una delle operazioni seguenti:  
  
-   **Formattato** se il disco rigido interno è formattato con NTFS o ReFS, il server assegna una lettera di unità e il disco rigido è presente il **i dischi rigidi** scheda. È ora possibile creare o spostare le cartelle del server per il nuovo disco rigido.  
  
-   **Non è formattato** se il disco rigido interno è formattato, viene visualizzato l'avviso seguente: uno o più unità disco rigida non formattate sono connessi al server. Utilizzare la procedura seguente per formattare il disco rigido.  
  
#### <a name="to-format-the-hard-disk"></a>Per formattare il disco rigido  
  
1.  Aprire il Dashboard.  
  
2.  Nel riquadro di spostamento, fare clic sull'icona di avviso per avviare il **Visualizzatore avvisi** in Windows Server Essentials, o **il monitoraggio dello stato** scheda in Windows Server Essentials.  
  
3.  Nel **Visualizzatore avvisi** o **il monitoraggio dello stato** scheda, fare clic sull'avviso e quindi nel riquadro attività fare clic su **risolvere questo problema**.  
  
4.  Seguire le istruzioni per completare l'aggiunta guidata unità disco rigido nuovo.  
  
###  <a name="BKMK_Clean"></a>Per pulire l'unità disco rigido  
  
1.  Aprire il Dashboard.  
  
2.  Nel riquadro di spostamento, fare clic su **archiviazione**, quindi fare clic su **i dischi rigidi**.  
  
3.  Nel **unità disco rigido** , selezionare la lettera di unità assegnata al disco rigido appena aggiunto e nel riquadro attività fare clic su **visualizzare le proprietà dell'unità disco rigido**.  
  
4.  In **< Driveletter\ > proprietà**via il **generale** scheda, fare clic su **pulitura unità**.  
  
##  <a name="BKMK_Check"></a>Eseguire controlli e operazioni di ripristino sulle unità disco rigido  
 Le unità disco rigido controllare e riparare processo di verifica l'integrità del file system archiviato nei dischi rigidi. Viene eseguito un **chkdsk** processo nel volume che in cui sono archiviati i file di backup. Il problema segnalato nell'avviso seguente può essere risolto eseguendo un controllo e ripristino sui dischi rigidi:  
  
-   Uno o più dischi rigidi nel Backup del Server devono essere selezionate.  
  
#### <a name="to-check-and-repair-hard-drives"></a>Per controllare e ripristinare i dischi rigidi  
  
1.  Aprire il Dashboard.  
  
2.  Fare clic su **cartelle Server e i dischi rigidi**, quindi fare clic su **i dischi rigidi**.  
  
3.  Selezionare il disco rigido in cui viene visualizzato l'errore e quindi seleziona **visualizzare le proprietà dell'unità disco rigido**.  
  
4.  Nel **controlla e Ripristina** scheda, fare clic su **controlla e Ripristina**.  
  
##  <a name="BKMK_3"></a>Dischi rigidi in formato  
 Quando viene rilevato un disco rigido interno non formattato nel server, un avviso di integrità Guida l'utente attraverso il processo di formattazione. L'aggiunta guidata nuovo disco rigido unità illustra la formattazione del disco rigido e consente di configurare l'unità disco rigido in uno dei modi seguenti:  
  
1.  Formattare il disco rigido e creare automaticamente un'unità su di esso. Se si sceglie questa opzione, quando viene completata la procedura guidata, viene creato un disco rigido logico formattato con il file system NTFS.  
  
2.  Formattare il disco rigido e configurarlo per il backup di server. Se si sceglie questa opzione, viene avviata la configurazione Server Backup guidata e illustra la configurazione di backup del server.  
  
3.  Se esiste un t lo spazio di archiviazione, utilizzare il nuovo disco rigido per creare uno spazio di archiviazione. È necessario disporre di almeno due unità disco rigido per creare uno spazio di archiviazione.  
  
4.  Se esiste già uno spazio di archiviazione, utilizzare il nuovo disco rigido per aumentare la capacità di un pool di archiviazione. Questa opzione viene visualizzata solo se è presente uno spazio di archiviazione creato nel server. Se si sceglie questa opzione, la procedura guidata aggiungerà il disco rigido al pool di archiviazione.  
  
##  <a name="BKMK_4"></a>Aggiungere un nuovo disco rigido  
 Quando si collega una nuova unità disco rigido in un server che esegue Windows Server Essentials, è possibile:  
  
-   [Utilizzare il nuovo disco rigido per archiviare le cartelle del server](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4a)  
  
-   [Utilizzare il nuovo disco rigido per archiviare i backup del server](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4b)  
  
-   [Utilizzare il nuovo disco rigido per aumentare la capacità del pool di archiviazione](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4c)  
  
###  <a name="BKMK_4a"></a>Utilizzare il nuovo disco rigido per archiviare le cartelle del server  
 Per usare il nuovo disco rigido per archiviare le cartelle del server, è possibile aggiungere una nuova cartella del server per l'unità disco rigido o spostare una cartella esistente di server nell'unità disco rigido.  
  
##### <a name="to-store-server-folders"></a>Per archiviare le cartelle server  
  
1.  Aprire il Dashboard.  
  
2.  Fare clic su di **archiviazione** scheda e quindi fare clic su **cartelle Server**.  
  
3.  Nel **attività cartelle Server** riquadro, effettuare una delle operazioni seguenti:  
  
    1.  Per aggiungere una cartella del server, fare clic su **aggiungere una cartella**.  
  
    2.  Per spostare una cartella del server, selezionare la cartella che si desidera spostare il nuovo disco rigido e quindi fare clic su **spostare una cartella**.  
  
    > [!NOTE]
    >  Se si seleziona come percorso di cartella del server senza creare una cartella passare all'unità disco rigido, viene visualizzato il messaggio di errore seguente: **una directory radice (ad esempio C:\\ d:\\.) non può essere aggiunto come una cartella del server. Creare una nuova cartella o selezionarne uno esistente nella directory radice e quindi riprovare**. Per risolvere questo errore, creare una nuova cartella nel disco rigido appena aggiunto e quindi selezionare la nuova cartella come percorso in cui archiviare le cartelle del server.  
  
4.  Seguire le istruzioni per completare la procedura guidata.  
  
 Per ulteriori informazioni sullo spostamento delle cartelle server, vedere [aggiungere o spostare una cartella del server](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5).  
  
###  <a name="BKMK_4b"></a>Utilizzare il nuovo disco rigido per archiviare i backup del server  
 È possibile utilizzare il nuovo disco rigido aggiunto per archiviare i backup del server.  
  
##### <a name="to-store-server-backups"></a>Per archiviare i backup del server  
  
1.  Aprire il Dashboard.  
  
2.  Fare clic su di **dispositivi** scheda, selezionare il server nel riquadro elenco e quindi dal riquadro attività eseguire una delle operazioni seguenti:  
  
    1.  Se non è configurato nel server di Backup del Server, fare clic su **Configura Backup Server**.  
  
    2.  Se il server è configurato il Backup di Server, fare clic su **Personalizza Backup Server**.  
  
     Verrà visualizzata la configurazione guidata Backup Server.  
  
3.  Nel **selezionare la destinazione di Backup** pagina, selezionare il nuovo disco rigido come destinazione del backup.  
  
4.  Seguire le istruzioni per completare la procedura guidata.  
  
###  <a name="BKMK_4c"></a>Utilizzare il nuovo disco rigido per aumentare la capacità del pool di archiviazione  
 Quando la capacità del pool di archiviazione è quasi scarica, viene visualizzato un avviso che informa che è possibile aumentare la capacità del pool di archiviazione aggiungendo un nuovo disco rigido al pool di archiviazione mediante l'aumenta capacità di una procedura guidata Pool di archiviazione.  
  
> [!NOTE]
>  È possibile eseguire questa procedura solo se hai creato un pool di archiviazione nel server.  
  
##### <a name="to-increase-storage-pool-capacity"></a>Per aumentare la capacità del pool di archiviazione  
  
1.  Aprire il Dashboard.  
  
2.  Fare clic su di **archiviazione** scheda e quindi fare clic su **i dischi rigidi**.  
  
3.  Selezionare l'unità da cui viene visualizzata una bassa capacità.  
  
4.  Dal riquadro attività selezionare **aumenta la capacità del pool di archiviazione**. Verrà visualizzata l'aumenta capacità di una procedura guidata Pool di archiviazione.  
  
5.  Seguire le istruzioni per completare la procedura guidata.  
  
##  <a name="BKMK_5"></a>Panoramica di spazi di archiviazione  
 Spazi di archiviazione consente raggruppare i dischi in un pool di archiviazione. È quindi possibile utilizzare capacità del pool per creare spazi di archiviazione. Spazi di archiviazione sono unità virtuali visualizzate nel **unità disco rigido** scheda del Dashboard. È possibile utilizzare spazi di archiviazione come qualsiasi altra unità, pertanto, è facile da utilizzare i file su di essi. Quando si esegue bassa capacità del pool, è possibile creare spazi di archiviazione di grandi dimensioni e aggiungere altre unità al pool di archiviazione. Se si dispone di due o più dischi nel pool di archiviazione, è possibile creare spazi di archiviazione con mirroring a due vie che non saranno interessati da un errore di unità? o anche l'errore di due unità? se si crea uno spazio di archiviazione con mirroring a tre vie.  
  
 Per creare uno spazio di archiviazione, è sufficiente una o più unità aggiuntive inoltre nell'unità in cui è installato Windows. Queste unità possono essere dischi rigidi interni o esterni, o unità SSD. È possibile utilizzare un'ampia gamma di tipi di unità con spazi di archiviazione, incluse le unità USB, SATA e SAS.  
  
> [!NOTE]
>  Se si configura gli spazi di archiviazione in un server che esegue Windows Server Essentials, è possibile eseguire un ripristino delle impostazioni predefinite con il **Pulisci dati** opzione. La soluzione per questo problema consiste nel rimuovere innanzitutto gli spazi di archiviazione e quindi eseguire un ripristino delle impostazioni predefinite con il **Pulisci dati** opzione.  
  
 Per ulteriori informazioni su spazi di archiviazione, vedere [archiviazione spazi spesso domande frequenti](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx).  
  
##  <a name="BKMK_6"></a>Creare uno spazio di archiviazione  
 Per iniziare a usare con spazi di archiviazione nel server, è necessario soddisfare i requisiti minimi seguenti:  
  
-   Il server che esegue Windows Server Essentials deve essere collegato a unità fisiche aggiuntive (non solo l'unità di avvio), le unità non devono ospitare volumi e devono avere una capacità minima di 10 GB. Per creare un pool di archiviazione, è necessaria un'unità fisica Per creare uno spazio di archiviazione con mirroring resiliente, è necessario un minimo di due unità fisiche.  
  
-   Per creare uno spazio di archiviazione con resilienza attraverso la parità o mirroring a 2 vie, sono necessari almeno due unità fisiche.  
  
> [!NOTE]
>  Se si configura gli spazi di archiviazione in un server che esegue Windows Server Essentials, è possibile eseguire un ripristino delle impostazioni predefinite con il **Pulisci dati** opzione. La soluzione per questo problema consiste nel rimuovere innanzitutto gli spazi di archiviazione e quindi eseguire un ripristino delle impostazioni predefinite con il **Pulisci dati** opzione.  
  
#### <a name="to-create-a-storage-space-in-windows-server-essentials"></a>Per creare uno spazio di archiviazione in Windows Server Essentials  
  
1.  Aggiungere o connettere tutte le unità da raggruppare con spazi di archiviazione nel server che esegue Windows Server Essentials.  
  
2.  Dal Dashboard, fare clic su **avanzate: gestione spazi di archiviazione**.  
  
3.  Fare clic su **creare un nuovo pool e spazio di archiviazione**.  
  
4.  Selezionare le unità che si desidera aggiungere al nuovo spazio di archiviazione, quindi fare clic su **creare pool**.  
  
5.  Assegnare l'unità di un nome e una lettera e quindi scegli un layout della schermata. **Mirroring a 2 vie**, **mirroring a tre vie**, e **parità** per proteggere i file nello spazio di archiviazione da errori delle unità.  
  
6.  Immettere la dimensione massima possibile raggiungere lo spazio di archiviazione e quindi fare clic su **creare spazio di archiviazione**.  
  
 In Windows Server Essentials, è possibile creare uno spazio di archiviazione bidirezionale con mirroring utilizzando la creazione guidata di spazio di archiviazione dal Dashboard.  
  
#### <a name="to-create-a-storage-space-in-windows-server-essentials"></a>Per creare uno spazio di archiviazione in Windows Server Essentials  
  
1.  Aggiungere o connettere tutte le unità da raggruppare con spazi di archiviazione nel server che esegue Windows Server Essentials.  
  
2.  Dal Dashboard, fare clic su **gestire spazi di archiviazione**. La creazione guidata di spazio di archiviazione viene visualizzato.  
  
3.  Seguire le istruzioni per completare la procedura guidata.  
  
 Per informazioni su come aumentare la capacità del pool di archiviazione, vedere [utilizzare il nuovo disco rigido per aumentare la capacità del pool di archiviazione](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4c).  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire le cartelle Server](Manage-Server-Folders-in-Windows-Server-Essentials.md)  
  
-   [Usare Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)
