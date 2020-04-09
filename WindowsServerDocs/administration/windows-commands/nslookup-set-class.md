---
title: nslookup set class
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ed826400-40da-42b6-b7f0-95db73790723
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bc3bb4e36582f01584c0b89a12d43874322c3190
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838584"
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
| Classe \<>  | La classe predefinita è IN. Di seguito sono elencati i valori validi per questo comando.</br>-IN: specifica la classe Internet.</br>-CHAOS: specifica la classe Chaos.</br>-Esiodo: specifica la classe MIT Athena Esiodo.</br>-ANY: specifica uno dei caratteri jolly elencati in precedenza. |
|   {Guida   |                                                                                                                                        ?}                                                                                                                                         |

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)