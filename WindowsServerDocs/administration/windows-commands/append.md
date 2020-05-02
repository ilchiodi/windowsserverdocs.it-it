---
title: append
description: Argomento di riferimento per il comando APPEND, che consente ai programmi di aprire i file di dati nelle directory specificate, come se si trovassero nella directory corrente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9c3fea20-9502-40ad-a442-7a927aad88eb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 562a13c6b1a47e43bb66548902f0b8e57e789a34
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718993"
---
# <a name="append"></a>append

Consente ai programmi di aprire i file di dati nelle directory specificate come se si trovassero nella directory corrente. Se utilizzata senza parametri, **Append** Visualizza l'elenco di directory accodato.

> [!NOTE]
> Questo comando non è supportato in Windows 10.

## <a name="syntax"></a>Sintassi

```
append [[<drive>:]<path>[;...]] [/x[:on|:off]] [/path:[:on|:off] [/e]
append ;
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `[\<drive>:]<path>` | Specifica un'unità e una directory da accodare. |
| /x: on | Applica le directory accodate alle ricerche di file e all'avvio delle applicazioni. |
| /x: disattivato | Applica le directory accodate solo alle richieste per aprire i file. L'opzione **/x: off** è l'impostazione predefinita. |
| /Path: on | Applica le directory accodate alle richieste di file che già specificano un percorso. **/Path: on** è l'impostazione predefinita. |
| /Path: disattivato | Disattiva l'effetto di **/Path: on**. |
| /e | Archivia una copia dell'elenco di directory accodate in una variabile di ambiente denominata APPEND. **/e** può essere usato solo la prima volta che si usa **Append** dopo l'avvio del sistema. |
| ; | Cancella l'elenco di directory aggiunte. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="examples"></a>Esempi

Per cancellare l'elenco delle directory accodate, digitare:

```
append ;
```

Per archiviare una copia della directory accodata in una variabile di ambiente denominata *Append*, digitare:

```
append /e
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
