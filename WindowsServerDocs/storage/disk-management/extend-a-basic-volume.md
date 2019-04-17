---
title: Estendere un volume di base
description: "Questo articolo illustra come aggiungere spazio su unità logiche e primarie estendendo un volume di base"
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 29b7b61f7edc20edda7bc18b82db17447badc0f2
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="extend-a-basic-volume"></a>Estendere un volume di base

> **Si applica a:** Windows 10, Windows 8.1, Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

È possibile aggiungere spazio a unità logiche e partizioni primarie esistenti estendendole nello spazio adiacente non allocato sullo stesso disco. Per estendere un volume di base, deve essere non elaborato (non formattato con un file system) o formattato con il file system NTFS. È possibile estendere un'unità logica all'interno di spazio libero contiguo nella partizione estesa che lo contiene. Se si estende un'unità logica oltre lo spazio libero disponibile nella partizione estesa, la partizione estesa aumenta per contenere l'unità logica.

Per le unità logiche e i volumi di avvio o di sistema, è possibile estendere il volume solo nello spazio contiguo e solo se il disco può essere aggiornato a un disco dinamico. Per altri volumi, è possibile estendere il volume nello spazio non contiguo, ma verrà richiesto di convertire il disco in dinamico.

## <a name="extending-a-basic-volume"></a>Estensione di un volume di base

-   [Tramite l'interfaccia di Windows](#BKMK_WINUI)
-   [Tramite la riga di comando](#BKMK_CMD)

<a href="" id="BKMK_WINUI"></a>
#### <a name="to-extend-a-basic-volume-using-the-windows-interface"></a>Per estendere un volume di base tramite l'interfaccia di Windows

1.  In Gestione disco fare clic con il pulsante destro del mouse sul volume di base che si desidera estendere.

2.  Fare clic su **Estendi volume**.

3.  Seguire le istruzioni visualizzate.

<a href="" id="BKMK_CMD"></a>
#### <a name="to-extend-a-basic-volume-using-a-command-line"></a>Per estendere un volume di base tramite la riga di comando

1.  Aprire un prompt dei comandi e digitare `diskpart`.

2.  Al prompt **DISKPART** digitare `list volume`. Prendere nota del volume di base da estendere.

3.  Al prompt **DISKPART** digitare `select volume <volumenumber>`. In questo modo si seleziona il volume di base *volumenumber* che si desidera estendere nello spazio vuoto contiguo sullo stesso disco.

4.  Al prompt **DISKPART** digitare `extend [size=<size>]`. In questo modo si estende il volume selezionato per *dimensioni* in megabyte (MB).

<br />

| Valore | Descrizione |
| --- | --- |
| <p>**list volume**</p> | <p>Visualizza un elenco di volumi di base e dinamici su tutti i dischi.</p> |
| <p>**select volume**</p> | <p>Seleziona il volume specificato, dove <em>volumenumber</em> è il numero del volume, assegnandogli lo stato attivo. Se non è specificato alcun volume, il comando **select** visualizza un elenco di volumi correnti con stato attivo. È possibile specificare il volume in base al numero, alla lettera di unità o al percorso del punto di montaggio. Su un disco di base la selezione di un volume restituisce anche lo stato attivo della partizione corrispondente.</p> |
| <p>**extend**</p> | <p><ul><li>Estende il volume con lo stato attivo nello spazio non allocato contiguo. Per i volumi di base, lo spazio non allocato deve essere nello stesso disco e deve seguire (appartenere all'offset del settore superiore) la partizione con lo stato attivo. Un volume dinamico semplice o con spanning può essere esteso in qualsiasi spazio vuoto su qualsiasi disco dinamico. Tramite questo comando è possibile estendere un volume esistente nello spazio appena creato.</p></li ><p><li>Se la partizione è stata precedentemente formattata con il file system NTFS, il file system viene esteso automaticamente fino a occupare la partizione più grande. Non si verifica alcuna perdita di dati. Se la partizione è stata precedentemente formattata con qualsiasi formato di file system diverso da NTFS, il comando ha esito negativo senza apportare alcuna modifica alla partizione.</p></li></ul>|
| <p>**size=** <em>size</em></p> | <p>La quantità di spazio, in megabyte (MB), da aggiungere alla partizione corrente. Se non si specifica una dimensione, il disco viene esteso in modo da occupare tutto lo spazio non allocato contiguo.</p> |

## <a name="additional-considerations"></a>Considerazioni aggiuntive

-   Se il disco non contiene partizioni di avvio o di sistema, è possibile estendere il volume in altri dischi non di avvio o non di sistema, ma il disco verrà convertito in un disco dinamico (se può essere aggiornato).

## <a name="see-also"></a>Vedere anche

-   [Notazione sintassi riga di comando](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)

