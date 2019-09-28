---
title: Inizializzare nuovi dischi
description: Come inizializzare nuovi dischi con Gestione disco, preparandoli per l'uso. Include anche collegamenti alla risoluzione dei problemi.
ms.date: 06/07/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: b38fd0b88cea3fcc386959c08af1169302ddaa1c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385904"
---
# <a name="initialize-new-disks"></a>Inizializzare nuovi dischi

> **Si applica a:** Windows 10, Windows 8.1, Windows 7, Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Se aggiungi un nuovo disco al PC e questo non viene visualizzato in Esplora file, potresti dover [aggiungere una lettera di unità](change-a-drive-letter.md) o inizializzarlo prima di poterlo usare. Puoi inizializzare un'unità solo se non è ancora formattata. L'inizializzazione di un disco ne cancella tutto il contenuto e lo prepara per l'uso in Windows, dopodiché puoi formattarlo e archiviarvi file.

> [!WARNING]
> Se il disco contiene già file che ti interessano, non inizializzarlo o perderai tutti i file. Ti consigliamo invece di seguire le indicazioni per la risoluzione dei problemi per verificare se sia ancora possibile leggere i file. Vedi [Lo stato di un disco è Non inizializzato o il disco è mancante](troubleshooting-disk-management.md#a-disks-status-is-not-initialized-or-the-disk-is-missing).

## <a name="to-initialize-new-disks"></a>Per inizializzare nuovi dischi

Ecco come inizializzare un nuovo disco tramite Gestione disco. Se preferisci usare PowerShell, esegui invece il cmdlet [initialize-disk](https://docs.microsoft.com/powershell/module/storage/initialize-disk).

1. Apri Gestione disco con autorizzazioni di amministratore. 
 
    A questo scopo, nella casella di ricerca sulla barra delle applicazioni digita **Gestione disco**, tieni selezionato (o fai clic con il pulsante destro del mouse) **Gestione disco** e quindi scegli **Esegui come amministratore** > **Sì**. Se non riesci ad aprire lo strumento come amministratore, digita invece **Gestione computer** e quindi passa ad **Archiviazione** > **Gestione disco**.
1. In Gestione disco fai clic con il pulsante destro del mouse sul disco che vuoi inizializzare e quindi scegli **Inizializza disco**. Se il disco è elencato come *Offline*, prima di tutto fai clic con il pulsante destro del mouse e scegli **Online**.

     Alcune unità USB non hanno la possibilità di essere inizializzate, ma è solo possibile formattarle e associarle a una [lettera di unità](change-a-drive-letter.md).

    ![Gestione disco che mostra un disco non formattato con il menu di scelta rapida Inizializza disco visualizzato](media/uninitialized-disk.PNG)
2. Nella finestra di dialogo **Inizializza disco** (mostrata qui) verifica che sia selezionato il disco corretto e quindi fai clic su **OK** per accettare lo stile di partizione predefinito. Se vuoi modificare lo stile di partizione (GPT o MBR), vedi [Informazioni sugli stili di partizione: GPT e MBR](#about-partition-styles---gpt-and-mbr).

     Lo stato del disco cambia per un momento in **Inizializzazione in corso** e quindi passa allo stato **Online**. Se l'inizializzazione non riesce per qualche motivo, vedi [Lo stato di un disco è Non inizializzato o il disco è mancante](troubleshooting-disk-management.md#a-disks-status-is-not-initialized-or-the-disk-is-missing).

    ![Finestra di dialogo Inizializza disco con lo stile di partizione GPT selezionato](media/initialize-disk.PNG)

## <a name="about-partition-styles---gpt-and-mbr"></a>Informazioni sugli stili di partizione: GPT e MBR

I dischi possono essere suddivisi in più blocchi chiamati partizioni. Ogni partizione, anche se ne è presente una sola, deve avere uno stile di partizione, GPT o MBR. Windows usa lo stile di partizione per identificare come accedere ai dati sul disco.

Questo non è molto incoraggiante, ma in definitiva oggi non devi preoccuparti dello stile di partizione, perché Windows usa automaticamente il tipo di disco appropriato.

La maggior parte dei PC usa il tipo di disco GPT (GUID Partition Table, tabella delle partizioni GUID) per le unità disco rigido ed SSD. GPT è più affidabile e permette volumi maggiori di 2 TB. Il tipo di disco MBR (Master Boot Record, record di avvio principale) viene usato dai PC a 32 bit, i PC obsoleti e le unità rimovibili, come le schede di memoria.

Per convertire un disco da MBR a GPT o viceversa, devi prima di tutto eliminare i volumi dal disco, cancellandone tutto il contenuto. Per altre informazioni, vedi [Convertire un disco MBR in disco GPT](change-an-mbr-disk-into-a-gpt-disk.md), o [convertire un disco GPT in disco MBR](change-a-gpt-disk-into-an-mbr-disk.md).