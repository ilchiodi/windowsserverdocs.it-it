---
title: richiesta FTP
description: Argomento di riferimento per il comando della richiesta FTP, che consente di attivare e disattivare la modalità di richiesta.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 930df39b-45c4-4e0b-bfe2-1d1963be817a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 57f0e1eead36665c19845944bf22325b1aecebbb
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820381"
---
# <a name="ftp-prompt"></a>richiesta FTP

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Attiva e disattiva la modalità di richiesta. Per impostazione predefinita, la modalità di richiesta è attivata. Se la modalità di richiesta è attivata, il comando FTP richiede durante i trasferimenti di più file per consentire di recuperare o archiviare i file in modo selettivo.

> [!NOTE]
> È possibile utilizzare i comandi [FTP mget](ftp-mget.md) e [ftp mput](ftp-mput_1.md) per trasferire tutti i file quando la modalità richiesta è disattivata.

## <a name="syntax"></a>Sintassi

```
prompt
```

### <a name="examples"></a>Esempi

Per attivare e disattivare la modalità di richiesta, digitare:

```
prompt
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Altre informazioni aggiuntive sull'FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
