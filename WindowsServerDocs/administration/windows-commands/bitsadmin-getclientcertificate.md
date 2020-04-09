---
title: GetClientCertificate Bitsadmin
description: Windows Commands Topic for **BITSAdmin GetClientCertificate**, che recupera il certificato client dal processo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4fc8f408-085e-43a0-9fa8-3d798ef107b1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7c29d5c64fd172cfdd2d5d93c5ed22d519077806
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850764"
---
# <a name="bitsadmin-getclientcertificate"></a>GetClientCertificate Bitsadmin

Recupera il certificato client dal processo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getclientcertificate <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente viene recuperato il certificato client per il processo denominato *myDownloadJob*.

```
C:\>bitsadmin /getclientcertificate myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)