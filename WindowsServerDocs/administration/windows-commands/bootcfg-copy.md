---
title: bootcfg copy
description: Argomento di riferimento per il comando di copia bootcfg, che consente di eseguire una copia di una voce di avvio esistente a cui è possibile aggiungere opzioni della riga di comando.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2a236c2a-8675-444d-b695-9cbc9aff643b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 033227ab1d9efcdf1d58708a75085067766a6af2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82709678"
---
# <a name="bootcfg-copy"></a>bootcfg copy

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea una copia di una voce di avvio esistente, a cui è possibile aggiungere opzioni della riga di comando.

## <a name="syntax"></a>Sintassi

```
bootcfg /copy [/s <computer> [/u <domain>\<user> /p <password>]] [/d <description>] [/id <osentrylinenum>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `/s <computer>` | Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale. |
| `/u <domain>\<user>`  | Esegue il comando con le autorizzazioni dell'account dell'utente specificato da `<user>` o `<domain>\<user>`. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso. |
| `/p <password>` | Specifica la password dell'account utente specificato nella **/u** parametro. |
| `/d <description>` | Specifica la descrizione per la nuova voce del sistema operativo. |
| `/id <osentrylinenum>` | Specifica il numero di riga voce del sistema operativo in della sezione [operating systems] del file Boot. ini alla quale aggiungere opzioni di caricamento del sistema operativo. La prima riga dopo la sezione [operating systems] sezione di intestazione è 1. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="examples"></a>Esempi

Per copiare la voce di avvio 1 e immettere \ABC Server \ come descrizione:

```
bootcfg /copy /d \ABC Server\ /id 1
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando bootcfg](bootcfg.md)
