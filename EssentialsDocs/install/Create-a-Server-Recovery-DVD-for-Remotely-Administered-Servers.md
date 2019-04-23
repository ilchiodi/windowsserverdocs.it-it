---
title: Creare un DVD di ripristino del server per server amministrati da postazione remota
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844352"
---
# <a name="create-a-server-recovery-dvd-for-remotely-administered-servers"></a>Creare un DVD di ripristino del server per server amministrati da postazione remota

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_HeadlessRecovery"></a> Creare un DVD di ripristino server per server amministrati da postazione remota  
 Esistono due modelli per il ripristino delle impostazioni di fabbrica e il ripristino del server, che differiscono a seconda dell’hardware ricevuto dal cliente.  
  
 **Server amministrato da postazione remota**  
  
 Questa opzione di ripristino del server richiede che il processo venga eseguito da un computer client che si trova nella stessa rete. Dal momento che per eseguire il ripristino delle impostazioni di fabbrica è necessario che un'immagine specifica dell'hardware venga fornita con il server, il partner deve creare il DVD di recupero del server.  
  
 **Server amministrato localmente**  
  
 Questo è il modello classico, in cui il server viene amministrato dalla console server. Il supporto di installazione del server viene utilizzato per eseguire un ripristino. Questo metodo richiede che il server consegnato disponga di un’uscita video e di un lettore DVD. Il cliente esegue l’avvio dal supporto di installazione del server e quindi sceglie il metodo di ripristino appropriato. Non è necessario creare un DVD di ripristino server per i server che vengono amministrati a livello locale.  
  
> [!NOTE]
>  Per il modello di server amministrato a livello locale, il cliente deve effettuare un ripristino delle impostazioni di fabbrica eseguendo una nuova installazione. Tuttavia, le personalizzazioni del partner andranno perse.  
  
 Sono disponibili due tipi di ripristino del server.  
  
 **Ripristino delle impostazioni predefinite**  
  
 Questo tipo di ripristino consente di riportare il server allo stato originario, ovvero allo stato del server fornito dal produttore. Al termine del ripristino delle impostazioni di fabbrica, all'utente viene chiesto di eseguire la configurazione iniziale del server, esattamente come al primo avvio del server; tutte le impostazioni e personalizzazioni andranno perse. Questo è anche detto 0 giorni.? Dal momento che per eseguire il ripristino delle impostazioni di fabbrica è necessario che un'immagine specifica dell'hardware venga fornita con il server, il partner deve creare il DVD di recupero del server.  
  
 **Ripristino bare metal**  
  
 Questo tipo di ripristino presuppone che il backup del server sia stato configurato ed eseguito correttamente almeno una volta prima del guasto del server. Il ripristino bare metal (bare metal restore, BMR) supporta il ripristino del sistema e delle unità di avvio soltanto a partire da un precedente backup del server.  
  
 Al termine del ripristino bare metal, il server viene riportato nello stato in cui si trovava al momento del backup utilizzato per il ripristino. In genere si tratta dell’ultimo backup effettuato; in alcuni casi può invece trattarsi di un backup precedente. Dopo aver ripristinato il sistema, è possibile ripristinare i dati utilizzando la procedura guidata Ripristino file e cartelle. Il ripristino bare metal rappresenta il metodo preferito di ripristino del server, in quanto consente di ripristinare tutte le impostazioni e le configurazioni, a differenza del ripristino delle impostazioni di fabbrica che riporta il server in uno stato "giorno 0".  
  
### <a name="remotely-administered-server-recovery"></a>Recupero di server amministrato da postazione remota  
 Questa sezione descrive le personalizzazioni necessarie che il partner deve eseguire e i supporti che è necessario fornire con ciascun server. Prima di scendere nel dettaglio, esaminiamo l’esperienza di utilizzo da parte del cliente.  
  
 In questo scenario, il cliente "stato s server non funziona più. La causa potrebbe essere un virus, un errore del disco rigido o altro. Il cliente inserisce il DVD di ripristino server in un computer client che si trova nella stessa rete del server. L’applicazione di ripristino del server guida il cliente nei tre passaggi necessari a ripristinare il server:  
  
1.  Creazione di un’unità flash USB di avvio da usare per riavviare il server in modalità di ripristino. L’unità flash USB deve avere una capacità di almeno 8 GB.  
  
2.  Rilevamento del server in modalità di ripristino.  
  
3.  Possibilità per il cliente di scegliere il ripristino delle impostazioni di fabbrica o il ripristino bare metal, e conseguente ripristino del server a uno stato operativo.  
  
### <a name="steps-for-creating-a-server-recovery-dvd-for-multiple-language-support"></a>Passaggi per la creazione di un DVD di ripristino server il supporto multilingue  
 Ci sono sei passaggi principali per creare un DVD di ripristino server  
  
1.  [(Facoltativo) Aggiornare WinPE](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Updating)  
  
2.  [Raccogliere le immagini di reimpostazione della factory e i file XML](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Collecting)  
  
3.  [Creare il DVD di ripristino server](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Creating)  
  
4.  [Personalizzare la procedura guidata](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Customizing)  
  
5.  [Creare il file ISO](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_CreatingISO)  
  
6.  [Testare il DVD di ripristino](Create-a-Server-Recovery-DVD-for-Remotely-Administered-Servers.md#BKMK_Testing)  
  
####  <a name="BKMK_Updating"></a> Passaggio 1: (Facoltativo) Aggiornamento di WinPE  
 ADK include una copia di Windows PE che è personalizzata. Dopo l’avvio di questa immagine, viene avviato automaticamente il beacon utilizzato dall’applicazione di ripristino client per collegarsi a un server in modalità di ripristino.  
  
 È necessario personalizzare ulteriormente Windows PE aggiungendo eventuali driver hardware specifici, quali i driver di rete o dei controller del disco. Dopo l’avvio da WinPE, è necessario che i dischi rigidi del sistema siano riconoscibili e che la connessione di rete sia funzionante.  
  
####  <a name="BKMK_Collecting"></a> Passaggio 2: Raccolta delle immagini e dei file XML per il ripristino delle impostazioni di fabbrica  
 Per riportare un server alle impostazioni predefinite di fabbrica, è necessario acquisire le due immagini riportate di seguito:  
  
-   L'immagine dell'unità di sistema  
  
-   La partizione riservata per il sistema  
  
 Per l’acquisizione di queste immagini viene fornito lo strumento GenDiskXML.exe. GenDiskXML.exe acquisisce anche un file denominato disk.xml, che viene utilizzato dal processo di ripristino per ricreare la configurazione del disco.  
  
1.  Dopo Sysprep, riavviare il sistema utilizzando una qualsiasi versione a 64 bit di Windows PE.  
  
2.  Per indirizzare l’output dei file .xml e .wim a una fonte esterna, eseguire `GenDiskXML /outputdir:<path>` . Nel passaggio successivo i file vengono aggiunti al DVD.  
  
    > [!NOTE]
    >  Il file .wim viene separato in modo da soddisfare il requisito FAT32 secondo cui nessun file può avere dimensioni maggiori di 4 GB. Durante la procedura, per consentire il processo di separazione è necessario che la capacità della destinazione utilizzata per acquisire i file .wim sia maggiore di 8 GB.  
  
####  <a name="BKMK_Creating"></a> Passaggio 3: Creazione del DVD di ripristino server  
 Ogni server inviato dal produttore deve essere dotato di un DVD di ripristino server. Il DVD degli strumenti ADK include i file necessari per creare il DVD.  
  
###### <a name="to-create-the-server-recovery-dvd"></a>Creazione del DVD di ripristino del server  
  
1.  Creare una cartella di lavoro da utilizzare come percorso dell’immagine ISO finale.  
  
2.  Dal CD del partner, copiare i contenuti della cartella **Server Recovery** nella cartella di lavoro creata nel Passo 1.  
  
3.  Copiare il file disk.xml che è stato creato al momento dell’esecuzione di GenDiskXML.exe nella cartella **Reset**.  
  
4.  Copiare i file delle immagini che sono stati creati al momento dell'esecuzione di **GenDiskXML.exe** nella cartella **Reset**. Si tratta di file .wim e .swm di numero variabile.  
  
5.  Rimuovere GenDiskXML.exe dalla cartella. Tale file viene utilizzato soltanto per le operazioni di fabbrica e non deve essere incluso nel DVD destinato al cliente finale.  
  
####  <a name="BKMK_Customizing"></a> Passaggio 4: Personalizzazione della procedura guidata  
 È necessario personalizzare l’applicazione di ripristino del server con un’immagine del dispositivo e con il testo che descrive come avviare il dispositivo specifico in modalità di ripristino. Poiché questa pagina della procedura guidata Ripristino file e cartelle dipende dall’hardware installato, i passaggi necessari per avviare il server in modalità di ripristino possono variare.  
  
> [!NOTE]
>  I nomi file elencati devono corrispondere esattamente.  
  
1.  Nella pagina della procedura guidata è presente un collegamento alla guida aggiuntiva. Se è presente questo file .chm, sostituisce FWLink per la guida Web. Il file della guida si trova in:  
  
     < DVD Root\>\\$OEM$ \Customization\\< nome delle impostazioni cultura\>\RestartHelp.chm  
  
2.  Questo file contiene il testo che sarà visibile al cliente nella pagina della procedura guidata. Il testo deve spiegare come avviare il server in modalità di ripristino. Il controllo è scorrevole; ciò pone soltanto un limite pratico alla quantità di testo che è possibile aggiungere.  
  
     Il file seguente viene utilizzato per sostituire l’immagine campione nella procedura guidata e concerne principalmente la personalizzazione. Deve essere un file .png. Le dimensioni del file devono essere 256x256 pixel; in caso contrario, il file verrà ritagliato al momento della visualizzazione nella procedura guidata.  
  
     < DVD Root\>\\$OEM$ \Customization\\< nome delle impostazioni cultura\>\RestartInstructions.rtf  
  
3.  < DVD Root\>\\$OEM$ \Customization\\< nome delle impostazioni cultura\>\ServerImage.png  
  
 Durante la conversione del DVD di ripristino server per il supporto multilingue, assicurarsi di effettuare le seguenti operazioni:  
  
1.  Deve essere sempre presente la cartella en-us. Se l’applicazione di ripristino del server non trova i file relativi alle impostazioni cultura che corrispondono al computer client sul quale è in esecuzione, viene utilizzata la cartella en-us.  
  
2.  In ogni cartella di impostazioni cultura creata, aggiungere i tre file di personalizzazione (.png, .chm e .rtf).  
  
3.  Copiare entrambe le cartelle di lingua da Language Pack\\< CultureName\>\Server Recovery nella radice del DVD di ripristino del server. Ad esempio: Le cartelle ES ed ES-ES verranno entrambe copiate nella radice del DVD per supportare lo spagnolo.  
  
4.  Finalizzazione del file ISO.  
  
 I nomi delle impostazioni cultura supportati includono:  

|-|-|  
|-cs-CZ<br /><br /> -de-DE<br /><br /> -en-US<br /><br /> -es-ES<br /><br /> -fr-FR<br /><br /> -hu-HU<br /><br /> -it-IT<br /><br /> -ja-JP<br /><br /> -ko-KR<br /><br /> - nl-NL|- pl-PL<br /><br /> -pt-BR<br /><br /> -pt-PT<br /><br /> -ru-RU<br /><br /> -sv-SE<br /><br /> -tr-TR<br /><br /> - zh-CN<br /><br /> - zh-HK<br /><br /> - zh-TW
  
####  <a name="BKMK_CreatingISO"></a> Passaggio 5: Creazione del file ISO  
 È possibile masterizzare su un DVD la cartella creata e tutto il contenuto. Si tratta del DVD che verrà fornito ai clienti in dotazione al nuovo server.  
  
####  <a name="BKMK_Testing"></a> Passaggio 6: Test del DVD di ripristino  
 Dopo avere completato l’installazione del server, configurare il backup del server, eseguire un backup del server, quindi verificare il DVD di ripristino.  
  
###### <a name="to-configure-and-run-a-server-backup"></a>Configurazione ed esecuzione di un backup del server  
  
1.  Aprire il dashboard, fare clic sulla scheda **Dispositivi** e quindi selezionare **Imposta backup del server** nel riquadro **Attività**.  
  
2.  Configurare un backup del server seguendo le istruzioni della procedura guidata. Per eseguire il backup è necessario un disco rigido esterno.  
  
3.  Nel riquadro **Attività** , fare clic su **Avvia un backup del server** , quindi seguire le istruzioni della procedura guidata.  
  
4.  Al termine del backup, verificare che sia stato eseguito correttamente.  
  
###### <a name="to-restore-a-server"></a>Per ripristinare un server  
  
1.  Inserire il DVD di ripristino creato in precedenza in un computer client connesso alla stessa rete del server tramite hub o switch.  
  
2.  Fare doppio clic su **setup. exe**. La procedura guidata Ripristino server permette di effettuare la stessa procedura che dovrà essere seguita dal cliente.  
  
3.  Fare clic su **Ripristina il server da un backup**, quindi seguire le istruzioni della procedura guidata.  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Testare l'esperienza dei clienti](Testing-the-Customer-Experience.md)