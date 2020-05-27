---
title: ktmutil
description: Argomento di riferimento per il comando ktmutil, che avvia l'utilità Gestione transazioni kernel.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 53bc56df-f0e5-443b-ab20-bbf8b11d4a9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ca26d2289e32d8eb618ab7cc9393678a16e318b
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817311"
---
# <a name="ktmutil"></a>ktmutil

Avvia l'utilità Gestione transazioni kernel. Se usato senza parametri, **ktmutil** Visualizza i sottocomandi disponibili.

## <a name="syntax"></a>Sintassi

```
ktmutil list tms
ktmutil list transactions [{TmGUID}]
ktmutil resolve complete {TmGUID} {RmGUID} {EnGUID}
ktmutil resolve commit {TxGUID}
ktmutil resolve rollback {TxGUID}
ktmutil force commit {GUID}
ktmutil force rollback {GUID}
ktmutil forget
```

## <a name="examples"></a>Esempi


Per forzare una transazione InDoubt con GUID 311a9209-03f4-11dc-918f-00188b8f707b al commit, digitare:

```
ktmutil force commit {311a9209-03f4-11dc-918f-00188b8f707b}
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)