---
title: bitsadmin getproxybypasslist
description: Argomento di riferimento per il comando Bitsadmin getproxybypasslist, che consente di recuperare l'elenco di bypass proxy per il processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 50959be3-7014-4bc9-9a7b-68f1ff94a94a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3b4f37d09521c28d55104975ed754a10e8df011e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717667"
---
# <a name="bitsadmin-getproxybypasslist"></a>bitsadmin getproxybypasslist

Recupera l'elenco di bypass del proxy per il processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getproxybypasslist <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

### <a name="remarks"></a>Osservazioni

L'elenco di bypass contiene i nomi host o gli indirizzi IP, o entrambi, che non devono essere instradati tramite un proxy. L'elenco può contenere `<local>` per fare riferimento a tutti i server nella stessa LAN. L'elenco può essere punto e virgola (;) o delimitato da spazi.

## <a name="examples"></a>Esempi

Per recuperare l'elenco di bypass del proxy per il processo denominato *myDownloadJob*:

```
bitsadmin /getproxybypasslist myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
