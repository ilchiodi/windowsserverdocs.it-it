---
title: recover
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cf9be2e3-90c8-4773-a201-dc503b91948e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b316ed26f008a62f88aaeb4a7a7f3030d08f1588
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722618"
---
# <a name="recover"></a>recover



Recupera le informazioni leggibili da un disco difettoso o difettoso.



## <a name="syntax"></a>Sintassi

```
recover [<Drive>:][<Path>]<FileName>
```

### <a name="parameters"></a>Parametri

|           Parametro           |                                          Descrizione                                          |
|-------------------------------|-----------------------------------------------------------------------------------------------|
| [\<Unità>:] [<Path>]<FileName> | Specifica il percorso e il nome del file che si desidera ripristinare. Il *nome file* è obbligatorio. |
|              /?               |                             Visualizza la guida al prompt dei comandi.                              |

## <a name="remarks"></a>Osservazioni

-   Il comando di **ripristino** legge un file, settoriale per settore, e recupera i dati dai settori validi. I dati nei settori danneggiati vengono persi.
-   I settori danneggiati segnalati da **chkdsk** sono stati contrassegnati come non validi quando il disco è stato preparato per l'operazione. Non costituiscono alcun rischio e il **ripristino** non influisce su di essi.
-   Poiché tutti i dati nei settori danneggiati vengono persi quando si ripristina un file, è necessario recuperare un solo file alla volta.
-   Non è possibile usare i caratteri jolly (**&#42;** e **?**) con il comando **Recover** . È necessario specificare un file (e il percorso del file se non è presente nella directory corrente).

## <a name="examples"></a>Esempi

Per ripristinare il file Story. txt nella directory \Fiction sull'unità D, digitare:
```
recover d:\fiction\story.txt 
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)