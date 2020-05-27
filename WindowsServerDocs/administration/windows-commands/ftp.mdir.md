---
title: mdir FTP
description: Argomento di riferimento per il comando FTP mdir, che visualizza un elenco di directory di file e sottodirectory in una directory remota.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 90eec45b-558b-4b8d-bbe4-b56d98e1ca70
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b192e6de23105fcc696d8369ce0280167a201e20
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820221"
---
# <a name="ftp-mdir"></a>mdir FTP

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza un elenco di directory di file e le sottodirectory in una directory remota.

## <a name="syntax"></a>Sintassi

```
mdir <remotefile>[...] <localfile>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<remotefile>` | Specifica la directory o file per il quale si desidera visualizzare un elenco. È possibile specificare più *remoteFiles*. Digitare un trattino (-) per utilizzare la directory di lavoro corrente nel computer remoto. |
| `<localfile>` | Specifica un file locale per archiviare l'elenco. Questo parametro è obbligatorio. Digitare un trattino (-) per visualizzare l'elenco sullo schermo. |

### <a name="examples"></a>Esempi

Per visualizzare un elenco di directory di *dir1* e *dir2* sullo schermo, digitare:

```
mdir dir1 dir2 -
```

Per salvare l'elenco di directory combinate di *dir1* e *dir2* in un file locale denominato *dirlist. txt*, digitare:

```
mdir dir1 dir2 dirlist.txt
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Altre informazioni aggiuntive sull'FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
