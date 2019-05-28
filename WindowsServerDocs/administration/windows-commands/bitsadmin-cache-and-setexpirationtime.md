---
title: setexpirationtime e cache bitsadmin
description: Argomento i comandi di Windows per **bitsadmin cache e setexpirationtime** -imposta l'ora di scadenza della cache.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 00ea6e4e-b707-4b31-88dd-b61a78565c8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8e896df0a88c0cfc4eec07aba4807f184e7abe32
ms.sourcegitcommit: b190fac4bfa5599751a60d3fc3b4c4a64dd9afd7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/22/2019
ms.locfileid: "66008932"
---
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="bitsadmin-cache-and-setexpirationtime"></a>setexpirationtime e cache bitsadmin
Imposta l'ora di scadenza della cache.
## <a name="syntax"></a>Sintassi
```
bitsadmin /Cache /SetExpirationtime secs
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|secondi|Il numero di secondi fino alla scadenza della cache.|
## <a name="BKMK_examples"></a>Esempi
Nell'esempio seguente la cache scadrà tra 60 secondi.
```
C:\>bitsadmin /Cache / SetExpirationtime 60
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
