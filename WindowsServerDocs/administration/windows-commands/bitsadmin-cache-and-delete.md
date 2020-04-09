---
title: Bitsadmin cache ed eliminazione
description: Argomento dei comandi di Windows per la **cache Bitsadmin e Delete**, che elimina una voce della cache specifica.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 22540273-55a5-46ea-869b-6df2aa6808a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0fd7f1db83a62dd9c1085d6afdcf509c1c3ac8cf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850944"
---
# <a name="bitsadmin-cache-and-delete"></a>Bitsadmin cache ed eliminazione

Elimina una voce della cache specifica.

## <a name="syntax"></a>Sintassi

```
bitsadmin /cache /delete recordID
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| recordID | GUID associato alla voce della cache. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente viene eliminata la voce della cache con RecordID di {6511FB02-E195-40A2-B595-E8E2F8F47702}.

```
C:\>bitsadmin /cache /delete {6511FB02-E195-40A2-B595-E8E2F8F47702}
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)