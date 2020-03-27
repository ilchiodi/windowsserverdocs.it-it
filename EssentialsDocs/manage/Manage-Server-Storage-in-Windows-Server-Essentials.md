---
title: Gestire l'archiviazione server in Windows Server Essentials
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1836682e-c7bb-4dd5-a2b5-6ff032693574
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c865f2b4ba21079d1b679f94e84a50665410cb2b
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311099"
---
# <a name="manage-server-storage-in-windows-server-essentials"></a>Gestire l'archiviazione server in Windows Server Essentials

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
   
 Windows Server Essentials consente di gestire tutta l'archiviazione server (inclusi dischi rigidi e spazi di archiviazione) dalle pagine **Unità disco rigido** della scheda **Archiviazione** del dashboard.  
  
 Le sezioni seguenti forniscono informazioni utili per aumentare lo spazio di archiviazione server, comprendere e usare gli spazi di archiviazione e gestire i dischi rigidi:  
  
-   [Gestire i dischi rigidi tramite il dashboard](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Aumentare lo spazio di archiviazione nel server](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Eseguire controlli e riparazioni sui dischi rigidi](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_Check)  
  
-   [Formattare i dischi rigidi](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Aggiungi un nuovo disco rigido](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Panoramica di spazi di archiviazione](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Crea uno spazio di archiviazione](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_6)  
  
##  <a name="manage-hard-drives-using-the-dashboard"></a><a name="BKMK_1"></a>Gestire i dischi rigidi tramite il dashboard  
 Windows Server Essentials consente di gestire tutti i dischi rigidi connessi al server tramite il dashboard. Sulla scheda **Archiviazione** del dashboard, in **Unità disco rigido** sono visualizzati tutti i dischi rigidi disponibili nel server per l'archiviazione dei dati e dei backup del server. Il server monitora lo spazio disponibile in ogni disco rigido e visualizza un avviso se lo spazio sta per esaurirsi. La scheda **Unità disco rigido** mostra le informazioni seguenti:  
  
- Nome di ogni disco rigido  
  
- Capacità di ogni disco rigido  
  
- Quantità di spazio usato in ogni disco rigido  
  
- Quantità di spazio disponibile in ogni disco rigido  
  
- Stato di ogni disco rigido (se lo stato è vuoto significa che il disco rigido funziona correttamente)  
  
- Riquadro dei dettagli, che mostra tutte le informazioni sullo stack di archiviazione (per pool di archiviazione, spazio di archiviazione e disco rigido) se il disco rigido selezionato si trova in uno spazio di archiviazione (e non in un disco fisico)  
  
  La tabella seguente elenca le attività di gestione dei dischi rigidi che è possibile eseguire dal dashboard, con le relative descrizioni. Alcune attività vengono visualizzate solo quando viene selezionato un disco rigido.  
  
### <a name="available-hard-drive-management-tasks"></a>Attività di gestione dei dischi rigidi disponibili  
  
|Nome attività|Descrizione|  
|---------------|-----------------|  
|**Visualizzare le proprietà del disco rigido**|Apre la pagina _NomeDiscoRigido_**Proprietà** . Questa attività viene visualizzata quando viene selezionato il disco rigido. La scheda **Generale** della pagina *NomeDiscoRigido* - Proprietà include le attività aggiuntive seguenti:<br /><br /> **pulitura unità**-   : consente di pulire i file non usati nel disco rigido. questa attività è disponibile solo in Windows Server Essentials.<br />-   **Check and Repair**: controlla se nel disco rigido sono presenti errori file System e tenta di ripristinare automaticamente gli errori rilevati.<br /><br /> La scheda **Copie shadow** della pagina _NomeDiscoRigido_**Proprietà** consente di abilitare le copie shadow. Questa scheda mostra anche la successiva esecuzione pianificata per le copie shadow.|  
|**Gestire spazi di archiviazione**|**Nota:** Per Windows Server Essentials, questa attività viene visualizzata solo quando è presente uno spazio di archiviazione esistente.<br /><br /> Apre il pannello di controllo **Spazi di archiviazione**, da cui è possibile creare e gestire pool di archiviazione e spazi di archiviazione.|  
|**Crea uno spazio di archiviazione**|Apre la procedura guidata Crea spazio di archiviazione, che consente di usare uno o più dischi rigidi per aumentare la capacità di un pool di archiviazione.|  
|**Aumentare la capacità del pool di archiviazione**|**Nota:** Questa attività è visibile solo se il disco rigido selezionato si trova in uno spazio di archiviazione.<br /><br /> Apre la procedura guidata Aumenta capacità di un pool di archiviazione, che consente di usare uno o più dischi rigidi per aumentare la capacità di un pool di archiviazione.|  
  
##  <a name="increase-storage-on-the-server"></a><a name="BKMK_2"></a>Aumentare lo spazio di archiviazione nel server  
 Per aumentare lo spazio di archiviazione nel server, è possibile aggiungere un ulteriore disco rigido interno al server. Per aggiungere l'ulteriore disco rigido interno, è necessario arrestare il server, aggiungere il disco rigido interno e quindi riavviare il server. Non è necessario arrestare il server se il disco rigido è collegato al controller SCSI. In tal caso, il disco rigido può essere collegato mentre il server è in esecuzione.  
  
 A seconda del fatto che il disco rigido da aggiungere sia o meno formattato, eseguire una delle operazioni seguenti:  
  
-   **Formattato** Se il disco rigido interno è formattato con NTFS o ReFS, il server assegna al disco una lettera di unità e il disco rigido viene visualizzato nella scheda **unità disco rigido** . È ora possibile creare o spostare le cartelle del server nel nuovo disco rigido.  
  
-   **Non formattato** Se il disco rigido interno non è formattato, viene visualizzato l'avviso seguente: Al server sono connesse una o più unità disco rigido non formattate. Usare la procedura seguente per formattare il disco rigido.  
  
#### <a name="to-format-the-hard-disk"></a>Per formattare il disco rigido  
  
1.  Aprire il Dashboard.  
  
2.  Nel riquadro di spostamento fare clic sull'icona di avviso per avviare il **Visualizzatore avvisi** in Windows Server Essentials o la scheda **monitoraggio dello stato** in Windows Server Essentials.  
  
3.  Nel **Visualizzatore avvisi** o nella scheda **Monitoraggio stato** fare clic sull'avviso e quindi nel riquadro attività fare clic su **Risolvi il problema**.  
  
4.  Seguire le istruzioni per completare la procedura guidata Aggiungi nuova unità disco rigido.  
  
###  <a name="to-clean-up-the-hard-drive"></a><a name="BKMK_Clean"></a>Per pulire il disco rigido  
  
1.  Aprire il Dashboard.  
  
2.  Nel riquadro di spostamento fare clic su **Archiviazione** e quindi su **Unità disco rigido**.  
  
3.  Nella sezione **Unità disco rigido** selezionare la lettera di unità assegnata al nuovo disco rigido aggiunto e nel riquadro attività fare clic su **Visualizza le proprietà dell'unità disco rigido**.  
  
4.  In **< proprietà\> LetteraUnità**, nella scheda **generale** , fare clic su **pulizia unità**.  
  
##  <a name="perform-checks-and-repairs-on-hard-drives"></a><a name="BKMK_Check"></a>Eseguire controlli e riparazioni sui dischi rigidi  
 Il processo di controllo e ripristino dei dischi rigidi verifica l'integrità del file system archiviato nei dischi rigidi. Viene eseguito un processo **chkdsk** nel volume in cui sono archiviati i file di backup. Il problema segnalato nell'avviso seguente può essere risolto eseguendo un processo di controllo e ripristino sui dischi rigidi:  
  
-   È necessario eseguire il controllo su una o più unità disco rigido incluse nel backup del server.  
  
#### <a name="to-check-and-repair-hard-drives"></a>Per controllare e ripristinare i dischi rigidi  
  
1.  Aprire il Dashboard.  
  
2.  Fare clic su **Cartelle server e dischi rigidi** e quindi su **Unità disco rigido**.  
  
3.  Selezionare il disco rigido per cui viene visualizzato l'errore e quindi selezionare **Visualizza le proprietà dell'unità disco rigido**.  
  
4.  Nella scheda **Controlla e ripristina** fare clic su **Controlla e ripristina**.  
  
##  <a name="format-hard-drives"></a><a name="BKMK_3"></a>Formattare i dischi rigidi  
 Quando viene rilevato un disco rigido interno non formattato nel server, un avviso di integrità guida l'utente nel processo di formattazione. La procedura guidata Aggiungi nuova unità disco rigido indica i passaggi per la formattazione del disco rigido e consente di configurare il disco rigido in uno dei modi seguenti:  
  
1.  Formattare il disco rigido e creare automaticamente un'unità nel disco. Se si sceglie questa opzione, al termine della procedura guidata viene creato un disco rigido logico formattato con il file system NTFS.  
  
2.  Formattare il disco rigido e configurarlo per il backup del server. Se si sceglie questa opzione, viene avviata la procedura guidata Configura backup server, che indica i passaggi per la configurazione del backup del server.  
  
3.  Se non esiste uno spazio di archiviazione, usare il nuovo disco rigido per creare uno spazio di archiviazione. Per creare uno spazio di archiviazione, è necessario avere almeno due dischi rigidi.  
  
4.  Se è già presente uno spazio di archiviazione, usare il nuovo disco rigido per aumentare la capacità di un pool di archiviazione. Questa opzione viene visualizzata solo se nel server è già stato creato uno spazio di archiviazione. Se si sceglie questa opzione, la procedura guidata aggiungerà il disco rigido al pool di archiviazione.  
  
##  <a name="add-a-new-hard-drive"></a><a name="BKMK_4"></a>Aggiungi un nuovo disco rigido  
 Quando si collega un nuovo disco rigido a un server che esegue Windows Server Essentials, è possibile:  
  
-   [Usare il nuovo disco rigido per archiviare le cartelle del server](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4a)  
  
-   [Usare il nuovo disco rigido per archiviare i backup del server](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4b)  
  
-   [Usare il nuovo disco rigido per aumentare la capacità del pool di archiviazione](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4c)  
  
###  <a name="use-the-new-hard-drive-to-store-server-folders"></a><a name="BKMK_4a"></a>Usare il nuovo disco rigido per archiviare le cartelle del server  
 Per usare il nuovo disco rigido per archiviare le cartelle del server, è possibile aggiungere una nuova cartella del server al disco rigido o spostare nel disco una cartella del server esistente.  
  
##### <a name="to-store-server-folders"></a>Per archiviare le cartelle del server  
  
1. Aprire il Dashboard.  
  
2. Fare clic sulla scheda **ARCHIVIAZIONE** e quindi su **Cartelle server**.  
  
3. Nel riquadro **Attività Cartelle server** eseguire una delle operazioni seguenti:  
  
   1.  Per aggiungere una cartella del server, fare clic su **Aggiungi una cartella**.  
  
   2.  Per spostare una cartella del server, selezionare la cartella che si vuole spostare nel nuovo disco rigido e quindi fare clic su **Sposta una cartella**.  
  
   > [!NOTE]
   >  Se si passa al disco rigido e lo si seleziona come percorso della cartella del server senza creare una cartella, viene visualizzato il messaggio di errore seguente: **una directory radice (ad esempio C:\\, D:\\) non può essere aggiunta come cartella del server. Creare una nuova cartella o selezionarne una esistente nella directory radice, quindi riprovare**. Per risolvere l'errore, creare una nuova cartella nel disco rigido appena aggiunto e quindi selezionare la nuova cartella come percorso di archiviazione delle cartelle del server.  
  
4. Seguire le istruzioni per completare la procedura guidata.  
  
   Per altre informazioni sullo spostamento delle cartelle del server, vedere [Add or move a server folder](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5).  
  
###  <a name="use-the-new-hard-drive-to-store-server-backups"></a><a name="BKMK_4b"></a>Usare il nuovo disco rigido per archiviare i backup del server  
 È possibile usare il nuovo disco rigido aggiunto per archiviare i backup del server.  
  
##### <a name="to-store-server-backups"></a>Per archiviare i backup del server  
  
1. Aprire il Dashboard.  
  
2. Fare clic sulla scheda **Dispositivi**, selezionare il server nel riquadro elenco e quindi dal riquadro attività eseguire una delle operazioni seguenti:  
  
   1. Se nel server non è configurato il backup del server, fare clic su **Configura Backup server**.  
  
   2. Se nel server è configurato il backup del server, fare clic su **Personalizza Backup server**.  
  
      Verrà visualizzata la procedura guidata Configura Backup server.  
  
3. Nella pagina **Seleziona destinazione del backup** selezionare il nuovo disco rigido come destinazione del backup.  
  
4. Seguire le istruzioni per completare la procedura guidata.  
  
###  <a name="use-the-new-hard-drive-to-increase-storage-pool-capacity"></a><a name="BKMK_4c"></a>Usare il nuovo disco rigido per aumentare la capacità del pool di archiviazione  
 Quando la capacità del pool di archiviazione sta per esaurirsi, viene visualizzato un avviso che informa che è possibile aumentare la capacità del pool di archiviazione aggiungendo un nuovo disco rigido al pool tramite la procedura guidata Aumenta capacità di un pool di archiviazione.  
  
> [!NOTE]
>  È possibile eseguire questa procedura solo se nel server è stato creato un pool di archiviazione.  
  
##### <a name="to-increase-storage-pool-capacity"></a>Per aumenta la capacità del pool di archiviazione  
  
1.  Aprire il Dashboard.  
  
2.  Fare clic sulla scheda **Archiviazione** e quindi su **Unità disco rigido**.  
  
3.  Selezionare l'unità la cui capacità sta per esaurirsi.  
  
4.  Nel riquadro attività selezionare **Aumenta la capacità del pool di archiviazione**. Verrà visualizzata la procedura guidata Aumenta la capacità del pool di archiviazione.  
  
5.  Seguire le istruzioni per completare la procedura guidata.  
  
##  <a name="storage-spaces-overview"></a><a name="BKMK_5"></a>Panoramica di spazi di archiviazione  
 La funzionalità Spazi di archiviazione consente di raggruppare i dischi in un pool di archiviazione. È quindi possibile usare la capacità del pool per creare spazi di archiviazione. Gli spazi di archiviazione sono unità virtuali visualizzate nella scheda **Unità disco rigido** del dashboard. È possibile usare spazi di archiviazione come qualsiasi altra unità, pertanto è facile lavorare con i file in essi contenuti. Quando la capacità del pool sta per esaurirsi, è possibile creare spazi di archiviazione di grandi dimensioni e aggiungere altre unità al pool di archiviazione. Se si dispone di due o più dischi all'interno del pool di archiviazione, è possibile creare spazi di archiviazione con un mirroring a due vie che non sarà influenzato da un errore dell'unità? o anche l'errore di due unità? se si crea uno spazio di archiviazione con mirroring a tre vie.  
  
 Per creare uno spazio di archiviazione, sono sufficienti una o più unità aggiuntive oltre all'unità in cui è installato Windows. Queste unità possono essere dischi rigidi interni o esterni oppure unità SSD (Solid State Drive). È possibile usare diversi tipi di unità per gli spazi di archiviazione, tra cui unità USB, SATA e SAS.  
  
> [!NOTE]
>  Se si configurano spazi di archiviazione in un server che esegue Windows Server Essentials, non è possibile eseguire un ripristino delle impostazioni predefinite con l'opzione **Pulisci dati** . La soluzione alternativa per questo problema consiste nel rimuovere innanzitutto gli spazi di archiviazione e quindi eseguire un ripristino delle impostazioni predefinite con l'opzione **Pulisci dati**.  
  
 Per altre informazioni sulla funzionalità Spazi di archiviazione, vedere [Domande frequenti su Spazi di archiviazione](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx).  
  
##  <a name="create-a-storage-space"></a><a name="BKMK_6"></a>Crea uno spazio di archiviazione  
 Per iniziare a usare la funzionalità Spazi di archiviazione nel server, è necessario soddisfare i requisiti minimi seguenti:  
  
-   Il server che esegue Windows Server Essentials deve essere collegato a unità fisiche aggiuntive (non solo l'unità di avvio), le unità non devono ospitare volumi e devono avere una capacità minima di 10 GB. Per creare un pool di archiviazione, è necessaria un'unità fisica. Per creare uno spazio di archiviazione con mirroring resiliente, sono necessarie almeno due unità fisiche.  
  
-   Per creare uno spazio di archiviazione con resilienza attraverso la parità o il mirroring a 2 vie, sono necessarie almeno due unità fisiche.  
  
> [!NOTE]
>  Se si configurano spazi di archiviazione in un server che esegue Windows Server Essentials, non è possibile eseguire un ripristino delle impostazioni predefinite con l'opzione **Pulisci dati** . La soluzione alternativa per questo problema consiste nel rimuovere innanzitutto gli spazi di archiviazione e quindi eseguire un ripristino delle impostazioni predefinite con l'opzione **Pulisci dati**.  
  
#### <a name="to-create-a-storage-space-in-windows-server-essentials"></a>Per creare uno spazio di archiviazione in Windows Server Essentials  
  
1. Aggiungere o connettere tutte le unità da raggruppare con Spazi di archiviazione nel server che esegue Windows Server Essentials.  
  
2. Dal dashboard fare clic su **Avanzato: Gestione spazi di archiviazione**.  
  
3. Fare clic su **Crea nuovo pool e spazio di archiviazione**.  
  
4. Selezionare le unità da aggiungere al nuovo spazio di archiviazione e quindi fare clic su **Crea pool**.  
  
5. Assegnare all'unità un nome e una lettera e quindi scegliere un layout. **Mirroring a 2 vie**, **Mirroring a 3 vie** e **Parità** possono aiutare a proteggere i file nello spazio di archiviazione in caso di errori delle unità.  
  
6. Immettere la dimensione massima per lo spazio di archiviazione e quindi fare clic su **Crea spazio di archiviazione**.  
  
   In Windows Server Essentials è possibile creare uno spazio di archiviazione con mirroring a due vie usando la procedura guidata crea spazio di archiviazione dal dashboard.  
  
#### <a name="to-create-a-storage-space-in-windows-server-essentials"></a>Per creare uno spazio di archiviazione in Windows Server Essentials  
  
1. Aggiungere o connettere tutte le unità da raggruppare con Spazi di archiviazione nel server che esegue Windows Server Essentials.  
  
2. Dal dashboard fare clic su **Gestione spazi di archiviazione**. Verrà visualizzata la procedura guidata Crea spazio di archiviazione.  
  
3. Seguire le istruzioni per completare la procedura guidata.  
  
   Per informazioni su come aumentare la capacità del pool di archiviazione, vedere [Use the new hard drive to increase storage pool capacity](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4c).  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestisci cartelle server](Manage-Server-Folders-in-Windows-Server-Essentials.md)  
  
-   [Usare Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)
