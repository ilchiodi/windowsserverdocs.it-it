---
ms.assetid: e5f55f3e-8d2a-4526-8d67-36a539126c22
title: La suddivisione in livelli di fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: dcb69e4e9c71a723bfd735eb7915472f1232a92b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859252"
---
# <a name="fsutil-tiering"></a>La suddivisione in livelli di fsutil
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10

Abilita la gestione di funzioni di livello di archiviazione, ad esempio l'impostazione e la disabilitazione di flag e visualizzazione di un elenco dei livelli.

## <a name="syntax"></a>Sintassi

```
fsutil tiering [clearflags] <volume> <flags>
fsutil tiering [queryflags] <volume>
fsutil tiering [regionlist] <volume>
fsutil tiering [setflags] <volume> <flags>
fsutil tiering [tierlist] <volume>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|-------------|---------------|
|ClearFlags|Disabilita il suddivisione in livelli nel flag di comportamento di un volume.|
|\<volume>|Specifica il volume.|
|/TrNH|Per i volumi con l'archiviazione a livelli, fa in modo che punti più caldi riuniti per essere disabilitata.<br /><br>Si applica solo a NTFS e ReFS.|
|queryflags|Esegue una query il suddivisione in livelli nel flag di comportamento di un volume.|
|regionlist|Elenca le aree su più livelli di un volume e i livelli di archiviazione corrispondenti.|
|chiamate setFlags|Abilita il suddivisione in livelli nel flag di comportamento di un volume.|
|tierlist|Elenca le tieres di archiviazione associato a un volume.|


### <a name="examples"></a>Esempi

Per eseguire una query il flag nel volume C, digitare:

```
fsutil tiering clearflags C:
```

Per impostare il flag nel volume C, digitare:

```
fsutil tiering setflags C: /TrNH
```

Per cancellare il flag nel volume C, digitare:

```
fsutil tiering clearflags C: /TrNH
```

Per elencare le aree di volume C e i livelli di archiviazione corrispondenti, digitare:

```
fsutil tiering regionlist C:
```

Per elencare i livelli di volume C, digitare:

```
fsutil tiering tierlist C:
```



### <a name="additional-references"></a>Altri riferimenti
[Chiave sintassi della riga di comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

