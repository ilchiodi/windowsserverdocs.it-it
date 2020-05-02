---
title: nslookup set class
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ed826400-40da-42b6-b7f0-95db73790723
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b1ae3a5336815a5273aafa976b1dcad8b60fac9b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723649"
---
# <a name="nslookup-set-class"></a>nslookup set class



Modifica la classe di query. La classe specifica il gruppo di protocolli delle informazioni.

## <a name="syntax"></a>Sintassi

```
set class=<Class>
```

### <a name="parameters"></a>Parametri

| Parametro |                                                                                                                                    Descrizione                                                                                                                                    |
|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \<Class>  | La classe predefinita è IN. Di seguito sono elencati i valori validi per questo comando.</br>-IN: specifica la classe Internet.</br>-CHAOS: specifica la classe Chaos.</br>-Esiodo: specifica la classe MIT Athena Esiodo.</br>-ANY: specifica uno dei caratteri jolly elencati in precedenza. |
|   {Guida   |                                                                                                                                        ?}                                                                                                                                         |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)