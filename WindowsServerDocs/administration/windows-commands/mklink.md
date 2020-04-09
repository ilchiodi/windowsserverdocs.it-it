---
title: mklink
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0ce4df22-2dbc-48fc-9c16-b721ae85f857
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3d6328d972b07b1bebd272788b896fd491e47380
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839554"
---
# <a name="mklink"></a>mklink
Crea un collegamento simbolico.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
mklink [[/d] | [/h] | [/j]] <Link> <Target>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/d|Crea un collegamento simbolico di directory. Per impostazione predefinita, **mklink** crea un collegamento simbolico file.|
|/h|Crea un collegamento reale anziché un collegamento simbolico.|
|/j|Crea una giunzione di directory.|
|Collegamento \<>|Specifica il nome del collegamento simbolico da creare.|
|> di destinazione \<|Specifica il percorso (relativo o assoluto) a cui fa riferimento il nuovo collegamento simbolico.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente viene illustrata la creazione e la rimozione di un collegamento simbolico denominato MyFile e MyFile. file dalla directory radice alla directory \Users\User1\Documents e un file di esempio. file che si trova nella directory:
```
mklink /d \MyFolder \Users\User1\Documents
mklink /h \MyFile.file \User1\Documents\example.file
rd \MyFolder
del \MyFile.file
```
## <a name="additional-references"></a>Altri riferimenti
-   [Nuovo elemento](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/new-item?view=powershell-6)
-   [del](https://docs.microsoft.com/windows-server/administration/windows-commands/del)
-   [rmdir](https://docs.microsoft.com/windows-server/administration/windows-commands/rd)
