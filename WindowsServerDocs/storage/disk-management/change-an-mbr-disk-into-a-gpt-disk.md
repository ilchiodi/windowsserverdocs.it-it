---
title: Modificare un disco MBR (Record di avvio principale, Master Boot Record) in un disco GPT (Tabella di partizione GUID - GUID partition table)
description: Descrive come modificare un disco MBR (Record di avvio principale, Master Boot Record) in un disco GPT (Tabella di partizione GUID - GUID partition table)
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: f28d151d3b1d6b19b9ca330c5ff8576a53acbfa5
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="change-a-master-boot-record-mbr-disk-into-a-guid-partition-table-gpt-disk"></a>Modificare un disco MBR (Record di avvio principale, Master Boot Record) in un disco GPT (Tabella di partizione GUID - GUID partition table)

> **Si applica a:** Windows 10, Windows 8.1, Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

I dischi MBR (Master Boot Record) usano la tabella di partizione BIOS standard. I dischi GPT (GUID Partition Table) utilizzano Unified Extensible Firmware Interface (UEFI). Uno dei vantaggi dei dischi GPT è la possibilità di avere più di quattro partizioni in ciascun disco. GPT è inoltre necessario per i dischi più grandi di due terabyte (TB).

È possibile modificare un disco dallo stile di partizione MBR a GPT, purché il disco non contenga partizioni o volumi.
Non è possibile utilizzare lo stile di partizione GPT su supporti rimovibili o i dischi di cluster connessi a bus SCSI o Fibre Channel condivisi che vengono utilizzati dal servizio Cluster.

> [!NOTE]
> Prima di convertire i dischi, chiudere tutti i programmi in esecuzione su tali dischi.

## <a name="changing-an-mbr-disk-into-a-gpt-disk"></a>Modifica di un disco MBR in un disco GPT

-   [Tramite l'interfaccia di Windows](#BKMK_WINUI)
-   [Tramite la riga di comando](#BKMK_CMD)

> [!NOTE]
> Per completare questi passaggi, è necessario essere membri almeno del gruppo **Backup Operators** o **Administrators**.

<a id="BKMK_WINUI"></a>
#### <a name="to-change-an-mbr-disk-into-a-gpt-disk-using-the-windows-interface"></a>Per modificare un disco MBR in un disco GPT tramite l'interfaccia di Windows

1.  Eseguire il backup o spostare i dati sul disco MBR di base che si desidera convertire in disco GPT.

2.  Se il disco contiene partizioni o volumi, fare clic con il pulsante destro del mouse su ciascuno di essi e quindi fare clic su **Elimina partizione** o **Elimina Volume**.

3.  Fare clic con il pulsante destro del mouse sul disco MBR che si desidera convertire in disco GPT, quindi fare clic su **Converti in disco GPT**.

<a id="BKMK_CMD"></a>
#### <a name="to-change-an-mbr-disk-into-a-gpt-disk-using-a-command-line"></a>Per modificare un disco MBR in un disco GPT tramite la riga di comando

1.  Eseguire il backup o spostare i dati sul disco MBR di base che si desidera convertire in disco GPT.

2.  Aprire un prompt dei comandi con privilegi elevati facendo clic con il pulsante destro del mouse su **Prompt dei comandi** e selezionando **Esegui come amministratore**.

3. Digitare `diskpart`. Se il disco non contiene partizioni o volumi, andare al passaggio 6.

4.  Al prompt **DISKPART** digitare `list disk`. Prendere nota del numero di disco che si desidera convertire.

5.  Al prompt **DISKPART** digitare `select disk <disknumber>`.

6.  Al prompt **DISKPART** digitare `clean`.

> [!NOTE]
> L'esecuzione del comando **clean** eliminerà tutte le partizioni o volumi sul disco.

7.  Al prompt **DISKPART** digitare `convert gpt`.

<br />

| Valore  | Descrizione  |
| ----- | ----|
| <p>**list disk**</p> | <p>Visualizza un elenco di dischi e le relative informazioni, ad esempio le dimensioni, quantità di spazio libero, se il disco è un disco di base o dinamico e se il disco usa lo stile di partizione MBR (Record di avvio principale, Master Boot Record) o GPT (Tabella di partizione GUID, GUID Partition Table). Il disco contrassegnato con un asterisco (*) ha lo stato attivo.</p> |
| <p>**select disk** <em>disknumber</em></p> | <p>Seleziona il disco specificato, dove <em>disknumber</em> è il numero del disco, assegnandogli lo stato attivo.</p> |
| <p>**clean**</p> | <p>Rimuove tutte le partizioni o volumi dal disco con lo stato attivo.</p>  |
| <p>**convert gpt**</p>| <p>Converte un disco di base vuoto con lo stile di partizione MBR in un disco di base con lo stile di partizione GPT.</p> |

## <a name="see-also"></a>Vedere anche

-   [Notazione sintassi riga di comando](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)


