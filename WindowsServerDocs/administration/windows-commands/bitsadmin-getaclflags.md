---
title: bitsadmin getaclflags
description: Argomento di riferimento per il comando Bitsadmin GETACLFLAGS, che consente di recuperare i flag di propagazione dell'elenco di controllo di accesso (ACL).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 99266def-7479-4430-a61c-98ec433fa88b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a9ca541b488c3c83e7a64a138bae0914001778e3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718179"
---
# <a name="bitsadmin-getaclflags"></a>bitsadmin getaclflags

Recupera i flag di propagazione dell'elenco di controllo di accesso (ACL), che indicano se gli elementi vengono ereditati dagli oggetti figlio.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getaclflags <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| processo | Nome visualizzato o GUID del processo. |

### <a name="remarks"></a>Osservazioni

Restituisce uno o più dei seguenti valori di flag:

- **o** -copia le informazioni sul proprietario con il file.

- **g** -copia le informazioni sul gruppo con il file.

- **d** : copia le informazioni dell'elenco di controllo di accesso discrezionale (DACL) con file.

- **s** : copia le informazioni dell'elenco di controllo di accesso di sistema (SACL) con file.

## <a name="examples"></a>Esempi

Per recuperare i flag di propagazione dell'elenco di controllo di accesso per il processo denominato *myDownloadJob*:

```
bitsadmin /getaclflags myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
