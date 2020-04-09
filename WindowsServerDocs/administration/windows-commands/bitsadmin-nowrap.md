---
title: bitsadmin nowrap
description: Windows Commands Topic for **BITSAdmin nowrap**, che tronca qualsiasi riga di testo di output che si estende oltre il bordo all'estrema destra della finestra di comando.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 85a47b90-783a-41e4-96f2-81f26ae8ca93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f9f1db370d8a8917aa03a414a27623a1024df192
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850184"
---
# <a name="bitsadmin-nowrap"></a>bitsadmin nowrap

Tronca qualsiasi riga di testo di output che si estende oltre il bordo destro della finestra di comando. Per impostazione predefinita, tutte le opzioni, ad eccezione dell'opzione di **monitoraggio** , incapsulano l'output. Specificare l'opzione **nowrap** prima di altre opzioni.

## <a name="syntax"></a>Sintassi

```
bitsadmin /nowrap
```

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente viene recuperato lo stato per il processo denominato *myDownloadJob* e non viene eseguito il wrapping dell'output.

```
C:\>bitsadmin /nowrap /getstate myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)