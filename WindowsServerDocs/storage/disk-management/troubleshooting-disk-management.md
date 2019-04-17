---
title: Risoluzione dei problemi relativi a Gestione disco
description: Questo articolo descrive come risolvere i problemi di Gestione disco
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e4b361631799dccbc2b77fb5aa909052532a057d
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="troubleshooting-disk-management"></a>Risoluzione dei problemi relativi a Gestione disco

> **Si applica a:** Windows 10, Windows 8.1, Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento sono elencati alcuni problemi comuni che possono verificarsi quando si utilizza Gestione disco.

<a id="BKMK_1"></a>

## <a name="partitions-on-basic-disks-added-to-the-system-do-not-appear-in-the-disk-management-volume-list-view"></a>Le partizioni su dischi di base aggiunti al sistema non sono presenti nella visualizzazione Elenco volumi di Gestione disco.

**Causa:** i volumi nei dischi di base aggiunti al sistema non vengono montati automaticamente e le lettere di unità non vengono assegnate per impostazione predefinita.
I dischi dinamici vengono visualizzati come **esterni** quando vengono aggiunti al sistema. Per usare i volumi, devi importare i dischi **esterni** e portare i volumi **online**.
I dispositivi multimediali rimovibili (ad esempio unità Zip o Jaz) e i dischi ottici (ad esempio CD-ROM o DVD-RAM) vengono sempre montati automaticamente dal sistema.

**Soluzione:** monta manualmente i volumi di base assegnando lettere di unità o creando punti di montaggio mediante Gestione disco o i comandi [DiskPart](http://go.microsoft.com/fwlink/?LinkId=64110) o [mountvol](http://go.microsoft.com/fwlink/?LinkId=64111).

<a id="BKMK_2"></a>

## <a name="a-basic-disks-status-is-not-initialized"></a>Lo stato di un disco di base è Non inizializzato.

**Causa:** il disco non contiene una firma valida. Dopo che hai installato un nuovo disco, è necessario che il sistema operativo scriva una firma del disco, l'indicatore di fine settore e un record di avvio principale o una tabella di partizione GUID affinché possa creare partizioni sul disco. Quando avvii Gestione disco per la prima volta dopo l'installazione di un nuovo disco, viene visualizzata una procedura guidata che fornisce un elenco dei nuovi dischi rilevati dal sistema operativo. Se annulli la procedura guidata prima che venga scritta la firma del disco, il disco rimane in stato **Non inizializzato**.

**Soluzione:** inizializza il disco. Lo stato del disco viene impostato per un breve periodo su **Inizializzazione in corso** e quindi sullo stato **Online**. Per istruzioni su come inizializzare un disco, vedi [Inizializzare nuovi dischi](initialize-new-disks.md). 

<a id="BKMK_3"></a>

## <a name="a-basic-or-dynamic-disks-status-is-unreadable"></a>Lo stato di un disco di base o dinamico è Illeggibile.

**Causa:** il disco di base o dinamico non è accessibile e il problema potrebbe essere dovuto a un errore hardware, un errore di I/O o di danneggiamento. La copia del disco del database di configurazione del disco di sistema potrebbe essere danneggiata. Un'icona di errore viene visualizzata sui dischi con stato **Illeggibile**.

È inoltre possibile che lo stato **Illeggibile** venga visualizzato per i dischi durante lo spinup o quando Gestione disco esegue di nuovo l'analisi di tutti i dischi nel sistema. In alcuni casi, non è possibile recuperare un disco illeggibile che restituisce un errore. Per i dischi dinamici, lo stato **illeggibile** è in genere dovuto a errori di I/O o di danneggiamento su una parte del disco, anziché a un guasto dell'intero disco.

**Soluzione:** esegui di nuovo l'analisi dei dischi o riavvia il computer per verificare se lo stato del disco cambia.

<a id="BKMK_4"></a>

## <a name="a-dynamic-disks-status-is-foreign"></a>Lo stato di un disco dinamico è Esterno.

**Causa:** lo stato **Esterno** viene visualizzato quando si sposta un disco dinamico nel computer locale da un altro computer che esegue i sistemi operativi Windows 2000, Windows XP Professional, Windows XP 64-Bit Edition o Windows Server 2003. Un'icona di avviso viene visualizzata sui dischi con stato **Esterno**.

In alcuni casi, è possibile che lo stato **Esterno** venga visualizzato per un disco connesso in precedenza al sistema. I dati di configurazione per i dischi dinamici vengono archiviati in tutti i dischi dinamici, le informazioni relative a quali dischi sono di proprietà del sistema vengono pertanto perse qualora si verifichino problemi per tutti i dischi dinamici.

**Soluzione:** aggiungi il disco alla configurazione di sistema del computer in modo da poter accedere ai dati sul disco. Per aggiungere un disco alla configurazione di sistema del computer, importa il disco esterno (fai clic con il pulsante destro del mouse sul disco e quindi fai clic su **Importazione dischi esterni**). I volumi esistenti sul disco esterno diventano visibili e accessibili quando si importa il disco. 

<a id="BKMK_5"></a>

## <a name="a-dynamic-disks-status-is-online-errors"></a>Lo stato di un disco dinamico è Online (errori).

**Causa:** il disco dinamico presenta errori di I/O su un'area del disco. Un'icona di avviso viene visualizzata sui dischi dinamici con gli errori.

**Soluzione:** se gli errori di I/O sono temporanei, riattiva il disco per tornare allo stato **Online**.

<a id="BKMK_6"></a>

## <a name="a-dynamic-disks-status-is-offline-or-missing"></a>Lo stato di un disco dinamico è Offline o Mancante.

**Causa:** un disco dinamico **Offline** potrebbe essere danneggiato o non disponibile in modo intermittente. Un'icona di errore viene visualizzata sul disco dinamico offline.

Se lo stato del disco è **Offline** e il nome del disco cambia in **Mancante**, il disco è stato recentemente disponibile nel sistema, ma ora non è più possibile trovarlo o identificarlo. Il disco mancante potrebbe essere danneggiato, spento o disconnesso.

**Soluzione:** per portare di nuovo online un disco in stato Offline o Mancante:
1. Risolvi eventuali problemi relativi al disco, al controller o ai cavi. 
2. Verifica che il disco fisico sia acceso, alimentato correttamente e collegato al computer. 
3. Utilizza quindi il comando **Riattiva disco specificato** per portare di nuovo il disco online.

4. Se lo stato del disco rimane **Offline** e il nome del disco resta **Mancante** e tu ritieni che non sia possibile risolvere il problema del disco, puoi rimuovere il disco dal sistema facendo clic con il pulsante destro del mouse sul disco, quindi facendo clic su **Rimuovi il disco**. Prima di rimuovere il disco, devi tuttavia eliminare tutti i volumi (o mirror) sul disco. Puoi salvare i volumi con mirroring nel disco rimuovendo il mirror anziché l'intero volume. Quando elimini un volume, i relativi dati vengono cancellati definitivamente. Devi pertanto rimuovere un disco solo se hai la certezza che sia definitivamente danneggiato o inutilizzabile.

**Per portare Online un disco Offline il cui nome è ancora Disco \# (non mancante), prova una o più delle seguenti procedure:**

1. In Gestione disco fai clic con il pulsante destro del mouse sul disco e quindi fai clic su **Riattiva disco specificato** per portare il disco online. Se lo stato del disco rimane **Offline**, controlla i cavi e il controller del disco e assicurati che il disco fisico sia integro. Correggi eventuali problemi e prova di nuovo a riattivare il disco. Se la riattivazione del disco ha esito positivo, i volumi sul disco tornano automaticamente allo stato **Integro**.
2. In Visualizzatore eventi controlla la presenza nei registri eventi di eventuali errori correlati al disco, ad esempio errori simili a "No good config copies". Se nel registro eventi è presente questo errore, contatta il [Servizio Supporto Tecnico Clienti Microsoft](https://msdn.microsoft.com/library/aa263468(v=vs.60).aspx).

3. Prova a spostare il disco in un altro computer. Se il disco può essere impostato sullo stato **Online** su un altro computer, il problema è probabilmente dovuto alla configurazione del computer in cui il disco non passa allo stato **Online**.

4. Prova a spostare il disco su un altro computer che dispone di dischi dinamici. Importa il disco in tale computer e quindi spostalo di nuovo nel computer in cui non si riesce a impostare lo stato **Online**. 

<a id="BKMK_7"></a>

## <a name="a-basic-or-dynamic-volumes-status-is-failed"></a>Lo stato di un volume di base o dinamico è Errore.

**Causa:** il volume di base o dinamico non viene avviato automaticamente, il disco o il file system è danneggiato. A meno che non sia possibile ripristinare il disco o file system, lo stato **Errore** indica una perdita di dati.

**Soluzione:** se si tratta di un volume di base con stato **Errore**:

-   Verifica che il disco fisico sottostante sia acceso, alimentato correttamente e collegato al computer. Per i volumi di base non è possibile eseguire altre azioni.

    Se si tratta di un volume dinamico con stato **Errore**: 
-   Assicurati che i dischi sottostanti siano online. In caso contrario, riporta i dischi in stato **Online**. In caso di esito positivo, il volume viene riavviato automaticamente e torna allo stato **Integro**. Se il disco dinamico torna allo stato **Online**, ma non allo stato **Integro**, puoi riattivare il volume manualmente. 
    
-   Se il volume dinamico è un volume con mirroring o RAID-5 con dati obsoleti, non verrà riavviato automaticamente quando riporti in stato Online il disco sottostante. Se i dischi che contengono dati correnti vengono disconnessi, porta online prima tali dischi (per consentire la sincronizzazione dei dati). Altrimenti, riavvia manualmente il volume con mirroring o RAID-5 e quindi esegui lo strumento di controllo degli errori del o Chkdsk.exe.

<a id="BKMK_8"></a>

## <a name="a-basic-or-dynamic-volumes-status-is-unknown"></a>Lo stato di un volume di base o dinamico è Sconosciuto.

**Causa:** lo stato **Sconosciuto** si verifica quando il settore di avvio per il volume è danneggiato (probabilmente a causa di un virus) e non è più possibile accedere ai dati nel volume. Lo stato **Sconosciuto** viene visualizzato anche quando si installa un nuovo disco ma non si completa correttamente la procedura guidata per creare una firma del disco.

**Soluzione:** inizializza il disco. Per istruzioni, vedi [Inizializzare nuovi dischi](initialize-new-disks.md). 

<a id="BKMK_9"></a>

## <a name="a-dynamic-volumes-status-is-data-incomplete"></a>Lo stato di un volume dinamico è Dati incompleti.

**Causa:** alcuni dischi, ma non tutti, sono stati spostati in un volume multidisco. I dati su questo volume verranno eliminati definitivamente, a meno che i dischi rimanenti che contengono questo volume non vengano spostati e importati.

**Soluzione:**  
1. Sposta tutti i dischi che contengono il volume multidisco nel computer.

2. Importa i dischi. Per istruzioni su come spostare e importare i dischi, vedi [Spostare i dischi in un altro computer](move-disks-to-another-computer.md).

Se il volume multidisco non è più necessario, puoi importare il disco e creare nuovi volumi in esso. A tale scopo: 

1. Fai clic con il pulsante destro del mouse sul volume con stato **Errore** o **Errore di ridondanza**, quindi fai clic su **Elimina il volume**. 

2. Fai clic con il pulsante destro del mouse sul disco e quindi scegli **Nuovo volume**.

<a id="BKMK_10"></a>

## <a name="a-dynamic-volumes-status-is-healthy-at-risk"></a>Lo stato di un volume dinamico è Integro (a rischio).

**Causa:** indica che il volume dinamico è attualmente accessibile, ma sono stati rilevati errori di I/O del disco dinamico sottostante. Se viene rilevato un errore di I/O in qualsiasi parte di un disco dinamico, per tutti i volumi nel disco viene visualizzato lo stato **Integro (a rischio)** e un'icona di avviso appare sul volume.

Quando lo stato del volume è **Integro (a rischio)**, lo stato di un disco sottostante è in genere **Online (errori)**.

**Soluzione:**  
1. Riporta il disco sottostante in stato **Online**. Dopo che il disco è riportato **Online**, il volume dovrebbe tornare in stato **Integro**. Se lo stato **Integro (a rischio)** persiste, il disco potrebbe essere danneggiato. 

2. Esegui il backup dei dati e sostituisci il disco il prima possibile. 

<a id="BKMK_11"></a>

## <a name="cannot-manage-striped-volumes-using-disk-management-or-diskpart"></a>Non è possibile gestire volumi con striping tramite Gestione disco o DiskPart.

**Causa:** alcuni prodotti di gestione dei dischi non Microsoft sostituiscono Gestione dischi logici (LDM, Logical Disk Manager) di Microsoft. Ciò può causare la disabilitazione di Gestione dischi logici.

**Soluzione:** se utilizzi software di gestione dei dischi non Microsoft che ha causato la disabilitazione di Gestione dischi logici, dovrai contattare il fornitore del software di gestione dei dischi non Microsoft per assistenza nella risoluzione dei problemi con la configurazione del disco.

<a id="BKMK_virtdisk"></a>

## <a name="disk-management-cannot-start-the-virtual-disk-service"></a>Gestione disco non è in grado di avviare Servizio dischi virtuali.

**Causa:** se un computer remoto non supporta Servizio dischi virtuali (VDS) o se non è possibile stabilire una connessione al computer remoto a causa di un blocco di Windows Firewall, è possibile che venga visualizzato questo errore.

**Soluzione:**  
1. Se il computer remoto supporta VDS, puoi configurare Windows Defender Firewall in modo da consentire le connessioni VDS. Se il computer remoto non supporta VDS, puoi utilizzare Connessione Desktop remoto per eseguire la connessione e quindi eseguire Gestione disco direttamente nel computer remoto.

2. Per gestire i dischi in computer remoti che supportano VDS, devi configurare Windows Defender Firewall sia nel computer locale (su cui è eseguito Gestione disco) sia nel computer remoto.

3. Nel computer locale configura Windows Defender Firewall per abilitare l'eccezione Gestione volumi remota.

<br />

> [!NOTE]
> L'eccezione Gestione volumi remota include le eccezioni per Vds.exe, Vdsldr.exe e la porta TCP 135.

<br />

 > [!NOTE]
 > Le connessioni remote in gruppi di lavoro non sono supportate. Il computer locale e il computer remoto devono essere entrambi membri di un dominio.