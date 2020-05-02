---
title: bitsadmin info
description: Argomento di riferimento per il comando Bitsadmin info, che visualizza le informazioni di riepilogo sul processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5c306677-0d64-41c0-8276-5bba7750cecb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 904f70c82ab4bcc4fb25f759898674cc719b1954
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717433"
---
# <a name="bitsadmin-info"></a>bitsadmin info

Visualizza le informazioni di riepilogo sul processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /info <job> [/verbose]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |
| /verbose | Facoltativa. Fornisce informazioni dettagliate su ogni processo. |

## <a name="examples"></a>Esempi

Per recuperare le informazioni sul processo denominato *myDownloadJob*:

```
bitsadmin /info myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [bitsadmin info](bitsadmin-info.md)
