---
title: Bitsadmin util e REPAIRSERVICE
description: Argomento dei comandi di Windows per **Bitsadmin util e REPAIRSERVICE** -Command usati per risolvere i problemi noti con diverse versioni del servizio BITS.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ac7baeb-4340-4186-bfcb-66478195378d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ab06ac9c784cfa438eb285c28f0e661cf4b8302
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380284"
---
# <a name="bitsadmin-util-and-repairservice"></a>Bitsadmin util e REPAIRSERVICE

Se non è possibile avviare BITS, usare questa opzione per correggere i problemi noti con diverse versioni di BITS.

**BITSAdmin 1,5 e versioni precedenti:**  non supportato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /Util /RepairService [/Force]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Force|Facoltativo: Elimina e ricrea il servizio.|

## <a name="remarks"></a>Osservazioni

Questa opzione risolve gli errori correlati alla configurazione e alle dipendenze del servizio non corrette nei servizi Windows (ad esempio LANManworkstation) e nella directory di rete. Questa opzione genera un output che indica se i problemi sono stati risolti.

> [!NOTE]
> Se BITS ricrea il servizio, la stringa di descrizione del servizio può essere impostata su inglese in un sistema localizzato.

> [!IMPORTANT]
> Questo comando non è supportato in Windows Vista.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente consente di ripristinare la configurazione del servizio BITS.
```
C:\>bitsadmin /Util /RepairService
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)