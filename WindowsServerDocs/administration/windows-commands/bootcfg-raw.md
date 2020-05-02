---
title: bootcfg raw
description: Argomento di riferimento per il comando bootcfg RAW, che aggiunge le opzioni di caricamento del sistema operativo, specificate come stringa, a una voce del sistema operativo nella sezione del sistema operativo del file Boot. ini.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e3458749-b0a0-460f-a022-3ff199a71f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b9127b278a41bb8f1f36f7b45c544bf09f5c4ff7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82708950"
---
# <a name="bootcfg-raw"></a>bootcfg raw

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Aggiunge le opzioni di caricamento del sistema operativo specificate come stringa a una voce del sistema operativo di [i sistemi operativi] sezione del file Boot. ini. Questo comando sovrascrive eventuali opzioni di immissione del sistema operativo esistenti.

## <a name="syntax"></a>Sintassi

```
bootcfg /raw [/s <computer> [/u <domain>\<user> /p <password>]] <osloadoptionsstring> [/id <osentrylinenum>] [/a]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `/s <computer>` | Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale. |
| `/u <domain>\<user>`  | Esegue il comando con le autorizzazioni dell'account dell'utente specificato da `<user>` o `<domain>\<user>`. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso. |
| `/p <password>` | Specifica la password dell'account utente specificato nella **/u** parametro. |
| `<osloadoptionsstring>` | Specifica le opzioni di caricamento del sistema operativo da aggiungere alla voce del sistema operativo. Queste opzioni di caricamento sostituiscono eventuali opzioni di caricamento esistenti associate alla voce del sistema operativo. Non esiste alcuna convalida per il `<osloadoptions>` parametro.
| `/id <osentrylinenum>` | Specifica il numero di riga voce del sistema operativo in della sezione [operating systems] del file Boot. ini alla quale aggiungere opzioni di caricamento del sistema operativo. La prima riga dopo la sezione [operating systems] sezione di intestazione è 1. |
| /a | Specifica le opzioni del sistema operativo da aggiungere alle opzioni del sistema operativo esistente. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="examples"></a>Esempi

Il testo deve contenere opzioni di caricamento del sistema operativo valido, ad esempio **/debug**, **/fastdetect**, **/nodebug**, **/baudrate**, **/crashdebug**, e **/sos**.

Per aggiungere **/debug/fastdetect** alla fine della prima voce del sistema operativo, sostituendo le opzioni di immissione del sistema operativo precedenti:

```
bootcfg /raw /debug /fastdetect /id 1
```

Per usare il comando **bootcfg/RAW** :

```
bootcfg /raw /debug /sos /id 2
bootcfg /raw /s srvmain /u maindom\hiropln /p p@ssW23 /crashdebug  /id 2
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando bootcfg](bootcfg.md)
