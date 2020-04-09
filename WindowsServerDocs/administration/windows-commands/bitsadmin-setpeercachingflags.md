---
title: setpeercachingflags Bitsadmin
description: Windows Commands argomento for Bitsadmin setpeercachingflags, che imposta i flag che determinano se i file del processo possono essere memorizzati nella cache e serviti ai peer e se il processo può scaricare il contenuto dai peer.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3f54a127-fb68-49a5-b843-664ec833df67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d19e4d14b47e4aa96e9ad9d4367e872350ad4d43
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849244"
---
# <a name="bitsadmin-setpeercachingflags"></a>setpeercachingflags Bitsadmin

Imposta i flag che determinano se i file del processo possono essere memorizzato nella cache e forniti a colleghi e se il processo è possibile scaricare contenuto da peer.

## <a name="syntax"></a>Sintassi

```
bitsadmin /SetPeerCachingFlags <Job> <value> 
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Job|Nome visualizzato o il GUID del processo|
|Valore|Il valore è un intero senza segno con l'interpretazione seguente per i bit nella rappresentazione binaria.</br>1-il processo può scaricare il contenuto dai peer.</br>2-i file del processo possono essere memorizzati nella cache e serviti ai peer.|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente imposta i flag per il processo denominato *myJob* che consente di scaricare contenuto da peer.
```
C:\>bitsadmin / SetPeerCachingFlags myJob 1 
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)