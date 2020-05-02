---
title: bitsadmin create
description: Argomento di riferimento per il comando Bitsadmin create, che crea un processo di trasferimento con il nome visualizzato specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9a8c53af-900b-4c24-9265-5b8b08213fac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 728027eb4680805c1f9a2afc32d8d37a14239597
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718215"
---
# <a name="bitsadmin-create"></a>bitsadmin create

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea un processo di trasferimento con il nome visualizzato specificato.

> [!NOTE]
> I tipi di parametro **tra/upload** e **/upload-Reply** non sono supportati da BITS 1,2 e versioni precedenti.

## <a name="syntax"></a>Sintassi

```
bitsadmin /create [type] displayname
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ------- | -------- |
| type | Sono disponibili tre tipi di processi:<ul><li>**Scaricare.** Trasferisce i dati da un server a un file locale.</li><li>**/Upload.** Trasferisce i dati da un file locale a un server.</li><li>**/Upload-Reply.** Trasferisce i dati da un file locale a un server e riceve un file di risposta dal server.</li></ul>Se non è specificato, il valore predefinito di questo parametro è **/download** . |
| displayname | Nome visualizzato assegnato al processo appena creato. |

## <a name="examples"></a>Esempi

Per creare un processo di download denominato *myDownloadJob*:

```
bitsadmin /create myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin Resume](bitsadmin-resume.md)

- [comando Bitsadmin](bitsadmin.md)
