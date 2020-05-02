---
title: bitsadmin getclientcertificate
description: Argomento di riferimento per il comando Bitsadmin GetClientCertificate, che consente di recuperare il certificato client dal processo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4fc8f408-085e-43a0-9fa8-3d798ef107b1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d2582950dd02ca1880e4765fb974c83c423b22bb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718128"
---
# <a name="bitsadmin-getclientcertificate"></a>bitsadmin getclientcertificate

Recupera il certificato client dal processo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getclientcertificate <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Per recuperare il certificato client per il processo denominato *myDownloadJob*:

```
bitsadmin /getclientcertificate myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
