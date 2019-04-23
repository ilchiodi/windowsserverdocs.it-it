---
title: REPAIRSERVICE e bitsadmin util
description: Argomento i comandi di Windows per **util bitsadmin e repairservice** -comando usato per risolvere i problemi noti con diverse versioni del servizio BITS.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: bc5101378a389c865f5753146b711be0d15c6785
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852092"
---
# <a name="bitsadmin-util-and-repairservice"></a>REPAIRSERVICE e bitsadmin util

Se BITS non viene avviato, è possibile usare questa opzione per risolvere i problemi noti con diverse versioni di bit.

**BITSAdmin 1.5 e versioni precedenti:** non supportato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /Util /RepairService [/Force]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Force|Facoltativo: Elimina e ricrea il servizio.|

## <a name="remarks"></a>Note

Questa opzione consente di risolvere gli errori correlati al corretto servizio configurazione e le dipendenze da servizi di Windows (ad esempio LANManworkstation) e alla directory di rete. Questa opzione genera output che indica se i problemi che sono stati risolti.

> [!NOTE]
> Se BITS ricrea il servizio, la stringa di descrizione servizio può essere impostata su inglese in un sistema localizzato.

> [!IMPORTANT]
> Questo comando non è supportato in Windows Vista.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente consente di ripristinare la configurazione del servizio BITS.
```
C:\>bitsadmin /Util /RepairService
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)