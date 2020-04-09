---
title: bitsadmin getminretrydelay
description: Argomento dei comandi di Windows per **BITSAdmin getminretrydelay**, che consente di recuperare il tempo, in secondi, di attesa del servizio dopo aver rilevato un errore temporaneo prima di provare a trasferire il file.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 54f0abab-c129-40ed-a603-50f464d26011
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d79ffdf1f45b0198b4af535ed83154c3c2ec24f4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850624"
---
# <a name="bitsadmin-getminretrydelay"></a>bitsadmin getminretrydelay

Recupera l'intervallo di tempo, in secondi, durante il quale il servizio resterà in attesa di un errore temporaneo prima di tentare di trasferire il file.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getminretrydelay <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente viene recuperato il ritardo minimo tra tentativi per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /getminretrydelay myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)