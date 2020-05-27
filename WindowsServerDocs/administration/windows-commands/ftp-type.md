---
title: tipo FTP
description: Argomento di riferimento per il comando di tipo FTP, che consente di impostare o visualizzare il tipo di trasferimento di file.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6e96dcd4-08f8-4e7b-90b7-1e1761fea4c7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 02b6d5b4bd7944c9f4126ba4877360de02586cfb
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820271"
---
# <a name="ftp-type"></a>tipo FTP

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta o Visualizza il tipo di trasferimento di file. Il comando **FTP** supporta i tipi di trasferimento di file di immagine binari e ASCII (impostazione predefinita):

- Si consiglia di utilizzare ASCII per il trasferimento di file di testo. In QUESTA modalità, vengono eseguite le conversioni di caratteri da e verso il set di caratteri standard di rete. Ad esempio, caratteri di fine della riga vengono convertiti in base alle esigenze, in base al sistema operativo di destinazione.

- Quando si trasferiscono file eseguibili, è consigliabile usare binary. In modalità binaria, i file vengono trasferiti in unità di un byte.

## <a name="syntax"></a>Sintassi

```
type [<typename>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `[<typename>]` | Specifica il tipo di trasferimento di file. Se non si specifica questo parametro, viene visualizzato il tipo corrente.|

### <a name="examples"></a>Esempi

Per impostare il tipo di trasferimento file su ASCII, digitare:

```
type ascii
```

Per impostare il tipo di file di trasferimento su Binary, digitare:

```
type binary
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [Altre informazioni aggiuntive sull'FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
