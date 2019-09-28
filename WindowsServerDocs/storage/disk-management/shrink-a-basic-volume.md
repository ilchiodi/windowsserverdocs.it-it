---
title: Ridurre un volume di base
description: Questo articolo descrive come ridurre un volume di base
ms.date: 06/07/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 2baf24ed656ef06d44dff93180701d25e6852500
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385864"
---
# <a name="shrink-a-basic-volume"></a>Ridurre un volume di base

> **Si applica a:** Windows 10, Windows 8.1, Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puoi diminuire lo spazio usato da unità logiche e partizioni primarie riducendole nello spazio contiguo adiacente sullo stesso disco. Ad esempio, se ti accorgi che ti serve una partizione aggiuntiva ma non hai dischi aggiuntivi, puoi ridurre la partizione esistente dalla fine del volume in modo da creare nuovo spazio non allocato da usare per una nuova partizione. L'operazione di riduzione può essere bloccata dalla presenza di determinati tipi di file. Per altre informazioni, vedi [Considerazioni aggiuntive](#additional-considerations) 

Quando riduci una partizione, tutti i file ordinari vengono automaticamente rilocati nel disco in modo da creare nuovo spazio non allocato. Non è necessario riformattare il disco per ridurre la partizione.

> [!CAUTION]
> Nel caso di una partizione non elaborata (ovvero senza file system) che contiene dati (ad esempio un file di database), la riduzione della partizione può comportare l'eliminazione definitiva dei dati.

## <a name="shrinking-a-basic-volume"></a>Riduzione di un volume di base

> [!NOTE]
> Per completare questi passaggi, devi essere un membro del gruppo **Backup Operators** o **Administrators**.

#### <a name="to-shrink-a-basic-volume-using-the-windows-interface"></a>Per ridurre un volume di base tramite l'interfaccia di Windows

1.  In Gestione disco fai clic con il pulsante destro del mouse sul volume di base che vuoi ridurre.

2.  Fai clic su **Riduci volume**.

3.  Segui le istruzioni visualizzate.


> [!NOTE]
> Puoi ridurre solo i volumi di base senza file system o che usano il file system NTFS.

#### <a name="to-shrink-a-basic-volume-using-a-command-line"></a>Per ridurre un volume di base tramite la riga di comando

1.  Aprire un prompt dei comandi e digitare `diskpart`.

2.  Al prompt **DISKPART** digita `list volume`. Prendi nota del numero del volume semplice che vuoi ridurre.

3.  Al prompt **DISKPART** digita `select volume <volumenumber>`. Viene selezionato il volume semplice *volumenumber* che vuoi ridurre.

4.  Al prompt **DISKPART** digita `shrink [desired=<desiredsize>] [minimum=<minimumsize>]`. Il volume selezionato viene ridotto a *desiredsize* in megabyte (MB), se possibile, o a *minimumsize* se *desiredsize* è troppo grande.

| Valore             | Descrizione |
| ---               | ----------- |
| **list volume** | Visualizza un elenco di volumi di base e dinamici su tutti i dischi. |
| **select volume** | Seleziona il volume specificato, dove <em>volumenumber</em> è il numero del volume, assegnando lo stato attivo a questo volume. Se non è specificato alcun volume, il comando **select** visualizza un elenco di volumi correnti con stato attivo. Puoi specificare il volume in base al numero, alla lettera di unità o al percorso del punto di montaggio. Su un disco di base la selezione di un volume imposta anche lo stato attivo della partizione corrispondente. |
| **shrink** | Riduce il volume con lo stato attivo per creare spazio non allocato. Non si verifica alcuna perdita di dati. Se la partizione include file fissi (ad esempio, il file di paging o l'area di archiviazione della copia shadow), il volume verrà ridotto fino al punto in cui si trovano i file fissi. |
| **desired=** <em>desiredsize</em> | Quantità di spazio, in megabyte, da recuperare per la partizione corrente. |
| **minimum=** <em>minimumsize</em> | Quantità di spazio minima, in megabyte, da recuperare per la partizione corrente. Se non specifichi una dimensione minima o desiderata, il comando recupererà la quantità massima di spazio possibile. |

## <a name="additional-considerations"></a>Considerazioni aggiuntive

-   Quando riduci una partizione, determinati file (ad esempio, il file di paging o l'area di archiviazione copia shadow) non possono essere rilocati automaticamente e non puoi diminuire lo spazio allocato oltre il punto in cui si trovano i file fissi. Se l'operazione di riduzione non riesce, controlla l'evento 259 nel registro applicazioni, che permette di identificare il file fisso. Se sai quali sono i cluster associati al file che impediscono l'operazione di riduzione, puoi anche usare il comando **fsutil** da un prompt dei comandi (digita **fsutil volume querycluster /?** per l'utilizzo). Se specifichi il parametro **querycluster**, l'output del comando identificherà il file fisso che impedisce il completamento dell'operazione di riduzione.
In alcuni casi, puoi rilocare il file temporaneamente. Ad esempio, se devi ridurre ulteriormente la partizione, puoi utilizzare il Pannello di controllo per spostare il file di paging o le copie shadow archiviate in un altro disco, eliminare le copie shadow archiviate, ridurre il volume e quindi riportare il file di paging sul disco. Se il numero di cluster non validi rilevati dal nuovo mapping dei cluster dinamici non validi è troppo elevato, puoi ridurre la partizione. In questo caso, valuta la possibilità di spostare i dati e sostituire il disco.

-  Non usare una copia a livello di blocco per trasferire i dati. In questo modo, viene copiata anche una tabella dei settori danneggiati e il nuovo disco considererà questi settori come danneggiati anche se non lo sono.

-   Puoi ridurre le partizioni primarie e le unità logiche in partizioni non elaborate (senza file system) o in partizioni con file system NTFS.

## <a name="see-also"></a>Vedi anche

-   [Gestire volumi di base](manage-basic-volumes.md)