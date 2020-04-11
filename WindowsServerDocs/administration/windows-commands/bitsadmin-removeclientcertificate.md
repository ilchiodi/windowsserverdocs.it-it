---
title: removeclientcertificate Bitsadmin
description: Windows Commands Topic for **BITSAdmin removeclientcertificate**, che rimuove il certificato client dal processo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b417c3e5-aadd-4fcc-968f-45d8b67ca516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 312226b73b91385436e15c4afbb49df161258768
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123102"
---
# <a name="bitsadmin-removeclientcertificate"></a>removeclientcertificate Bitsadmin

Rimuove il certificato del client dal processo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /removeclientcertificate <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Nell'esempio seguente viene rimosso il certificato client dal processo denominato *myDownloadJob*.

```
C:\>bitsadmin /removeclientcertificate myDownloadJob 
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)