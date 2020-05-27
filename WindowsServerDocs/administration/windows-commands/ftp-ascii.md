---
title: ASCII FTP
description: Argomento di riferimento per il comando FTP ASCII, che imposta il tipo di trasferimento di file su ASCII.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 523be48e-eab0-4237-8fb5-ca222824f0b6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f9bf3f278bb478c7244f90533a689f41fd910783
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820041"
---
# <a name="ftp-ascii"></a>ASCII FTP

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta il tipo di trasferimento di file in formato ASCII. Il comando **FTP** supporta sia i tipi ASCII (impostazione predefinita) sia i tipi di trasferimento di file di immagine binari, ma è consigliabile utilizzare ASCII per il trasferimento di file di testo. In QUESTA modalità, vengono eseguite le conversioni di caratteri da e verso il set di caratteri standard di rete. Ad esempio, caratteri di fine della riga vengono convertiti in base alle esigenze, in base al sistema operativo di destinazione.

## <a name="syntax"></a>Sintassi

```
ascii
```

### <a name="examples"></a>Esempi

Per impostare il tipo di trasferimento file su ASCII, digitare:

```
ascii
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando binario FTP](ftp-binary.md)

- [Altre informazioni aggiuntive sull'FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
