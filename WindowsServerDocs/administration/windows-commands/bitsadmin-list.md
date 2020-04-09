---
title: bitsadmin list
description: Windows Commands argomento for **bitsadmin list**, che elenca i processi di trasferimento di proprietà dell'utente corrente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1416965e-e0e6-49cf-b1d4-b286d3cf8716
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1883da7bfa71a41952f6f67e25eca4dbbdd3353c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850324"
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

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente vengono recuperate le informazioni sui processi di proprietà dell'utente corrente.

```
C:\>bitsadmin /list
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)