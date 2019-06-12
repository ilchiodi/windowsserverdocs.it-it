---
title: bootcfg timeout
description: Argomento i comandi di Windows per **timeout bootcfg** -modifica il valore di timeout del sistema operativo.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aa858eac-2bb7-4a27-a9bc-3e4a6eb8b2c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1fc33d2d20d6d2532c5ed1f33e27a768935d1e85
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434646"
---
# <a name="bootcfg-timeout"></a>bootcfg timeout

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifica il valore di timeout del sistema operativo.

## <a name="syntax"></a>Sintassi
```
bootcfg /timeout <timeOutValue> [/s <computer> [/u <Domain\User>/p <Password>]]
```
## <a name="parameters"></a>Parametri

|        Parametro        |                                                                                                                                                                                  Descrizione                                                                                                                                                                                   |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /timeout <timeOutValue> | Specifica il valore di timeout nella sezione [boot loader]. Il <timeOutValue> è il numero di secondi, l'utente deve selezionare un sistema operativo dalla schermata del caricatore di avvio prima di NTLDR carica il valore predefinito. Intervallo valido per <timeOutValue> è compreso tra 0 e 999. Se il valore è 0, NTLDR avvia immediatamente il sistema operativo predefinito senza visualizzare la schermata del caricatore di avvio. |
|      /s <computer>      |                                                                                                                               Specifica il nome o indirizzo IP di un computer remoto (non utilizzare le barre rovesciate). Il valore predefinito è il computer locale.                                                                                                                               |
|    /u < dominio\utente >     |                                                                                       Esegue il comando con le autorizzazioni dell'account dell'utente specificato da <User> o < dominio\utente >. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso.                                                                                        |
|      /p <Password>      |                                                                                                                                            Specifica il <Password> dell'account utente specificato nella **/u** parametro.                                                                                                                                             |
|           /?            |                                                                                                                                                                      Visualizza la guida al prompt dei comandi.                                                                                                                                                                      |

## <a name="BKMK_examples"></a>Esempi
Gli esempi seguenti illustrano come utilizzare il **bootcfg /timeout** comando:
```
bootcfg /timeout 30
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /timeout 50
```
#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
