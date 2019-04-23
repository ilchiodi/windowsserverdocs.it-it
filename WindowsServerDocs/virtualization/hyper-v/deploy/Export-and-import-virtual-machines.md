---
title: Esportare e importare macchine virtuali
description: Illustra come esportare e importare le macchine virtuali usando Hyper-V Manager o Windows PowerShell.
ms.prod: windows-server-threshold
author: KBDAzure
ms.author: kathydav
manager: dongill
ms.technology: compute-hyper-v
ms.date: 12/13/2016
ms.topic: article
ms.assetid: 7fd996f5-1ea9-4b16-9776-85fb39a3aa34
ms.openlocfilehash: b326fe8d7ff05ba73fc94225fa38921b42eb3fc6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844322"
---
>Si applica a: Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

# <a name="export-and-import-virtual-machines"></a>Esportazione e importazione di macchine virtuali

Questo articolo illustra come esportare e importare una macchina virtuale, che è un modo rapido per spostare o copiare. Questo articolo illustra anche alcune delle scelte per impostarlo come quando si esegue un'esportazione o importazione.

## <a name="export-a-virtual-machine"></a>Esportare una macchina virtuale

Un'esportazione raccoglie tutti i file necessari in un'unità, file disco rigido virtuale, i file di configurazione macchina virtuale e tutti i file del checkpoint. È possibile farlo in una macchina virtuale che si trova in uno stato avviato o arrestato.

### <a name="using-hyper-v-manager"></a>Utilizzo della console di gestione di Hyper-V

Per creare l'esportazione di una macchina virtuale:

1. Nella console di gestione Hyper-V la macchina virtuale e scegliere **esportare**.

2. Scegliere la posizione in cui archiviare i file esportati e fare clic su **esportare**.

Al termine dell'esportazione, è possibile visualizzare tutti i file esportati nel percorso di esportazione.

### <a name="using-powershell"></a>Mediante PowerShell

Aprire una sessione come amministratore ed eseguire un comando simile al seguente, dopo aver sostituito \<nome della macchina virtuale\> e \<percorso\>:

```powershell
Export-VM -Name \<vm name\> -Path \<path\>
```

Per informazioni dettagliate, vedere [Export-VM](https://docs.microsoft.com/powershell/module/hyper-v/export-vm).

## <a name="import-a-virtual-machine"></a>Importare una macchina virtuale 

L'importazione di una macchina virtuale esegue la registrazione della macchina virtuale con l'host di Hyper-V. È possibile importare nuovamente in host o il nuovo host. Se si sta importando nello stesso host, non devi esportare in primo luogo, la macchina virtuale perché Hyper-V tenta di ricreare la macchina virtuale dal file disponibili. Importazione di una macchina virtuale lo registra in modo che può essere utilizzato nell'host Hyper-V.

La procedura guidata Importa macchina virtuale consente inoltre di correggere le incompatibilità che possono essere presenti quando si spostano da un host a altro. Si tratta in genere le differenze nell'hardware fisico, ad esempio memoria, commutatori virtuali e processori virtuali.

### <a name="import-using-hyper-v-manager"></a>Importazione tramite Gestione di Hyper-V

Per importare una macchina virtuale:

1. Dal **azioni** dal menu Gestione di Hyper-V, fare clic su **Importa macchina virtuale**.

2. Fare clic su **Avanti**.

3. Selezionare la cartella che contiene i file esportati e fare clic su **successivo**.

4. Selezionare la macchina virtuale da importare.

5. Selezionare il tipo di importazione, quindi scegliere **successivo**. (Per le descrizioni, vedere [importare tipi](#import-types)riportato di seguito.)

6. Scegliere **Fine**.

### <a name="import-using-powershell"></a>Importare con PowerShell

Usare la **Import-VM** cmdlet, dopo l'esempio per il tipo di importazione desiderati. Per le descrizioni dei tipi, vedere [importare tipi](#import-types)riportato di seguito. 

#### <a name="register-in-place"></a>Registrare posto

Questo tipo di importazione vengono utilizzati i file in cui si trovano al momento dell'importazione e mantiene l'ID. della macchina virtuale Il comando seguente illustra un esempio di un file di importazione. Eseguire un comando simile con i propri valori.

```powershell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' 
```

#### <a name="restore"></a>Ripristinare

Per importare la macchina virtuale che specifica il percorso per i file della macchina virtuale, eseguire un comando simile al seguente, sostituendo gli esempi con i propri valori:

```powershell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' -Copy -VhdDestinationPath 'D:\Virtual Machines\WIN10DOC' -VirtualMachinePath 'D:\Virtual Machines\WIN10DOC'
```

#### <a name="import-as-a-copy"></a>Importare una copia

Per completare un'importazione di copiare e spostare i file di macchina virtuale nel percorso di Hyper-V predefinito, eseguire un comando simile al seguente, sostituendo gli esempi con i propri valori:

``` PowerShell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' -Copy -GenerateNewId
```

Per informazioni dettagliate, vedere [Import-VM](https://docs.microsoft.com/powershell/module/hyper-v/import-vm).

### <a name="import-types"></a>Importare i tipi

Hyper-V offre tre tipi di importazione:

- **Registra sul posto** : questo tipo si presuppone che i file di esportazione si trovano nel percorso è possibile archiviare ed eseguire la macchina virtuale. La macchina virtuale importata ha lo stesso ID che aveva al momento dell'esportazione. Per questo motivo, se la macchina virtuale è già registrata con Hyper-V, deve essere eliminata prima il funzionamento dell'importazione. Al termine dell'importazione, i file di esportazione diventano l'esecuzione dello stato di file e non può essere rimosso.

- **Ripristinare la macchina virtuale** : ripristinare la macchina virtuale in un percorso prescelto o usare il valore predefinito per Hyper-V. Questo tipo di importazione crea una copia dei file esportati e li sposta nel percorso selezionato. Quando viene importata, la macchina virtuale ha lo stesso ID che aveva al momento dell'esportazione. Per questo motivo, se la macchina virtuale è già in esecuzione in Hyper-V, deve essere eliminata prima di poter completare l'importazione. Al termine dell'importazione, i file esportati rimangono invariati e possono rimuovere o importati nuovamente.

- **Copiare la macchina virtuale** – è simile al tipo di ripristino in quanto si seleziona un percorso per i file. La differenza è che la macchina virtuale importata ha un nuovo ID univoco, è possibile importare la macchina virtuale nello stesso host più volte.

