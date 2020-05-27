---
title: valore letterale FTP
description: Argomento di riferimento per il comando FTP Literal, che invia argomenti Verbatim al server FTP remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fb81aa2d-07fa-4e79-bf44-1fb5526fdf14
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5015f2184c9273aae6dbd01b18ee1f540d5b9aa3
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820161"
---
# <a name="ftp-literal"></a>valore letterale FTP

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Invia argomenti Verbatim al server FTP remoto. Viene restituito un singolo codice di risposta FTP.

> [!NOTE]
> Questo comando è lo stesso del [comando di virgolette FTP](ftp-quote.md).

## <a name="syntax"></a>Sintassi

```
literal <argument> [ ]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<argument>` | Specifica l'argomento da inviare al server FTP. |

### <a name="examples"></a>Esempi

Per inviare un comando **Quit** al server FTP remoto, digitare:

```
literal quit
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando di virgolette FTP](ftp-quote.md)

- [Altre informazioni aggiuntive sull'FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
