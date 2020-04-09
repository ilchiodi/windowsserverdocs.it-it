---
title: Bitsadmin util e REPAIRSERVICE
description: Windows Commands Topic for Bitsadmin util and REPAIRSERVICE, che corregge i problemi noti in varie versioni del servizio BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ac7baeb-4340-4186-bfcb-66478195378d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: aaaa6edab22031dc53d266984bb669634e3bb362
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848894"
---
# <a name="bitsadmin-util-and-repairservice"></a>Bitsadmin util e REPAIRSERVICE

Se non è possibile avviare BITS, usare questa opzione per correggere i problemi noti in diverse versioni di BITS.

**BITSAdmin 1,5 e versioni precedenti:**  non supportato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /Util /RepairService [/Force]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Force|Facoltativo: Elimina e ricrea il servizio.|

## <a name="remarks"></a>Note

Questa opzione risolve gli errori correlati alla configurazione e alle dipendenze del servizio non corrette nei servizi Windows (ad esempio LANManworkstation) e nella directory di rete. Questa opzione genera un output che indica se i problemi sono stati risolti.

> [!NOTE]
> Se BITS ricrea il servizio, la stringa di descrizione del servizio può essere impostata su inglese in un sistema localizzato.

> [!IMPORTANT]
> Questo comando non è supportato in Windows Vista.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente consente di ripristinare la configurazione del servizio BITS.
```
C:\>bitsadmin /Util /RepairService
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)