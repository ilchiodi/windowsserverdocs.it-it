---
title: Creare i file di disco rigido virtuale di Hyper-V impostato
description: Passaggi per creare un file VHDset 2016 Hyper-v
author: jiwool
ms.author: jiwool
manager: senthilr
ms.date: 01/26/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: compute-hyper-v
ms.assetid: 444e1496-9e5a-41cf-bfbc-306e2ed8e00a
audience: IT Pros
ms.reviewer: kathydav
ms.openlocfilehash: a5a6f79d362b9058ca29d979457a1dcdfc0c9f82
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445696"
---
# <a name="create-hyper-v-vhd-set-files"></a>Creare i file di disco rigido virtuale di Hyper-V impostato
I file di disco rigido virtuale impostato sono un nuovo modello di disco virtuale condiviso per i cluster guest in Windows Server 2016. File disco rigido virtuale impostato supportano il ridimensionamento online di dischi virtuali condivisi, supportano la Replica Hyper-V e possono essere incluso nel Checkpoint coerente con l'applicazione. 

File disco rigido virtuale impostato usano un nuovo tipo di file di disco rigido virtuale. DISCHI RIGIDI VIRTUALI. File disco rigido virtuale impostato archiviano le informazioni di checkpoint sul disco virtuale gruppo usato nei cluster guest, sotto forma di metadati.

Hyper-V gestisce tutti gli aspetti della gestione di catene di checkpoint e unione del disco rigido virtuale condiviso impostare. Software di gestione è possibile eseguire operazioni su disco, ad esempio il ridimensionamento online sui file di disco rigido virtuale impostato nello stesso modo per. File VHDX. Ciò significa che il software di gestione non è necessario conoscere il formato del file disco rigido virtuale impostato.

## <a name="create-a-vhd-set-file-from-hyper-v-manager"></a>Creare un file di disco rigido virtuale impostare dalla console di gestione Hyper-V

1.  Aprire la console di gestione di Hyper-V. Fare clic su **avviare**, scegliere **Strumenti di amministrazione**, quindi fare clic su **gestione di Hyper-V**.
2.  Nel riquadro azioni fare clic su **Nuovo** e quindi su **Disco rigido**.
3.  Nel **Selezione formato disco** pagina, selezionare **VHD impostare** come il formato del disco rigido virtuale.
4.  Continuare con le pagine della procedura guidata per personalizzare il disco rigido virtuale. Per passare alla pagina successiva della procedura guidata, fare clic su **Avanti**. Per passare direttamente a una pagina specifica, fare clic sul relativo nome nel riquadro a sinistra.
5.  Dopo aver configurato il disco rigido virtuale, fare clic su **Fine**.

## <a name="create-a-vhd-set-file-from-windows-powershell"></a>Creare un file di disco rigido virtuale impostato da Windows PowerShell

Usare la [New-VHD](https://technet.microsoft.com/library/hh848503.aspx) cmdlet con il tipo di file. Dischi rigidi VIRTUALI nel percorso del file. Questo esempio crea un file disco rigido virtuale impostato denominato base.vhds che dimensioni pari a 10 GB.

``` PowerShell
PS c:\>New-VHD -Path c:\base.vhds -SizeBytes 10GB
```

## <a name="migrate-a-shared-vhdx-file-to-a-vhd-set-file"></a>Eseguire la migrazione di un file VHDX condiviso in un file disco rigido virtuale impostato

La migrazione di un file VHDX condiviso esistente in un VHD, è necessario portare offline la macchina virtuale. Questo è il processo consigliato usando Windows PowerShell:

1. Rimuovere il file VHDX dalla macchina virtuale. Ad esempio, eseguire: 
   ``` PowerShell
   PS c:\>Remove-VMHardDiskDrive existing.vhdx
   ```
  
2. Convertire il disco VHDX in un VHD. Ad esempio, eseguire:
   ``` PowerShell
   PS c:\>Convert-VHD existing.vhdx new.vhds
   ```
  
3. Aggiungere i dischi rigidi VIRTUALI per la macchina virtuale. Ad esempio, eseguire:
   ``` PowerShell
   PS c:\>Add-VMHardDiskDrive new.vhds
   ```
  



