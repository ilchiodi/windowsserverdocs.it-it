---
title: bootcfg copy
description: 'Windows Commands Topic for **bootcfg copy** : crea una copia di una voce di avvio esistente, a cui è possibile aggiungere opzioni della riga di comando.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 42a408443cbe6722c25780f7c27d70b05da7eb8e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380124"
---
# <a name="bootcfg-copy"></a>bootcfg copy

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea una copia di una voce di avvio esistente, a cui è possibile aggiungere opzioni della riga di comando.

## <a name="syntax"></a>Sintassi
```
bootcfg /copy [/s <computer> [/u <Domain>\<User> /p <Password>]] [/d <Description>] [/id <OSEntryLineNum>]
```
## <a name="parameters"></a>Parametri

|      Parametro       |                                                                                             Descrizione                                                                                             |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                         Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale.                                          |
| /u <Domain>\\<User>  | Esegue il comando con le autorizzazioni dell'account dell'utente specificato da <User>o <Domain>\\<User>. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso. |
|    /p <Password>     |                                                        Specifica la password dell'account utente specificato nella **/u** parametro.                                                        |
|   /d <Description>   |                                                                    Specifica la descrizione per la nuova voce del sistema operativo.                                                                    |
| <OSEntryLineNum>/ID |         Specifica il numero di riga voce del sistema operativo in della sezione [operating systems] del file Boot. ini da copiare. La prima riga dopo la sezione [operating systems] sezione di intestazione è 1.         |
|          /?          |                                                                                Visualizza la guida al prompt dei comandi.                                                                                 |

## <a name="BKMK_examples"></a>Esempi
Gli esempi seguenti illustrano come utilizzare il **bootcfg /copy** comando per copiare la voce di avvio 1 e immettere "\ABC Server\\" come descrizione:
```
bootcfg /copy /d "\ABC Server\" /id 1
```
#### <a name="additional-references"></a>riferimenti aggiuntivi
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
