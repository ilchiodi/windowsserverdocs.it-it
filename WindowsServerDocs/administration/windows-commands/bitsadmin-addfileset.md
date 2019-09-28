---
title: bitsadmin addfileset
description: 'Argomento dei comandi di Windows per **BITSAdmin ADDFILESET** : aggiunge uno o più file al processo specificato.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 464f2da151d5a7bfffde286e52d9158560d48dcc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381986"
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
|TextFile|Un file di testo, ogni riga contenente un nome di file remoto e un nome di file locale.</br>Nota: I nomi sono delimitati da spazi. Le righe che iniziano con un carattere # vengono considerate come commenti.|

## <a name="BKMK_examples"></a>Esempi

```
C:\>bitsadmin /addfileset files.txt
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)