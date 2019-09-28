---
title: Bitsadmin cache e setexpirationtime
description: 'Argomento dei comandi di Windows per la **cache Bitsadmin e setexpirationtime** : imposta la scadenza della cache.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 386c6659e4410b41669ade39d8af97829d81a1cb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381917"
---
>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="bitsadmin-cache-and-setexpirationtime"></a>Bitsadmin cache e setexpirationtime
Imposta l'ora di scadenza della cache.
## <a name="syntax"></a>Sintassi
```
bitsadmin /Cache /SetExpirationtime secs
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|secondi|Numero di secondi fino alla scadenza della cache.|
## <a name="BKMK_examples"></a>Esempi
L'esempio seguente scade la cache in 60 secondi.
```
C:\>bitsadmin /Cache / SetExpirationtime 60
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
