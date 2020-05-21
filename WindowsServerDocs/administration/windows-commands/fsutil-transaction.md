---
title: fsutil transaction
description: Argomento di riferimento per il comando fsutil transaction, che gestisce le transazioni NTFS.
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: f2eefaaf-2817-4ac7-abac-d2b65fa971dc
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: fc81934c5838fd81722b27a7b7e57b14709ed26a
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83437046"
---
# <a name="fsutil-transaction"></a>fsutil transaction

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Gestisce le transazioni NTFS.

## <a name="syntax"></a>Sintassi

```
fsutil transaction [commit] <GUID>
fsutil transaction [fileinfo] <filename>
fsutil transaction [list]
fsutil transaction [query] [{files | all}] <GUID>
fsutil transaction [rollback] <GUID>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| commit | Contrassegna la fine di una transazione specificata implicita o esplicita riuscita. |
| `<GUID>` | Specifica il valore GUID che rappresenta una transazione. |
| FileInfo  | Visualizza le informazioni sulle transazioni per il file specificato. |
| `<filename>` | Specifica il percorso completo e il nome file. |
| list | Visualizza un elenco di transazioni attualmente in esecuzione. |
| query | Visualizza le informazioni per la transazione specificata.<ul><li>Se `fsutil transaction query files` si specifica, le informazioni sul file vengono visualizzate solo per la transazione specificata.</li><li>Se `fsutil transaction query all` si specifica, verranno visualizzate tutte le informazioni per la transazione.</li></ul> |
| rollback | Esegue il rollback di una transazione specificata fino all'inizio. |

### <a name="examples"></a>Esempi

Per visualizzare le informazioni sulle transazioni per il file *c:\test.txt*, digitare:

```
fsutil transaction fileinfo c:\test.txt
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [fsutil](fsutil.md)

- [NTFS transazionale](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc730726(v=ws.10))
