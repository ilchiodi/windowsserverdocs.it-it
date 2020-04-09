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
ms.openlocfilehash: 38cf00dc48ff036e7d710fb7436f00fd9381dd0b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849874"
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

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente viene rimosso il certificato client dal processo denominato *myDownloadJob*.

```
C:\>bitsadmin /removeclientcertificate myDownloadJob 
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)