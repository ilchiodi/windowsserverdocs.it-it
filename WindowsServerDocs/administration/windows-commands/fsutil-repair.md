---
ms.assetid: 62d77150-1d9e-4069-ab4a-299f33024912
title: Ripristina fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 18c06b1f426105b098a5dc7e992b1e3becd3a4ca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846682"
---
# <a name="fsutil-repair"></a>Ripristina fsutil
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Consente di amministrare e monitorare NTFS ripara automaticamente le operazioni di ripristino.

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
|Enumerare|Enumera le voci di log danneggiamenti del volume.|
|\<volumepath>|Specifica il volume come il nome di unità seguito da due punti.|
|\<LogName>|$Danneggiare - il set di confirmed danneggiamenti del volume.<br />$Verificare - un set di possibili, non verificati danneggiamenti del volume.|
|avviare|Avvia riparazione automatica di NTFS.|
|\<FileReference>|Specifica l'ID di file specifico per volume NTFS (numero di riferimento di file). Il riferimento al file include il numero di segmenti del file.|
|query|Esegue una query lo stato di riparazione automatica di volume NTFS.|
|set|Imposta lo stato del volume di riparazione automatica.|
|\<Flag >|Specifica il metodo di correzione da utilizzare quando si imposta lo stato del volume di riparazione automatica.<br /><br />Il **flag** parametro può essere impostato su tre valori:<br /><br />-   **0x01**: Consente il ripristino generale.<br />-   **0x09**: Genera un avviso sulla potenziale perdita di dati senza correzione.<br />-   **0x00**: Disabilita NTFS ripara automaticamente le operazioni di ripristino.|
|Stato|Esegue una query lo stato di danneggiamento del sistema o per un determinato volume.|
|attesa|Attende che restituisce al completamento. Se NTFS ha rilevato un problema in un volume in cui è in corso operazioni di ripristino, questa opzione consente al sistema in attesa finché la riparazione è stata completata prima di eseguire qualsiasi script in sospeso.|
|[WaitType {0&#124;1}]|Indica se deve attendere il ripristino corrente per completare o di attesa per tutte le correzioni per il completamento. *WaitType* possono essere impostate sui valori seguenti:<br /><br />-   **0**: Attende che tutte le correzioni per il completamento. (valore predefinito)<br />-   **1**: Attende il ripristino corrente per il completamento.|

## <a name="remarks"></a>Note

-   Riparazione automatica di NTFS tenta di correggere i danneggiamenti del file system NTFS, in linea, senza richiedere **Chkdsk.exe** da eseguire. Questa funzionalità è stata introdotta in Windows Server 2008. Per altre informazioni, vedere [Self correzione NTFS](https://go.microsoft.com/fwlink/?LinkID=165401).

## <a name="BKMK_examples"></a>Esempi

Per enumerare i danneggiamenti confermati di un volume, digitare:

```
fsutil repair enumerate C: $Corrupt 
```

Per abilitare la funzionalità di riparazione automatica di ripristino sull'unità C, digitare:

```
fsutil repair set c: 1
```

Per disabilitare la funzionalità di riparazione automatica di ripristino sull'unità C, digitare:

```
fsutil repair set c: 0
```

#### <a name="additional-references"></a>Altri riferimenti
[Chiave sintassi della riga di comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[La correzione NTFS automatica](https://go.microsoft.com/fwlink/?LinkID=165401)


