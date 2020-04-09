---
title: ktmutil
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 53bc56df-f0e5-443b-ab20-bbf8b11d4a9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2e65312ea4bb3169b90c2550b8b945919b86587f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841194"
---
# <a name="ktmutil"></a>ktmutil



Avvia l'utilità Gestione transazioni kernel. Se usato senza parametri, **ktmutil** Visualizza i sottocomandi disponibili.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
ktmutil list tms 
ktmutil list transactions [{TmGuid}]
ktmutil resolve complete {TmGuid} {RmGuid} {EnGuid}
ktmutil resolve commit {TxGuid}
ktmutil resolve rollback {TxGuid}
ktmutil force commit {??Guid}
ktmutil force rollback {??Guid}
ktmutil forget
```

### <a name="parameters"></a>Parametri

## <a name="remarks"></a>Note

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per forzare una transazione InDoubt con GUID 311a9209-03f4-11dc-918f-00188b8f707b al commit, digitare:
```
ktmutil force commit {311a9209-03f4-11dc-918f-00188b8f707b}
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)