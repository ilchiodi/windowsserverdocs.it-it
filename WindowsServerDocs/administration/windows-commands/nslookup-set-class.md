---
title: nslookup set class
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 7953f450c17afdee849515f8d8945631a30f4b98
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436846"
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
| \<Classe >  | La classe predefinita è in. Di seguito sono elencati i valori validi per questo comando.</br>- IN: Specifica la classe di Internet.</br>- CHAOS: Specifica la classe di Chaos.</br>- HESIOD: Specifica la classe Athena Hesiod MIT.</br>-ANY: Specifica uno qualsiasi dei caratteri jolly elencate in precedenza. |
|   Guida {   |                                                                                                                                        ?}                                                                                                                                         |

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)