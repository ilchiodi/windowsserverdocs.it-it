---
ms.assetid: f2eefaaf-2817-4ac7-abac-d2b65fa971dc
title: Fsutil transazione
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: aa6692dbb8af1ec832650971c6723c060fc2cd56
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844014"
---
# <a name="fsutil-transaction"></a>Fsutil transazione
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7, Windows 2008, Windows Vista

Gestisce le transazioni NTFS.

Per esempi relativi all'uso di questo comando, vedere [esempi](#BKMK_examples) .

## <a name="syntax"></a>Sintassi

```
fsutil transaction [commit] <GUID>
fsutil transaction [fileinfo] <Filename>
fsutil transaction [list]
fsutil transaction [query] [{Files|All}] <GUID>
fsutil transaction [rollback] <GUID>
```

#### <a name="parameters"></a>Parametri

| Parametro  |                                                                                                                                                     Descrizione                                                                                                                                                     |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   commit   |                                                                                                                      Contrassegna la fine di una transazione specificata implicita o esplicita riuscita.                                                                                                                      |
|   <GUID>   |                                                                                                                               Specifica il valore GUID che rappresenta una transazione.                                                                                                                               |
|  FileInfo  |                                                                                                                              Visualizza le informazioni sulle transazioni per il file specificato.                                                                                                                               |
| <Filename> |                                                                                                                                         Specifica il percorso completo e il nome file.                                                                                                                                          |
|    list    |                                                                                                                                 Visualizza un elenco di transazioni attualmente in esecuzione.                                                                                                                                  |
|   query    | Visualizza le informazioni per la transazione specificata.<p>-Se viene specificato il **file fsutil transaction query** , le informazioni sul file verranno visualizzate solo per la transazione specificata.<br />-Se si specifica **fsutil transaction query All** , verranno visualizzate tutte le informazioni per la transazione. |
|  rollback  |                                                                                                                                Esegue il rollback di una transazione specificata fino all'inizio.                                                                                                                                 |

### <a name="remarks"></a>Note

-   Transactional NTFS è stato introdotto in Windows Server 2008.

### <a name="examples"></a><a name="BKMK_examples"></a>Esempi
Per visualizzare le informazioni sulle transazioni per il file c:\test.txt, digitare:

```
fsutil transaction fileinfo c:\test.txt  
```

### <a name="additional-references"></a>Altri riferimenti
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Fsutil](Fsutil.md)

[NTFS transazionale](https://go.microsoft.com/fwlink/?LinkID=165402)


