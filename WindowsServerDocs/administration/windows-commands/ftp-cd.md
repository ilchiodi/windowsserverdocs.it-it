---
title: CD FTP
description: Argomento di riferimento per il comando CD FTP, che consente di modificare la directory di lavoro nel computer remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a574855a-31b4-45c6-bce2-581c7231c99b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dbeff9c2fca654120347f0f616325b700f5c9be3
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820001"
---
# <a name="ftp-cd"></a>CD FTP

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifica la directory di lavoro nel computer remoto.

## <a name="syntax"></a>Sintassi

```
cd <remotedirectory>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| <remotedirectory> | Specifica la directory sul computer remoto a cui si desidera modificare. |

### <a name="examples"></a>Esempi

Per modificare la directory nel computer remoto in *docs*, digitare:

```
cd Docs
```

Per modificare la directory nel computer remoto in *video di maggio*, digitare:

```
cd  May Videos
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Altre informazioni aggiuntive sull'FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
