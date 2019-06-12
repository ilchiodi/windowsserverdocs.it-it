---
title: bootcfg copy
description: Argomento i comandi di Windows per **copia bootcfg** -esegue una copia di una voce di avvio esistenti, in cui è possibile aggiungere le opzioni della riga di comando.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2a236c2a-8675-444d-b695-9cbc9aff643b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b76ecfe953d1a462e311fdaaeba35e8f962165c4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434864"
---
# <a name="bootcfg-copy"></a>bootcfg copy

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea una copia di una voce di avvio esistente, a cui è possibile aggiungere opzioni della riga di comando.

## <a name="syntax"></a>Sintassi
```
bootcfg /copy [/s <computer> [/u <Domain>\<User> /p <Password>]] [/d <Description>] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parametri

|      Parametro       |                                                                                             Descrizione                                                                                             |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                         Specifica il nome o indirizzo IP di un computer remoto (non utilizzare le barre rovesciate). Il valore predefinito è il computer locale.                                          |
| /u <Domain>\\<User>  | Esegue il comando con le autorizzazioni dell'account dell'utente specificato da <User>oppure <Domain> \\ <User>. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso. |
|    /p <Password>     |                                                        Specifica la password dell'account utente specificato nella **/u** parametro.                                                        |
|   /d <Description>   |                                                                    Specifica la descrizione per la nuova voce del sistema operativo.                                                                    |
| /id <OSEntryLineNum> |         Specifica il numero di riga voce del sistema operativo in della sezione [operating systems] del file Boot. ini da copiare. La prima riga dopo la sezione [operating systems] sezione di intestazione è 1.         |
|          /?          |                                                                                Visualizza la guida al prompt dei comandi.                                                                                 |

## <a name="BKMK_examples"></a>Esempi
Gli esempi seguenti illustrano come utilizzare il **bootcfg /copy** comando per copiare la voce di avvio 1 e immettere "\ABC Server\\" come descrizione:
```
bootcfg /copy /d "\ABC Server\" /id 1
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
