---
title: Estendere un volume di base
description: Questo articolo descrive come aggiungere spazio su unità logiche e primarie estendendo un volume di base
ms.date: 06/07/2019
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 4cad773746ae64a2244178be83e4d59c7c44b6a7
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "66812434"
---
# <a name="extend-a-basic-volume"></a>Estendere un volume di base

> **Si applica a:** Windows 10, Windows 8.1, Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puoi aggiungere spazio a unità logiche e partizioni primarie esistenti estendendole nello spazio adiacente non allocato sullo stesso disco. Per estendere un volume di base, questo deve essere non elaborato (non formattato con un file system) o formattato con il file system NTFS. Puoi estendere un'unità logica all'interno dello spazio libero contiguo nella partizione estesa che la contiene. Se estendi un'unità logica oltre lo spazio libero disponibile nella partizione estesa, la partizione estesa aumenta per contenere l'unità logica.

Per le unità logiche e i volumi di avvio o di sistema, puoi estendere il volume solo nello spazio contiguo e solo se il disco può essere aggiornato a disco dinamico. Per altri volumi, puoi estendere il volume nello spazio non contiguo, ma ti verrà chiesto di convertire il disco in dinamico.

## <a name="extending-a-basic-volume"></a>Estensione di un volume di base

#### <a name="to-extend-a-basic-volume-using-the-windows-interface"></a>Per estendere un volume di base tramite l'interfaccia di Windows

1. In Gestione disco fai clic con il pulsante destro del mouse sul volume di base che vuoi estendere.

2. Fai clic su **Estendi volume**.

3. Segui le istruzioni visualizzate.

#### <a name="to-extend-a-basic-volume-using-a-command-line"></a>Per estendere un volume di base tramite la riga di comando

1. Aprire un prompt dei comandi e digitare `diskpart`.

2. Al prompt **DISKPART** digita `list volume`. Prendi nota del volume di base che vuoi estendere.

3. Al prompt **DISKPART** digita `select volume <volumenumber>`. In questo modo viene selezionato il volume di base *volumenumber* che vuoi estendere nello spazio vuoto contiguo sullo stesso disco.

4. Al prompt **DISKPART** digita `extend [size=<size>]`. In questo modo viene esteso il volume selezionato per *size* in megabyte (MB).

| Valore | Descrizione |
| --- | --- |
| **list volume** | Visualizza un elenco di volumi di base e dinamici su tutti i dischi. |
| **select volume** | Seleziona il volume specificato, dove <em>volumenumber</em> è il numero del volume, assegnando lo stato attivo a questo volume. Se non è specificato alcun volume, il comando **select** visualizza un elenco di volumi correnti con stato attivo. Puoi specificare il volume in base al numero, alla lettera di unità o al percorso del punto di montaggio. Su un disco di base la selezione di un volume imposta anche lo stato attivo della partizione corrispondente. |
| **extend** | <ul><li>Estende il volume con lo stato attivo nello spazio non allocato contiguo. Per i volumi di base, lo spazio non allocato deve essere nello stesso disco e deve seguire (appartenere all'offset del settore superiore) la partizione con lo stato attivo. Un volume dinamico semplice o con spanning può essere esteso in qualsiasi spazio vuoto su qualsiasi disco dinamico. Tramite questo comando puoi estendere un volume esistente nello spazio appena creato.</li ><li>Se la partizione è stata precedentemente formattata con il file system NTFS, il file system viene esteso automaticamente fino a occupare la partizione più grande. Non si verifica alcuna perdita di dati. Se la partizione è stata precedentemente formattata con qualsiasi formato di file system diverso da NTFS, il comando non riesce senza modifiche alla partizione.</li></ul> |
| **size=** <em>size</em> | Quantità di spazio, in megabyte (MB), da aggiungere alla partizione corrente. Se non specifichi una dimensione, il disco viene esteso in modo da occupare tutto lo spazio non allocato contiguo. |

## <a name="additional-considerations"></a>Considerazioni aggiuntive

-   Se il disco non contiene partizioni di avvio o di sistema, puoi estendere il volume in altri dischi non di avvio o non di sistema, ma il disco verrà convertito in disco dinamico (se può essere aggiornato).

## <a name="see-also"></a>Vedi anche

-   [Notazione della sintassi della riga di comando](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)
