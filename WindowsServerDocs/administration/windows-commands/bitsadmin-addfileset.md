---
title: bitsadmin addfileset
description: Argomento i comandi di Windows per **bitsadmin addfileset** -aggiunge uno o più file al processo specificato.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 75466994-262f-4724-b14d-f813c5397675
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8f6ff32dfa6042272c68647477d77183ce9cb76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889442"
---
# <a name="bitsadmin-addfileset"></a>bitsadmin addfileset

Aggiunge uno o più file al processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /addfileset <Job> <TextFile>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|TextFile|Un file di testo, ogni riga che contiene un server remoto e un nome di file locale.</br>Nota: I nomi sono delimitati da spazi. Le righe che iniziano con un carattere # vengono trattate come commento.|

## <a name="BKMK_examples"></a>Esempi

```
C:\>bitsadmin /addfileset files.txt
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)