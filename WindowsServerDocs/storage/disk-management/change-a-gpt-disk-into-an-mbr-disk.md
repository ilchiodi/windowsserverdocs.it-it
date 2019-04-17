---
title: Modificare un disco GPT (Tabella di partizione GUID - GUID partition table) in un disco MBR (Record di avvio principale, Master Boot Record)
description: Descrive come modificare un disco con stile di partizione GPT (Tabella di partizione GUID - GUID partition table) in un disco con stile di partizione MBR (Record di avvio principale, Master Boot Record).
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 8efe6617c46267f7e165550de82b18421ab563ca
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="change-a-guid-partition-table-gpt-disk-into-a-master-boot-record-mbr-disk"></a>Modificare un disco GPT (Tabella di partizione GUID - GUID partition table) in un disco MBR (Record di avvio principale, Master Boot Record)

> **Si applica a:** Windows 10, Windows 8.1, Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

I dischi MBR (Master Boot Record) usano la tabella di partizione BIOS standard. I dischi GPT (GUID Partition Table) utilizzano Unified Extensible Firmware Interface (UEFI). I dischi MBR non supportano più di quattro partizioni in ciascun disco. Il metodo di partizione MBR non è consigliato per i dischi più grandi di due terabyte (TB).

È possibile modificare un disco dallo stile di partizione GPT a MBR, purché il disco sia vuoto e non contenga volumi.

> [!NOTE]
> Prima di convertire i dischi, chiudere tutti i programmi in esecuzione su tali dischi.

<a name="changing-a-gpt-disk-into-an-mbr-disk"></a>Modifica di un disco GPT in un disco MBR
-------------------------------------------------------

-   [Tramite l'interfaccia di Windows](#BKMK_WINUI)
-   [Tramite la riga di comando](#BKMK_CMD)

> [!NOTE]
> Per completare questi passaggi, è necessario essere membri almeno del gruppo **Backup Operators** o **Administrators**.

 <a id="BKMK_WINUI"></a>
#### <a name="to-change-a-gpt-disk-into-an-mbr-disk-using-the-windows-interface"></a>Per modificare un disco GPT in un disco MBR tramite l'interfaccia di Windows:

1.  Eseguire il backup di tutti i volumi sul disco GPT di base che si desidera convertire in un disco MBR.

2.  Se il disco contiene partizioni o volumi, fare clic con il pulsante destro del mouse su ciascuno di essi e quindi fare clic su **Elimina Volume**.

3.  Fare clic con il pulsante destro del mouse sul disco GPT che si desidera convertire in un disco MBR, quindi fare clic su **Converti in disco MBR**.

 <a id="BKMK_CMD"></a>
#### <a name="to-change-a-gpt-disk-into-an-mbr-disk-using-a-command-line"></a>Per modificare un disco GPT in un disco MBR tramite la riga di comando

1.  Eseguire il backup di tutti i volumi sul disco GPT di base che si desidera convertire in un disco MBR.

2.  Aprire un prompt dei comandi con privilegi elevati facendo clic con il pulsante destro del mouse su **Prompt dei comandi** e selezionando **Esegui come amministratore**.

3. Digitare `diskpart`. Se il disco non contiene partizioni, andare al passaggio 6.

4.  Al prompt **DISKPART** digitare `list disk`. Prendere nota del numero di disco che si desidera eliminare.

5.  Al prompt **DISKPART** digitare `select disk <disknumber>`.

6.  Al prompt **DISKPART** digitare `clean`.

> [!IMPORTANT]
> L'esecuzione del comando **clean** eliminerà tutte le partizioni o volumi sul disco.

7.  Al prompt **DISKPART** digitare `convert mbr`.

<br />

| Valore | Descrizione |
| --- | --- |
| <p>**list disk**</p> | <p>Visualizza un elenco di dischi e le relative informazioni, ad esempio le dimensioni, quantità di spazio libero, se il disco è un disco di base o dinamico e se il disco usa lo stile di partizione MBR (Record di avvio principale, Master Boot Record) o GPT (Tabella di partizione GUID, GUID Partition Table). Il disco contrassegnato con un asterisco (*) ha lo stato attivo.</p> |
| <p>**select disk**</p> | <p>Seleziona il disco specificato, dove <em>disknumber</em> è il numero del disco, assegnandogli lo stato attivo.</p> | <p>**clean**</p> | <p>Rimuove tutte le partizioni o volumi dal disco con lo stato attivo.</p> |
| <p>**convert mbr**</p> | <p>Converte un disco di base vuoto con lo stile di partizione GPT in un disco di base con lo stile di partizione MBR.</p>

## <a name="see-also"></a>Vedere anche

-   [Notazione sintassi riga di comando](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)


