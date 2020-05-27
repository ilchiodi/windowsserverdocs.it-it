---
title: remotehelp FTP
description: Argomento di riferimento per il comando ftp remotehelp, che visualizza la guida per i comandi remoti.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef23adf3-ead4-44c8-ac1d-c8a6f4b2bf73
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 659de5487890b50aab9004f650e4584085e7306c
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820321"
---
# <a name="ftp-remotehelp"></a>remotehelp FTP

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza la guida per i comandi remoti.

## <a name="syntax"></a>Sintassi

```
remotehelp [<command>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ------- | -------- |
| `[<command>]` | Specifica il nome del comando su cui si desidera visualizzare la guida. Se `<command>` non è specificato, questo comando Visualizza un elenco di tutti i comandi remoti. È anche possibile eseguire comandi remoti usando una [virgoletta FTP](ftp-quote.md) o un [valore letterale FTP](ftp-literal_1.md). |

### <a name="examples"></a>Esempi

Per visualizzare un elenco di comandi remoti, digitare:

```
remotehelp
```

Per visualizzare la sintassi del comando *feat* remote, digitare:

```
remotehelp feat
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [virgolette FTP](ftp-quote.md)

- [valore letterale FTP](ftp-literal_1.md)

- [Altre informazioni aggiuntive sull'FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
