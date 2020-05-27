---
title: LCD FTP
description: Argomento di riferimento per il comando FTP LCD, che consente di modificare la directory di lavoro nel computer locale.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 60a25808-6abb-408b-8373-0bbdcd0994b4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c941adcc8c56d2598ad4ff56852309a256656878
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820131"
---
# <a name="ftp-lcd"></a>LCD FTP

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifica la directory di lavoro nel computer locale. Per impostazione predefinita, la directory di lavoro è la directory in cui è stato avviato il comando **FTP** .

## <a name="syntax"></a>Sintassi

```
lcd [<directory>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `[<directory>]` | Specifica la directory del computer locale a cui si desidera modificare. Se la *directory* non è specificata, la directory di lavoro corrente viene modificata nella directory predefinita. |

### <a name="examples"></a>Esempi

Per impostare la directory di lavoro sul computer locale su *c:\Dir1*, digitare:

```
lcd c:\dir1
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Altre informazioni aggiuntive sull'FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
