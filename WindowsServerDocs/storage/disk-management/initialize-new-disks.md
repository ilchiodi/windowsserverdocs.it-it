---
title: Inizializzare nuovi dischi
description: Viene descritto come inizializzare nuovi dischi con Gestione disco, ottenendoli pronto per l'uso. Include anche collegamenti alla risoluzione dei problemi.
ms.date: 10/24/2018
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e009780d83220b528ba7dac6e2561be36e662f71
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192756"
---
# <a name="initialize-new-disks"></a>Inizializzare nuovi dischi

> **Si applica a:** Windows 10, Windows 8.1, Windows 7, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se si aggiunge un nuovo disco al computer e non viene visualizzata in Esplora File, potrebbe essere necessario [aggiungere una lettera di unità](change-a-drive-letter.md), o inizializzarlo prima di poterla usare. È possibile inizializzarla solo un'unità non è ancora formattato. Inizializzazione di un disco Cancella tutto il suo contenuto e lo prepara per l'utilizzo da Windows, dopo il quale è possibile formattarlo e quindi archiviare i file disponibili.

> [!WARNING]
> Se il disco contiene già i file che si è interessati, non inizializzarlo, si perderanno tutti i file. Al contrario si consiglia di risoluzione dei problemi del disco per verificare se è possibile leggere i file, vedere [lo stato del disco non inizializzato o il disco risulta manca interamente](troubleshooting-disk-management.md#a-disks-status-is-not-initialized-or-the-disk-is-missing).

## <a name="to-initialize-new-disks"></a>Per inizializzare nuovi dischi

Di seguito viene illustrato come inizializzare un nuovo disco tramite Gestione disco. Se si preferisce usare PowerShell, usare il [initialize-disk](https://docs.microsoft.com/powershell/module/storage/initialize-disk) cmdlet invece.

1. Aprire Gestione disco con le autorizzazioni di amministratore. 
 
    A tale scopo, nella casella di ricerca della barra delle applicazioni, digitare **Gestione disco**, selezionare e tenere premuto (o destro) **Gestione disco**, quindi selezionare **Esegui come amministratore**  >  **Sì**. Se non è possibile aprirlo come amministratore, digitare **Gestione Computer** invece e quindi aprire **archiviazione** > **Gestione disco**.
1. In Gestione disco, fare doppio clic il disco da inizializzare e quindi fare clic su **Inizializza disco** (illustrato di seguito). Se il disco viene elencato come *Offline*, prima di tutto pulsante destro del mouse e selezionare **Online**.

     Si noti che alcune unità USB non hanno la possibilità di essere inizializzato, è sufficiente ottenere formattati e un [lettera unità](change-a-drive-letter.md).

    ![Gestione disco che mostra un disco non formattato con il menu di scelta rapida Inizializza disco visualizzato](media\uninitialized-disk.PNG)
2. Nel **Inizializza disco** finestra di dialogo (illustrato di seguito), controllare per verificare che il disco corretto sia selezionato e quindi fare clic su **OK** accettare lo stile di partizione predefinito. Se si desidera modificare, vedere (GPT o MBR) di stile partizione [sugli stili di partizione - GPT e MBR](#about-partition-styles---gpt-and-mbr).

     Passa rapidamente lo stato del disco da **Initializing** e quindi per il **Online** dello stato. Se l'inizializzazione ha esito negativo per qualche motivo, vedere [lo stato del disco non inizializzato o il disco risulta manca interamente](troubleshooting-disk-management.md#a-disks-status-is-not-initialized-or-the-disk-is-missing).

    ![Nella finestra di dialogo Inizializza disco selezionato lo stile di partizione GPT](media\initialize-disk.PNG)

## <a name="about-partition-styles---gpt-and-mbr"></a>Informazioni sugli stili di partizione - GPT e MBR

I dischi possono venga diviso in blocchi più chiamati partizioni. Ogni partizione: anche se è presente un solo - deve disporre di uno stile di partizione - MBR o GPT. Windows Usa lo stile di partizione per informazioni su come accedere ai dati sul disco.

Come utili come ciò probabilmente non è disponibile, la conclusione è che oggi, in genere non è necessario preoccuparsi di stile di partizione, Windows utilizza automaticamente il tipo di disco appropriato.

La maggior parte dei PC Usa il tipo di disco della tabella di partizione GUID (GPT) per i dischi rigidi e unità SSD. GPT è più affidabile e consente di volumi di dimensioni superiori a 2 TB. Il tipo di disco il record di avvio principale (MBR, Master Boot Record) precedenti viene utilizzato da PC a 32 bit, i PC obsoleti e le unità rimovibili, ad esempio le schede di memoria.

Per convertire un disco da MBR a GPT o viceversa, è innanzitutto necessario eliminare tutti i volumi dal disco, cancellando tutti gli elementi sul disco. Per altre informazioni, vedi [convertire un disco MBR in un disco GPT](change-an-mbr-disk-into-a-gpt-disk.md), o [convertire un disco GPT in un disco MBR](change-a-gpt-disk-into-an-mbr-disk.md).