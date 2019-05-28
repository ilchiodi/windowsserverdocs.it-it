---
title: Risoluzione dei problemi relativi a Gestione disco
description: Questo articolo descrive come risolvere i problemi di Gestione disco
ms.date: 12/22/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c234828706d999fe049626a2fd98db70e612766f
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192745"
---
# <a name="troubleshooting-disk-management"></a>Risoluzione dei problemi relativi a Gestione disco

> **Si applica a:** Windows 10, Windows 8.1, Windows 7, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento sono elencati alcuni problemi comuni che possono verificarsi quando si utilizza Gestione disco.

> [!TIP]
> Se si verifica un errore o qualcosa non funziona quando si seguono queste procedure: nessun problema È una considerevole quantità di informazioni [della community di Microsoft](https://answers.microsoft.com/en-us/windows) del sito - prova a cercare il [i file, cartelle e archiviazione](https://answers.microsoft.com/en-us/windows/forum/windows_10-files?sort=lastreplydate&dir=desc&tab=All&status=all&mod=&modAge=&advFil=&postedAfter=&postedBefore=&threadType=all&isFilterExpanded=true&tm=1514405359639) sezione e se è ancora necessaria assistenza, pubblicare una domanda e Microsoft o altri membri del community tenterà di Guida in linea. Se hai commenti e suggerimenti su come migliorare questi argomenti, saremo lieti di rispondere! Rispondere solo il *è utile questa pagina?* richiesto e lasciare eventuali commenti non esiste o nel thread di pubblico di commenti nella parte inferiore di questo argomento.

## <a name="a-disks-status-is-not-initialized-or-the-disk-is-missing"></a>Stato del disco non inizializzato o il disco manca

![Gestione disco che mostra un disco sconosciuto che deve essere inizializzato.](media\uninitialized-disk.PNG)

**Causa:** Se si dispone di un disco non viene visualizzato in Esplora File ed è elencato in Gestione disco come *non inizializzata*, è possibile che il disco non ha una firma valida. In sostanza, ciò significa che il disco sia mai stato inizializzato e formattato, o la formattazione del disco è stata danneggiata in qualche modo. 

È anche possibile che il disco ha riscontrato problemi hardware o problemi di inserimento di, ma diciamo che in alcuni paragrafi più sopra.

**Soluzione:**   se l'unità è nuovo e deve essere inizializzata e cancellazione di tutti i dati su di esso, la soluzione è semplice, vedere [inizializzare nuovi dischi](initialize-new-disks.md). Tuttavia, è un probabile hai già provato questa e non funzionava. O forse si dispone di un disco pieno di file importanti e non si desidera cancellare il disco dall'inizializzazione.

Esistono una serie di motivi un disco potrebbe essere mancante o non è possibile inizializzare, con un comune motivo perché il disco ha esito negativo. Solo moltissime operazioni da eseguire per risolvere un disco guasto, ma Ecco alcune procedure per tentare di vedere se è possibile ottenere il funzionamento anche in questo caso. Se il disco appropriato dopo che uno di questi passaggi, non preoccuparsi con i passaggi successivi, semplicemente avviare nuovamente, festeggiare e forse aggiornare le copie di backup.

1. Esaminare il disco in Gestione disco. Se viene visualizzato *Offline* come illustrato in questo caso, provare a facendovi e selezionando **Online**.

    ![Disco appare offline](media/offline-disk.png)
1. Se il disco viene visualizzato in Gestione disco come *Online*, e ha una partizione primaria elencato come *integro*, come illustrato di seguito, che è un buon segno.

    ![Disco indicato come online con un volume integro](media/healthy-volume.png)
    - Se la partizione è un file system, ma senza lettera di unità (ad esempio, /e:), vedere [modifica di una lettera di unità](change-a-drive-letter.md) per aggiungere manualmente una lettera di unità.
    - Se non dispone di un file system (NTFS, ReFS, FAT32 o exFAT) e si conosce il disco è vuoto, la partizione e scegliere **formato**. Formattazione di un disco Cancella tutti i dati su di esso, pertanto non se si sta provando a ripristinare i file dal disco, invece, andare al passaggio successivo.
1. Se si dispone di un riferimento esterno del disco, scollegare il disco, il plug-in e quindi selezionare **azione** > **Ripeti analisi dischi**. 
2. Arresta il PC, disattivare il disco rigido esterno (se si tratta di un disco esterno con un cavo di alimentazione) e quindi riattivare il PC e il disco.
    Per disattivare il PC in Windows 10, selezionare il pulsante Start, selezionare il pulsante di alimentazione e quindi selezionare **arrestare**.
1. Collegare il disco di una porta USB differente che è direttamente sul PC (non in un hub).
    In alcuni casi i dischi USB non ottenere alimentazione sufficiente da alcune porte o di altri problemi con porte specifiche. Questo avviene in genere con gli hub USB, ma in alcuni casi esistono le differenze tra le porte in un PC, pertanto, se disponibili, provare a poche porte diverse.
1. Provare un cavo diversi.
    Potrebbe sembrare non avere senso, ma i cavi molto esito negativo, quindi provare a usare un cavo diversi per collegare il disco. Se presente un disco interno in un PC desktop, si sarà probabilmente necessario arrestare il computer prima di cambiare i cavi, vedere il manuale del PC per i dettagli.
1. Controllare Gestione dispositivi per i problemi.
    Premere e tenere premuto il pulsante Start, quindi selezionare Gestione di dispositivi dal menu di scelta rapida (o fare doppio clic su). Cercare tutti i dispositivi con un punto esclamativo accanto, o altri problemi, fare doppio clic sul dispositivo e quindi leggere il relativo stato.

    Ecco un elenco di [codici di errore in Gestione dispositivi](https://support.microsoft.com/help/310123/error-codes-in-device-manager-in-windows), ma un approccio che a volte works consiste nel fare clic sul dispositivo problematico, selezionare **dispositivo Disinstalla**e quindi **azione**  >  **Rileva modifiche hardware**.
    ![Gestione di dispositivi che mostra un dispositivo USB sconosciuto](media\device-manager.PNG)
1. Collegare il disco in un computer diverso.
    
    Se il disco non funziona in un altro computer, è un buon segno che c'è un valore non valido in corso il disco e non al computer. Nessun divertente, sappiamo. Esistono alcuni altri passaggi in cui è possibile provare [errore di unità USB esterna "È necessario inizializzare il disco prima che Gestione dischi logici relativi diritti di accesso"](https://social.technet.microsoft.com/Forums/windows/en-US/2b069948-82e9-49ef-bbb7-e44ec7bfebdb/forum-faq-external-usb-drive-error-you-must-initialize-the-disk-before-logical-disk-manager-can?forum=w7itprohardware), ma può essere ora per cercare e richiedere la Guida al [community di Microsoft](https://answers.microsoft.com/en-us/windows) del sito oppure contattare il produttore del disco.

    Se non è semplicemente possibile ottenere il funzionamento, sono disponibili anche le app che è possono provare a ripristinare i dati da un disco guasto, o se i file sono molto importanti, è possibile pagare un laboratorio di ripristino dei dati per tentare di ripristinare tali componenti. Se si trova qualcosa che funziona per l'utente, segnalarlo nella sezione commenti.

> [!IMPORTANT]
> Errore dei dischi, molto spesso, pertanto è importante eseguire regolarmente il backup tutti i file che si è interessati. Se si dispone di un disco che a volte non viene visualizzato o fornisce gli errori, prendere in considerazione questo promemoria per controllare i metodi di backup. Se si è leggermente indietro - tutti Affrontiamo è OK. La migliore soluzione di backup è quello che viene utilizzato, quindi è consigliabile per trovare quello più adatto per l'utente e prosegue con quello.

> [!TIP]
Per informazioni su come usare le app integrate di Windows per i file di backup in un'unità esterna, ad esempio un'unità USB, vedi [backup e ripristino dei file](https://support.microsoft.com/help/17143/windows-10-back-up-your-files). È anche possibile salvare i file in Microsoft OneDrive, che esegue la sincronizzazione dei file dal computer nel cloud. Se il disco rigido non riesce, sarà comunque in grado di ottenere tutti i file archiviati in OneDrive da OneDrive.com. Per altre informazioni, vedi [OneDrive nel PC](https://support.microsoft.com/help/17184/windows-10-onedrive).

## <a name="a-basic-or-dynamic-disks-status-is-unreadable"></a>Stato del disco di base o dinamico è illeggibile

**Causa:**  il disco di base o dinamico non è accessibile e potrebbe essere che si siano verificati errori hardware, danneggiamenti o errori dei / o. La copia del disco del database di configurazione del disco di sistema potrebbe essere danneggiata. Un'icona di errore viene visualizzata sui dischi con stato **Illeggibile**.

È inoltre possibile che lo stato **Illeggibile** venga visualizzato per i dischi durante lo spinup o quando Gestione disco esegue di nuovo l'analisi di tutti i dischi nel sistema. In alcuni casi, non è possibile recuperare un disco illeggibile che restituisce un errore. Per i dischi dinamici, lo stato **illeggibile** è in genere dovuto a errori di I/O o di danneggiamento su una parte del disco, anziché a un guasto dell'intero disco.

**Soluzione:**  Ripeti analisi dischi o riavviare il computer per verificare se cambia lo stato del disco. Provare anche i passaggi di risoluzione dei problemi descritti in [lo stato del disco non inizializzato o il disco risulta manca interamente](#a-disks-status-is-not-initialized-or-the-disk-is-missing).

## <a name="a-dynamic-disks-status-is-foreign"></a>Stato di un disco dinamico è esterno

**Causa:**  le **esterna** stato si verifica quando si sposta un disco dinamico al computer locale da PC a un altro computer. Un'icona di avviso viene visualizzata sui dischi con stato **Esterno**.

In alcuni casi, è possibile che lo stato **Esterno** venga visualizzato per un disco connesso in precedenza al sistema. I dati di configurazione per i dischi dinamici vengono archiviati in tutti i dischi dinamici, le informazioni relative a quali dischi sono di proprietà del sistema vengono pertanto perse qualora si verifichino problemi per tutti i dischi dinamici.

**Soluzione:**  aggiungere il disco alla configurazione del sistema del computer in modo che è possibile accedere ai dati sul disco. Per aggiungere un disco alla configurazione di sistema del computer, importa il disco esterno (fai clic con il pulsante destro del mouse sul disco e quindi fai clic su **Importazione dischi esterni**). I volumi esistenti sul disco esterno diventano visibili e accessibili quando si importa il disco. 

## <a name="a-dynamic-disks-status-is-online-errors"></a>Lo stato di un disco dinamico è Online (errori)

**Causa:**  il disco dinamico contiene errori dei / o su un'area del disco. Un'icona di avviso viene visualizzata sui dischi dinamici con gli errori.

**Soluzione:**   se gli errori dei / o sono temporanei, riattivare il disco per restituirlo al **Online** dello stato.

## <a name="a-dynamic-disks-status-is-offline-or-missing"></a>Lo stato di un disco dinamico è Offline o mancante

**Causa:**  un' **Offline** disco dinamico potrebbe essere danneggiato o non è disponibile in modo intermittente. Un'icona di errore viene visualizzata sul disco dinamico offline.

Se lo stato del disco è **Offline** e il nome del disco cambia in **Mancante**, il disco è stato recentemente disponibile nel sistema, ma ora non è più possibile trovarlo o identificarlo. Il disco mancante potrebbe essere danneggiato, spento o disconnesso.

**Soluzione:** Per portare un disco Offline e non in linea:

1. Risolvi eventuali problemi relativi al disco, al controller o ai cavi. 
2. Verifica che il disco fisico sia acceso, alimentato correttamente e collegato al computer. 
3. Utilizza quindi il comando **Riattiva disco specificato** per portare di nuovo il disco online.
4. Ripetere i passaggi di risoluzione dei problemi descritti in [lo stato del disco non inizializzato o il disco risulta manca interamente](#a-disks-status-is-not-initialized-or-the-disk-is-missing).
5. Se lo stato del disco rimane **Offline** e il nome del disco resta **Mancante** e tu ritieni che non sia possibile risolvere il problema del disco, puoi rimuovere il disco dal sistema facendo clic con il pulsante destro del mouse sul disco, quindi facendo clic su **Rimuovi il disco**. Prima di rimuovere il disco, devi tuttavia eliminare tutti i volumi (o mirror) sul disco. Puoi salvare i volumi con mirroring nel disco rimuovendo il mirror anziché l'intero volume. Quando elimini un volume, i relativi dati vengono cancellati definitivamente. Devi pertanto rimuovere un disco solo se hai la certezza che sia definitivamente danneggiato o inutilizzabile.

**Per portare un disco è Offline e viene denominato disco \# (non mancano) online, provare uno o più delle seguenti procedure:**

1. In Gestione disco fai clic con il pulsante destro del mouse sul disco e quindi fai clic su **Riattiva disco specificato** per portare il disco online. Se lo stato del disco rimane **Offline**, controlla i cavi e il controller del disco e assicurati che il disco fisico sia integro. Correggi eventuali problemi e prova di nuovo a riattivare il disco. Se la riattivazione del disco ha esito positivo, i volumi sul disco tornano automaticamente allo stato **Integro**.
2. In Visualizzatore eventi controlla la presenza nei registri eventi di eventuali errori correlati al disco, ad esempio errori simili a "No good config copies". Se nel registro eventi è presente questo errore, contatta il [Servizio Supporto Tecnico Clienti Microsoft](https://msdn.microsoft.com/library/aa263468(v=vs.60).aspx).

3. Prova a spostare il disco in un altro computer. Se il disco può essere impostato sullo stato **Online** su un altro computer, il problema è probabilmente dovuto alla configurazione del computer in cui il disco non passa allo stato **Online**.

4. Prova a spostare il disco su un altro computer che dispone di dischi dinamici. Importa il disco in tale computer e quindi spostalo di nuovo nel computer in cui non si riesce a impostare lo stato **Online**. 

## <a name="a-basic-or-dynamic-volumes-status-is-failed"></a>Stato di un volume di base o dinamico è non riuscito

**Causa:**   non può essere avviato automaticamente il volume di base o dinamico, il disco è danneggiato o il file system è danneggiato. A meno che non sia possibile ripristinare il disco o file system, lo stato **Errore** indica una perdita di dati.

**Soluzione:**

Se il volume è un volume di base con **Failed** stato:

- Verifica che il disco fisico sottostante sia acceso, alimentato correttamente e collegato al computer.
- Ripetere i passaggi di risoluzione dei problemi descritti in [lo stato del disco non inizializzato o il disco risulta manca interamente](#a-disks-status-is-not-initialized-or-the-disk-is-missing).

Se si tratta di un volume dinamico con stato **Errore**:

-   Assicurati che i dischi sottostanti siano online. In caso contrario, riporta i dischi in stato **Online**. In caso di esito positivo, il volume viene riavviato automaticamente e torna allo stato **Integro**. Se il disco dinamico torna allo stato **Online**, ma non allo stato **Integro**, puoi riattivare il volume manualmente.
-   Se il volume dinamico è un volume con mirroring o RAID-5 con dati obsoleti, non verrà riavviato automaticamente quando riporti in stato Online il disco sottostante. Se i dischi che contengono dati correnti vengono disconnessi, porta online prima tali dischi (per consentire la sincronizzazione dei dati). Altrimenti, riavvia manualmente il volume con mirroring o RAID-5 e quindi esegui lo strumento di controllo degli errori del o Chkdsk.exe.
- Ripetere i passaggi di risoluzione dei problemi descritti in [lo stato del disco non inizializzato o il disco risulta manca interamente](#a-disks-status-is-not-initialized-or-the-disk-is-missing).

## <a name="a-basic-or-dynamic-volumes-status-is-unknown"></a>Lo stato di un volume di base o dinamico è sconosciuto

**Causa:**   le **sconosciuto** stato si verifica quando il settore di avvio per il volume è danneggiato (forse a causa di un virus) ed è non possibile accedere non è più dati nel volume. Lo stato **Sconosciuto** viene visualizzato anche quando si installa un nuovo disco ma non si completa correttamente la procedura guidata per creare una firma del disco.

**Soluzione**  inizializzare il disco. Per istruzioni, vedi [Inizializzare nuovi dischi](initialize-new-disks.md).

## <a name="a-dynamic-volumes-status-is-data-incomplete"></a>Lo stato di un volume dinamico è dati incompleti

**Causa:** Sono stati spostati, ma non tutti i dischi in un volume su più dischi. I dati su questo volume verranno eliminati definitivamente, a meno che i dischi rimanenti che contengono questo volume non vengano spostati e importati.

**Soluzione:**

1. Sposta tutti i dischi che contengono il volume multidisco nel computer.
2. Importa i dischi. Per istruzioni su come spostare e importare i dischi, vedi [Spostare i dischi in un altro computer](move-disks-to-another-computer.md).

Se il volume multidisco non è più necessario, puoi importare il disco e creare nuovi volumi in esso. A tale scopo:

1. Fai clic con il pulsante destro del mouse sul volume con stato **Errore** o **Errore di ridondanza**, quindi fai clic su **Elimina il volume**.
2. Fare clic con il pulsante destro del mouse sul disco e quindi scegliere **Nuovo volume**.

## <a name="a-dynamic-volumes-status-is-healthy-at-risk"></a>Lo stato di un volume dinamico è integro (a rischio)

**Causa:**   indica che il volume dinamico è attualmente accessibile, ma sono stati rilevati errori dei / o del disco dinamico sottostante. Se viene rilevato un errore di I/O in qualsiasi parte di un disco dinamico, per tutti i volumi nel disco viene visualizzato lo stato **Integro (a rischio)** e un'icona di avviso appare sul volume.

Quando lo stato del volume è **Integro (a rischio)** , lo stato di un disco sottostante è in genere **Online (errori)** .

**Soluzione:**  
1. Riporta il disco sottostante in stato **Online**. Dopo che il disco è riportato **Online**, il volume dovrebbe tornare in stato **Integro**. Se lo stato **Integro (a rischio)** persiste, il disco potrebbe essere danneggiato. 

2. Esegui il backup dei dati e sostituisci il disco il prima possibile. 

## <a name="cannot-manage-striped-volumes-using-disk-management-or-diskpart"></a>Non è possibile gestire i volumi con striping tramite Gestione disco o DiskPart

**Causa:**   alcuni prodotti di gestione non Microsoft disco sostituire Manager Microsoft dischi logici (LDM) per la gestione avanzata del disco, che è possibile disabilitare la gestione dischi LOGICI.

**Soluzione:**   se si usa il software di gestione del disco non Microsoft che ha disabilitato Gestione dischi LOGICI, è necessario contattare il fornitore del software di gestione del disco non Microsoft per assistenza nella risoluzione dei problemi con il disco configurazione.

## <a name="disk-management-cannot-start-the-virtual-disk-service"></a>Gestione disco non è possibile avviare il servizio dischi virtuali

**Causa:**   se un computer remoto non supporta il servizio dischi virtuali (VDS) o se si è bloccato dal Firewall di Windows non è possibile stabilire una connessione al computer remoto, questo errore può verificarsi.

**Soluzione:**

1. Se il computer remoto supporta VDS, puoi configurare Windows Defender Firewall in modo da consentire le connessioni VDS. Se il computer remoto non supporta VDS, puoi utilizzare Connessione Desktop remoto per eseguire la connessione e quindi eseguire Gestione disco direttamente nel computer remoto.
2. Per gestire i dischi in computer remoti che supportano VDS, devi configurare Windows Defender Firewall sia nel computer locale (su cui è eseguito Gestione disco) sia nel computer remoto.
3. Nel computer locale configura Windows Defender Firewall per abilitare l'eccezione Gestione volumi remota.


> [!NOTE]
> L'eccezione Gestione volumi remota include le eccezioni per Vds.exe, Vdsldr.exe e la porta TCP 135.


 > [!NOTE]
 > Le connessioni remote in gruppi di lavoro non sono supportate. Il computer locale e il computer remoto devono essere entrambi membri di un dominio.