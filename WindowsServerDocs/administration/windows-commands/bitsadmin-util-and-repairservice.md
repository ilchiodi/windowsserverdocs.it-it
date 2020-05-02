---
title: bitsadmin util e repairservice
description: Argomento di riferimento per il comando Bitsadmin util e REPAIRSERVICE, che corregge i problemi noti in varie versioni del servizio BITS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ac7baeb-4340-4186-bfcb-66478195378d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0104a3f2ace972821151bf5083f9b0795e427ff1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82707649"
---
# <a name="bitsadmin-util-and-repairservice"></a>bitsadmin util e repairservice

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

Per ripristinare la configurazione del servizio BITS:

```
bitsadmin /util /repairservice
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando util Bitsadmin](bitsadmin-util.md)

- [comando Bitsadmin](bitsadmin.md)
