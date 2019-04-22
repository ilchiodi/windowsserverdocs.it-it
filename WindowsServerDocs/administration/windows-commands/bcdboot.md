---
title: bcdboot
description: Argomento i comandi di Windows per **bcdboot** - rapidamente consente di impostare una partizione di sistema, o ripristinare l'ambiente di avvio che si trova nella partizione di sistema.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e62a250e-08e9-47f6-89d1-e6002560ab57
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 78838dd6567ad886948df8ac21425a8f9b596d5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825882"
---
# <a name="bcdboot"></a>bcdboot



Consente di configurare rapidamente una partizione di sistema o per ripristinare l'ambiente di avvio che si trova nella partizione di sistema. La partizione di sistema viene configurata tramite la copia di un semplice set di file di dati di configurazione di avvio (BCD) a una partizione vuota esistente.

Per altre informazioni su BCDboot, incluse le informazioni su dove trovare BCDboot ed esempi su come usare questo comando, vedere la [opzioni della riga di comando di BCDboot](https://technet.microsoft.com/library/hh824874.aspx) argomento.

## <a name="syntax"></a>Sintassi

```
bcdboot <source> [/l] [/s]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|origine|Specifica il percorso della directory di Windows da utilizzare come origine per la copia dei file dell'ambiente di avvio.|
|/l|Specifica le impostazioni locali. Le impostazioni locali predefinite sono inglese Stati Uniti.|
|/s|Specifica la lettera di volume della partizione di sistema. Il valore predefinito è la partizione di sistema identificata dal firmware.|

## <a name="BKMK_examples"></a>Esempi

Per altri esempi di come usare questo comando, vedere la [opzioni della riga di comando di BCDboot](https://technet.microsoft.com/library/hh824874.aspx) argomento.

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)