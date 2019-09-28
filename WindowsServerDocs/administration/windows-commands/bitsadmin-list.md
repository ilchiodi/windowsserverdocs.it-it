---
title: bitsadmin list
description: Argomento comandi di Windows per l' **elenco Bitsadmin** -elenca i processi di trasferimento di proprietà dell'utente corrente.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1416965e-e0e6-49cf-b1d4-b286d3cf8716
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd4787f51dc2a7843ff6cf5c4f786658e530ad8f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381103"
---
# <a name="bitsadmin-list"></a>bitsadmin list



Elenca i processi di trasferimento di proprietà dell'utente corrente.

## <a name="syntax"></a>Sintassi

```
bitsadmin /List [/allusers][/verbose]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/Allusers|Facoltativo: elenca i processi per tutti gli utenti|
|/Verbose|Facoltativo: fornisce informazioni dettagliate per ogni processo.|

## <a name="remarks"></a>Note

Per usare il parametro/ALLUSERS, è necessario disporre dei privilegi di amministratore

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente vengono recuperate le informazioni sui processi di proprietà dell'utente corrente.
```
C:\>bitsadmin /List 
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)