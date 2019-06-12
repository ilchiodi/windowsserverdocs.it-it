---
title: makecab
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 7b120cf990abe2024fd6c96ca2f1ef11fa2350ae
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437536"
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
-   Fare riferimento a [formato Microsoft Cabinet](https://go.microsoft.com/fwlink/?LinkId=226852) su MSDN per informazioni su directive_file.

## <a name="additional-references"></a>Riferimenti aggiuntivi
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

