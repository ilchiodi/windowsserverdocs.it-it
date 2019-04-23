---
title: bitsadmin list
description: Argomento i comandi di Windows per **bitsadmin elenco** -vengono elencati i processi di trasferimento di proprietà dell'utente corrente.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f0b88001b9c4ae01b57006ffeef66dec0348ca77
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873862"
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
|/Allusers|Facoltativo: Elenca i processi per tutti gli utenti|
|/ Verbose|Facoltativo: vengono fornite informazioni dettagliate per ogni processo.|

## <a name="remarks"></a>Note

È necessario disporre dei privilegi di amministratore per utilizzare il parametro /allusers

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente recupera le informazioni sui processi di proprietà dell'utente corrente.
```
C:\>bitsadmin /List 
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)