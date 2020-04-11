---
title: bitsadmin setminretrydelay
description: Argomento dei comandi di Windows per **BITSAdmin setminretrydelay**, che consente di impostare il periodo di tempo minimo, in secondi, che BITS attende dopo aver rilevato un errore temporaneo prima di provare a trasferire il file.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ce8674ca-6cc5-4bb2-8dda-7dfbb1cd6830
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ddae9a62a49ca07bb03649f131a0a1ebad8ee3fe
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122889"
---
# <a name="bitsadmin-setminretrydelay"></a>bitsadmin setminretrydelay

Imposta il periodo di tempo minimo, espresso in secondi, di attesa dei bit dopo che si è verificato un errore temporaneo prima di provare a trasferire il file.

## <a name="syntax"></a>Sintassi

```
bitsadmin /setminretrydelay <job> <retrydelay>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| lavoro | Nome visualizzato o GUID del processo. |
| retrydelay | Tempo minimo di attesa dei bit dopo un errore durante il trasferimento, in secondi. |

## <a name="examples"></a>Esempi

Nell'esempio seguente imposta il ritardo minimo per il processo denominato *myDownloadJob* a 35 secondi.

```
C:\>bitsadmin /setminretrydelay myDownloadJob 35
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)