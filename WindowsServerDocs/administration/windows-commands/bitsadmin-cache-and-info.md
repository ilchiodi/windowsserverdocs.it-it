---
title: informazioni e cache Bitsadmin
description: Argomento dei comandi di Windows per la **cache e le informazioni di Bitsadmin**, che consente di scaricare una voce della cache specifica.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 15975cbf-dba6-49ca-a725-d15ce1952de5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e9c6ce1eb972a76408483b8a27a3abca5500e56
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850894"
---
# <a name="bitsadmin-cache-and-info"></a>informazioni e cache Bitsadmin

Dump di una voce della cache specifica.

## <a name="syntax"></a>Sintassi

```
bitsadmin /cache /info recordID [/verbose]
```

### <a name="parameters"></a>Parametri

| Paramreter | Descrizione |
| -------------- | -------------- |
| recordID | GUID associato alla voce della cache. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente viene eseguito il dump della voce della cache con il valore recordID di {6511FB02-E195-40A2-B595-E8E2F8F47702}.

```
C:\>bitsadmin /cache /info {6511FB02-E195-40A2-B595-E8E2F8F47702}
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)