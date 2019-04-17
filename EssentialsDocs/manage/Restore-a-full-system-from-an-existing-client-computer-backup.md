---
title: Ripristinare un intero sistema dal backup di un computer client esistente
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47e498a6-1b71-47de-88f6-8c13c221d108
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cfc4d1ce461e1e1cbb9b99970355c4dc7241911b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="restore-a-full-system-from-an-existing-client-computer-backup"></a>Ripristinare un intero sistema dal backup di un computer client esistente

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials 
  
 Gli errori hardware e sistema operativo sono rari, ma possono verificarsi. Ventola potrebbe surriscaldare la scheda madre di un computer e renderlo inutilizzabile. Il sistema operativo potrebbe danneggiato e non riavviarsi. Incendi e allagamenti possa causare danni permanenti all'hardware. Potrebbe non riuscire a un'unità disco rigido o si potrebbe decidere di sostituirla con un disco rigido più grande.  
  
 Questo documento vengono fornite informazioni sugli argomenti seguenti:  
  
-   [Che cos'è il ripristino completo del sistema del computer?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_WhatIs)  
  
-   [Come funziona l'ambiente di ripristino?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_HowDoes)  
  
-   [Creare un'unità flash USB avviabile per ripristinare un computer client](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_CreateBootable)  
  
-   [Con la procedura guidata Ripristino configurazione di sistema completo](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_Using)  
  
-   [Dove posso trovare i driver per l'hardware?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_FindDrivers)  
  
##  <a name="BKMK_WhatIs"></a>Che cos'è il ripristino completo del sistema del computer?  
 Nel caso in cui si sostituisce un disco rigido o il computer risulta danneggiato nel punto in cui non è possibile utilizzare o non si avvia, è possibile ripristinare il sistema da un backup precedente del computer. Un ripristino completo del sistema restituisce il sistema allo stato in cui si trovava al momento del backup.  
  
> [!IMPORTANT]
>  È possibile eseguire un ripristino completo del sistema in hardware del computer, ad esempio una scheda di sistema, che non è simile all'hardware del computer che viene sostituito. Un sistema operativo installato è dipende in grande misura l'hardware sottostante del computer. Tuttavia, è possibile eseguire un ripristino completo del sistema a un disco rigido di dimensioni pari o superiore a quello che viene sostituito.  
  
 Quando si esegue un ripristino completo del sistema, è possibile scegliere un backup del computer specifico per ripristinare il sistema, con tutte le applicazioni, configurazioni e impostazioni personalizzate per l'utente prima dell'errore, della situazione di emergenza o del furto. È anche possibile scegliere i volumi che si desidera ripristinare.  
  
 Durante la pianificazione o la preparazione per ripristinare il sistema completo su un computer di rete, considerare quanto segue:  
  
### <a name="windows-preinstallation-environment"></a>Ambiente preinstallazione di Windows  
 Ambiente preinstallazione di Windows (Windows PE) è un sistema operativo minimo il compito di preparare un computer per l'installazione di Windows. Per i server che esegue Windows Server Essentials, Windows PE viene installato automaticamente quando si inserisce il supporto di ripristino in un computer da ripristinare. Per i server che esegue Windows Server Essentials, Windows PE viene installato automaticamente quando si avvia il computer con il servizio Ripristino client o con l'unità flash USB.  
  
 Windows PE non supporta le connessioni wireless. Per questo motivo, il computer da ripristinare deve essere fisicamente connesso alla rete small business.  
  
### <a name="bitlocker"></a>BitLocker  
 Crittografia unità BitLocker (BitLocker) è una funzionalità di protezione dati che è disponibile in alcune versioni di Windows Vista, Windows 7 e Windows 8. BitLocker protegge contro il furto di dati o l'esposizione di computer smarriti o rubati e offre un'eliminazione più sicura dei dati quando i computer vengono rimosse.  
  
 **Per Windows Server Essentials:** se il computer da ripristinare era stato crittografato con BitLocker (se era solo l'unità del sistema operativo o l'unità del sistema operativo e singolo o più altre unità fisse), è possibile utilizzare comunque il supporto di ripristino di sistema completo contenuto sul CD fornito con il server e la procedura guidata Ripristino configurazione di sistema completo per reinstallare l'immagine dell'unità disco rigido, compreso il sistema operativo, da un backup e ripristinare i dati nel computer nuovo o riparato.  
  
 **Per Windows Server Essentials:** se il computer da ripristinare era stato crittografato con BitLocker (se era solo l'unità del sistema operativo o l'unità del sistema operativo e singolo o più altre unità fisse), è comunque possibile utilizzare la procedura guidata Ripristino configurazione di sistema completo a installare nuovamente l'immagine dell'unità disco rigido, compreso il sistema operativo, da un backup e ripristinare i dati nel computer nuovo o riparato.  
  
 Quando il server esegue il backup di unità, cartelle e file, una versione non crittografata viene salvata nel server. Durante il ripristino completo del sistema, questa versione non crittografata viene copiata nel computer.  
  
> [!NOTE]
>  Dopo un ripristino completo del sistema ha esito positivo, è necessario riattivare BitLocker sul computer.  
>   
>  Per istruzioni su come abilitare BitLocker sui computer che eseguono Windows 8, vedere [BitLocker: come abilitare BitLocker](https://go.microsoft.com/fwlink/p/?LinkID=254918).  
>   
>  Per istruzioni su come abilitare BitLocker sui computer che eseguono Windows 7, vedere [crittografia unità BitLocker Step-by-Step Guida per Windows 7](https://go.microsoft.com/fwlink/p/?LinkId=140225).  
  
 Per ulteriori informazioni sulle nozioni di base sulla crittografia unità BitLocker, vedere [BitLocker spesso domande frequenti](https://go.microsoft.com/fwlink/p/?LinkID=254917).  
  
### <a name="encrypting-file-system-encrypted-files"></a>La crittografia dei file crittografati al File System  
 La funzionalità di crittografia File System (EFS) in Windows può fornire crittografia a livello di file aggiuntive basate sull'utente per diversi livelli di sicurezza tra più utenti dello stesso computer. È importante notare che, diversamente dalle unità crittografate con BitLocker, file e cartelle crittografati EFS continuano ad essere crittografati in ogni backup del computer. EFS non è disponibile in Windows XP Home Edition, Windows Vista Starter, Windows Vista Home Basic, Windows Vista Home Premium, Windows 7 Starter, Windows 7 Home Basic, Windows 7 Home Premium o Windows 8. È disponibile solo in Windows 8 Pro.  
  
> [!WARNING]
>  Diversamente da BitLocker, è possibile accedere solo i file protetti da EFS dall'interno del sistema operativo che li ha crittografati.  
  
### <a name="disk-partitions"></a>Partizioni del disco  
 Se la dimensione di unità disco rigido sul nuovo computer è pari o superiori dell'originale, il disco rigido unità disco viene automaticamente riformattato e ripartizionato. Fare riferimento al grafico sotto per le specifiche:  
  
|Computer originale|Computer nuovo o ripristinato|  
|-----------------------|------------------------------|  
|Singolo disco con più partizioni|Singolo disco con più partizioni e l'eventuale spazio aggiuntivo viene allocato all'ultima partizione|  
|Singolo disco con una singola partizione|Singolo disco con una singola partizione e tutto lo spazio disponibile viene utilizzato per la singola partizione|  
  
> [!NOTE]
>  Se vi sono differenze di layout di dimensioni e della partizione del disco tra originale e il computer nuovo o ripristinato, è necessario utilizzare Gestione disco per creare le partizioni appropriate nel computer nuovo o ripristinato. Tale operazione può essere eseguita la procedura guidata Ripristino configurazione di sistema completo.  
  
### <a name="raid-and-dynamic-disks"></a>RAID e dischi dinamici  
 Eseguire il backup redundant array of independent disks (RAID) e i dischi dinamici non è supportato.  
  
##  <a name="BKMK_HowDoes"></a>Come funziona l'ambiente di ripristino?  
 Il supporto di ripristino del sistema fornito con Windows ServerÂ® 2012 Essentials consente di installare nel computer Ambiente preinstallazione di Windows (Windows PE). Windows PE sostituisce l'ambiente MS-DOS e contiene i file di programma dei componenti di base per Windows. In Windows Server Essentials, esistono due modi per ripristinare un sistema: utilizza il servizio Ripristino client, che utilizza una rete e non si basa sul supporto o il dispositivo USB con un'unità flash.  
  
> [!NOTE]
>  Windows PE non supporta le connessioni wireless. Per questo motivo, il computer da ripristinare deve essere fisicamente connesso alla rete small business.  
  
 L'ambiente di ripristino di sistema include (x86) 32 bit e 64 bit (x64) i file di programma. Dopo aver inserito il sistema di supporto di ripristino, scegliere la versione appropriata dei file. Il valore predefinito è (x86) 32 bit e viene selezionata automaticamente se non si sceglie entro 30 secondi. Se sono presenti aggiornamenti del sistema completo ripristino i file di programma nel server, i file aggiornati vengono scaricati automaticamente al computer.  
  
 Dopo aver configurato l'ambiente di preinstallazione, viene avviata la procedura guidata Ripristino configurazione di sistema completo. La procedura guidata consente di ripristinare il computer da un backup precedente. È inoltre possibile utilizzare l'ambiente di ripristino di sistema per ripristinare un backup in un nuovo computer con hardware simile.  
  
 Nella maggior parte dei casi, i file di programma e i driver contenuti l'una nell'ambiente di ripristino sono sufficienti per riavviare il computer nuovo o ripristinato. A seconda dell'hardware del computer nuovo o ripristinato, l'ambiente di ripristino di sistema potrebbe non includere tutto lo spazio di archiviazione e che sono necessari quando si riavvia il computer nuovo o ripristinato driver della scheda di rete. La procedura guidata Ripristino configurazione di sistema completo ti offre la possibilità di installare i driver, se necessario. Per informazioni su come trovare i driver hardware, vedere [dove posso trovare i driver per l'hardware?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_FindDrivers). Per informazioni su come usare il supporto di ripristino di sistema, vedere [utilizzando la procedura guidata Ripristino configurazione di sistema completo](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_Using).  
  
##  <a name="BKMK_CreateBootable"></a>Creare un'unità flash USB avviabile per ripristinare un computer client  
 Se è necessario ripristinare un computer client da un backup esistente, ma non riesce a individuare il CD di ripristino fornito con il server (in Windows Server Essentials) o non si desidera configurare il servizio Ripristino client sul server (in Windows Server Essentials), è possibile creare un'unità flash USB avviabile. È quindi possibile utilizzare l'unità flash USB per avviare il computer client e il ripristino del sistema. L'unità flash USB che usi deve essere almeno 1 GB o superiori.  
  
#### <a name="to-create-a-bootable-usb-flash-drive"></a>Per creare un'unità flash USB avviabile  
  
1.  Aprire il **Dashboard**.  
  
2.  Fare clic su di **dispositivi** scheda.  
  
3.  In **attività** riquadro, fare clic su **le impostazioni di personalizzare il Backup del Computer e cronologia File**.  
  
    > [!NOTE]
    >  In Windows Server Essentials, fare clic su **attività di backup computer Client**.  
  
4.  Fare clic su di **strumenti** scheda e quindi fare clic su **Crea chiave** nel **ripristino del Computer** sezione. Apre la creazione guidata chiave di ripristino Computer.  
  
5.  Inserire un 1 GB o nel server di un'unità flash USB più grande e quindi segui le istruzioni della procedura guidata.  
  
    > [!CAUTION]
    >  Verranno eliminati tutti i dati nell'unità flash USB.  
  
##  <a name="BKMK_Using"></a>Con la procedura guidata Ripristino configurazione di sistema completo  
 Dopo aver usato il ripristino media, servizio Ripristino client o unità flash USB per avviare il computer e verificare che tutti i componenti hardware siano caricati sul computer client nuovo o ripristinato, la procedura guidata Ripristino configurazione di sistema completo viene visualizzato. Questa procedura guidata consente di accedere il server, il backup del computer e i volumi di origine che si desidera ripristinare sul computer ed esegue l'effettivo processo di ripristino.  
  
> [!NOTE]
>   Windows Server Essentials non supporta gli scenari di ripristino seguenti:  
>   
>  -   Computer basato su ripristino di un disco di Record di avvio principale (MBR) per un Unified Extensible Firmware Interface (UEFI).  
> -   Ripristinare un backup UEFI/GPT su un sistema BIOS.  
>   
>  Se si ripristinano i dati in uno di questi scenari, non sarà in grado di avviare il sistema. Inoltre, potrebbe non essere in grado di utilizzare dischi rigidi che sono più grandi di due terabyte di dimensioni.  
  
 **Prerequisiti:**  
  
-   Processo di ripristino prima di avviare il sistema completo, usare un cavo di rete (una connessione cablata) per connettere il computer alla stessa rete del server. Assicurati di avere accesso a tutte le unità disco rigide del computer client.  
  
    > [!WARNING]
    >  Non tentare di eseguire un ripristino completo del sistema a un computer che utilizza una connessione wireless alla rete.  
  
-   Se sai che manca il computer di rete critiche o driver di dispositivo di archiviazione, è necessario individuare e copiarli su un'unità flash prima di avviare il processo di ripristino di sistema completo. Per Windows Server Essentials: Se si utilizza il supporto di ripristino di sistema completo fornito su CD, il CD deve restare nell'unità durante la prima fase del processo di ripristino di sistema completo. Pertanto, non è necessario copiare i driver mancanti su un CD o DVD a meno che non si dispone di una seconda unità CD/DVD. Al contrario, copiare i driver mancanti su un'unità flash USB.  
  
     Per informazioni su come trovare i driver per il computer, vedere [dove posso trovare i driver per l'hardware?](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_FindDrivers)  
  
-   Per Windows Server Essentials: Se non è possibile individuare il CD di ripristino configurazione di Windows Server Essentials, è possibile creare un'unità flash USB avviabile. Per ulteriori informazioni, vedere [creare un'unità flash USB avviabile per ripristinare un computer client](Restore-a-full-system-from-an-existing-client-computer-backup.md#BKMK_CreateBootable).  
  
#### <a name="to-use-the-full-system-restore-wizard"></a>Per utilizzare la procedura guidata Ripristino configurazione di sistema completo  
  
1.  Eseguire una delle operazioni seguenti:  
  
    -   Windows Server Essentials: Attivare il computer client che si desidera ripristinare, inserire il supporto di ripristino e spegnere il computer.  
  
         Riaccendere il computer durante Self Test POST (Power On), premere il tasto funzione appropriato (tasto-F) per accedere al Menu dispositivo di avvio e quindi seleziona l'unità CD/DVD. L'avvio di Windows Boot Manager.  
  
    -   Windows Server Essentials: Se si utilizza il servizio Ripristino client, riavviare il computer con il **avvio dalla rete** opzione. In caso contrario, avviare il computer con la chiave USB.  
  
         Riaccendere il computer e durante Self Test POST (Power On), premere il tasto funzione appropriato (tasto-F) per accedere al Menu dispositivo di avvio e quindi seleziona **avvio dalla rete** (o è possibile scegliere di eseguire l'avvio dalla chiave USB). L'avvio di Windows Boot Manager.  
  
    > [!NOTE]
    >  Controllare la documentazione fornita dal produttore del computer per determinare il tasto funzione che consente di accedere al Menu dispositivo di avvio.  
  
2.  Il supporto di ripristino di computer contiene (x86) 32 bit e opzioni di avvio a 64 bit (x64). In Windows Boot Manager, scegliere **ripristino configurazione di sistema completo (x86)** o **ripristino configurazione di sistema completo (x64)**. Se i driver dell'hardware computer sono a 32 bit, scegliere x86; Se sono a 64 bit, scegliere x64. Caricamento dei file di Windows, e la procedura guidata Ripristino configurazione di sistema completo esegue una verifica per assicurarsi che siano disponibili tutti i driver hardware.  
  
3.  Nel **procedura guidata Ripristino configurazione di sistema completo** finestra, scegliere la lingua desiderata e quindi fare clic sulla freccia.  
  
4.  Scegliere il **formato ora e valuta**e **tastiera o metodo di input** per questo computer. Fare clic su **continuare**.  
  
5.  Se mancano alcuni driver, viene visualizzato il messaggio che il processo di ripristino Impossibile verificare i driver. Fare clic su **Chiudi**e quindi nella finestra di dialogo iniziale scegliere **caricare driver**.  
  
    1.  Nel **rileva Hardware** la finestra di dialogo, fare clic su **installare driver**.  
  
    2.  Inserisci l'unità flash USB che contiene i driver dell'hardware, quindi il **installa driver** nella finestra di dialogo fare clic su **analisi**.  
  
    3.  Nel **installa driver** la finestra di dialogo, fare clic su **OK** quando vengono trovati i driver.  
  
    4.  Nel **rileva Hardware** la finestra di dialogo, fare clic su **continua**.  
  
6.  Se tutti i driver sono stati rilevati durante il controllo iniziale o quando sono installati tutti i driver critici, scegliere il **ripristino completo del sistema** finestra, fare clic su **continua**.  
  
7.  Nel **iniziale per la procedura guidata Ripristino configurazione di sistema completo** pagina, fare clic su **Avanti**.  
  
8.  La procedura guidata consente di cercare il server.  
  
    1.  Se la procedura guidata è possibile individuare il server, è l'opzione per ripetere la ricerca o per immettere l'indirizzo IP del server.  
  
    2.  Se sono stati rilevati più server, verrà chiesto di selezionarne uno.  
  
    3.  Se il server è disponibile, il **accedere a < YourServerName\ >** verrà visualizzata la pagina.  
  
9. Nel **accedere a < YourServerName\ >** digitare *< AdministratorAccountName\ >* nel **nome utente** casella di testo e la password dell'account amministratore nel **Password** casella di testo, quindi fare clic su **Avanti**.  
  
    > [!IMPORTANT]
    >  È necessario utilizzare un account amministratore creato in inglese. Se non hai uno, è necessario creare un nuovo account amministratore. A tale scopo, aprire il **utenti** scheda nel dashboard di server, quindi impostare il formato di lingua della tastiera su inglese e quindi eseguire il **aggiungere un account utente** attività per creare l'account amministratore. Successivamente, utilizzare il nuovo account amministratore per continuare a ripristinare il computer client.  
  
10. Nel **selezionare un computer da ripristinare** pagina, selezionare il computer che si desidera ripristinare e quindi fare clic su **Avanti**. È possibile scegliere **< ComputerName\ >: (questo computer)** o un diverso computer in rete dal **un altro computer** dall'elenco a discesa.  
  
    > [!NOTE]
    >  Se si tratta di un computer che è noto al server (ad esempio, un nuovo o un computer riassegnato), il **computer** opzione non è disponibile.  
  
11. Nel **selezionare un backup da ripristinare** pagina, esaminare l'elenco di backup disponibili e selezionare quello che si desidera ripristinare sul computer.  
  
    > [!NOTE]
    >  Si consiglia di selezionare un backup completato correttamente (segno di spunta verde). Questo contribuisce a garantire che tutti i file di sistema e i dati vengono ripristinati correttamente.  
  
12. (*Facoltativo*) Selezionare un backup e quindi fare clic su **dettagli** per aprire il **dettagli Backup** pagina e visualizzare ulteriori informazioni sul backup. Utilizzare le informazioni sul **dettagli Backup** pagina per confrontare più backup e decidere quali backup è la scelta migliore. Fare clic su **Chiudi** nel **dettagli Backup** per tornare alla pagina di **selezionare un backup da ripristinare** pagina.  
  
13. Nel **selezionare un backup da ripristinare** pagina, selezionare un backup e quindi fare clic su di **Avanti** pulsante.  
  
14. Nel **selezionare opzione di ripristino** pagina, fare clic su una delle operazioni seguenti e quindi fare clic su **Avanti**.  
  
    > [!NOTE]
    >  Questa pagina non viene visualizzata se il partizionamento automatico non è supportato.  
  
    1.  **Automaticamente la procedura guidata di ripristinare completamente il computer (scelta consigliato)**. Questa opzione consente di garantire che il computer sarà ripristinato allo stato in cui si trovava prima la data e ora del backup scelto. Se si sceglie questa opzione, andare al passaggio 15.  
  
    2.  **Consenti la selezione volumi da ripristinare (avanzata)**. Questa opzione consente di scegliere i volumi che si desidera ripristinare e in cui vuoi ripristinarli. È inoltre possibile creare partizioni sul disco rigido.  
  
15. Nel **selezionare i volumi da ripristinare** pagina, è possibile scegliere i volumi che si desidera ripristinare.  
  
    > [!NOTE]
    >  Questa pagina viene visualizzata se sono presenti più dischi rigidi nel computer di origine di backup o se l'unità di destinazione di ripristino è meno spazio di archiviazione rispetto all'unità di backup di origine.  
  
    1.  La procedura guidata tenta di individuare i volumi di origine e di destinazione. È necessario verificare che il mapping predefinito sia corretto.  
  
        1.  Per deselezionare un volume, fare clic sulla freccia del menu elenco per il volume e quindi fare clic su **Nessuno**.  
  
        2.  Dopo aver selezionato i volumi, fare clic su **Avanti**.  
  
    2.  Se il volume di origine e il volume di destinazione sono le stesse dimensioni o se la dimensione di origine è inferiore rispetto alla destinazione, tra i due verrà visualizzata una freccia verde. Se è presente una mancata corrispondenza dimensioni del volume (dove il volume di origine è maggiore il volume di destinazione), tra l'origine e di destinazione verrà visualizzata una X rossa.  
  
        > [!NOTE]
        >  Una X rossa viene visualizzata anche quando:  
        >   
        >  -   Le dimensioni del settore del disco del volume di origine non corrispondano la dimensione di settore del disco del volume di destinazione. Ciò può verificarsi se si sostituisce il disco fisico con un disco che ha una dimensione di settore diverse, oppure se si configurano spazi di archiviazione (che potrebbero avere una dimensione di settore diverse rispetto a quello del disco fisico).  
        > -   Raggiungi il limite del numero di cluster. Per ripristinare il volume di origine per il volume di destinazione, è necessario formattare il volume di destinazione con le stesse dimensioni di cluster come volume di origine. Se il volume di destinazione è troppo grande, e se la dimensione del cluster è troppo piccola, è possibile raggiungere limite del numero di cluster.  
  
        1.  Fare clic su **Esegui Gestione disco (avanzata)**e creare un nuovo volume che ha le stesse dimensioni del volume riservato per il sistema.  
  
            > [!NOTE]
            >  Se un computer client è Unified Extensible Firmware Interface (UEFI) in base, è necessario utilizzare il **diskpart** strumento per inizializzare il disco di sistema. A tale scopo, aprire una finestra di comando (tenere premuto Ctrl + Alt + Maiusc per 5 secondi nell'ambiente WinPE), eseguire **diskpart.exe**, e quindi eseguire i comandi diskpart seguenti:  
            >   
            >  1.  **DISKPART > disco elenco**  
            > 2.  **DISKPART > select disk #***< disk\ >*  
            > 3.  **DISKPART > pulita**  
            > 4.  **DISKPART > convertire gpt**  
            > 5.  **DISKPART > creare partizione efi dimensioni =***100* (in cui *100* è un esempio di dimensione di partizione in MB, deve essere uguale a quella della partizione originale)  
            > 6.  **DISKPART > creare partizione msr dimensioni =***128* (in cui *128* è un esempio di dimensione di partizione in MB, deve essere uguale a quella della partizione originale)  
            > 7.  **DISKPART > uscire da**  
  
        2.  *(Facoltativo) *Selezionare l'opzione **non assegnare una lettera di unità o percorso unità**.  
  
        3.  Formattare il volume come **NTFS**.  
  
        4.  Al termine di formattazione, fare doppio clic su nuovo volume di sistema, quindi fare clic su **Contrassegna partizione come attiva**.  
  
        5.  Se sono necessari ulteriori volumi che devono corrispondere ad altri volumi nel backup, ripetere i passaggi *ii* tramite *iv* per creare e attivare i volumi e quindi chiudere **Gestione disco**.  
  
        6.  Nel **selezionare i volumi da ripristinare** pagina, associare il sistema riservato volume di origine di backup per il volume della stessa dimensione creato nel passaggio *v*.  
  
        7.  Eseguire il mapping di tutti gli altri volumi di origine per i volumi di destinazione corrispondenti.  
  
        8.  Fare clic su **Avanti** per continuare con il ripristino.  
  
16. Nel **confermare i volumi da ripristinare** pagina, esaminare il mapping e quindi fare clic su **Avanti**. Se è necessario apportare modifiche, fare clic su **Indietro**e quindi ripetere il passaggio 14.  
  
17. Il **ripristino < ComputerName\ > da < data e ora di backup\ >** pagina indica l'avanzamento del processo di ripristino.  
  
18. Nel **il ripristino è stata completata correttamente** pagina, rimuovere il supporto di ripristino e quindi fare clic su **fine**. Il riavvio del computer.  
  
    > [!IMPORTANT]
    >  Se la crittografia unità BitLocker è stata abilitata nel computer prima del ripristino, è necessario attivare BitLocker manualmente dopo il riavvio del computer.  
  
##  <a name="BKMK_FindDrivers"></a>Dove posso trovare i driver per l'hardware?  
 A seconda dell'hardware del computer nuovo o ripristinato, il supporto di ripristino potrebbe non includere tutto lo spazio di archiviazione e che sono necessari quando si riavvia il computer ripristinato driver della scheda di rete. È necessario determinare quali driver mancanti, individuare i driver nei supporti esistenti o sul sito Web s produttore, copiarli su un'unità flash e quindi copiarli dall'unità flash al nuovo o ripristino computer quando si esegue la procedura guidata Ripristino configurazione di sistema completo.  
  
 Quando un computer viene eseguito il backup, i driver per il computer vengono salvati nel backup. Se il supporto di ripristino non include tutti i driver necessari, è possibile aprire una copia di backup per il computer e quindi copiare i driver su un'unità flash USB.  
  
#### <a name="to-copy-drivers-from-a-backup-to-a-usb-flash-drive"></a>Per copiare i driver da un backup in un'unità flash USB  
  
1.  In un altro computer, aprire il Dashboard.  
  
2.  Fare clic su **dispositivi**e quindi fare clic sul computer per cui sono necessari i driver.  
  
3.  Fare clic su **ripristinare file o cartelle per il computer**. Il ripristino di file o cartelle guidata verrà visualizzata.  
  
4.  Fare clic sul backup più recente ha esito positivo, quindi fare clic su **Avanti**.  
  
5.  Fare clic su un volume per aprire e quindi fare clic su **Avanti**. Verrà visualizzata una finestra in cui sono elencati i file e cartelle nel backup.  
  
6.  Inserisci l'unità flash USB in un connettore USB nel computer e quindi copiare i driver per ripristino completo del sistema cartella nell'unità flash USB.  
  
    > [!NOTE]
    >  Potrebbe essere necessario fare clic su **di un livello** fino a quando non si raggiunge la radice del volume di sistema.  
  
7.  Rimuovere l'unità flash e quindi inserirla nel computer che si desidera ripristinare.  
  
 È possibile utilizzare l'unità flash USB per installare i driver per il computer da ripristinare. Il ripristino di file o la procedura guidata cartelle Cerca altri driver su questa unità flash USB mentre si utilizza la procedura guidata Ripristino configurazione di sistema completo. Il driver che con maggiore probabilità saranno necessari sono i driver della scheda di rete e i driver di dispositivo di archiviazione.  
  
## <a name="see-also"></a>Vedere anche  
  
-   [Gestire Backup e ripristino](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Gestire Windows Server Essentials](Manage-Windows-Server-Essentials.md)
