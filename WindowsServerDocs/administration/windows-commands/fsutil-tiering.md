---
ms.assetid: e5f55f3e-8d2a-4526-8d67-36a539126c22
title: Suddivisione in livelli fsutil
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 6863940d69e30f4984897a7e03369a834da21d1d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376778"
---
# <a name="fsutil-tiering"></a>Suddivisione in livelli fsutil
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10

Consente la gestione delle funzioni del livello di archiviazione, ad esempio l'impostazione e la disabilitazione dei flag e l'elenco di livelli.

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
|ClearFlags|Disabilita i flag di comportamento di suddivisione in livelli di un volume.|
|\<volume >|Specifica il volume.|
|/TrNH|Per i volumi con archiviazione a livelli, determina la disabilitazione della raccolta di calore.<br /><br>Si applica solo a NTFS e ReFS.|
|queryflags|Esegue una query sui flag di comportamento di suddivisione in livelli di un volume.|
|area geografica|Elenca le aree a livelli di un volume e i rispettivi livelli di archiviazione.|
|SetFlags|Abilita i flag di comportamento di suddivisione in livelli di un volume.|
|suddivisione in livelli|Elenca i livelli di archiviazione associati a un volume.|


### <a name="examples"></a>Esempi

Per eseguire una query sui flag del volume C, digitare:

```
fsutil tiering clearflags C:
```

Per impostare i flag sul volume C, digitare:

```
fsutil tiering setflags C: /TrNH
```

Per cancellare i flag sul volume C, digitare:

```
fsutil tiering clearflags C: /TrNH
```

Per elencare le aree del volume C e i rispettivi livelli di archiviazione, digitare:

```
fsutil tiering regionlist C:
```

Per elencare i livelli del volume C, digitare:

```
fsutil tiering tierlist C:
```



### <a name="additional-references"></a>Altri riferimenti
[Indicazioni generali sulla sintassi della riga di comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

