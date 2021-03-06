---
title: bcdboot
description: Argomento di riferimento per il comando BCDboot, che consente di configurare rapidamente una partizione di sistema o di ripristinare l'ambiente di avvio che si trova nella partizione di sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e62a250e-08e9-47f6-89d1-e6002560ab57
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 67aacb3a575e0cdd08af5372b403916961d223c6
ms.sourcegitcommit: ef089864980a1d4793a35cbf4cbdd02ce1962054
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/28/2020
ms.locfileid: "84149774"
---
# <a name="bcdboot"></a>bcdboot

Consente di configurare rapidamente una partizione di sistema o di ripristinare l'ambiente di avvio che si trova nella partizione di sistema. La partizione di sistema viene configurata copiando un semplice set di file di dati configurazione di avvio (BCD) in una partizione vuota esistente.

## <a name="syntax"></a>Sintassi

```
bcdboot <source> [/l] [/s]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| origine | Specifica il percorso della directory di Windows da utilizzare come origine per la copia dei file dell'ambiente di avvio. |
| /l | Specifica le impostazioni locali. Le impostazioni locali predefinite sono inglesi (Stati Uniti). |
| /s | Specifica la lettera del volume della partizione di sistema. Il valore predefinito è la partizione di sistema identificata dal firmware. |

## <a name="examples"></a>Esempi

Per informazioni su dove trovare BCDboot ed esempi sull'uso di questo comando, vedere l'argomento [Opzioni della riga di comando di BCDboot](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh824874(v=win.10)) .

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
