---
title: Modificare un disco MBR (Record di avvio principale, Master Boot Record) in un disco GPT (Tabella di partizione GUID - GUID partition table)
description: Descrive come modificare un disco MBR (Record di avvio principale, Master Boot Record) in un disco GPT (Tabella di partizione GUID - GUID partition table)
ms.date: 06/19/2018
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 8af76edbdb5fc2aa5768811f01c563aab1a89fc6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849602"
---
# <a name="convert-an-mbr-disk-into-a-gpt-disk"></a>Convertire un disco MBR in un disco GPT

> **Si applica a:** Windows 10, Windows 8.1, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

I dischi MBR (Master Boot Record) usano la tabella di partizione BIOS standard. I dischi GPT (GUID Partition Table) utilizzano Unified Extensible Firmware Interface (UEFI). Uno dei vantaggi dei dischi GPT è la possibilità di avere più di quattro partizioni in ciascun disco. GPT è inoltre necessario per i dischi più grandi di due terabyte (TB).

È possibile modificare un disco dallo stile di partizione MBR a GPT, purché il disco non contenga partizioni o volumi.


> [!NOTE]
> Prima di convertire un disco, eseguire il backup tutti i dati su di esso e chiudere tutti i programmi che accedono al disco.


> [!NOTE]
> Per completare questi passaggi, devi essere un membro del gruppo **Backup Operators** o **Administrators**.

<a id="BKMK_WINUI"></a>

## <a name="converting-using-the-windows-interface"></a>Conversione tramite l'interfaccia di Windows

1.  Eseguire il backup o spostare i dati sul disco MBR di base che si desidera convertire in disco GPT.

2.  Se il disco contiene partizioni o volumi, fare clic con il pulsante destro del mouse su ciascuno di essi e quindi fare clic su **Elimina partizione** o **Elimina Volume**.

3.  Fare clic con il pulsante destro del mouse sul disco MBR che si desidera convertire in disco GPT, quindi fare clic su **Converti in disco GPT**.

<a id="BKMK_CMD"></a>

## <a name="converting-using-a-command-line"></a>Conversione tramite la riga di comando

Usare la procedura seguente per convertire un disco MBR vuoto in un disco GPT. È inoltre disponibile un MBR2GPT. Strumento di file EXE che è possibile usare, ma è un po' complessa - vedere [partizione convertire MBR a GPT](https://docs.microsoft.com/windows/deployment/mbr-to-gpt) per altri dettagli.

1.  Eseguire il backup o spostare i dati sul disco MBR di base che si desidera convertire in disco GPT.

2.  Aprire un prompt dei comandi con privilegi elevati facendo clic con il pulsante destro del mouse su **Prompt dei comandi** e selezionando **Esegui come amministratore**.

3. Digitare `diskpart`. Se il disco non contiene partizioni o volumi, andare al passaggio 6.

4.  Al prompt **DISKPART** digita `list disk`. Prendere nota del numero di disco che si desidera convertire.

5.  Al prompt **DISKPART** digita `select disk <disknumber>`.

6.  Al prompt **DISKPART** digita `clean`.

    > [!NOTE]
    > L'esecuzione del comando **clean** eliminerà tutte le partizioni o volumi sul disco.

7.  Al prompt **DISKPART** digita `convert gpt`.

<br />

| Value  | Descrizione  |
| ----- | ----|
| <p>**disco di elenco**</p> | <p>Visualizza un elenco di dischi e le relative informazioni, ad esempio le dimensioni, quantità di spazio libero, se il disco è un disco di base o dinamico e se il disco usa lo stile di partizione MBR (Record di avvio principale, Master Boot Record) o GPT (Tabella di partizione GUID, GUID Partition Table). Il disco contrassegnato con un asterisco (*) ha lo stato attivo.</p> |
| <p>**Selezionare disco** <em>disknumber</em></p> | <p>Seleziona il disco specificato, dove <em>disknumber</em> è il numero del disco, assegnandogli lo stato attivo.</p> |
| <p>**clean**</p> | <p>Rimuove tutte le partizioni o volumi dal disco con lo stato attivo.</p>  |
| <p>**convert gpt**</p>| <p>Converte un disco di base vuoto con lo stile di partizione MBR in un disco di base con lo stile di partizione GPT.</p> |

## <a name="see-also"></a>Vedere anche

-   [Notazione della sintassi della riga di comando](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)


