---
title: recuperare
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cf9be2e3-90c8-4773-a201-dc503b91948e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 415efe2d1e60ca70d68059b5702108440da735f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371774"
---
# <a name="recover"></a>recuperare



Recupera le informazioni leggibili da un disco difettoso o difettoso.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
recover [<Drive>:][<Path>]<FileName>
```

## <a name="parameters"></a>Parametri

|           Parametro           |                                          Descrizione                                          |
|-------------------------------|-----------------------------------------------------------------------------------------------|
| [\<Drive >:] [<Path>] <FileName> | Specifica il percorso e il nome del file che si desidera ripristinare. Il *nome file* è obbligatorio. |
|              /?               |                             Visualizza la guida al prompt dei comandi.                              |

## <a name="remarks"></a>Note

-   Il comando di **ripristino** legge un file, settoriale per settore, e recupera i dati dai settori validi. I dati nei settori danneggiati vengono persi.
-   I settori danneggiati segnalati da **chkdsk** sono stati contrassegnati come "errati" quando il disco è stato preparato per l'operazione. Non costituiscono alcun rischio e il **ripristino** non influisce su di essi.
-   Poiché tutti i dati nei settori danneggiati vengono persi quando si ripristina un file, è necessario recuperare un solo file alla volta.
-   Non è possibile usare i caratteri **&#42;** jolly (e **?** ) con il comando **Recover** . È necessario specificare un file (e il percorso del file se non è presente nella directory corrente).

## <a name="BKMK_examples"></a>Esempi

Per ripristinare il file Story. txt nella directory \Fiction sull'unità D, digitare:
```
recover d:\fiction\story.txt 
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)