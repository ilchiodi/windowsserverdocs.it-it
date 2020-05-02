---
title: bitsadmin wrap
description: Argomento di riferimento per il comando Bitsadmin wrap, che esegue il wrapping di qualsiasi riga di testo di output che si estende oltre il bordo all'estrema destra della finestra di comando alla riga successiva.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 14e57522-539d-4621-ad15-09f7a44ccab7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8c1c2c78fd3cc78674ef497526ba236ad058fe83
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82707569"
---
# <a name="bitsadmin-wrap"></a>bitsadmin wrap

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esegue il wrapping di tutte le righe di output testo che si estende oltre il bordo all'estrema destra della finestra di comando nella riga successiva. È necessario specificare questa opzione prima di tutte le altre opzioni.

Per impostazione predefinita, tutte le opzioni, ad eccezione dell'opzione di [monitoraggio Bitsadmin](bitsadmin-monitor.md) , incapsulano il testo di output.

## <a name="syntax"></a>Sintassi

```
bitsadmin /wrap <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ---------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Per recuperare le informazioni per il processo denominato *myDownloadJob* ed eseguire il wrapping del testo di output:

```
bitsadmin /wrap /info myDownloadJob /verbose
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
