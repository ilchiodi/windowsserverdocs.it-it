---
title: Estendere un volume di base
description: Puoi aggiungere spazio a un volume esistente in Windows, estenderlo in uno spazio vuoto nell'unità, ma solo se lo spazio vuoto non contiene un volume (non allocato) e si trova immediatamente dopo il volume che vuoi estendere, senza altri volumi tra i due. Questo articolo descrive come eseguire l'operazione di estensione del volume.
ms.date: 12/19/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c72b242437c4c308da77a25e06f3d76e4c65f480
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "75351894"
---
# <a name="extend-a-basic-volume"></a>Estendere un volume di base

> **Si applica a:** Windows 10, Windows 8.1, Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Puoi usare Gestione disco per aggiungere spazio a un volume esistente, estenderlo in uno spazio vuoto nell'unità, ma solo se lo spazio vuoto non contiene un volume (è non allocato) e si trova immediatamente dopo il volume che vuoi estendere, senza altri volumi tra i due, come illustrato nell'immagine seguente. Il volume da estendere deve essere formattato con i file system NTFS o ReFS.

:::image type="content" source="media/extend-volume-space-highlighted.png" alt-text="Gestione disco che illustra lo spazio libero in cui si può estendere un volume.":::

## <a name="to-extend-a-volume-by-using-disk-management"></a>Per estendere un volume usando Gestione disco

Ecco come estendere un volume in uno spazio vuoto subito dopo il volume sull'unità:

1. Apri Gestione disco con autorizzazioni di amministratore.

   Per eseguire in modo semplice questa operazione, digita **Gestione computer** nella casella di ricerca sulla barra delle applicazioni, seleziona e tieni premuto (oppure fai clic con il pulsante destro del mouse) **Gestione computer** e quindi scegli **Esegui come amministratore** > **Sì**. Dopo l'apertura di Gestione computer, passa ad **Archiviazione** > **Gestione disco**.
2. Seleziona e tieni premuto (oppure fai clic con il pulsante destro del mouse) il volume che vuoi estendere e quindi scegli **Estendi volume**.

   Se **Estendi volume** è disabilitato, verifica quanto segue:
    - Gli strumenti Gestione disco o Gestione computer sono stati aperti con autorizzazioni di amministratore
    - Lo spazio non allocato è immediatamente dopo il volume (a destra), come illustrato nella figura precedente. Se è presente un altro volume tra lo spazio non allocato e il volume che vuoi estendere, puoi eliminare il volume compreso e tutti i file che contiene (assicurati di eseguire prima il backup o lo spostamento dei file importanti), usare un'app di partizionamento del disco non Microsoft che possa spostare i volumi senza eliminare definitivamente i dati oppure ignorare l'estensione del volume e creare invece un volume separato nello spazio non allocato.
    - Il volume è formattato con il file system NTFS o ReFS. Non puoi estendere altri file system, devi quindi spostare o eseguire il backup dei file nel volume e infine formattare il volume con il file system NTFS o ReFS.
    - Se il disco è più grande di 2 TB, assicurati che usi lo schema di partizionamento GPT. Per usare più di 2 TB su un disco, deve essere inizializzato con lo schema di partizionamento GPT. Per eseguire la conversione in GPT, vedi [Modificare un disco MBR in un disco GPT](change-an-mbr-disk-into-a-gpt-disk.md).
    - Se non riesci ancora a estendere il volume, cerca nel sito della [community Microsoft - File, cartelle e archiviazione in linea](https://answers.microsoft.com/en-us/windows/forum/windows_10-files?sort=lastreplydate&dir=desc&tab=All&status=all&mod=&modAge=&advFil=&postedAfter=&postedBefore=&threadType=all&isFilterExpanded=true&tm=1514405359639) e se non trovi una risposta, inserisci una domanda perché Microsoft o altri membri della community possano provare ad aiutarti. In alternativa, [contatta il Supporto tecnico Microsoft](https://support.microsoft.com/contactus/).

3. Seleziona **Avanti** e quindi nella pagina **Seleziona dischi** della procedura guidata (illustrata qui) specifica l'estensione del volume. In genere è consigliabile usare il valore predefinito, che usa tutto lo spazio disponibile, ma puoi usare un valore inferiore se vuoi creare volumi aggiuntivi nello spazio libero.

   :::image type="content" source="media/extend-volume-wizard.png" alt-text="Procedura guidata Estendi volume che illustra un volume esteso in modo da richiedere tutto lo spazio disponibile":::

4. Seleziona **Avanti** e quindi **Fine** per estendere il volume.

## <a name="to-extend-a-volume-by-using-powershell"></a>Per estendere un volume usando PowerShell

1. Seleziona e tieni premuto (oppure fai clic con il pulsante destro del mouse) il pulsante Start e quindi scegli Windows PowerShell (Amministratore).
2. Immetti il comando seguente per ridimensionare il volume alla dimensione massima, specificando la lettera dell'unità del volume che vuoi estendere nella variabile *$drive_letter*:

   ```PowerShell
   # Variable specifying the drive you want to extend
   $drive_letter = "C"

   # Script to get the partition sizes and then resize the volume
   $size = (Get-PartitionSupportedSize -DriveLetter $drive_letter)
   Resize-Partition -DriveLetter $drive_letter -Size $size.SizeMax
   ```

## <a name="see-slso"></a>Vedi anche

- [Resize-Partition](https://docs.microsoft.com/powershell/module/storage/resize-partition)
- [Extend](https://docs.microsoft.com/windows-server/administration/windows-commands/extend)
