---
title: Creare un DVD di ripristino Server per server amministrati da postazione remota
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6141fa69-5952-4e3c-a868-40ef3f4badd2
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 7bfe1686ac84962cdb4ab1cde8d6ca5226cb9d44
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="create-a-server-recovery-dvd-for-remotely-administered-servers"></a>Creare un DVD di ripristino Server per server amministrati da postazione remota

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_HeadlessRecovery"></a>Creare un DVD di ripristino server per server amministrati da postazione remota  
 Esistono due modelli per il ripristino delle impostazioni di fabbrica reimpostazione e server e che differiscono a seconda dell'hardware ricevuto dal cliente.  
  
 **Server amministrato in modalità remota**  
  
 Questa opzione di ripristino server richiede che il processo viene eseguito da un computer client nella stessa rete. Poiché è necessario che un'immagine specifiche dell'hardware venga fornita con il server di fabbrica, il partner deve creare il DVD di ripristino server.  
  
 **Server amministrato localmente**  
  
 Questo è il modello classico in cui il server viene amministrato dalla console di server. Il supporto di installazione del server viene utilizzato per eseguire un ripristino. Ciò richiede che il server viene fornito con la possibilità di visualizzare l'output video oltre a un lettore DVD. Il cliente viene avviato dal supporto di installazione di server e quindi sceglie il metodo di ripristino appropriato. Non devi creare un DVD di ripristino server per i server che vengono amministrati a livello locale.  
  
> [!NOTE]
>  Per il modello di server amministrato localmente, il cliente può portare a termine un'impostazioni di fabbrica eseguendo una nuova installazione. Tuttavia, sono personalizzazioni andranno perse il partner.  
  
 Esistono due tipi di ripristino del server.  
  
 **Impostazioni predefinite**  
  
 Questo tipo di ripristino restituisce il server allo stato originale che esisteva quando il server è stato fornito dal produttore. A seguito di fabbrica, verrà chiesto di eseguire la configurazione iniziale del server, esattamente come la prima volta che è attivata e tutte le impostazioni e personalizzazioni andranno perse. Questo è anche detta giorno 0.? Poiché è necessario che un'immagine specifiche dell'hardware venga fornita con il server di fabbrica, il partner deve creare il DVD di ripristino server.  
  
 **Ripristino bare metal**  
  
 Questo tipo di ripristino si presuppone che sia configurato un backup del server e che il backup del server completata correttamente almeno una volta prima dell'errore del server. Ripristino bare metal (BMR) supporta il ripristino delle unità di sistema e di avvio solo da un precedente backup del server.  
  
 Ripristino bare metal, il server viene restituito allo stato in cui si trovava al momento del backup utilizzato per il ripristino. Si tratta in genere il backup più recente, ma in alcuni casi, potrebbe essere un backup precedente. Ripristino dei dati viene eseguito dopo che il sistema viene ripristinato utilizzando il ripristino guidato file e cartelle. Ripristino bare metal è il metodo di ripristino server preferito poiché tutte le impostazioni e configurazione vengono restituiti, mentre di fabbrica restituisce il server a uno stato di giorno 0.  
  
### <a name="remotely-administered-server-recovery"></a>Recupero di server amministrato in modalità remota  
 Questa sezione descrive le personalizzazioni necessarie che il partner deve eseguire e i supporti che è necessario fornire con ciascun server. Prima di scendere nel dettaglio, esaminiamo l'esperienza dei clienti.  
  
 In questo scenario, il cliente "stato s server non funziona più. Questo potrebbe essere causato da un virus, un errore del disco rigido o altra causa di alcuni. Il cliente inserisce il DVD di ripristino server in un computer client che si trova nella stessa rete del server. L'applicazione di ripristino server Guida il cliente nei tre passaggi necessari a ripristinare il server:  
  
1.  Crea un'unità flash USB avviabile che viene utilizzata per riavviare il server in modalità di ripristino. L'unità flash USB deve essere 8 GB o superiori.  
  
2.  Rilevamento del server in modalità di ripristino.  
  
3.  Consente agli utenti di scegliere di fabbrica o un ripristino bare metal e quindi restituisce il server a uno stato funzionante.  
  
### <a name="steps-for-creating-a-server-recovery-dvd-for-multiple-language-support"></a>Passaggi per la creazione di un DVD di ripristino server per la lingua più supportare  
 Ci sono sei passaggi principali per creare un DVD di ripristino server  
  
1.  [(Facoltativo) Aggiornamento di WinPE](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Updating)  
  
2.  [Raccogliere le immagini di ripristino delle impostazioni di fabbrica e il file XML](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Collecting)  
  
3.  [Creare il DVD di ripristino server](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Creating)  
  
4.  [Personalizzare la procedura guidata](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Customizing)  
  
5.  [Creare il file ISO](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_CreatingISO)  
  
6.  [Testare il DVD di ripristino](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Testing)  
  
####  <a name="BKMK_Updating"></a>Passaggio 1: (Facoltativo) aggiornare WinPE  
 ADK include una copia di Windows PE che è personalizzata. Quando questa immagine viene avviata, viene avviato automaticamente il beacon utilizzato dall'applicazione di ripristino client per connettersi a un server in modalità di ripristino.  
  
 È necessario personalizzare ulteriormente aggiungendo eventuali driver hardware specifici, ad esempio le reti o driver di controller del disco Windows PE. Dopo l'avvio da WinPE, i dischi rigidi nel sistema devono essere riconoscibile e la rete deve essere in funzione.  
  
####  <a name="BKMK_Collecting"></a>Passaggio 2: Raccogliere le immagini di ripristino delle impostazioni di fabbrica e il file XML  
 Per ripristinare un server di impostazioni predefinite di fabbrica, è necessario acquisire le seguenti due immagini:  
  
-   L'immagine dell'unità del sistema  
  
-   La partizione di sistema riservato  
  
 Per acquisire queste immagini, viene fornito lo strumento GenDiskXML.exe. GenDiskXML.exe acquisisce anche un file denominato disk.xml che viene utilizzato dal processo di ripristino per ricreare la configurazione del disco.  
  
1.  Dopo Sysprep, riavviare il sistema utilizzando una qualsiasi versione a 64 bit di Windows PE.  
  
2.  Trasferire i file XML e WIM in un'origine esterna, eseguire `GenDiskXML /outputdir:<path>`per generare i file XML e WIM in qualsiasi origine esterna. I file vengono aggiunti al DVD nel passaggio successivo.  
  
    > [!NOTE]
    >  Il file. wim viene separato in modo per soddisfare il requisito FAT32 secondo cui nessun file più grandi di 4 GB. Durante il processo, deve essere maggiore di 8 GB per consentire il processo di separazione necessario che la capacità della destinazione utilizzata per acquisire i file con estensione wim.  
  
####  <a name="BKMK_Creating"></a>Passaggio 3: Creare il DVD di ripristino server  
 Ogni server fornito dal produttore deve essere accompagnata da un DVD di ripristino server. Il DVD degli strumenti ADK include i file necessari per creare il DVD.  
  
###### <a name="to-create-the-server-recovery-dvd"></a>Per creare il DVD di ripristino server  
  
1.  Creare una cartella di lavoro da utilizzare come percorso per archiviare l'immagine ISO finale.  
  
2.  Dal CD del partner, copiare il contenuto del **Server Recovery** cartella nella cartella di lavoro creata nel passaggio 1.  
  
3.  Copiare il file disk.xml creato durante l'esecuzione di GenDiskXML.exe il **reimpostare** cartella.  
  
4.  Copiare i file di immagine che sono stati creati durante l'esecuzione **GenDiskXML.exe** per il **reimpostare** cartella. I file sono i file con estensione wim e. swm e il numero di file può variare.  
  
5.  Rimuovere GenDiskXML.exe dalla cartella. Viene utilizzato solo per le operazioni di fabbrica e non deve essere incluso nel DVD destinato al cliente.  
  
####  <a name="BKMK_Customizing"></a>Passaggio 4: Personalizzare la procedura guidata  
 L'applicazione di ripristino del server deve essere personalizzato con un'immagine del dispositivo e il testo che descrive come avviare il dispositivo specifico in modalità di ripristino. Poiché la pagina del ripristino guidato file e cartelle specifici dell'hardware, i passaggi necessari per avviare il server in modalità di ripristino variano.  
  
> [!NOTE]
>  I nomi file elencati devono corrispondere esattamente.  
  
1.  Pagina della procedura guidata è presente un collegamento per ulteriori informazioni. Se esiste, questo file. chm sostituisce FWLink per la Guida web. Il file della Guida si trova in:  
  
     < DVD root > \\$OEM$\Customization\\ < impostazioni cultura name\ > \RestartHelp.chm  
  
2.  Questo file contiene il testo che il cliente vede la pagina della procedura guidata. Il testo deve spiegare come avviare il server in modalità di ripristino. Il controllo è scorrevole; Ciò pone un limite pratico alla quantità di testo che può essere aggiunto.  
  
     Il file seguente viene utilizzato per sostituire l'immagine campione nella procedura guidata e concerne principalmente la personalizzazione. Deve essere un file con estensione png. Le dimensioni del file devono essere 256 x 256 pixel o verrà ritagliato quando viene visualizzato nella procedura guidata.  
  
     < DVD root > \\$OEM$\Customization\\ < impostazioni cultura name\ > \RestartInstructions.rtf  
  
3.  < DVD root > \\$OEM$\Customization\\ < impostazioni cultura name\ > \ServerImage.png  
  
 Durante la conversione del DVD per supportare più lingue di ripristino del server, assicurarsi di effettuare le operazioni seguenti:  
  
1.  È necessario che il en-us cartella. Se l'applicazione di ripristino del server non trova i file specifici delle impostazioni cultura che corrispondono al computer client in cui viene eseguito, viene eseguito il fallback a en-us.  
  
2.  In ogni cartella impostazioni cultura creata, aggiungere i tre file di programma di personalizzazione (.png, chm e. RTF).  
  
3.  Copiare entrambe le cartelle della lingua da Language Packs\\ < CultureName\ > \Server Recovery alla radice del DVD di ripristino server. Ad esempio: entrambe le cartelle ES ed ES-ES verrebbero copiate nella radice del DVD per supportare lo spagnolo.  
  
4.  Finalizzazione del file ISO.  
  
 Nomi delle impostazioni cultura supportati includono:  

|-|-|  
|-Cs-CZ<br /><br /> -De-DE<br /><br /> -En-US<br /><br /> -Es-ES<br /><br /> -Fr-FR<br /><br /> -Hu-HU<br /><br /> -It-IT<br /><br /> -Ja-JP<br /><br /> -Ko-KR<br /><br /> -Nl-NL &-pl-PL<br /><br /> -Pt-BR<br /><br /> -Pt-PT<br /><br /> -Ru-RU<br /><br /> -Sv-SE<br /><br /> -Tr-TR<br /><br /> -Zh-CN<br /><br /> -Zh-HK<br /><br /> -Zh-TW
  
####  <a name="BKMK_CreatingISO"></a>Passaggio 5: Creare il file ISO  
 La cartella creata e tutto il contenuto può essere masterizzato in un DVD. Si tratta del DVD che verrà fornito ai clienti con il nuovo server.  
  
####  <a name="BKMK_Testing"></a>Passaggio 6: Testare il DVD di ripristino  
 Dopo aver completato l'installazione del server, configurare il backup del server, eseguire un backup del server e quindi testare il DVD di ripristino.  
  
###### <a name="to-configure-and-run-a-server-backup"></a>Per configurare ed eseguire un backup del server  
  
1.  Aprire il Dashboard, fare clic su di **dispositivi** scheda e quindi fare clic su **imposta backup del server di** nel **attività** riquadro.  
  
2.  Seguire le istruzioni della procedura guidata per configurare un backup del server di backup. È necessario un disco rigido esterno per il backup.  
  
3.  Fare clic su **avviare un backup per il server** nel **attività** riquadro e quindi segui le istruzioni della procedura guidata.  
  
4.  Al termine del backup, verificare che sia stato eseguito correttamente.  
  
###### <a name="to-restore-a-server"></a>Per ripristinare un server  
  
1.  Inserire il DVD creato di ripristino in un computer client che è connesso alla stessa rete del server tramite un hub o un commutatore.  
  
2.  Fare doppio clic su **setup.exe**. La procedura guidata ripristino Server permette di effettuare la stessa procedura che segue il cliente.  
  
3.  Fare clic su **ripristinare il server da un backup**e quindi segui le istruzioni della procedura guidata.  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](Testing-the-Customer-Experience.md)