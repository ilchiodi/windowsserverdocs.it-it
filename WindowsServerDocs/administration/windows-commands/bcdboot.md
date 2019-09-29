---
title: bcdboot
description: "Argomento dei comandi di Windows per **BCDboot** : configurare rapidamente una partizione di sistema o ripristinare l'ambiente di avvio che si trova nella partizione di sistema."
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: a1c0f505180a503617335cc9575fea3d346bbe02
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383444"
---
# <a name="bcdboot"></a>bcdboot



Consente di configurare rapidamente una partizione di sistema o di ripristinare l'ambiente di avvio che si trova nella partizione di sistema. La partizione di sistema viene configurata copiando un semplice set di file di dati configurazione di avvio (BCD) in una partizione vuota esistente.

Per ulteriori informazioni su BCDboot, incluse informazioni su dove trovare BCDboot ed esempi sull'uso di questo comando, vedere l'argomento [Opzioni della riga di comando di BCDboot](https://technet.microsoft.com/library/hh824874.aspx) .

## <a name="syntax"></a>Sintassi

```
bcdboot <source> [/l] [/s]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|source|Specifica il percorso della directory di Windows da utilizzare come origine per la copia dei file dell'ambiente di avvio.|
|/l|Specifica le impostazioni locali. Le impostazioni locali predefinite sono inglesi (Stati Uniti).|
|/s|Specifica la lettera del volume della partizione di sistema. Il valore predefinito è la partizione di sistema identificata dal firmware.|

## <a name="BKMK_examples"></a>Esempi

Per ulteriori esempi di utilizzo di questo comando, vedere l'argomento [Opzioni della riga di comando BCDboot](https://technet.microsoft.com/library/hh824874.aspx) .

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)