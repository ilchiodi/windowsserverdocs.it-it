---
title: recuperare
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d6b9b5544394bfc69a2dc9f7be26ed8355a3f690
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441965"
---
# <a name="recover"></a>recuperare



Recupera le informazioni leggibili da un disco danneggiato o difettoso.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
recover [<Drive>:][<Path>]<FileName>
```

## <a name="parameters"></a>Parametri

|           Parametro           |                                          Descrizione                                          |
|-------------------------------|-----------------------------------------------------------------------------------------------|
| [\<Drive>:][<Path>]<FileName> | Specifica il percorso e nome del file che si desidera ripristinare. *Nome del file* è obbligatorio. |
|              /?               |                             Visualizza la guida al prompt dei comandi.                              |

## <a name="remarks"></a>Note

-   Il **ripristinare** comando legge un file di settore per settore e recupera i dati dai settori in buone condizioni. I dati contenuti nei settori vengono persi.
-   Settori danneggiati segnalati dal **chkdsk** sono stati contrassegnati come "negative" quando il disco è stato preparato per l'operazione. Essi non rappresentano alcun rischio, e **ripristinare** non verranno influenzati.
-   Poiché tutti i dati contenuti nei settori viene perso quando si ripristina un file, è consigliabile ripristinare un solo file alla volta.
-   Non è possibile utilizzare caratteri jolly ( **&#42;** e **?** ) con i **ripristinare** comando. È necessario specificare un file (e il percorso del file se non è presente nella directory corrente).

## <a name="BKMK_examples"></a>Esempi

Per ripristinare il file storia. txt nella directory \Fiction nell'unità D, digitare:
```
recover d:\fiction\story.txt 
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)