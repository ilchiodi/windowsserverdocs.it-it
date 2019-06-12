---
ms.assetid: f2eefaaf-2817-4ac7-abac-d2b65fa971dc
title: Fsutil transazione
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: c225c99919a2558559b1ec7a47b61d716e199a73
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439004"
---
# <a name="fsutil-transaction"></a>Fsutil transazione
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7, Windows 2008, Windows Vista

Gestisce le transazioni di NTFS.

Per esempi di come usare questo comando, vedere [esempi](#BKMK_examples) .

## <a name="syntax"></a>Sintassi

```
fsutil transaction [commit] <GUID>
fsutil transaction [fileinfo] <Filename>
fsutil transaction [list]
fsutil transaction [query] [{Files|All}] <GUID>
fsutil transaction [rollback] <GUID>
```

### <a name="parameters"></a>Parametri

| Parametro  |                                                                                                                                                     Descrizione                                                                                                                                                     |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   Eseguire il commit   |                                                                                                                      Contrassegna la fine di una transazione specificata ha esito positivo implicita o esplicita.                                                                                                                      |
|   <GUID>   |                                                                                                                               Specifica il valore GUID che rappresenta una transazione.                                                                                                                               |
|  fileinfo  |                                                                                                                              Visualizza le informazioni sulle transazioni per il file specificato.                                                                                                                               |
| <Filename> |                                                                                                                                         Specifica il percorso completo e nome file.                                                                                                                                          |
|    list    |                                                                                                                                 Visualizza un elenco di transazioni in corso.                                                                                                                                  |
|   query    | Visualizza le informazioni per la transazione specificata.<br /><br />-Se **fsutil transazione di eseguire query sui file** viene specificato, le informazioni del file verranno visualizzate solo per la transazione specificata.<br />-Se **fsutil transazione query tutti** viene specificato, verranno visualizzate tutte le informazioni per la transazione. |
|  Eseguire il rollback  |                                                                                                                                Esegue il rollback di una transazione specifica all'inizio.                                                                                                                                 |

### <a name="remarks"></a>Note

-   Transactional NTFS è stato introdotto in Windows Server 2008.

### <a name="BKMK_examples"></a>Esempi
Per visualizzare le informazioni sulle transazioni per c:\test.txt. file, digitare:

```
fsutil transaction fileinfo c:\test.txt  
```

### <a name="additional-references"></a>Altri riferimenti
[Indicazioni generali sulla sintassi della riga di comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[Transactional NTFS](https://go.microsoft.com/fwlink/?LinkID=165402)


