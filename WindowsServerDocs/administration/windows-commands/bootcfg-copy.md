---
title: bootcfg copy
description: Windows Commands argomento per bootcfg copy, che crea una copia di una voce di avvio esistente, a cui è possibile aggiungere opzioni della riga di comando.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2a236c2a-8675-444d-b695-9cbc9aff643b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5194418a07aece4f15a84c3eccbc044431a865b9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848684"
---
# <a name="bootcfg-copy"></a>bootcfg copy

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea una copia di una voce di avvio esistente, a cui è possibile aggiungere opzioni della riga di comando.

## <a name="syntax"></a>Sintassi
```
bootcfg /copy [/s <computer> [/u <Domain>\<User> /p <Password>]] [/d <Description>] [/id <OSEntryLineNum>]
```
### <a name="parameters"></a>Parametri

|      Parametro       |                                                                                             Descrizione                                                                                             |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s <computer>     |                                         Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale.                                          |
| /u <Domain>\\<User>  | Esegue il comando con le autorizzazioni dell'account dell'utente specificato da <User>o <Domain>\\<User>. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso. |
|    /p <Password>     |                                                        Specifica la password dell'account utente specificato nella **/u** parametro.                                                        |
|   /d <Description>   |                                                                    Specifica la descrizione per la nuova voce del sistema operativo.                                                                    |
| <OSEntryLineNum>/ID |         Specifica il numero di riga voce del sistema operativo in della sezione [operating systems] del file Boot. ini da copiare. La prima riga dopo la sezione [operating systems] sezione di intestazione è 1.         |
|          /?          |                                                                                Visualizza la guida al prompt dei comandi.                                                                                 |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Gli esempi seguenti illustrano come è possibile usare il comando **bootcfg/copy** per copiare la voce di avvio 1 e immettere \ABC server\\ come descrizione:
```
bootcfg /copy /d \ABC Server\ /id 1
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
