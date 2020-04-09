---
title: recover
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cf9be2e3-90c8-4773-a201-dc503b91948e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 172471c5c56823e29dd0882072920db3d77639da
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836724"
---
# <a name="recover"></a>recover



Recupera le informazioni leggibili da un disco difettoso o difettoso.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
recover [<Drive>:][<Path>]<FileName>
```

### <a name="parameters"></a>Parametri

|           Parametro           |                                          Descrizione                                          |
|-------------------------------|-----------------------------------------------------------------------------------------------|
| [\<unità >:] [<Path>]<FileName> | Specifica il percorso e il nome del file che si desidera ripristinare. Il *nome file* è obbligatorio. |
|              /?               |                             Visualizza la guida al prompt dei comandi.                              |

## <a name="remarks"></a>Note

-   Il comando di **ripristino** legge un file, settoriale per settore, e recupera i dati dai settori validi. I dati nei settori danneggiati vengono persi.
-   I settori danneggiati segnalati da **chkdsk** sono stati contrassegnati come non validi quando il disco è stato preparato per l'operazione. Non costituiscono alcun rischio e il **ripristino** non influisce su di essi.
-   Poiché tutti i dati nei settori danneggiati vengono persi quando si ripristina un file, è necessario recuperare un solo file alla volta.
-   Non è possibile usare i caratteri **&#42;** jolly (e **?** ) con il comando **Recover** . È necessario specificare un file (e il percorso del file se non è presente nella directory corrente).

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per ripristinare il file Story. txt nella directory \Fiction sull'unità D, digitare:
```
recover d:\fiction\story.txt 
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)