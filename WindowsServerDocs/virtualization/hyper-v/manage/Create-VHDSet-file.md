---
title: Creare file di set VHD Hyper-V
description: Passaggi per la creazione di un file VHDset in Hyper-v 2016
author: jiwool
ms.author: jiwool
manager: senthilr
ms.date: 01/26/2017
ms.topic: article
ms.prod: windows-server
ms.technology: compute-hyper-v
ms.assetid: 444e1496-9e5a-41cf-bfbc-306e2ed8e00a
audience: IT Pros
ms.reviewer: kathydav
ms.openlocfilehash: ea78bf9cb892f8e8cb41f357242f3b38a5bca934
ms.sourcegitcommit: d669d4af166b9018bcf18dc79cb621a5fee80042
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/22/2020
ms.locfileid: "82037130"
---
# <a name="create-hyper-v-vhd-set-files"></a>Creare file di set VHD Hyper-V
I file di set VHD sono un nuovo modello di disco virtuale condiviso per i cluster guest in Windows Server 2016. I file di set VHD supportano il ridimensionamento online di dischi virtuali condivisi, supportano la replica Hyper-V e possono essere inclusi in Checkpoint coerenti con l'applicazione. 

I file di set VHD utilizzano un nuovo tipo di file VHD,. VHD. I file di set VHD archiviano le informazioni di checkpoint sul disco virtuale di gruppo usato nei cluster guest, sotto forma di metadati.

Hyper-V gestisce tutti gli aspetti della gestione delle catene di checkpoint e l'Unione del set di dischi rigidi virtuali condivisi. Il software di gestione può eseguire operazioni su disco come il ridimensionamento online nei file di set VHD nello stesso modo in cui avviene per. File VHDX. Ciò significa che non è necessario che il software di gestione conosca il formato del file di set VHD.

> [!NOTE]  
> È importante valutare l'effetto dei file di set VHD prima della distribuzione nell'ambiente di produzione. Assicurarsi che non vi sia alcuna riduzione delle prestazioni o funzionale nell'ambiente, ad esempio la latenza del disco.

## <a name="create-a-vhd-set-file-from-hyper-v-manager"></a>Creazione di un file di set VHD dalla console di gestione di Hyper-V

1.  Aprire la console di gestione di Hyper-V. Fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione** e quindi **Console di gestione di Hyper-V**.
2.  Nel riquadro azioni fare clic su **Nuovo** e quindi su **Disco rigido**.
3.  Nella pagina Selezione **formato disco** selezionare **VHD impostato** come formato del disco rigido virtuale.
4.  Continuare con le pagine della procedura guidata per personalizzare il disco rigido virtuale. Per passare alla pagina successiva della procedura guidata, fare clic su **Avanti**. Per passare direttamente a una pagina specifica, fare clic sul relativo nome nel riquadro a sinistra.
5.  Dopo aver configurato il disco rigido virtuale, fare clic su **Fine**.

## <a name="create-a-vhd-set-file-from-windows-powershell"></a>Creare un file di set VHD da Windows PowerShell

Usare il cmdlet [New-VHD](https://technet.microsoft.com/library/hh848503.aspx) con il tipo di file. Dischi rigidi virtuali nel percorso del file. In questo esempio viene creato un file di set VHD denominato base. vhd con dimensioni pari a 10 gigabyte.

``` PowerShell
PS c:\>New-VHD -Path c:\base.vhds -SizeBytes 10GB
```

## <a name="migrate-a-shared-vhdx-file-to-a-vhd-set-file"></a>Eseguire la migrazione di un file VHDX condiviso a un file di set VHD

Per eseguire la migrazione di un VHDX condiviso esistente a un VHD è necessario portare la macchina virtuale offline. Questo è il processo consigliato con Windows PowerShell:

1. Rimuovere il VHDX dalla macchina virtuale. Ad esempio, eseguire: 
   ``` PowerShell
   PS c:\>Remove-VMHardDiskDrive existing.vhdx
   ```
  
2. Convertire VHDX in un VHD. Ad esempio, eseguire:
   ``` PowerShell
   PS c:\>Convert-VHD existing.vhdx new.vhds
   ```
  
3. Aggiungere i dischi rigidi virtuali alla macchina virtuale. Ad esempio, eseguire:
   ``` PowerShell
   PS c:\>Add-VMHardDiskDrive new.vhds
   ```
  



