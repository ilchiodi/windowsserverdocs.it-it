---
title: bitsadmin list
description: Argomento di riferimento per il comando Bitsadmin list, che elenca i processi di trasferimento di proprietà dell'utente corrente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1416965e-e0e6-49cf-b1d4-b286d3cf8716
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 262589293a147cc1bae98da8fdca047c5f914094
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717426"
---
# <a name="bitsadmin-list"></a>bitsadmin list

Elenca i processi di trasferimento di proprietà dell'utente corrente.

## <a name="syntax"></a>Sintassi

```
bitsadmin /list [/allusers][/verbose]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| /allusers | Facoltativa. Elenca i processi per tutti gli utenti. Per utilizzare questo parametro, è necessario disporre dei privilegi di amministratore. |
| /verbose | Facoltativa. Fornisce informazioni dettagliate su ogni processo. |

## <a name="examples"></a>Esempi

Per recuperare informazioni sui processi di proprietà dell'utente corrente.

```
bitsadmin /list
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
