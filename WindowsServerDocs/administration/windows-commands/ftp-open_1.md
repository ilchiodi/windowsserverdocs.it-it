---
title: FTP aperto
description: Argomento di riferimento per il comando di apertura FTP, che si connette al server FTP specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4b61926a-dc60-4b4c-96d3-64e5c91c18ba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63e428164ece405a6a83041edd46ffe332b13c3a
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820401"
---
# <a name="ftp-open"></a>FTP aperto

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Stabilisce la connessione al server FTP specificato.

## <a name="syntax"></a>Sintassi

```
open <computer> [<port>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<computer>` | Specifica il computer remoto a cui si sta tentando di connettersi. È possibile utilizzare un nome di computer o un indirizzo IP (in tal caso, è necessario che sia disponibile un server DNS o un file hosts). |
| `[<port>]` | Specifica un numero di porta TCP da utilizzare per la connessione a un server FTP. Per impostazione predefinita, viene usata la porta TCP 21. |

### <a name="examples"></a>Esempi

Per connettersi al server FTP in *FTP.Microsoft.com*, digitare:

```
open ftp.microsoft.com
```

Per connettersi al server FTP in *FTP.Microsoft.com* che è in ascolto sulla porta TCP *755*, digitare:

```
open ftp.microsoft.com 755
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Altre informazioni aggiuntive sull'FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
