---
title: makecab
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4da95297-c593-427b-9f76-2f389c46cbf4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5a0bf4afda09f0e0e8416777a2cfd1404d4bf59a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724226"
---
# <a name="makecab"></a>makecab

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Comprimere i file esistenti in un file cabinet (CAB).
## <a name="syntax"></a>Sintassi
```
makecab [/v[n]] [/d var=<value> ...] [/l <dir>] <source> [<destination>]
makecab [/v[<n>]] [/d var=<value> ...] /f <directives_file> [...]
```
#### <a name="parameters"></a>Parametri

|      Parametro       |                                                                        Descrizione                                                                        |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
|       <source>       |                                                                     File da comprimere.                                                                     |
|    <destination>     | Nome file da assegnare al file compresso. Se omesso, l'ultimo carattere del nome del file di origine viene sostituito con un carattere di sottolineatura (_) e utilizzato come destinazione. |
| /f < directives_file > |                                                   Un file con **makecab** direttive (può essere ripetuto).                                                   |
|    /d var =<value>    |                                                          Definisce una variabile con il valore specificato.                                                           |
|       /l<dir>       |                                               Posizione in cui collocare destinazione (valore predefinito è la directory corrente).                                               |
|       /v[<n>]        |                                                    Impostare il debug a livello di dettaglio (0 = none,..., 3 = full).                                                     |
|          /?          |                                                           Visualizza la guida al prompt dei comandi.                                                            |

## <a name="remarks"></a>Osservazioni
-   Per informazioni su directive_file, vedere [Microsoft Cabinet Format](https://go.microsoft.com/fwlink/?LinkId=226852) su MSDN.

## <a name="additional-references"></a>Riferimenti aggiuntivi
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

