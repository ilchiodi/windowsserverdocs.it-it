---
title: bitsadmin getsecurityflags
description: Argomento di riferimento per il comando Bitsadmin getsecurityflags, che segnala i flag di sicurezza HTTP per il reindirizzamento dell'URL e i controlli eseguiti sul certificato del server durante il trasferimento.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c2e73519-34f4-487b-af11-97d5d08ef9bb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 41b710f9897f24eb4161d9379dc3b1f89b141472
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717546"
---
# <a name="bitsadmin-getsecurityflags"></a>bitsadmin getsecurityflags

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Flag di protezione report HTTP per il reindirizzamento degli URL e i controlli eseguiti sul certificato del server durante il trasferimento.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getsecurityflags <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="examples"></a>Esempi

Per recuperare i flag di sicurezza da un processo denominato *myDownloadJob*:

```
bitsadmin /getsecurityflags myDownloadJob
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
