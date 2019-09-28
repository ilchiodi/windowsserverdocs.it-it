---
title: makecab
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4da95297-c593-427b-9f76-2f389c46cbf4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b0231b6f1ddd3e81caa7544587f764e2308015b8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374151"
---
# <a name="makecab"></a>makecab

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Comprimere i file esistenti in un file cabinet (CAB).
## <a name="syntax"></a>Sintassi
```
makecab [/v[n]] [/d var=<value> ...] [/l <dir>] <source> [<destination>]
makecab [/v[<n>]] [/d var=<value> ...] /f <directives_file> [...]
```
### <a name="parameters"></a>Parametri

|      Parametro       |                                                                        Descrizione                                                                        |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
|       <source>       |                                                                     File da comprimere.                                                                     |
|    <destination>     | Nome file da assegnare al file compresso. Se omesso, l'ultimo carattere del nome del file di origine viene sostituito con un carattere di sottolineatura (_) e utilizzato come destinazione. |
| /f < directives_file > |                                                   Un file con **makecab** direttive (può essere ripetuto).                                                   |
|    /d var =<value>    |                                                          Definisce una variabile con il valore specificato.                                                           |
|       /l <dir>       |                                               Posizione in cui collocare destinazione (valore predefinito è la directory corrente).                                               |
|       /v[<n>]        |                                                    Impostare il debug a livello di dettaglio (0 = none,..., 3 = full).                                                     |
|          /?          |                                                           Visualizza la guida al prompt dei comandi.                                                            |

## <a name="remarks"></a>Note
-   Per informazioni su directive_file, vedere la pagina relativa al [formato Microsoft Cabinet](https://go.microsoft.com/fwlink/?LinkId=226852) in MSDN.

## <a name="additional-references"></a>Riferimenti aggiuntivi
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

