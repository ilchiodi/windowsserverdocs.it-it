---
title: append
description: 'Argomento relativo a comandi di Windows '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: fe641e1336c163b5e98421a5fc32f8dbe64023b0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435319"
---
# <a name="append"></a>append



Consente di aprire i file di dati nelle directory specificate come se fossero nella directory corrente. Se utilizzata senza parametri, **append** Visualizza l'elenco di directory aggiunte.

> [!NOTE]
> Questo comando non supportato in Windows 10.
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
| [\<Drive>:]<Path> |                                                                 Specifica l'unità e directory da aggiungere.                                                                  |
|       /x:on       |                                                  Si applica directory aggiunte per le ricerche di file e avvio di applicazioni.                                                  |
|      /x:off       |                                     Si applica directory aggiunte solo per le richieste di apertura file.</br>**/ x: off** è l'impostazione predefinita.                                     |
|     /path:on      |                               Si applica directory aggiunte alle richieste di file che è già specificato un percorso. **/Path: su** è l'impostazione predefinita.                               |
|     /path:off     |                                                                    Consente di disattivare l'effetto della **/path: su**.                                                                    |
|        /e         | Archivia una copia dell'elenco di directory aggiunte in una variabile di ambiente denominata APPEND. **/e** può essere utilizzata solo la prima volta Usa **accodare** dopo l'avvio del sistema. |
|         ;         |                                                                     Cancella l'elenco di directory aggiunte.                                                                     |
|        /?         |                                                                    Visualizza la guida al prompt dei comandi.                                                                     |

## <a name="BKMK_examples"></a>Esempi

Per cancellare l'elenco di directory aggiunte, digitare:
```
append ;
```
Per archiviare una copia della directory aggiunta a una variabile di ambiente denominata APPEND, digitare:
```
append /e
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
