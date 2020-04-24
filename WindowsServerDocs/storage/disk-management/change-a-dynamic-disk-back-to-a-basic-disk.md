---
title: Riconvertire un disco dinamico in disco di base
description: Descrive come riconvertire un disco dinamico in disco di base.
ms.date: 12/18/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 8ad14225592d627b6ff88b9e2286b686aa549392
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "75351947"
---
# <a name="change-a-dynamic-disk-back-to-a-basic-disk"></a>Riconvertire un disco dinamico in disco di base

> **Si applica a:** Windows 10, Windows 8.1, Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento descrive come eliminare tutti i dati su un disco dinamico e quindi riconvertirlo in disco di base. I dischi dinamici sono deprecati in Windows e non è più consigliabile usarli. Consigliamo invece di usare dischi di base o la tecnologia [Spazi di archiviazione](https://support.microsoft.com/help/12438/windows-10-storage-spaces) più recente quando vuoi unire in pool i dischi in volumi di grandi dimensioni. Se vuoi eseguire il mirroring del volume da cui viene avviato Windows, ti consigliamo di usare un controller RAID hardware, come quello incluso in molte schede madri.

> [!WARNING]
> Per riconvertire un disco dinamico in disco di base, devi eliminare tutti i volumi dal disco, cancellando in modo permanente tutti i dati sul disco. Assicurati di eseguire il backup di tutti i dati che vuoi conservare prima di continuare.

## <a name="to-change-a-dynamic-disk-back-to-a-basic-disk-by-using-disk-management"></a>Per riconvertire un disco dinamico in disco di base tramite Gestione disco

1.  Esegui il backup di tutti i volumi sul disco che vuoi convertire da disco dinamico a disco di base.

2. Apri Gestione disco con autorizzazioni di amministratore.

   Per eseguire in modo semplice questa operazione, digita **Gestione computer** nella casella di ricerca sulla barra delle applicazioni, seleziona e tieni premuto (oppure fai clic con il pulsante destro del mouse) **Gestione computer** e quindi scegli **Esegui come amministratore** > **Sì**. Dopo l'apertura di Gestione computer, passa ad **Archiviazione** > **Gestione disco**.

2.  In Gestione disco seleziona e tieni premuto (oppure fai clic con il pulsante destro del mouse) ogni volume del disco dinamico che vuoi convertire in disco di base e quindi scegli **Elimina volume**.

3.  Dopo aver eliminato tutti i volumi sul disco, fai clic con il pulsante destro del mouse sul disco e quindi scegli **Converti in disco di base**.

## <a name="to-change-a-dynamic-disk-back-to-a-basic-disk-by-using-a-command-line"></a>Per riconvertire un disco dinamico in disco di base tramite una riga di comando

1.  Esegui il backup di tutti i volumi sul disco che vuoi convertire da disco dinamico a disco di base.

2.  Aprire un prompt dei comandi e digitare `diskpart`.

3.  Al prompt **DISKPART** digita `list disk`. Prendi nota del numero del disco che vuoi convertire in disco di base.

4.  Al prompt **DISKPART** digita `select disk <disknumber>`.

5.  Al prompt **DISKPART** digita `detail disk`.

6.  Per ogni volume sul disco, al prompt **DISKPART** digita `select volume= <volumenumber>` e quindi `delete volume`.

7.  Al prompt **DISKPART** digita `select disk <disknumber>`, specificando il numero del disco che vuoi convertire in disco di base.

8.  Al prompt **DISKPART** digita `convert basic`.

| Valore  | Descrizione |
| --- | --- |
| **list disk**                         | Visualizza un elenco di dischi e le relative informazioni, tra cui dimensioni, quantità di spazio libero, se il disco è un disco di base o dinamico e se il disco usa lo stile di partizione MBR (Master Boot Record, record di avvio principale) o GPT (GUID Partition Table, tabella delle partizioni GUID). Il disco contrassegnato con un asterisco (*) ha lo stato attivo. |
| **select disk** <em>disknumber</em>   | Seleziona il disco specificato, dove <em>disknumber</em> è il numero del disco, assegnando lo stato attivo a questo disco.  |
| **detail disk** <em>disknumber</em>   | Visualizza le proprietà del disco selezionato e i volumi presenti sul disco.  |
| **select volume** <em>disknumber</em> | Seleziona il volume specificato, dove <em>disknumber</em> è il numero del volume, assegnando lo stato attivo a questo volume. Se non è specificato alcun volume, il comando **select** visualizza un elenco di volumi correnti con stato attivo. Puoi specificare il volume in base al numero, alla lettera di unità o al percorso del punto di montaggio. Su un disco di base la selezione di un volume imposta anche lo stato attivo della partizione corrispondente. |
| **delete volume**                     | Elimina il volume selezionato. Non puoi eliminare il volume di sistema, il volume di avvio o qualsiasi volume che contiene il file di paging attivo o il dump di arresto anomalo del sistema (immagine della memoria). |
| **convert basic** | Converte un disco dinamico vuoto in disco di base.  |

## <a name="additional-considerations"></a>Considerazioni aggiuntive

-   Il disco non deve contenere volumi o dati prima di poterlo riconvertire in disco di base. Se vuoi conservare i dati, eseguine il backup o spostali su un altro volume prima di convertire il disco in disco di base.
-   Dopo aver riconvertito un disco dinamico in disco di base, puoi creare solo unità logiche e partizioni su questo disco.

## <a name="see-also"></a>Vedi anche

-   [Notazione della sintassi della riga di comando](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)