---
title: Bitsadmin util e REPAIRSERVICE
description: Windows Commands Topic for **Bitsadmin util and REPAIRSERVICE**, che corregge i problemi noti in varie versioni del servizio BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ac7baeb-4340-4186-bfcb-66478195378d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 164a402e7cbfc0a9223a97f4246eac84f0797aed
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122511"
---
# <a name="bitsadmin-util-and-repairservice"></a>Bitsadmin util e REPAIRSERVICE

Se non è possibile avviare BITS, questa opzione tenta di risolvere gli errori correlati alla configurazione e alle dipendenze non corrette dei servizi Windows (ad esempio LANManworkstation) e alla directory di rete. Questa opzione genera anche un output che indica se i problemi sono stati risolti.

> [!NOTE]
> Questo comando non è supportato da BITS 1,5 e versioni precedenti.

## <a name="syntax"></a>Sintassi

```
bitsadmin /util /repairservice [/force]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /Force | Facoltativa. Elimina e crea nuovamente il servizio.|

> [!NOTE]
> Se BITS crea nuovamente il servizio, la stringa di descrizione del servizio potrebbe essere impostata su inglese anche in un sistema localizzato.

## <a name="examples"></a>Esempi

Nell'esempio seguente viene ripristinata la configurazione del servizio BITS.

```
C:\>bitsadmin /util /repairservice
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)