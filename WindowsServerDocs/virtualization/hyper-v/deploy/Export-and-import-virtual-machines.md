---
title: Esportare e importare macchine virtuali
description: Viene illustrato come esportare e importare macchine virtuali utilizzando la console di gestione di Hyper-V o Windows PowerShell.
ms.prod: windows-server
author: KBDAzure
ms.author: kathydav
manager: dongill
ms.technology: compute-hyper-v
ms.date: 12/13/2016
ms.topic: article
ms.assetid: 7fd996f5-1ea9-4b16-9776-85fb39a3aa34
ms.openlocfilehash: 6e130ee8a040cd5b56908d77d91bf196a60de6f7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392982"
---
>Si applica a: Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019 Microsoft Hyper-V Server 2019

# <a name="export-and-import-virtual-machines"></a>Esportazione e importazione di macchine virtuali

Questo articolo illustra come esportare e importare una macchina virtuale, un modo rapido per spostarli o copiarli. Questo articolo illustra anche alcune delle scelte da effettuare durante un'esportazione o un'importazione.

## <a name="export-a-virtual-machine"></a>Esportare una macchina virtuale

Un'esportazione raccoglie tutti i file necessari in un'unità, ovvero i file del disco rigido virtuale, i file di configurazione delle macchine virtuali e tutti i file del checkpoint. Questa operazione può essere eseguita in una macchina virtuale che si trova nello stato avviato o arrestato.

### <a name="using-hyper-v-manager"></a>Utilizzo della console di gestione di Hyper-V

Per creare l'esportazione di una macchina virtuale:

1. Nella console di gestione di Hyper-V fare clic con il pulsante destro del mouse sulla macchina virtuale e selezionare **Esporta**.

2. Scegliere la posizione in cui archiviare i file esportati e fare clic su **Esporta**.

Al termine dell'esportazione, è possibile visualizzare tutti i file esportati nel percorso di esportazione.

### <a name="using-powershell"></a>Mediante PowerShell

Aprire una sessione come amministratore ed eseguire un comando simile al seguente, dopo aver sostituito \<VM @ no__t-1 e \<path @ no__t-3:

```powershell
Export-VM -Name \<vm name\> -Path \<path\>
```

Per informazioni dettagliate, vedere [Export-VM](https://docs.microsoft.com/powershell/module/hyper-v/export-vm).

## <a name="import-a-virtual-machine"></a>Importare una macchina virtuale 

L'importazione di una macchina virtuale esegue la registrazione della macchina virtuale con l'host di Hyper-V. È possibile importare di nuovo nell'host o in un nuovo host. Se si sta importando nello stesso host, non è necessario esportare prima la macchina virtuale, poiché Hyper-V tenta di ricreare la macchina virtuale dai file disponibili. L'importazione di una macchina virtuale lo registra per poter essere utilizzata nell'host Hyper-V.

L'importazione guidata macchina virtuale consente inoltre di correggere incompatibilità che possono esistere quando si passa da un host a un altro. Si tratta in genere di differenze nell'hardware fisico, ad esempio memoria, commutatori virtuali e processori virtuali.

### <a name="import-using-hyper-v-manager"></a>Importazione tramite la console di gestione di Hyper-V

Per importare una macchina virtuale:

1. Dal menu **azioni** nella console di gestione di Hyper-V fare clic su **Importa macchina virtuale**.

2. Fare clic su **Avanti**.

3. Selezionare la cartella che contiene i file esportati e fare clic su **Avanti**.

4. Selezionare la macchina virtuale da importare.

5. Scegliere il tipo di importazione e fare clic su **Avanti**. Per le descrizioni, vedere [tipi di importazione](#import-types), più avanti.

6. Scegliere **Fine**.

### <a name="import-using-powershell"></a>Importazione tramite PowerShell

Usare il cmdlet **Import-VM** , seguendo l'esempio per il tipo di importazione desiderata. Per le descrizioni dei tipi, vedere i [tipi di importazione](#import-types)più avanti. 

#### <a name="register-in-place"></a>Registra sul posto

Questo tipo di importazione USA i file in cui vengono archiviati al momento dell'importazione e mantiene l'ID della macchina virtuale. Il comando seguente mostra un esempio di un file di importazione. Eseguire un comando simile con valori personalizzati.

```powershell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' 
```

#### <a name="restore"></a>Ripristinare

Per importare la macchina virtuale specificando il percorso per i file della macchina virtuale, eseguire un comando simile al seguente, sostituendo gli esempi con i valori seguenti:

```powershell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' -Copy -VhdDestinationPath 'D:\Virtual Machines\WIN10DOC' -VirtualMachinePath 'D:\Virtual Machines\WIN10DOC'
```

#### <a name="import-as-a-copy"></a>Importa come copia

Per completare un'importazione di copia e spostare i file della macchina virtuale nel percorso predefinito di Hyper-V, eseguire un comando simile al seguente, sostituendo gli esempi con i valori seguenti:

``` PowerShell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' -Copy -GenerateNewId
```

Per informazioni dettagliate, vedere [Import-VM](https://docs.microsoft.com/powershell/module/hyper-v/import-vm).

### <a name="import-types"></a>Tipi di importazione

Hyper-V offre tre tipi di importazione:

- **Registra sul posto** : questo tipo presuppone che i file di esportazione si trovino nel percorso in cui verrà archiviata ed eseguita la macchina virtuale. La macchina virtuale importata ha lo stesso ID usato al momento dell'esportazione. Per questo motivo, se la macchina virtuale è già registrata con Hyper-V, è necessario eliminarla prima che l'importazione funzioni. Al termine dell'importazione, i file di esportazione diventano i file di stato in esecuzione e non possono essere rimossi.

- **Ripristinare la macchina virtuale** : ripristinare la macchina virtuale in un percorso scelto oppure utilizzare il valore predefinito per Hyper-V. Questo tipo di importazione crea una copia dei file esportati e li sposta nel percorso selezionato. Quando viene importata, la macchina virtuale ha lo stesso ID che aveva al momento dell'esportazione. Per questo motivo, se la macchina virtuale è già in esecuzione in Hyper-V, è necessario eliminarla prima di poter completare l'importazione. Al termine dell'importazione, i file esportati rimangono intatti e possono essere rimossi o importati nuovamente.

- **Copiare la macchina virtuale** : questa operazione è simile al tipo di ripristino perché si seleziona un percorso per i file. La differenza è che la macchina virtuale importata dispone di un nuovo ID univoco, il che significa che è possibile importare più volte la macchina virtuale nello stesso host.

