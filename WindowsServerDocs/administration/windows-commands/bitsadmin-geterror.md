---
title: bitsadmin geterror
description: Argomento dei comandi di Windows per **BITSAdmin GetError**, che recupera informazioni dettagliate sugli errori per il processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cbe5bca1-d2dd-4ce6-903f-f85de4a2ec6a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7c65b072bb190e3e516b917c310942146bb3f3d2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850704"
---
# <a name="bitsadmin-geterror"></a>bitsadmin geterror

Recupera informazioni dettagliate sugli errori per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /geterror <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente vengono recuperate le informazioni sull'errore per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /geterror myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)