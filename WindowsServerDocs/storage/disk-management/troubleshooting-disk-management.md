---
title: Risoluzione dei problemi relativi a Gestione disco
description: Questo articolo descrive come risolvere i problemi relativi a Gestione disco
ms.date: 12/20/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 7eeb462d31391a228ec0e89afb09673ef14b51cf
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "79323443"
---
# <a name="troubleshooting-disk-management"></a>Risoluzione dei problemi relativi a Gestione disco

> **Si applica a:** Windows 10, Windows 8.1, Windows 7, Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento elenca alcuni problemi comuni che potresti riscontrare quando usi Gestione disco e i passaggi da tentare per la risoluzione.

> [!TIP]
> Se ricevi un errore o qualcosa non funziona nel seguire queste procedure, non lasciarti prendere dal panico. Quanto descritto in questo argomento è solo il primo tentativo da effettuare. Sono disponibili moltissime informazioni sul sito della [community Microsoft](https://answers.microsoft.com/en-us/windows) nella sezione [File, cartelle e archiviazione in linea](https://answers.microsoft.com/en-us/windows/forum/windows_10-files?sort=lastreplydate&dir=desc&tab=All&status=all&mod=&modAge=&advFil=&postedAfter=&postedBefore=&threadType=all&isFilterExpanded=true&tm=1514405359639) relative all'ampia gamma di configurazioni hardware e software che puoi gestire. Se hai ancora bisogno di aiuto, inserisci una domanda o [contatta il Supporto tecnico Microsoft](https://support.microsoft.com/contactus/) o il produttore dell'hardware.

## <a name="how-to-open-disk-management"></a>Come aprire Gestione disco

Prima di iniziare a eseguire le operazioni più complesse, ecco un modo semplice per aprire Gestione disco, in caso non sia già aperto:

1. Digita **Gestione computer** nella casella di ricerca sulla barra delle applicazioni, seleziona e tieni premuto (oppure fai clic con il pulsante destro del mouse) **Gestione computer** e quindi scegli **Esegui come amministratore** > **Sì**.
2. Dopo l'apertura di Gestione computer, passa ad **Archiviazione** > **Gestione disco**.

## <a name="disks-that-are-missing-or-not-initialized-plus-general-troubleshooting-steps"></a>Dischi mancanti o non inizializzati, più passaggi generali per la risoluzione dei problemi

![Gestione disco che mostra un disco sconosciuto che deve essere inizializzato.](media/uninitialized-disk.PNG)

**Causa**: se uno dei dischi non è visualizzato in Esplora file ed è elencato in Gestione disco come *Non inizializzato*, è possibile che il disco non abbia una firma valida. In pratica questo significa che il disco non è mai stato inizializzato e formattato o che la formattazione ha danneggiato l'unità. 

È anche possibile che il disco abbia problemi hardware o di inserimento, ma torneremo a questo argomento tra pochi paragrafi.

**Soluzione:**   se l'unità è nuova e deve essere solo inizializzata, cancellando tutti i dati presenti, la soluzione è semplice: vedi [Inizializzare nuovi dischi](initialize-new-disks.md). Tuttavia, è piuttosto probabile che tu abbia già provato questa soluzione, senza successo. Oppure il disco potrebbe contenere molti file importanti, che non vuoi cancellare inizializzando il disco.

I motivi per cui un disco o una scheda di memoria può risultare mancante o l'inizializzazione può non riuscire sono diversi, il più comune è dovuto a un errore del disco. Le operazioni disponibili per correggere un disco non funzionante sono molte, ma ecco alcuni passaggi da provare per provare a farlo funzionare di nuovo. Se il disco funziona dopo uno di questi passaggi, puoi evitare i successivi. Non dovrai far altro che rilassarti, festeggiare e magari aggiornare i backup.

1. Osserva il disco in Gestione disco. Se viene visualizzato come *Offline*, come mostrato di seguito, prova a fare clic con il pulsante destro del mouse e a scegliere **Online**.

    ![Disco mostrato come offline](media/offline-disk.png)
2. Se il disco viene visualizzato in Gestione disco come *Online* e ha una partizione primaria elencata come *Integro*, come mostrato di seguito, si tratta di un buon segno.

    ![Disco indicato come online con un volume integro](media/healthy-volume.png)
    - Se una partizione include un file system, ma senza lettera di unità (ad esempio E:), vedi [Modificare una lettera di unità](change-a-drive-letter.md) per aggiungere manualmente una lettera di unità.
    - Se una partizione non include un file system (elencato come RAW anziché NTFS, ReFS, FAT32 o exFAT) e sai che il disco è vuoto, seleziona e tieni premuta (oppure fai clic con il pulsante destro del mouse) la partizione e scegli **Formatta**. Poiché la formattazione di un disco comporta la cancellazione di tutti i dati presenti nel disco stesso, evita questa operazione se stai provando a recuperare file dal disco e continua con il passaggio successivo.
    - Se la partizione è elencata come *Non allocata* e sai che la partizione è vuota, seleziona e tieni premuta (oppure fai clic con il pulsante destro del mouse) la partizione non allocata e quindi scegli **Nuovo volume semplice** e segui le istruzioni per creare un volume nello spazio disponibile. Evita questa operazione se stai provando a recuperare file da questa partizione e continua invece con il passaggio successivo.

    > [!NOTE]
    > Ignora tutte le partizioni elencate come **Partizione di sistema EFI** o **Partizione di ripristino**. Queste partizioni contengono file molto importanti, necessari per il corretto funzionamento del PC. È consigliabile lasciarle separate in modo che possano svolgere il proprio lavoro durante l'avvio del PC e il ripristino da eventuali problemi.
3. Se hai un disco esterno che non viene visualizzato, scollega il disco, ricollegalo e quindi seleziona **Azione** > **Ripeti analisi dischi**. 
4. Arresta il PC, disattiva il disco rigido esterno (se si tratta di un disco esterno con cavo di alimentazione) e quindi accendi di nuovo il PC e riattiva il disco.
    Per spegnere il PC in Windows 10, seleziona il pulsante Start, seleziona il pulsante di accensione e quindi **Arresta**.
5. Collega il disco tramite una porta USB diversa che si trovi direttamente sul PC (non in un hub).
    A volte i dischi USB non ricevono alimentazione sufficiente da alcune porte oppure hanno altri problemi con certe porte specifiche. Questo avviene in particolare con gli hub USB, ma a volte esistono differenze tra le porte su un PC e di conseguenza è utile provarne una diversa, se disponibile.
6. Prova un cavo diverso.
    Può sembrare strano, ma i cavi spesso smettono di funzionare. Per questo motivo, prova un cavo diverso per collegare il disco. Se hai un disco interno in un PC desktop, probabilmente dovrai arrestare il PC prima di cambiare il cavo. Per informazioni, vedi il manuale del PC.
7. Controlla Gestione dispositivi per verificare l'eventuale presenza di problemi.
    Seleziona e tieni premuto (oppure fai clic con il pulsante destro del mouse) il pulsante Start e quindi scegli Gestione dispositivi dal menu contestuale. Cerca i dispositivi con un punto esclamativo accanto o con altri problemi, fai doppio clic sul dispositivo e quindi leggine lo stato.

    Ecco un elenco di [codici di errore in Gestione dispositivi](https://support.microsoft.com/help/310123/error-codes-in-device-manager-in-windows), ma un approccio a volte utile consiste nel selezionare e tenere premuto (oppure fare clic con il pulsante destro del mouse) il dispositivo problematico, scegliere **Disinstalla dispositivo** e quindi **Azione** > **Rileva modifiche hardware**.

    ![Dispositivo USB sconosciuto indicato in Gestione dispositivi](media/device-manager.PNG)
8. Collega il disco a un computer diverso.
    
    Se il disco non funziona in un altro PC, questo è un buon indizio del fatto che il problema riguarda il disco e non il PC. In effetti, questa non è una buona notizia. Esistono altri passaggi che puoi provare quando si verifica l'errore relativo a un'unità USB esterna [È necessario inizializzare un disco per consentire a Gestione dischi logici di accedervi](https://social.technet.microsoft.com/Forums/windows/en-US/2b069948-82e9-49ef-bbb7-e44ec7bfebdb/forum-faq-external-usb-drive-error-you-must-initialize-the-disk-before-logical-disk-manager-can?forum=w7itprohardware), ma può essere il momento giusto per eseguire una ricerca del problema e chiedere aiuto nel sito della [community Microsoft](https://answers.microsoft.com/en-us/windows/forum/windows_10-files?sort=lastreplydate&dir=desc&tab=All&status=all&mod=&modAge=&advFil=&postedAfter=&postedBefore=&threadType=all&isFilterExpanded=true&tm=1514405359639) oppure per contattare il produttore del disco o il [Supporto tecnico Microsoft](https://support.microsoft.com/contactus/).

    Se non riesci a ripristinare il funzionamento del disco, sono disponibili alcune app che puoi usare per provare a recuperare i dati da un disco che non funziona o, se i file sono davvero importanti, puoi ricorrere a un laboratorio di recupero dati a pagamento. Se trovi una soluzione, segnalacela nella sezione dei commenti di seguito.

> [!IMPORTANT]
> Poiché gli errori dei dischi sono piuttosto comuni, è importante eseguire regolarmente il backup di tutti i file importanti. Se uno dei tuoi dischi a volte non viene visualizzato o restituisce errori, ricordati di controllare i tuoi metodi di backup. Se resti leggermente indietro non è un grosso problema, capita a tutti prima o poi. La migliore soluzione di backup è quella che viene utilizzata e di conseguenza ti incoraggiamo a trovarne una appropriata per la tua situazione e di usarla regolarmente.
> 
> [!TIP]
> Per informazioni su come usare le app integrate in Windows per eseguire il backup dei file in un'unità esterna, ad esempio un'unità USB, vedi [Backup e ripristino dei file](https://support.microsoft.com/help/17143/windows-10-back-up-your-files). Puoi anche salvare i file in Microsoft OneDrive, che ne esegue la sincronizzazione dal PC al cloud. Se il disco rigido non funziona, potrai comunque recuperare tutti i file archiviati in OneDrive da OneDrive.com. Per altre informazioni, vedi [OneDrive nel PC](https://support.microsoft.com/help/17184/windows-10-onedrive).

## <a name="a-basic-or-dynamic-disks-status-is-unreadable"></a>Lo stato di un disco di base o dinamico è Illeggibile

**Causa:**   il disco di base o dinamico non è accessibile e il problema può essere dovuto a un errore hardware, a un danno o a errori di I/O. La copia del disco del database di configurazione del disco di sistema può essere danneggiata. Un'icona di errore viene visualizzata accanto ai dischi con stato **Illeggibile**.

I dischi possono anche indicare lo stato **Illeggibile** durante lo spinup o quando Gestione disco esegue di nuovo l'analisi di tutti i dischi nel sistema. In alcuni casi, non è possibile recuperare un disco illeggibile che restituisce un errore. Per i dischi dinamici, lo stato **illeggibile** è in genere dovuto a danni o errori di I/O su una parte del disco, anziché al danneggiamento dell'intero disco.

**Soluzione:**   esegui di nuovo l'analisi dei dischi o riavvia il computer per verificare se lo stato del disco cambia. Prova anche i passaggi di risoluzione dei problemi descritti in [Lo stato di un disco è Non inizializzato o il disco è mancante](#disks-that-are-missing-or-not-initialized-plus-general-troubleshooting-steps).

## <a name="a-dynamic-disks-status-is-foreign"></a>Lo stato di un disco dinamico è Esterno

**Causa:**  viene indicato lo stato **Esterno** quando sposti un disco dinamico nel computer locale da un altro PC. Un'icona di avviso viene visualizzata accanto ai dischi con stato **Esterno**.

In alcuni casi, anche un disco che è stato precedentemente connesso al sistema può mostrare lo stato **Esterno**. Poiché i dati di configurazione per i dischi dinamici vengono archiviati in tutti i dischi dinamici, le informazioni relative a quali dischi sono di proprietà del sistema vengono perse se tutti i dischi dinamici restituiscono errori.

**Soluzione:**   aggiungi il disco alla configurazione di sistema del computer in modo da poter accedere ai dati sul disco. Per aggiungere un disco alla configurazione di sistema del computer, importa il disco esterno (selezionando e tenendo premuto (oppure facendo clic con il pulsante destro del mouse) il disco e quindi scegliendo **Importazione dischi esterni**). I volumi esistenti sul disco esterno diventano visibili e accessibili quando importi il disco. 

## <a name="a-dynamic-disks-status-is-online-errors"></a>Lo stato di un disco dinamico è Online (errori)

**Causa:**   il disco dinamico presenta errori di I/O in un'area del disco. Un'icona di avviso viene visualizzata accanto ai dischi dinamici con errori.

**Soluzione:**    se gli errori di I/O sono temporanei, riattiva il disco per ripristinarne lo stato **Online**.

## <a name="a-dynamic-disks-status-is-offline-or-missing"></a>Lo stato di un disco dinamico è Offline o Mancante

**Causa:**   un disco dinamico con stato **Offline** può essere danneggiato o non disponibile in modo intermittente. Un'icona di errore viene visualizzata accanto al disco dinamico offline.

Se lo stato del disco è **Offline** e il nome del disco cambia in **Mancante**, il disco è stato disponibile di recente nel sistema, ma non può più essere individuato o identificato. Il disco mancante può essere danneggiato, spento o disconnesso.

**Soluzione:** per portare di nuovo online un disco con stato Offline o Mancante:

1. Risolvi gli eventuali problemi relativi al disco, al controller o ai cavi. 
2. Assicurati che il disco fisico sia acceso, alimentato e collegato al computer. 
3. Usa quindi il comando **Riattiva disco specificato** per riportare online il disco.
4. Prova i passaggi di risoluzione dei problemi descritti in [Lo stato di un disco è Non inizializzato o il disco è mancante](#disks-that-are-missing-or-not-initialized-plus-general-troubleshooting-steps).
5. Se lo stato del disco resta **Offline** e il nome del disco resta **Mancante** e ritieni che non sia possibile risolvere il problema del disco, puoi rimuovere il disco dal sistema selezionando e tenendo premuto (oppure facendo clic con il pulsante destro del mouse) il disco e quindi scegliendo **Rimuovi disco**. Tuttavia, prima di poter rimuovere il disco, devi eliminare tutti i volumi (o mirror) sul disco. Puoi salvare i volumi con mirroring nel disco rimuovendo il mirror anziché l'intero volume. Poiché l'eliminazione di un volume comporta l'eliminazione dei dati presenti nel volume, devi rimuovere un disco solo se hai la certezza assoluta che il disco sia danneggiato e inutilizzabile in modo permanente.

**Per portare Online un disco Offline il cui nome è ancora Disco \# (non mancante), prova una o più delle procedure seguenti:**

1. In Gestione disco seleziona e tieni premuto (oppure fai clic con il pulsante destro del mouse) il disco e quindi scegli **Riattiva disco specificato** per riportare il disco online. Se lo stato del disco resta **Offline**, controlla i cavi e il controller del disco e assicurati che il disco fisico sia integro. Correggi eventuali problemi e prova a riattivare il disco. Se la riattivazione del disco riesce, i volumi sul disco tornano automaticamente allo stato **Integro**.
2. In Visualizzatore eventi controlla la presenza nei registri eventi di eventuali errori correlati al disco, ad esempio "No good config copies". Se il registro eventi contiene questo errore, contatta il [Servizio Supporto Tecnico Clienti Microsoft](https://msdn.microsoft.com/library/aa263468(v=vs.60).aspx).

3. Prova a spostare il disco in un altro computer. Se in un altro computer il disco torna allo stato **Online**, il problema è probabilmente dovuto alla configurazione del computer in cui il disco è indicato come **Online**.

4. Prova a spostare il disco in un altro computer in cui siano presenti dischi dinamici. Importa il disco nel computer e quindi sposta di nuovo il disco nel computer in cui non è indicato come **Online**. 

## <a name="a-basic-or-dynamic-volumes-status-is-failed"></a>Lo stato di un volume di base o dinamico è Non riuscito

**Causa:**    il volume di base o dinamico non viene avviato automaticamente oppure il disco o il file system è danneggiato. A meno che non sia possibile ripristinare il disco o il file system, lo stato **Errore** indica una perdita di dati.

**Soluzione:**

se si tratta di un volume di base con stato **Non riuscito**:

- Verifica che il disco fisico sottostante sia acceso, alimentato correttamente e collegato al computer.
- Prova i passaggi di risoluzione dei problemi descritti in [Lo stato di un disco è Non inizializzato o il disco è mancante](#disks-that-are-missing-or-not-initialized-plus-general-troubleshooting-steps).

Se si tratta di un volume dinamico con stato **Non riuscito**:

-   Verifica che i dischi sottostanti siano online. In caso contrario, riporta i dischi allo stato **Online**. Se questa operazione riesce, il volume viene automaticamente riavviato e torna allo stato **Integro**. Se il disco dinamico torna allo stato **Online**, ma il volume dinamico non torna allo stato **Integro**, puoi riattivare il volume manualmente.
-   Se il volume dinamico è un volume con mirroring o RAID-5 con dati obsoleti, non verrà riavviato automaticamente quando riporti il disco sottostante allo stato online. Se i dischi che contengono dati aggiornati vengono disconnessi, porta online prima di tutto questi dischi (per consentire la sincronizzazione dei dati). Altrimenti, riavvia manualmente il volume con mirroring o RAID-5 e quindi esegui lo strumento per il controllo degli errori o Chkdsk.exe.
- Prova i passaggi di risoluzione dei problemi descritti in [Lo stato di un disco è Non inizializzato o il disco è mancante](#disks-that-are-missing-or-not-initialized-plus-general-troubleshooting-steps).

## <a name="a-basic-or-dynamic-volumes-status-is-unknown"></a>Lo stato di un volume di base o dinamico è Sconosciuto

**Causa:**   viene indicato lo stato **Sconosciuto** quando il settore di avvio per il volume è danneggiato (possibilmente a causa di un virus) e non puoi più accedere ai dati nel volume. Lo stato **Sconosciuto** viene visualizzato anche quando installi un nuovo disco, ma senza completare la procedura guidata per creare una firma del disco.

**Soluzione:**   inizializza il disco. Per istruzioni, vedi [Inizializzare nuovi dischi](initialize-new-disks.md).

## <a name="a-dynamic-volumes-status-is-data-incomplete"></a>Lo stato di un volume dinamico è Dati incompleti

**Causa**: hai spostato alcuni ma non tutti i dischi in un volume multidisco. I dati su questo volume verranno eliminati definitivamente, a meno che non sposti e importi gli altri dischi che contengono il volume.

**Soluzione:**

1. sposta nel computer tutti i dischi che costituiscono il volume multidisco.
2. Importa i dischi. Per istruzioni su come spostare e importare dischi, vedi [Spostare dischi in un altro computer](move-disks-to-another-computer.md).

Se il volume multidisco non è più necessario, puoi importare il disco e creare nuovi volumi al suo interno. A tale scopo:

1. Seleziona e tieni premuto (oppure fai clic con il pulsante destro del mouse) il volume con stato **Errore** o **Errore di ridondanza** e quindi scegli **Elimina volume**.
2. Seleziona e tieni premuto (oppure fai clic con il pulsante destro del mouse) il disco e quindi scegli **Nuovo volume**.

## <a name="a-dynamic-volumes-status-is-healthy-at-risk"></a>Lo stato di un volume dinamico è Integro (a rischio)

**Causa:**   indica che il volume dinamico è attualmente accessibile, ma sono stati rilevati errori di I/O del disco dinamico sottostante. Se viene rilevato un errore di I/O in qualsiasi parte di un disco dinamico, per tutti i volumi nel disco viene indicato lo stato **Integro (a rischio)** con un'icona di avviso visualizzata sul volume.

Quando lo stato del volume è **Integro (a rischio)** , lo stato di un disco sottostante è in genere **Online (errori)** .

**Soluzione:**  
1. riporta il disco sottostante allo stato **Online**. Una volta riportato il disco allo stato **Online**, il volume dovrebbe tornare allo stato **Integro**. Se lo stato **Integro (a rischio)** persiste, il disco potrebbe essere danneggiato. 

2. Esegui il backup dei dati e sostituisci il disco non appena possibile.

## <a name="cannot-manage-striped-volumes-using-disk-management-or-diskpart"></a>Non è possibile gestire volumi con striping tramite Gestione disco o DiskPart

**Causa:**    alcuni prodotti di gestione dei dischi non Microsoft sostituiscono Gestione dischi logici di Microsoft per la gestione avanzata dei dischi. Questo può causare la disabilitazione di Gestione dischi logici.

**Soluzione:**    se usi un software di gestione dei dischi non Microsoft che ha causato la disabilitazione di Gestione dischi logici, dovrai contattare il fornitore del software di gestione dei dischi non Microsoft per assistenza nella risoluzione dei problemi relativi alla configurazione del disco.

## <a name="disk-management-cannot-start-the-virtual-disk-service"></a>Gestione disco non è in grado di avviare Servizio dischi virtuali

**Causa:**   se un computer remoto non supporta Servizio dischi virtuali (VDS) o se non riesci a stabilire una connessione al computer remoto a causa di un blocco di Windows Firewall, potresti ricevere questo errore.

**Soluzione:**

1. se il computer remoto supporta VDS, puoi configurare Windows Defender Firewall in modo da consentire le connessioni VDS. Se il computer remoto non supporta VDS, puoi usare Connessione Desktop remoto per stabilire la connessione al computer remoto e quindi eseguirvi Gestione disco direttamente.
2. Per gestire i dischi in computer remoti che supportano VDS, devi configurare Windows Defender Firewall sia nel computer locale (su cui viene eseguito Gestione disco) sia nel computer remoto.
3. Nel computer locale configura Windows Defender Firewall in modo da abilitare l'eccezione Gestione volumi remota.

> [!NOTE]
> L'eccezione Gestione volumi remota include le eccezioni per Vds.exe, Vdsldr.exe e la porta TCP 135.

> [!NOTE]
> Le connessioni remote in gruppi di lavoro non sono supportate. Il computer locale e il computer remoto devono essere entrambi membri di un dominio.

Vedi anche

- [Liberare spazio sull'unità in Windows 10](https://support.microsoft.com/help/12425/windows-10-free-up-drive-space)