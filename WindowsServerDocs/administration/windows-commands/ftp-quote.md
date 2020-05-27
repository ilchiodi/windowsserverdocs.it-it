---
title: virgolette FTP
description: Argomento di riferimento per il comando FTP quote, che invia argomenti Verbatim al server FTP remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4500a1d3-c091-42c7-a909-f61df7f2e993
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 87dd81d4feb6a5509a8609f5c479e3352d5fb7ea
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820341"
---
# <a name="ftp-quote"></a>virgolette FTP

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Invia argomenti Verbatim al server FTP remoto. Viene restituito un singolo codice di risposta FTP.

> [!NOTE]
> Questo comando corrisponde al [comando FTP Literal](ftp-literal_1.md).

## <a name="syntax"></a>Sintassi

```
quote <argument>[ ]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<argument>` | Specifica l'argomento da inviare al server FTP. |

### <a name="examples"></a>Esempi

Per inviare un comando **Quit** al server FTP remoto, digitare:

```
quote quit
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando valore letterale FTP](ftp-literal_1.md)

- [Altre informazioni aggiuntive sull'FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))