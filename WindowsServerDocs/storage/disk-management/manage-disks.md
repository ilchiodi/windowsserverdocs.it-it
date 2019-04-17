---
title: Gestire dischi
description: In questo articolo viene descritto come gestire dischi
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: ae96071733b961fbe65551120894c21c633db83e
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="manage-disks"></a>Gestire dischi

> **Si applica a:** Windows 10, Windows 8.1, Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questo argomento e i relativi argomenti secondari descrivono l'uso di Gestione disco per gestire i dischi in un computer e include informazioni sulla conversione dei dischi tra i diversi stili di partizione e il modo in cui Windows gestisce lo stato online dei nuovi dischi.

## <a name="converting-disk-types"></a>Conversione dei tipi di disco

Sebbene Gestione disco consenta di modificare i dischi tra i diversi tipi e gli stili di partizione, alcune delle operazioni sono irreversibili (a meno che non si riformatti l'unità). È consigliabile valutare attentamente il tipo di disco e lo stile partizione più appropriati per l'applicazione.

La tabella seguente mostra le opzioni per la conversione dei dischi tra i vari stili di partizione:

| Tipo di disco | Conversione in MBR  | Conversione in GPT| Conversione in disco dinamico |
| ---- | --- | --- |--- |
| <p>MBR (Record di avvio principale, Master Boot Record)</p> | <p>--</p> | <p>Consentito (se sul disco non sono presenti volumi).</p> | <p>Consentito, ma il disco potrebbe risultare non avviabile. Vedere la Nota.</p> |
| <p>GPT (Tabella di partizione GUID, GUID Partition Table)</p> | <p>Consentito (se sul disco non sono presenti volumi).</p> | <p>--</p>  | <p>Consentito, ma il disco potrebbe risultare non avviabile. Vedere la Nota.</p> |


> [!NOTE]
> In uno scenario di avvio multiplo, se l'avvio viene eseguito in un sistema operativo e si converte un disco MBR di base che contiene un sistema operativo alternativo in un disco dinamico, non sarà più possibile eseguire l'avvio con il sistema operativo alternativo.

## <a name="online-and-offline-status"></a>Stato online e offline

Gestione disco visualizza lo stato online e offline dei dischi. 

In Windows, per impostazione predefinita, tutti i dischi appena individuati vengono portati online con accesso in lettura e scrittura. In Windows Server, per impostazione predefinita, i dischi appena individuati vengono portati online con accesso in lettura e scrittura a meno che non si trovino in un bus condiviso (ad esempio, SCSI, iSCSI, Serial Attached SCSI o Fibre Channel). I dischi in un bus condiviso saranno offline la prima volta che vengono rilevati.

Se un disco è offline, è necessario portarlo online prima di poterlo inizializzare o creare volumi su di esso. 

-  Porta un disco online o portalo offline facendo clic con il pulsante destro del mouse sul nome del disco e quindi scegliendo l'azione appropriata.

## <a name="see-also"></a>Vedere anche

-   [Inizializzare nuovi dischi](initialize-new-disks.md)
-   [Spostare dischi in un altro computer](move-disks-to-another-computer.md)
-   [Convertire nuovamente un disco dinamico in un disco di base](change-a-dynamic-disk-back-to-a-basic-disk.md)
-   [Modificare un disco MBR (Record di avvio principale, Master Boot Record) in un disco GPT (Tabella di partizione GUID, GUID Partition Table)](change-an-mbr-disk-into-a-gpt-disk.md)
-   [Modificare un disco GPT (Tabella di partizione GUID, GUID Partition Table) in un disco MBR (Record di avvio principale, Master Boot Record)](change-a-gpt-disk-into-an-mbr-disk.md)
-   [Gestire dischi rigidi virtuali](manage-virtual-hard-disks.md)