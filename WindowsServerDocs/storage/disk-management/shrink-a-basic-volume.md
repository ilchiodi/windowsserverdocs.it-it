---
title: Ridurre un volume di base
description: Questo articolo descrive come ridurre un volume di base
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e54632b78fd67a65b51147323565130881d8d81b
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="shrink-a-basic-volume"></a>Ridurre un volume di base

> **Si applica a:** Windows 10, Windows 8.1, Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile diminuire lo spazio utilizzato da unità logiche e partizioni primarie riducendole nello spazio contiguo adiacente sullo stesso disco. Se ad esempio si individua l'esigenza di una partizione aggiuntiva, ma non si dispone di dischi aggiuntivi, è possibile ridurre la partizione esistente dalla fine del volume in modo da creare nuovo spazio non allocato utilizzabile per una nuova partizione. L'operazione di riduzione può essere bloccata dalla presenza di determinati tipi di file. Per altre informazioni, vedi [Considerazioni aggiuntive](#addcon) 

Quando si riduce una partizione, tutti i file ordinari vengono rilocati automaticamente nel disco in modo da creare nuovo spazio non allocato. Non è necessario riformattare il disco per ridurre la partizione.

> [!CAUTION]
> Nel caso di una partizione non elaborata (ovvero senza un file system) che contiene dati (ad esempio un file di database), la riduzione della partizione potrebbe implicare l'eliminazione definitiva dei dati.

## <a name="shrinking-a-basic-volume"></a>Riduzione di un volume di base

-   [Tramite l'interfaccia di Windows](#BKMK_WINUI)
-   [Tramite la riga di comando](#BKMK_CMD)

> [!NOTE]
> Per completare questi passaggi, devi essere un membro del gruppo **Backup Operators** o **Administrators**.

<a id="BKMK_WINUI"></a>
#### <a name="to-shrink-a-basic-volume-using-the-windows-interface"></a>Per ridurre un volume di base tramite l'interfaccia di Windows

1.  In Gestione disco fai clic sul volume di base che desideri ridurre.

2.  Fai clic su **Riduci volume**.

3.  Segui le istruzioni visualizzate.

<br />

> [!NOTE]
> Puoi ridurre solo i volumi di base senza file system o che utilizzano il file system NTFS.

<a id="BKMK_CMD"></a>
#### <a name="to-shrink-a-basic-volume-using-a-command-line"></a>Per ridurre un volume di base tramite la riga di comando

1.  Apri un prompt dei comandi e digita `diskpart`.

2.  Al prompt **DISKPART** digita `list volume`. Prendi nota del numero del volume semplice che desideri ridurre.

3.  Al prompt **DISKPART** digita `select volume <volumenumber>`. Viene selezionato il volume semplice *volumenumber* (numerovolume) che desideri ridurre.

4.  Al prompt **DISKPART** digita `shrink [desired=<desiredsize>] [minimum=<minimumsize>]`. Il volume selezionato viene ridotto da *desiredsize* (dimensionidesiderate) in megabyte (MB), se possibile, o a *minimumsize* (dimensioniminime) se *desiredsize* è troppo grande.

<br />

| Valore | Descrizione|
|---|---|
| <p>**list volume**</p> | <p>Visualizza un elenco di volumi di base e dinamici su tutti i dischi.</p>|
| <p>**select volume**</p> | <p>Seleziona il volume specificato, dove <em>volumenumber</em> è il numero del volume, assegnandogli lo stato attivo. Se non è specificato alcun volume, il comando **select** visualizza un elenco di volumi correnti con stato attivo. È possibile specificare il volume in base al numero, alla lettera di unità o al percorso del punto di montaggio. Su un disco di base la selezione di un volume imposta anche lo stato attivo della partizione corrispondente.</p> |
| <p>**shrink**</p> | <p>Riduce il volume con lo stato attivo per creare spazio non allocato. Non si verifica alcuna perdita di dati. Se la partizione include file fissi (ad esempio, il file di paging o l'area di archiviazione della copia shadow), il volume verrà ridotto fino al punto in cui si trovano i file fissi. |
| <p>**desired=** <em>desiredsize</em></p> | <p>La quantità di spazio, in megabyte, da recuperare per la partizione corrente.</p> |
| <p>**minimum=** <em>minimumsize</em></p> | <p>La quantità di spazio minima, in megabyte, da recuperare per la partizione corrente. Se non si specifica una dimensione minima o desiderata, il comando recupererà la quantità massima di spazio possibile.</p> 

<a id="addcon"></a>

## <a name="additional-considerations"></a>Considerazioni aggiuntive

-   Quando riduci una partizione, determinati file (ad esempio, il file di paging o l'area di archiviazione copia shadow) non possono essere rilocati automaticamente e non è possibile diminuire lo spazio allocato oltre il punto in cui si trovano i file fissi. Se l'operazione di riduzione non riesce, controlla l'evento 259 nel registro applicazioni, che consente di identificare il file fisso. Se sai quali sono i cluster associati al file che impedisce l'operazione di riduzione, puoi inoltre utilizzare il comando **fsutil** da un prompt dei comandi (digita **fsutil volume querycluster /?** per l'utilizzo). Se specifichi il parametro **querycluster**, l'output del comando identificherà il file fisso che impedisce il completamento dell'operazione di riduzione.
In alcuni casi, puoi rilocare il file temporaneamente. Ad esempio, se devi ridurre ulteriormente la partizione, puoi utilizzare il Pannello di controllo per spostare il file di paging o le copie shadow archiviate in un altro disco, eliminare le copie shadow archiviate, ridurre il volume e quindi spostare il file di paging sul disco. Se il numero di cluster non validi rilevati dal nuovo mapping dei cluster dinamici non validi è troppo elevato, puoi ridurre la partizione. In questo caso, dovresti valutare la possibilità di spostare i dati e sostituire il disco.

-  Non utilizzare una copia a livello di blocco per trasferire i dati. In questo modo viene copiata anche una tabella dei settori danneggiati e il nuovo disco considererà tali settori come danneggiati anche se non lo sono.

-   Puoi ridurre le partizioni primarie e le unità logiche in partizioni non elaborate (senza file system) o in partizioni con file system NTFS.

## <a name="see-also"></a>Vedi anche

-   [Gestire volumi di base](manage-basic-volumes.md)