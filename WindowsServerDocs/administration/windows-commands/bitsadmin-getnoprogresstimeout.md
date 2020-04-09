---
title: bitsadmin getnoprogresstimeout
description: Windows Commands Topic for **BITSAdmin getnoprogresstimeout**, che consente di recuperare il periodo di tempo, in secondi, durante il quale il servizio tenterà di trasferire il file dopo un errore temporaneo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9cd9b19b-cbb4-4352-8419-978080f016b6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cf2cfd77b494e221b60c8816ff46eed5f9252f39
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850604"
---
# <a name="bitsadmin-getnoprogresstimeout"></a>bitsadmin getnoprogresstimeout

Recupera l'intervallo di tempo, in secondi, durante il quale il servizio tenterà di trasferire il file dopo che si è verificato un errore temporaneo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getnoprogresstimeout <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente viene recuperato il valore di timeout dello stato di avanzamento per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /getnoprogresstimeout myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)