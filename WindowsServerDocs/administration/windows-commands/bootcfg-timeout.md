---
title: bootcfg timeout
description: Windows Commands Topic for bootcfg timeout, che modifica il valore di timeout del sistema operativo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aa858eac-2bb7-4a27-a9bc-3e4a6eb8b2c6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b56e609d5e3b7c92a887a98ae5d02bfbfb7a78e6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848494"
---
# <a name="bootcfg-timeout"></a>bootcfg timeout

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifica il valore di timeout del sistema operativo.

## <a name="syntax"></a>Sintassi

```
bootcfg /timeout <timeOutValue> [/s <computer> [/u <Domain\User>/p <Password>]]
```

### <a name="parameters"></a>Parametri


|        Parametro        |                                                                                                                                                                                  Descrizione                                                                                                                                                                                   |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <timeOutValue>/timeout | Specifica il valore di timeout nella sezione [boot loader]. Il <timeOutValue> è il numero di secondi, l'utente deve selezionare un sistema operativo dalla schermata del caricatore di avvio prima di NTLDR carica il valore predefinito. Intervallo valido per <timeOutValue> è compreso tra 0 e 999. Se il valore è 0, NTLDR avvia immediatamente il sistema operativo predefinito senza visualizzare la schermata del caricatore di avvio. |
|      /s <computer>      |                                                                                                                               Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale.                                                                                                                               |
|    /u < dominio\utente >     |                                                                                       Esegue il comando con le autorizzazioni dell'account dell'utente specificato da <User> o < dominio\utente >. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso.                                                                                        |
|      /p <Password>      |                                                                                                                                            Specifica il <Password> dell'account utente specificato nella **/u** parametro.                                                                                                                                             |
|           /?            |                                                                                                                                                                      Visualizza la guida al prompt dei comandi.                                                                                                                                                                      |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Gli esempi seguenti illustrano come utilizzare il **bootcfg /timeout** comando:
```
bootcfg /timeout 30
bootcfg /s srvmain /u maindom\hiropln /p p@ssW23 /timeout 50
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
