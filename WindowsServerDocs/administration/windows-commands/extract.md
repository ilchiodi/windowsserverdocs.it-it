---
title: extract
description: Argomento di riferimento per il comando Extract, che estrae i file da un percorso di origine.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 20dab03e-f6e1-4eb8-b8a1-fd6f1d97ee83
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dbadcc555fc9bb0b02e568b1126a317a9d59d336
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83437186"
---
# <a name="extract"></a>extract

Estrae i file da un file CAB o da un'origine.

## <a name="syntax"></a>Sintassi

```
extract [/y] [/a] [/d | /e] [/l dir] cabinet [filename ...]
extract [/y] source [newname]
extract [/y] /c source destination
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| archivio | Usare se si desidera estrarre due o più file. |
| filename | Nome del file da estrarre dal file CAB. I caratteri jolly e più nomi di file, separati da spazi vuoti, possono essere utilizzati. |
| origine | File compresso (un file CAB con un solo file). |
| newname | Nuovo nome di file per fornire il file estratto. Se omesso, viene utilizzato il nome originale. |
| /a | Elaborare TUTTI gli archivi. Segue catena partire nel primo file CAB indicato. |
| /C | Copiare i file di origine alla destinazione (per copiare dai dischi DMF). |
| /d | Visualizzare directory CAB (da utilizzare con il nome file per evitare di estrazione). |
| /e | Extract (usare anziché *.* Per estrarre tutti i file). |
| /l dir | Posizione in cui i file estratti (valore predefinito è la directory corrente). |
| /y | Non richiedere conferma prima di sovrascrivere un file esistente. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
