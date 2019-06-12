---
title: Convertire nuovamente un disco dinamico in un disco di base
description: Descrive come convertire nuovamente un disco dinamico in un disco di base.
ms.date: 06/07/2019
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 249db6d2779e696ef93fecfd11718dbcce8654be
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812470"
---
# <a name="change-a-dynamic-disk-back-to-a-basic-disk"></a>Convertire nuovamente un disco dinamico in un disco di base

> **Si applica a:** Windows 10, Windows 8.1, Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento viene descritto come eliminare tutti i dati su un disco dinamico e quindi riconvertirlo in un disco di base. I dischi dinamici sono deprecati in Windows e non è consigliabile usarli più. È invece consigliabile usando i dischi di base o la versione più recente [spazi di archiviazione](https://support.microsoft.com/help/12438/windows-10-storage-spaces) tecnologia quando si desidera dischi del pool di insieme in volumi più elevati. Se si desidera eseguire il mirroring del volume da cui si avvia Windows, è consigliabile utilizzare un controller RAID hardware, come quello incluso in molte schede madri.

> [!WARNING]
> Per convertire nuovamente un disco dinamico in un disco di base è necessario eliminare tutti i volumi dal disco, cancellando in modo permanente tutti i dati sul disco. Accertarsi di eseguire il backup di tutti i dati che si desidera conservare prima di procedere.

## <a name="changing-a-dynamic-disk-back-to-a-basic-disk"></a>Riconversione di un disco dinamico in un disco di base

-   [Tramite l'interfaccia di Windows](#to-change-a-dynamic-disk-back-to-a-basic-disk-using-the-windows-interface)
-   [Tramite la riga di comando](#to-change-a-dynamic-disk-back-to-a-basic-disk-using-a-command-line)

> [!NOTE]
> Per completare questi passaggi, devi essere un membro del gruppo **Backup Operators** o **Administrators**.

#### <a name="to-change-a-dynamic-disk-back-to-a-basic-disk-using-the-windows-interface"></a>Per convertire un disco dinamico nuovamente in un disco di base tramite l'interfaccia di Windows

1.  Eseguire il backup tutti i volumi sul disco che si desidera convertire da dinamico a base.

2.  In Gestione disco, fare clic con il pulsante destro del mouse su ciascun volume del disco dinamico che si desidera convertire in un disco di base e quindi fare clic su **Elimina Volume** per ogni volume sul disco.

3.  Quando tutti i volumi sul disco sono stati eliminati, fare clic con il pulsante destro del mouse sul disco e quindi fare clic su **Converti in disco di base**.

#### <a name="to-change-a-dynamic-disk-back-to-a-basic-disk-using-a-command-line"></a>Per convertire un disco dinamico nuovamente in un disco di base tramite una riga di comando

1.  Eseguire il backup tutti i volumi sul disco che si desidera convertire da dinamico a base.

2.  Aprire un prompt dei comandi e digitare `diskpart`.

3.  Al prompt **DISKPART** digita `list disk`. Prendere nota del numero di disco che si desidera convertire in base.

4.  Al prompt **DISKPART** digita `select disk <disknumber>`.

5.  Al prompt **DISKPART** digita `detail disk <disknumber>`.

6.  Per ogni volume sul disco, al prompt **DISKPART** digitare `select volume= <volumenumber>` e quindi digitare `delete volume`.

7.  Al prompt **DISKPART** digitare `select disk <disknumber>`, specificando il numero del disco che si desidera convertire in un disco di base.

8.  Al prompt **DISKPART** digita `convert basic`.


| Value  | Descrizione |
| --- | --- |
| **disco di elenco**                         | Visualizza un elenco di dischi e le relative informazioni, ad esempio le dimensioni, quantità di spazio libero, se il disco è un disco di base o dinamico e se il disco usa lo stile di partizione MBR (Record di avvio principale, Master Boot Record) o GPT (Tabella di partizione GUID, GUID Partition Table). Il disco contrassegnato con un asterisco (*) ha lo stato attivo. |
| **Selezionare disco** <em>disknumber</em>   | Seleziona il disco specificato, dove <em>disknumber</em> è il numero del disco, assegnandogli lo stato attivo.  |
| **disco dettaglio** <em>disknumber</em>   | Visualizza le proprietà del disco selezionato e i volumi presenti sul disco.  |
| **Selezionare volume** <em>disknumber</em> | Seleziona il volume specificato, dove <em>disknumber</em> è il numero del volume, assegnandogli lo stato attivo. Se non è specificato alcun volume, il comando **select** visualizza un elenco di volumi correnti con stato attivo. È possibile specificare il volume in base al numero, alla lettera di unità o al percorso del punto di montaggio. Su un disco di base la selezione di un volume imposta anche lo stato attivo della partizione corrispondente. |
| **Eliminazione del volume**                     | Elimina il volume selezionato. Non è possibile eliminare il volume di sistema, volume di avvio o qualsiasi volume che contiene il dump di arresto anomalo del sistema o file di paging active dump di memoria. |
| **convertire base** | Converte un disco dinamico vuoto in un disco di base.  |

## <a name="additional-considerations"></a>Considerazioni aggiuntive

-   Il disco non deve contenere volumi o dati prima di poterlo riconvertire in un disco di base. Se si desidera conservare i dati, eseguirne il backup o spostarli su un altro volume prima di convertire il disco in un disco di base.
-   Dopo aver convertito nuovamente un disco dinamico in un disco di base, è possibile creare solo unità logiche e partizioni su tale disco.

## <a name="see-also"></a>Vedere anche

-   [Notazione della sintassi della riga di comando](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)