---
title: Telnet aperto
description: Argomento di riferimento per Telnet aperto, che si connette a un server Telnet.
ms.prod: windows-server
ms.technology: manage-windows-commands
s.topic: article
ms.assetid: e30ad68c-2366-4754-ac36-311a2392902a vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ed2c00b44143942ba1ccb77eec17ca975f23b498
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821321"
---
# <a name="telnet-open"></a>Telnet: Apri

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Stabilisce la connessione a un server Telnet.

## <a name="syntax"></a>Sintassi
```
o[pen] <hostname> [<Port>]
```
#### <a name="parameters"></a>Parametri

| Parametro  |                                        Descrizione                                         |
|------------|--------------------------------------------------------------------------------------------|
| <hostname> |                         Specifica il nome del computer o l'indirizzo IP.                         |
|  [<Port>]  | Specifica la porta TCP su cui è in ascolto il server Telnet. Il valore predefinito è la porta TCP 23. |

## <a name="examples"></a>Esempi
Connettersi a un server Telnet in telnet.microsoft.com.
```
o telnet.microsoft.com
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
