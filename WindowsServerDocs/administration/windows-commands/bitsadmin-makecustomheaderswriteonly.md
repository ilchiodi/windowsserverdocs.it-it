---
title: makecustomheaderswriteonly Bitsadmin
description: Argomento dei comandi di Windows per **BITSAdmin makecustomheaderswriteonly**, che rendono le intestazioni HTTP personalizzate di un processo di sola scrittura.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 9183b1b5de51020c5c6d2efad2c0a788d158a183
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850244"
---
# <a name="bitsadmin-makecustomheaderswriteonly"></a>makecustomheaderswriteonly Bitsadmin

Rendere di sola scrittura le intestazioni HTTP personalizzate di un processo.

> [!Important]
> Questa azione non può essere annullata.

## <a name="syntax"></a>Sintassi

```
bitsadmin /makecustomheaderswriteonly <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente vengono scritte intestazioni HTTP personalizzate per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /makecustomheaderswriteonly myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)