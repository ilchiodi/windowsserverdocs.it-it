---
title: append
description: 'Argomento dei comandi di Windows per '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9c3fea20-9502-40ad-a442-7a927aad88eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fdc4243bee8055888b023a56921cef757dda6b7e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382746"
---
# <a name="append"></a>append



Consente ai programmi di aprire i file di dati nelle directory specificate come se si trovassero nella directory corrente. Se utilizzata senza parametri, **Append** Visualizza l'elenco di directory accodato.

> [!NOTE]
> Questo comando non è supportato in Windows 10.
>

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
append [[<Drive>:]<Path>[;...]] [/x[:on|:off]] [/path:[:on|:off] [/e] 
append ;
```

## <a name="parameters"></a>Parametri

|     Parametro     |                                                                                 Descrizione                                                                                 |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [\<Drive >:] <Path> |                                                                 Specifica un'unità e una directory da accodare.                                                                  |
|       /x: on       |                                                  Applica le directory accodate alle ricerche di file e all'avvio delle applicazioni.                                                  |
|      /x: disattivato       |                                     Applica le directory accodate solo alle richieste per aprire i file.</br>**/x: off** è l'impostazione predefinita.                                     |
|     /Path: on      |                               Applica le directory accodate alle richieste di file che già specificano un percorso. **/Path: on** è l'impostazione predefinita.                               |
|     /Path: disattivato     |                                                                    Disattiva l'effetto di **/Path: on**.                                                                    |
|        /e         | Archivia una copia dell'elenco di directory accodate in una variabile di ambiente denominata APPEND. **/e** può essere usato solo la prima volta che si usa **Append** dopo l'avvio del sistema. |
|         ;         |                                                                     Cancella l'elenco di directory aggiunte.                                                                     |
|        /?         |                                                                    Visualizza la guida al prompt dei comandi.                                                                     |

## <a name="BKMK_examples"></a>Esempi

Per cancellare l'elenco delle directory accodate, digitare:
```
append ;
```
Per archiviare una copia della directory accodata in una variabile di ambiente denominata APPEND, digitare:
```
append /e
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
