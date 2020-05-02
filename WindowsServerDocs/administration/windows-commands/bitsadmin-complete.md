---
title: bitsadmin complete
description: Argomento di riferimento per il comando Bitsadmin complete, che completa il processo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a5e86677-8f7b-43b3-929e-97706c57e7f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b61f3475afdb0e29e5777940e6426a04fe33e78
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718223"
---
# <a name="bitsadmin-complete"></a>bitsadmin complete

Completa il processo. Usare questa opzione dopo che il processo è stato spostato sullo stato trasferito. In caso contrario, saranno disponibili solo i file che sono stati trasferiti correttamente.

## <a name="syntax"></a>Sintassi

```
bitsadmin /complete <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| processo | Nome visualizzato o GUID del processo. |

## <a name="example"></a>Esempio

Per completare il processo *myDownloadJob* , dopo aver raggiunto lo `TRANSFERRED` stato:

```
bitsadmin /complete myDownloadJob
```

Se più processi usano *myDownloadJob* come nome, è necessario usare il GUID del processo per identificarlo in modo univoco per il completamento.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
