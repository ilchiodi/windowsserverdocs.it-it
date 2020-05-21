---
title: fsutil repair
description: Argomento di riferimento per il comando fsutil Repair, che consente di amministrare e monitorare le operazioni di ripristino con correzione automatica NTFS.
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
ms.assetid: 62d77150-1d9e-4069-ab4a-299f33024912
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 449bd39b6b2df0e302085b71ef9db87d2020b0f0
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83435756"
---
# <a name="fsutil-repair"></a>fsutil repair

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8

Amministra e monitora le operazioni di riparazione con correzione automatica NTFS. Il file NTFS con riparazione automatica tenta di correggere i danneggiamenti del file system NTFS online, senza che sia necessario eseguire **chkdsk. exe** . Per ulteriori informazioni, vedere la pagina relativa alla [correzione automatica di NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10)).

## <a name="syntax"></a>Sintassi

```
fsutil repair [enumerate] <volumepath> [<logname>]
fsutil repair [initiate] <volumepath> <filereference>
fsutil repair [query] <volumepath>
fsutil repair [set] <volumepath> <flags>
fsutil repair [wait][<waittype>] <volumepath>

```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| enumerare | Enumera le intere del log di danneggiamento di un volume. |
| `<logname>` | Può essere `$corrupt` , il set di danneggiamenti confermati nel volume o `$verify` , un set di possibili danneggiamenti non verificati nel volume. |
| avviare | Avvia la riparazione automatica di NTFS. |
| `<filereference>` | Specifica l'ID file specifico del volume NTFS (numero di riferimento al file). Il riferimento al file include il numero di segmenti del file. |
| query | Esegue una query sullo stato di correzione automatica del volume NTFS. |
| set | Imposta lo stato di correzione automatica del volume. |
| `<flags>` | Specifica il metodo di ripristino da utilizzare quando si imposta lo stato di correzione automatica del volume.<p>Questo parametro può essere impostato su tre valori:<ul><li>**0x01** -Abilita il ripristino generale.</li><li>**0x09** : avvisa sulla potenziale perdita di dati senza correzione.</li><li>**0x00** : Disabilita le operazioni di ripristino con correzione automatica NTFS.</li></ul> |
| state | Esegue una query sullo stato di danneggiamento del sistema o per un determinato volume. |
| wait | Attende il completamento delle riparazioni. Se NTFS ha rilevato un problema in un volume in cui vengono eseguite le riparazioni, questa opzione consente al sistema di attendere il completamento della riparazione prima di eseguire gli script in sospeso. |
| `[waittype {0|1}]` | Indica se attendere il completamento del ripristino corrente o attendere il completamento di tutte le riparazioni. Il parametro *waittype* può essere impostato sui valori seguenti:<ul><li>**0** : attende il completamento di tutte le riparazioni.  (valore predefinito)</li><li>**1** : attende il completamento del ripristino corrente.</li></ul> |

### <a name="examples"></a>Esempi

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

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [fsutil](fsutil.md)

- [Correzione automatica di NTFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771388(v=ws.10))
