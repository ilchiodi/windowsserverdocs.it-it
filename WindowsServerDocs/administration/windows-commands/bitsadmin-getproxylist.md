---
title: "Bitsadmin getproxys: Recupera l'elenco di proxy per il processo specificato."
description: Windows Commands argomento per **BITSAdmin getproxys**, che recupera l'elenco di proxy per il processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: eebfa727-d8f1-4ae3-9382-6d8ffe8c3df3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0c0d26fb074bd1b792caa7fe2ce8fd31b64365e2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850524"
---
# <a name="bitsadmin-getproxylist"></a>bitsadmin getproxylist

Recupera l'elenco delimitato da virgole dei server proxy da utilizzare per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getproxylist <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente recupera l'elenco di proxy per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /getproxylist myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)