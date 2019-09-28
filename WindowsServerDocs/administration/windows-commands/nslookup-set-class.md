---
title: nslookup set class
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed826400-40da-42b6-b7f0-95db73790723
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 312b409490603fcb0ded63a78f3a2936f5216de1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372984"
---
# <a name="nslookup-set-class"></a>nslookup set class



Modifica la classe di query. La classe specifica il gruppo di protocolli delle informazioni.

## <a name="syntax"></a>Sintassi

```
set class=<Class>
```

## <a name="parameters"></a>Parametri

| Parametro |                                                                                                                                    Descrizione                                                                                                                                    |
|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \<> Della classe  | La classe predefinita è IN. Di seguito sono elencati i valori validi per questo comando.</br>IN Specifica la classe Internet.</br>CAOS Specifica la classe Chaos.</br>ESIODO Specifica la classe MIT Athena Esiodo.</br>QUALSIASI Specifica uno dei caratteri jolly elencati in precedenza. |
|   {Guida   |                                                                                                                                        ?}                                                                                                                                         |

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)