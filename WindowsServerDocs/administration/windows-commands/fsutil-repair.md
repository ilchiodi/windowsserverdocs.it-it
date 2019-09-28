---
ms.assetid: 62d77150-1d9e-4069-ab4a-299f33024912
title: Ripristina fsutil
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 6e4e285bf8401d628f7e4bcbaeafb0c6defa3ad1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376906"
---
# <a name="fsutil-repair"></a>Ripristina fsutil
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Amministra e monitora le operazioni di riparazione con correzione automatica NTFS.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
fsutil repair [enumerate] <volumepath> [<LogName>]
fsutil repair [initiate] <VolumePath> <FileReference>
fsutil repair [query] <VolumePath>
fsutil repair [set] <VolumePath> <Flags>
fsutil repair [wait][<WaitType>] <VolumePath>

```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------------|---------------|
|enumerare|Enumera le intere del log di danneggiamento di un volume.|
|\<volumepath >|Specifica il volume come nome dell'unità seguito da due punti.|
|\<LogName >|$Corrupt: il set di danneggiamenti confermati nel volume.<br />$Verify: un set di potenziali danneggiamenti non verificati nel volume.|
|avviare|Avvia la riparazione automatica di NTFS.|
|\<FileReference >|Specifica l'ID file specifico del volume NTFS (numero di riferimento al file). Il riferimento al file include il numero di segmenti del file.|
|query|Esegue una query sullo stato di correzione automatica del volume NTFS.|
|set|Imposta lo stato di correzione automatica del volume.|
|\<Flags >|Specifica il metodo di ripristino da utilizzare quando si imposta lo stato di correzione automatica del volume.<br /><br />Il parametro **Flags** può essere impostato su tre valori:<br /><br />-   **0x01**: Abilita il ripristino generale.<br />-   **0x09**: Avvisa in caso di perdita di dati potenziale senza correzione.<br />-   **0x00**: Disabilita le operazioni di ripristino con correzione automatica NTFS.|
|state|Esegue una query sullo stato di danneggiamento del sistema o per un determinato volume.|
|attesa|Attende il completamento delle riparazioni. Se NTFS ha rilevato un problema in un volume in cui vengono eseguite le riparazioni, questa opzione consente al sistema di attendere il completamento della riparazione prima di eseguire gli script in sospeso.|
|[WaitType {0&#124;1}]|Indica se attendere il completamento del ripristino corrente o attendere il completamento di tutte le riparazioni. *WaitType* può essere impostato sui valori seguenti:<br /><br />-   **0**: Attende il completamento di tutte le riparazioni. (valore predefinito)<br />-   **1**: Attende il completamento del ripristino corrente.|

## <a name="remarks"></a>Note

-   Il file NTFS con riparazione automatica tenta di correggere i danneggiamenti del file system NTFS online, senza che sia necessario eseguire **chkdsk. exe** . Questa funzionalità è stata introdotta in Windows Server 2008. Per ulteriori informazioni, vedere la pagina relativa alla [correzione automatica di NTFS](https://go.microsoft.com/fwlink/?LinkID=165401).

## <a name="BKMK_examples"></a>Esempi

Per enumerare i danneggiamenti confermati di un volume, digitare:

```
fsutil repair enumerate C: $Corrupt 
```

Per abilitare la riparazione automatica sull'unità C, digitare:

```
fsutil repair set c: 1
```

Per disabilitare la riparazione con correzione automatica sull'unità C, digitare:

```
fsutil repair set c: 0
```

#### <a name="additional-references"></a>Altri riferimenti
[Indicazioni generali sulla sintassi della riga di comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[Correzione automatica di NTFS](https://go.microsoft.com/fwlink/?LinkID=165401)


